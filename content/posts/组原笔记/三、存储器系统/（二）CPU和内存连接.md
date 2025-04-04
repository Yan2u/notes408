---
title: （二）CPU和内存连接
date: 2025-03-16T20:43:12.853933+08:00
tags: ['组原笔记', '三、存储器系统', '（二）CPU和内存连接.md']
url: /posts/9Op9WKN3ym
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

<a href="/notes408/chapters_index"> Indexes </a> > <a href="/notes408/indexes/5RoFxkg3V7"> 组原笔记 </a> > <a href="/notes408/indexes/R8qYK2JUgK"> 三、存储器系统 </a> > （二）CPU和内存连接

### 1. 主存储器和 CPU 的连接

- 合理选择存储芯片；
- 连接地址线；
- 连接数据线；
- 连接读/写命令线；
- 连接片选线；

### 2. 主存储器扩展

#### 2.1. 位扩展

![image-20250314214150601](https://cloudflare-imgbed-ajc.pages.dev/file/1741959724588_image-20250314214150601.png)

- **情况**：**CPU 数据位数和存储芯片不同**，需要对存储芯片进行位扩展；
- **含义**：用多个存储器器件对字长进行扩充；
- **方法**：将多片存储器的地址、片选 CS、读写控制端 R/W 相应**并联**，数据端分别引出为低位和高位；

#### 2.2. 字扩展

![image-20250314214441363](https://cloudflare-imgbed-ajc.pages.dev/file/1741959888847_image-20250314214441363.png)

- **情况**：使用的存储器芯片**位数符合要求**，但**单元数目较少**时；
- **含义**：增加存储器中字的数量；
- **方法**：各芯片的**地址线**、数据线、读写控制线相应并联，而由**片选信号**来区分各芯片的地址范围；

#### 2.3. 字位扩展

![image-20250314214933394](https://cloudflare-imgbed-ajc.pages.dev/file/1741960179076_image-20250314214933394.png)

- 实际情况下，存储器往往需要同时进行**字扩展和位扩展**；
- 一般是**先扩字，再扩位**；
- **计算 1**：目标存储器容量 $M \times N$ 位，用 $L \times K$ 的芯片进行组装，则
  - 字扩展：需要 $M/L$ 个；
  - 位扩展：需要 $N/K$ 个；
  - 总共需要：$M/L\times N/K$ 个芯片；
- **计算 2**：目标容量 $x$，存储器芯片容量 $y$，则所需芯片**数量直接得到 $n=x/y$**；

### 3. 主存储器的存储单元

#### 3.1. 编址方式

- **按字节编址**：最小寻址单位为字节，相邻的存储地址指向相邻的字节；
- **按字编址**：最小寻址单位为字，相邻的存储地址指向相邻的字；
- **存储单元**是 CPU 对主存访存的**最小单位**；
  - 一个存储单元可能有多个字节；

#### 3.2. 小端和大端编址

- **小端编址**：数据高位放在相邻地址较大的那一个，数据低位放在较小的那一个；

- **大端编址**：数据高位放在相邻地址较小的那一个，数据低位放在较大的那一个；

- **单位为字**，即给出的是一个字内各字节如何排布；

- 例：数据为 `0x1234`，分两个地址存放，分别为 `0x00` 和 `0x01`，设 1 字 = 2 字节 = 16 bit；

  |   数据   | 高位 0x12 | 低位 0x34 |
  | :------: | :-------: | :-------: |
  | 小端编址 |   0x01    |   0x00    |
  | 大端编址 |   0x00    |   0x01    |

### 4. 主存地址译码方式

#### 4.1. 线选法

![image-20250315223805640](https://cloudflare-imgbed-ajc.pages.dev/file/1742049501221_image-20250315223805640.png)

- 地址线位数：$n=\log_2M$，$M$ 为地址数量；

#### 4.2. 重合法

![image-20250315223921078](https://cloudflare-imgbed-ajc.pages.dev/file/1742049589369_image-20250315223921078.png)

- 地址线位数：$n=(\log_2M) /2$（向下取整），$M$ 为地址个数；

### 5. 主存芯片引脚数量

|                          |        SRAM         |        DRAM         |
| :----------------------: | :-----------------: | :-----------------: |
|          地址线          |       线选法        |       重合法        |
|          数据线          | 和存储字长 MDR 一致 | 和存储字长 MDR 一致 |
|       读/写命令线        |       各 1 根       |       各 1 根       |
|      片选线（单个）      |          1          |          1          |
| 片选线（$M$ 个片选芯片） |     $\log_2 M$      |     $\log_2 M$      |

- 片选线说明
  - 若计算单个存储芯片的片选，则是 1 根；
  - 若有 $M$ 个有效存储芯片，则 $\log_2 M$；
- **注意**
  - 芯片数据线位数小于 MDR 时，需要进行位扩展；
  - 一般用**高位地址线当作片选线**，低位地址作为地址线；
  - **在某些特殊情况下，SRAM 的读写控制线也可以复用 1 根**；

### 习题

- 1. 32 位计算机按字节编址，小端编址。已知语句 `int i = 0;` 对应的机器码为 `C7 45 FC 00 00 00 00`，则语句 `int i = -64;` 对应的机器码是

  - A. `C7 45 FC C0 FF FF FF`；
  - B. `C7 45 FC 0C FF FF FF`；
  - C. `C7 45 FC FF FF FF C0`；
  - D. `C7 45 FC FF FF FF 0C`；
  - **答案**：A；
  - 解析：
    - 指令码 `C7 45 FC` 都是一样的，区别就在后面数据部分；-64 用 8 位补码表示为 $(1100\;0000)_2$，表示成 32 位要**前补符号位**，补成 $(FF\;FF\;FF\;C0)_{16}$；
    - **小端编址，低位在前**，注意是以字节（8 位）为单位，字节内部顺序不变，A 对；

- 2. 小端编址，且规定 int 和 short 分别是 32 位和 16 位。采用**边界对齐存储**。有一个结构体变量 record：`struct { int a; char b; short c; } record;`，且赋值 `record.a = 273`。若 record 的首地址为 0xC008，则 0xC008 地址的内容和 `record.c` 的地址分别为

  - A. 0x00，0xC00D；
  - B. 0x00，0xC00E；
  - C. 0x11，0xC00D；
  - D. 0x11，0xC00E；
  - **答案**：D
  - **解析**：
    - 边界对齐存储的含义：
      - 要求结构体的大小必须是其**最大成员的整数倍**（本题中为 `int a`，占 4 个字节）；
      - 对于占用 m 个字节的成员，要求其**首地址必须是 m 的整数倍**；
    - 所以，record 的存放情况如下：
      - 首 4 个字节，存放 `int a`；
      - 第 5 个字节，按照声明顺序，存放 `char b`；
      - 第 6 个字节，**因为边界对齐，不能存放 `short c`，留空**；
      - 第 7，8 个字节，存放 `short c`；
    - $(273)_{10}=(0000\;0111)_{16}$，小端编址，所以第 1 个字节存 0x11，第 2 个字节 0x01，其余为 0；
    - `short c` 的地址为：$(C008)_{16}+6=(C00E)_{16}$，选 D；

- 3. 用若干个 2K×4 位的芯片组成一个 8K×8 位的存储器，则地址 0x0B1F 所在芯片的最小地址为

  - **答案**：0x0800；

- 4. CPU 有 16 位地址线，8 位数据线，MREQ 为访存控制信号（低电平有效），WR 为读写控制（高电平读，低电平写），RAM 芯片可选择 1K×4 位，4K×8 位，8K×8 位；ROM 芯片可选则 2K×8 位，4K×8 位，8K×8 位；另外可选 74138 译码器和各种门电路。要求：主存地址 0x6000 ~ 0x67FF 为系统程序区，0x6800 ~ 0x6BFF 为用户程序区；**合理运用给出的芯片，说明 ROM 和 RAM 各选几片；**

  - **答案**：ROM 选 1 片 2K×8 位，RAM 选 2 片 1K×4 位，位扩展为 1K×8 位；
  - **解析**：注意系统程序区应该用 ROM 存，用户 RAM 存，所以算出两种地址的大小，再选择即可；

- 5. CPU 有 16 位地址线，8 位数据线，MREQ 为访存控制信号（低电平有效），WR 为读写控制（高电平读，低电平写），现设计存储系统，要求：(1) 最小 8K 为系统程序区，相邻的 16K 为用户程序区，最大的 4K 为系统工作区；

  - （1）写出各区对应的二进制地址码；
  - （2）确定选用芯片的数量及类型；
  - **解析**：16 根地址线，故地址码都应该是 16 位二进制；
    - 8K 系统程序：0000 0000 0000 0000 到 0001 1111 1111 1111；
    - 16K 用户程序：0010 0000 0000 0000 到 0101 1111 1111 1111；
    - 注意题目要求**最大的 4K 个地址**，故系统工作区应该是从最大地址（全 1）反推；
    - 4K 用户工作区，1111 0000 0000 0000 到 1111 1111 1111 1111；
    - 三个区域分别对应 ROM, RAM, RAM，系统程序区可用 8K×8 位，用户程序区可用 2 片 8K×8 位，系统工作选 4K×8 位；

- 6. 主存储器的字长为 32 位，容量 1MB。在按字节编址和按字编址的情况下，MAR 和 MDR 的位数各是多少？

  - **答案**：MDR 都是 32 位，按字节编址时，MAR 20 位，按字编址时，MAR 18 位 ；
  - **解析**：**MDR 位数和编址方式无关，仅和存储字长有关**，故 MDR 都是 32 位，然后计算地址数量即可得到 MAR 位数；

- 7. 一个存储器容量 256MB，由若干个 4MB 的 DRAM 芯片构成。该 DRAM 芯片的地址引脚和数据引脚总数为

  - A. 19
  - B. 22
  - C. 30
  - D. 36
  - **答案**：A
  - **解析**：区分这两个概念：**MAR 位数，地址引脚个数**，设地址数量 $M$；
    - 前者直接为 $\log_2 M$，但**后者不一定**，DRAM 为 1/2 倍，SRAM 为 1 倍；
    - 然后是**审题**：题目问的是该 DRAM 芯片，**并不是指的 256MB，而是 4MB 的单个存储芯片**；
    - 所以 $M=4\times1024\times 1024$，地址引脚个数 $\log_2 M/2=11$，数据引脚 8 个，总个数 19；

- 8. CPU 地址总线 $A_0\sim A_{15}$，$A_0$ 为最高位。现用 4K×4 位的芯片组成 16KB 存储器，则片选信号对应的位数为

  - A. $A_2A_3$；
  - B. $A_0A_1$；
  - C. $A_{12}A_{13}$；
  - D. $A_{14}A_{15}$；
  - **答案**：A
  - **解析**：
    - 首先需要位扩展，位扩展与片选无关。用于片选的芯片数量为 16K/4K = 4，即需要两位片选信号；
    - 对于每个 4K 芯片，需要 **12 位片内地址线**；
    - 通常片内地址是低位，片选是高位，故低 12 位作为片内地址，较高 2 位作为片选，还有 2 位留空，选 A；

