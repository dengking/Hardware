# RVV（RISC-V Vector Extension）完整详解

RVV 是 RISC-V 官方标准向量扩展，**v1.0 2021 正式冻结**，对标 ARM SVE（可变长向量），区别于 x86 AVX/ARM NEON 固定宽度SIMD；主打**向量长度无关（Vector-Length Agnostic）**，一套二进制可跑不同VLEN硬件，是RISC-V AI、DSP、图像信号处理的核心加速底座。

## 一、核心设计：与传统SIMD本质区别

### 1. 传统SIMD（AVX/NEON）

寄存器位宽硬编码：AVX2=256bit、NEON=128bit；
代码绑定硬件宽度，换更宽CPU必须重新编译，尾元素靠标量循环手动处理。

### 2. RVV 可变长向量（VLA）

硬件寄存器宽度`VLEN`可配置（128/256/512bit），**软件不写死向量元素个数**；
运行时通过`vsetvli`动态设置单次运算元素数量`VL`，硬件自动处理尾部不足一向量的元素，无多余标量分支。

## 二、核心硬件与控制参数（必懂四元组）

### 1. 硬件固定参数（芯片出厂确定）

1. **VLEN**：单个向量寄存器物理位宽，≥128bit，常见256/512bit
2. **ELEN**：硬件支持最大元素位宽，一般32/64bit（int32/float32/double）
3. 向量寄存器：32个 `v0~v31`；v0同时可作为掩码寄存器（mask）

### 2. 运行时动态参数（vtype CSR配置）

#### SEW（Selected Element Width）

当前向量元素位宽：8/16/32/64bit（i8/u8/f16/f32/double）

#### LMUL（寄存器分组倍数）

把多个向量寄存器合并成一组使用，取值 `1/8,1/4,1/2,1,2,4,8`

- m1：只用1个v寄存器；m2：2个v寄存器拼接，单次处理元素翻倍
- 公式：**VLMAX = LMUL × VLEN / SEW**
  VLMAX：当前配置下单条向量指令能处理的**最大元素个数**

#### VL（Vector Length）

本次循环实际要处理的元素数量，`0 < VL ≤ VLMAX`，由`vsetvli`返回。

#### 示例计算

VLEN=256bit，SEW=32bit(float)，LMUL=1
VLMAX = 1 × 256 / 32 = 8，一次最多算8个float。

### 3. 灵魂指令：vsetvli

```asm
vsetvli t0, a0, e32, m1, ta, ma
# t0 = 返回本次有效VL
# a0 = 用户期望处理总元素数
# e32 = SEW=32bit，m1=LMUL=1
# ta/ma：尾元素策略、内存访存对齐策略
```

每条向量循环开头必须调用，更新`vtype`/`vl`两个CSR寄存器，全局生效。

## 三、关键硬件特性（AI推理友好）

1. **原生掩码 Mask（v0）**
   每条算术/访存指令可加`.m`后缀，仅mask=1的元素参与计算，尾部数据天然支持掩码，不用分支跳转，量化CNN/稀疏矩阵性能大幅提升。
   
   ```asm
   vadd.vm v1, v2, v3, v0 # 带掩码向量加法
   ```

2. **丰富数据类型转换**
   i8/i16/f16/f32双向拓宽/截断，完美适配深度学习int8量化推理。

3. **分段内存访问（stride/单位置/块加载）**
   `vle`连续加载、`vlse`跨步加载、`vlux`非对齐加载，适配图像通道、卷积滑动窗口。

4. **寄存器分组LMUL**
   超大向量运算（矩阵乘）用m2/m4，一次性吞吐更多数据，减少循环次数。

5. **浮点/整数完整向量指令集**
   加减乘除、乘累加`vmacc`、归约求和`vredsum`、比较、移位、重排、洗牌。

## 四、RVV 1.0 vs 旧版0.7.1（工程避坑）

- 0.7.1：早期草稿，玄铁C910等老芯片在用，指令编码、vsetvli参数、内建函数不兼容；
- 1.0：标准稳定版，GCC/LLVM主线统一支持，新项目全部基于RVV1.0开发。

## 五、三层编程模型（从底层到上层）

### 1. 汇编层

直接写`vadd.vv`/`vle32.v`等向量指令，需手动管理`vsetvli`、mask、VL循环。

### 2. C 内建函数 intrinsics（主流开发）

头文件`<riscv_vector.h>`，类型`vfloat32m1_t`（float32，LMUL=m1），编译器自动插入`vsetvli`，屏蔽汇编细节。

```c
void saxpy(float *a, float *b, float *c, size_t n, float alpha) {
  size_t vl;
  for (; n > 0; n -= vl) {
    vl = __riscv_vsetvl_e32m1(n);
    vfloat32m1_t va = __riscv_vle32_v_f32m1(a, vl);
    vfloat32m1_t vb = __riscv_vle32_v_f32m1(b, vl);
    vfloat32m1_t vc = __riscv_vfmacc_vf_f32m1(vb, alpha, va, vl);
    __riscv_vse32_v_f32m1(c, vc, vl);
    a += vl; b += vl; c += vl;
  }
}
```

### 3. 编译器自动向量化（LLVM/MLIR/TVM）

不用手写intrinsic，循环经过向量化Pass自动生成RVV指令，AI框架核心依赖这条路线。

## 六、RVV 与 MLIR / TVM 编译链路（结合你前面问的内容）

### 1. MLIR 对 RVV 的支持

MLIR提供两层抽象对接RVV：

1. **通用 Vector Dialect**：平台无关可变长向量表示（`vector<vscale x N x T>`），适配RVV/SVE统一抽象；

2. **RVV Dialect**：RVV专属方言，封装`vsetvli`、向量load/store、掩码、LMUL等硬件特性；
   编译流水线：
   
   ```
   Linalg张量循环 → Vector Dialect向量化 → RVV Dialect Lowering → LLVM IR(RISCV-V intrinsics) → RVV机器码
   ```
   
   IREE、Buddy Compiler、Triton-RISCV均基于该链路做AI算子向量化。

### 2. TVM 对 RVV 的支持

TVM 在 **TIR张量IR** 层实现RVV向量化，搭配AutoScheduler/MetaSchedule自动搜索最优分块、LMUL、VL调度参数：

```
Relay计算图 → TIR循环分块 → TIR Vectorize向量化Pass → LLVM RISCV后端生成RVV
```

优势：自动针对RVV硬件做tile切分、掩码尾部优化、乘累加向量化，是RISC-V端AI模型部署主流工具链。

### 3. 统一底层基座：LLVM RISCV后端

LLVM原生支持RVV1.0，识别`vscale`可变长向量IR，自动选择RVV指令、管理vsetvli插入、mask生成；MLIR/TVM最终都下沉到LLVM IR产出RVV汇编。

## 七、RVV 适用场景

1. 边缘AI推理：int8量化CNN、LLM向量运算、矩阵乘加速；
2. DSP信号处理：FFT、滤波、音频编解码；
3. 多媒体：图像像素并行、视频缩放/滤波；
4. HPC高性能计算：科学数值模拟；
5. 嵌入式通用并行计算：数据拷贝、加密哈希。

## 八、RVV vs ARM SVE（同类可变长向量对比）

| 特性     | RVV 1.0        | ARM SVE               |
| ------ | -------------- | --------------------- |
| 寄存器数量  | 32个v0~v31      | 32个z0~z31             |
| 掩码寄存器  | v0复用，零额外寄存器    | 独立p0-p15 predicate寄存器 |
| LMUL分组 | 支持1/8~8倍寄存器拼接  | 无分组，仅单寄存器向量           |
| 标准状态   | RISC-V开源免费标准   | ARM私有授权扩展             |
| 工具链    | GCC/LLVM完整开源支持 | 依赖ARM商业工具链            |
| 硬件成本   | 嵌入式轻量化实现友好     | 面向高性能大核               |

## 九、一句话总结

RVV是RISC-V开源、硬件可配置的**可变长向量ISA**，核心创新是运行时动态向量长度，一套代码跨不同位宽RISC-V芯片；通过LLVM、MLIR、TVM完整工具链支撑AI与DSP高性能并行计算，是RISC-V生态向量加速的事实标准。

需要我给你一段MLIR Vector Dialect → RVV Dialect 的极简示例代码，或者TVM TIR RVV向量化模板吗？


