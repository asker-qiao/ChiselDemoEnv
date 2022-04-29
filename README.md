# Chisel Demo Environment

## 环境

+ ubuntu
+ mill 0.10.3
+ verilator
+ riscv-linux-gnu*
+ gtkwave

## 项目简介

### chisel-demo

一个chisel搭建的RISC-V单周期框架，风格偏向于经典五级流水线，意在便于向五级流水线过渡。

### NEMU

香山平台的全系统模拟器。在此处作为difftest的ref对象。

### abstract-machine

抽象机模型，提供多个不同的系统平台，用于生成对应的系统平台的可执行程序。

### am-kernels

提供一些源码文件，用于和abstract-machine一起编译成不同系统平台的程序。

## 实验流程

设置环境变量
```bash
chisel-demo-env$ source env.sh
```

### 编译模拟器

```bash
chisel-demo-env$ cd NEMU
chisel-demo-env/NEMU$ make riscv64-xs-ref_defconfig
chisel-demo-env/NEMU$ make -j
```

在
### 编译程序

```bash
chisel-demo-env$ cd am-kernels/tests/cpu-tests
chisel-demo-env/am-kernels/tests/cpu-tests$ make ARCH=riscv64-mycpu 
chisel-demo-env/am-kernels/tests/cpu-tests$ ls build
```

可以看到在`build`目录下生成了一些简单的程序（elf、bin、反汇编文件txt）用于后续处理器的测试。

### 生成处理器

在chisel-demo目录下执行Makefile脚本
```bash
chisel-demo-env$ cd chisel-demo
chisel-demo-env/chisel-demo$ make sim-verilog
```
会在`chisel-demo-env/chisel-demo/build`下生成SimTop.v等verilog文件。

### verilator生成仿真程序
假设已经安装好了verilator工具
```bash
chisel-demo-env/chisel-demo$ make emu
```
此时会在`chisel-demo-env/chisel-demo/build`下生成emu的elf文件，该文件即为处理器的仿真文件。
运行生成的仿真文件：
```bash
chisel-demo-env/chisel-demo$ build/emu -i ../am-kernels/tests/cpu-tests/build/dummy-riscv64-mycpu.bin --dump-wave --wave-path=wave.vcd
```

+ -i 参数是指定处理器仿真程序加载的测试程序
+ --dump-wave 是打开波形生成使能
+ --wave-path 是指定生成的波形文件，可忽略，忽略该参数将在`chisel-demo/build`目录下默认生成

执行上述命令后，可以看到下面的输出表示测试程序通过。
```bash
...
Core 0: HIT GOOD TRAP at pc = 0x80000030
...
```

并且在`chisel-demo-env/chisel-demo$`目录下可以看到上面指定输出的波形文件`wave.vcd`。
使用gtkwave打开该波形调试
```
gtkwave wave.vcd
```

现在的处理器只能跑通`chisel-demo-env/am-kernels/tests/cpu-tests/build`下的一个dummy-riscv64-mycpu程序，需要您来完善这个处理器，跑通`chisel-demo-env/am-kernels/tests/cpu-tests/build`目录下所有测试程序。