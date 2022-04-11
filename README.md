# Chisel Demo Environment

## 环境

+ ubuntu
+ mill
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