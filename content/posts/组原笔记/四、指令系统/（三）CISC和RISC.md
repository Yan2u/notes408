---
title: （三）CISC和RISC
date: 2025-03-23T22:22:34.002567+08:00
tags: ['组原笔记', '四、指令系统', '（三）CISC和RISC.md']
url: /posts/RLS2e21uiA
author: "Yan2u"
IsPost: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
showToc: true
TocOpen: false
ShowWordCount: true
UseHugoToc: true
---

<a href="/notes408/chapters_index"> Indexes </a> > <a href="/notes408/indexes/5RoFxkg3V7"> 组原笔记 </a> > <a href="/notes408/indexes/gOnff9ZsRW"> 四、指令系统 </a> > （三）CISC和RISC

### 1. CISC

#### 1.1. 概念

- **复杂指令系统 CISC（Complex Instruction Set Computer）**：指令系统中增加更多 (复杂) 的指令，来**提高操作系统的效率**；
- 为了做到程序 (向下) 兼容，指令系统向上**只能扩充而不能减去**。因此也使得指令系统越来越复杂；
- 采用 CISC 系统的计算机为复杂指令系统计算机；

#### 1.2. 特点

- 指令长度一般不固定；
- 不同指令之间，执行频率和执行时间差别可以很大；
- 访存指令不限于取数 LOAD 和存数 STORE；

### 2. RISC

#### 2.1. 概念

- **精简指令系统计算机RISC（Reduced Instruction Set Computer）**：对指令数目和寻址方式都做了精简；
  - 实现更容易，指令**并行度更好**，**编译器的效率更高**；
- 通过简化指令系统，使计算机的**结构更加简单合理**，从而提高运算速度；
- 兼容性较差；

#### 2.2. 特点

- 优先选取使用频率最高的一些简单指令，以及一些常用但不复杂的指令；
- 指令长度固定，指令格式种类少，寻址方式种类少；
- 不同指令之间各字段的划分比较一致，各字段的功能比较规整；
- 不同指令之间，执行频率和执行时间差别不大，**绝大多数在一个机器周期内完成**；
- **只有取数 LOAD / 存数 STORE 指令访问存储器，其余指令操作都在寄存器之间进行；**

### ℹ️3. 二者对比

![image-20250323221628762](https://cloudflare-imgbed-ajc.pages.dev/file/1742739430121_image-20250323221628762.png)

**几个关键的特性**

- RISC 的**程序较长**；
- RISC **寄存器较多**；
- RISC 一般使用**硬件布线**来实现，而**不是微程序控制器**；
  - **硬件布线**：使用逻辑门组合电路实现指令执行的逻辑；
- **机器周期 ≠ 时钟周期**，但是因为 RISC 指令简单，**方便实现指令流水线**，使得每个时钟周期都可执行一条 RISC 指令，因此**可以说，RISC 指令大多数在一个时钟周期内完成**；

