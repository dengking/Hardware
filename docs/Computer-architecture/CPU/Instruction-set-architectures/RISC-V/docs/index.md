# RISC-V docs

## 官方总入口（权威文档库）

### 在线浏览合集（Antora 全站，可直接打开PDF）

https://docs.riscv.org/

收录所有**已批准(Ratified)**稳定规范，无草稿，工业开发唯一标准参考。

> NOTE: **Antora** 是一套专门用来**管理、生成多版本、多仓库技术文档网站**的开源工具链（基于 Node.js，Asciidoctor 渲染）。RISC-V 官方选择 Antora 承载全部 ISA 规范，所以大家会说 **Antora 全站**。

### 官网规范总页

https://riscv.org/technical/specifications/

区分：ISA规范、Profile配置规范、非ISA配套规范、开发中草案。

### 源码仓库（可本地编译PDF）

ISA总手册：https://github.com/riscv/riscv-isa-manual

各扩展独立仓库：riscv-v-spec、riscv-b-spec、riscv-crypto等。

## 核心两大基础ISA手册（必看）

### Volume I：Unprivileged ISA（用户级指令集，20260120稳定版）

   **riscv-spec.pdf**

- 内容：RV32I/RV64I/RV128I基础整数指令；M/A/F/D/C/Zicsr/Zifencei等基础扩展；内存模型；指令编码；ABI基础；向量V扩展主体章节。

- 读者：编译器开发、应用/内核程序员、TVM/MLIR向量化开发。

### Volume II：Privileged Architecture（特权架构，20260120稳定版）

  **riscv-privileged.pdf**

- 内容：M/S/U/H四种运行模式；CSR控制状态寄存器；中断异常、PLIC；页表MMU；SBI固件接口；虚拟化扩展。

- 读者：OS内核、BIOS、CPU硬件设计、驱动开发。

## RVV向量扩展全套文档

### V扩展硬件ISA规范 V1.0（冻结标准，禁止改动）

仓库：https://github.com/riscv/riscv-v-spec

在线文档：https://docs.riscv.org/reference/isa/v20240411/unpriv/v-st-ext.html

核心内容：

- 32个v寄存器、VLEN/ELEN/SEW/LMUL定义
- `vsetvli`、向量load/store、掩码mask、算术/归约/置换指令
- 尾部ta/ma策略、vtype/vl/vstart/vcsr等向量CSR
- 硬件实现约束、内存一致性

### RVV C Intrinsic API 手册（编译器/TVM/MLIR必备）

仓库：https://github.com/riscv-non-isa/rvv-intrinsic-doc

在线查询工具：https://dzaima.github.io/intrinsics-viewer/

- 定义`<riscv_vector.h>`全部内建函数
- `vfloat32m1_t`等向量类型、LMUL/SEW参数映射
- GCC/Clang标准内置函数，手写RVV C代码唯一参考

### 中文RVV翻译文档

https://github.com/surez-ok/riscv-rvv-doc-zh

包含spec翻译+完整C intrinsics示例代码。

## 其他主流标准扩展独立规范

| 扩展         | 仓库地址                 | 用途            |
| ---------- | -------------------- | ------------- |
| B 位操作      | riscv/riscv-bitmanip | 位运算、掩码加速      |
| K 密码学      | riscv/riscv-crypto   | AES/SHA加密硬件指令 |
| P 打包SIMD   | riscv/riscv-p-spec   | DSP窄向量(区别RVV) |
| C 压缩指令     | 主手册内置                | 16位压缩指令       |
| Zfh/Zfhmin | 主手册内置                | 半精度浮点f16      |

## Non-ISA 配套系统规范（软硬件协同必备）

不属于指令集，但开发RISC-V平台必须参考：

### RISC-V ABI v1.0

定义ELF、寄存器调用约定、栈布局、数据对齐；编译器链接器标准。

### SBI Supervisor Binary Interface

M-mode固件与S-mode操作系统交互接口（OpenSBI）。

### Debug Spec（外部调试）

JTAG、DMI调试协议，OpenOCD适配依据。

### PLIC 中断控制器规范

全局中断控制器硬件标准。

### Profile 配置规范(RVA23/RVB23)

定义嵌入式/服务器标准扩展组合（比如RVA23=RV64GCV基础）。

## 中文权威文档资源

### 官方中文版ISA手册（中电标协RISC-V工委会翻译，对应20240411英文版）

仓库：https://gitee.com/riscv-ei/riscv-isa-manual

包含：

- 第一卷 非特权架构（RV32/RV64基础指令、RVV章节中文）
- 第二卷 特权架构（中断、MMU、SBI）

### 中科院CRVA免费中文PDF

  http://crva.ict.ac.cn/documents/
  《RISC-V指令集手册》入门精简中文版，适合新手快速通读。

### 国产厂商配套文档

- 芯来Nuclei（蜂鸟E203/E906/RVV核）：https://doc.nucleisys.com/
  提供MCU SDK、RVV工具链、内核用户手册
- 平头哥玄铁系列：平头哥开发者中心，含RVV优化、T-Head GCC工具链手册

## 编译器/TVM/MLIR 配套开发文档（AI向量编译方向）

### LLVM RISC-V后端文档

LLVM官网RISCV target文档，包含RVV vscale可变长向量 lowering逻辑。

### MLIR RVV Dialect 文档

MLIR官方文档 `Dialects/RISCV/RVV`，描述Vector Dialect转RVV IR流程。

### TVM RISC-V/RVV 部署文档

Apache TVM官网TIR向量化章节、MetaSchedule RVV自动调优教程。

### RuyiSDK 工具链手册（国内一站式RISC-V编译环境）

https://ruyi.iscas.ac.cn/ 

提供GCC/LLVM/QEMU/RVV编译全套教程

## 文档版本避坑要点

RVV 仅 **v1.0** 是稳定批准版；0.7.1为旧草稿，芯片/工具链不通用，不要混用。

ISA手册统一以 `YYYYMMDD` 日期为版本号，优先选用20240411及以上版本。

区分：

- Ratified（已批准，稳定商用）
- Draft（开发中，随时修改，仅做研究）

编译本地PDF：

```bash
git clone https://github.com/riscv/riscv-isa-manual
cd riscv-isa-manual
make
# build/目录生成 riscv-spec.pdf、riscv-privileged.pdf
```

## 学习阅读路线（按你的AI向量编译场景）

入门：中文精简ISA手册 → 看懂RV32/RV64基础指令、寄存器

向量核心：RVV v1.0 spec + RVV Intrinsic手册（重点vsetvli、LMUL、mask）

系统底层：特权架构SBI/PLIC（可选，做内核才需要）

编译链路：LLVM RISCV后端 → MLIR Vector/RVV Dialect → TVM TIR RVV向量化



