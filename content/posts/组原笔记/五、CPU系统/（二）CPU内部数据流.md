---
title: （二）CPU内部数据流
date: 2025-03-25T22:42:28.609505+08:00
tags: ['组原笔记', '五、CPU系统', '（二）CPU内部数据流.md']
url: /posts/abVVDhVNgh
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

<a href="/notes408/chapters_index"> Indexes </a> > <a href="/notes408/indexes/5RoFxkg3V7"> 组原笔记 </a> > <a href="/notes408/indexes/ZmYXio95fs"> 五、CPU系统 </a> > （二）CPU内部数据流

### 1. 指令周期

#### 1.1. 概念

- **指令周期**：一条指令**从取出到执行完成**所需要的时间；
- **机器周期**：指令周期划分为几个阶段，**每个阶段所需的时间也称CPU工作周期**（通常等于访存周期）
- **时钟周期**：时钟频率的倒数，**也称节拍或 T 周期**（通常等于 **CPU 执行一步操作所需时间**）
- **关系**：一个指令周期包含若干机器周期，一个机器周期包含若干时钟周期
- **指令周期的四个部分**：(1) 取指周期，(2) 间址周期，(3) 执行周期，(4) 中断周期；

### 2. 指令周期的数据流

#### 2.1. 取指周期

![image-20250324223533751](https://cloudflare-imgbed-ajc.pages.dev/file/1742826949236_image-20250324223533751.png)

- **执行步骤（图左侧）**
  - 1）$(PC)\rightarrow MAR$：PC 的内容（即要执行的指令地址）发给 MAR；
  - 2）$1\rightarrow R$：表示置位存储芯片的**读有效信号（R）**；
  - 3）$((MAR))\rightarrow MDR$：表示从内存中 MAR 表示的地址读取数据，存放到 MDR 中；
    - 注意，$MAR$ 表示的是寄存器，$(MAR)$ 才是表示取寄存器的值；
    - 故用两次括号，内层括号表示取 MAR 值，外层括号表示 MAR 的值作为地址，取内存的值；
    - 下面 MDR 亦同；
  - 4）$(MDR)\rightarrow IR$：MDR 存放的内容送到 IR；
  - 5）$OP(IR)\rightarrow CU$：**指令译码器提取操作码字段**，发给控制器 CU；
  - 6）$(PC)+1\rightarrow PC$：PC 自增；
- **读内存（LOAD）阶段**：图左侧框起来的三步，最后一步需要读取内存；
  - CPU **通过指令周期的阶段，来判断从 MDR 中数据的含义**；
  - 例如：当前是指令周期的取指阶段，则内存中读出来的（MDR 中的）就是指令；

#### 2.2. 间址周期

![image-20250324224924010](https://cloudflare-imgbed-ajc.pages.dev/file/1742828088367_image-20250324224924010.png)

- 考虑的是**一级间址**；
- **步骤**
  - 1）$AD(IR)\rightarrow MAR$：**指令译码器提取地址码字段**，发给 MAR；
  - 2）$1\rightarrow R$：置位存储芯片的**读有效信号（R）**；
  - 3）$((MAR))\rightarrow MDR$：从内存中 MAR 表示的地址读取数据，存放到 MDR 中；
  - 4）$(MDR)\rightarrow AD(IR)$：用 MDR 的内容更新地址码部分，**此时 $(MDR)$ 为有效地址 EA**；

#### 2.3. 执行周期

- **加法指令 `ADD x`**
  - 1）$AD(IR)\rightarrow MAR$：地址码发给 IR，这里是要读取地址 x 指向的值；
  - 2）$1\rightarrow R$：置位存储芯片的**读有效信号（R）**；
  - 3）$((MAR))\rightarrow MDR$：从内存中读取数据存到 MDR；
  - 4）$(ACC)+(MDR)\rightarrow ACC$：执行加法，**结果存回 ACC**；
  - x 为其中一个操作数的地址；
  - 隐含寻址：另一个操作数存在 ACC 中；
- **存数指令 `STA x`**
  - 1）$AD(IR)\rightarrow MAR$：地址码发给 IR，这里是要写入地址 x；
  - 2）$1\rightarrow W$：置位存储芯片的**写有效信号（W）**；
  - 3）$(ACC)\rightarrow MDR$：读 ACC 寄存器的值，存到 MDR；
  - 4）$(MDR)\rightarrow (MAR)$：MDR 的值存到指定内存单元中；
  - x 为要写入的地址；
  - 隐含寻址：要写入的数存在 ACC 中；
- **取数指令 `LDA x`**
  - 1）$AD(IR)\rightarrow MAR$：地址码发给 IR，这里是要读取地址 x 的值；
  - 2）$1\rightarrow R$：置位存储芯片的**读有效信号（R）**；
  - 3）$((MAR))\rightarrow MDR$：从内存中读取数据存到 MDR；
  - 4）$(MDR)\rightarrow ACC$：从 MDR 中读数，存到 ACC 中；
  - x 为要读取的地址；
  - 隐含寻址：取出的数存在 ACC 中；
- **转移指令（无条件）：`JMP x`**
  - $AD(IR)\rightarrow PC$：直接把地址码放到 PC；
- （了解）条件转移：`BAN x`
  - 含义：结果为负数则转移；
  - $S\times AD(IR)+\overline{S}\times (PC)\rightarrow PC$：符号位 $S=1$ 才转移，否则 PC 不变；
- 注意：**转移指令生效前，PC 已经因为取指而自增了**；

#### （了解）2.4. 中断周期

- 程序断点（即当前 PC 的值）**存入零地址**；
- **两种情况**：(1) 断点写入零地址，(2) 断点压入栈中；
- **断点写入零地址**
  - 1）$0\rightarrow MAR$；
  - 2）$1\rightarrow W$：置位写有效；
  - 3）$(PC)\rightarrow MDR$；
  - 4）$(MDR)\rightarrow (MAR)$：断点地址存入零地址内存单元；
  - 5）$VEC\rightarrow PC$：当前**中断服务函数的地址存入 PC**；
    - VEC：也称为 **(中断) 向量地址**；
    - 不同中断码及其服务地址连续存在一张表中，称为**中断向量表**；
  - 6）$0\rightarrow EINT$：**复位中断标志**位（关中断）
- **断点压入栈中**
  - 1）$(SP)-1\rightarrow MAR$：将当前栈顶 SP 再上一层的地址发给 MAR；
  - 2）$1\rightarrow W$：置位写有效；
  - 3）$(PC)\rightarrow MDR$；
  - 4）$(MDR)\rightarrow (MAR)$：断点地址写入栈顶上一层成为新栈顶，即压栈；
  - **5）和 6）同写入零地址的情况**

#### 2.5. 需要背的

- **取数和存数；**
- **加法；**
- **取指和间址周期；**
- 其他了解即可；

### 习题

- 1. CPU 主频 8MHz，平均每个机器周期包含 2 个时钟周期，每个指令周期包含 2.5 个机器周期。则

  - 1.1. 平均执行速度为多少 MIPS？
  - 1.2. 若主频不变，每个机器周期包含 4 个时钟周期，每个指令周期包含 5 个机器周期，则速度又是多少 MIPS？
  - **答案**：1）1.6 MIPS，2）0.4 MIPS；
  - **解析**：8MHz 指每秒有 $8\times10^6$ 个时钟周期，则
    - 1.1 中，每条指令的时钟周期为 $2\times2.5=5$，则速度 $8\times10^6/5=1.6\times10^6$，即 1.6 MIPS；
    - 1.2 同理；

- 2. 设 CPU 采用非总线结构，则

  - 2.1. 写出取指周期的全部微操作；

  - 2.2. `M` 表示主存地址，写出 `LDA M`，`STA M`，`ADD M` 在执行阶段的全部微操作；

  - 2.3. 在 2.2 基础上，若指令全部为间接寻址，写出全部微操作；

  - **答案**

    - 1）2.1 节内容；

    - 2）以 `LDA M` 为例

      ```
      AD(IR) -> MAR
      1 -> R
      (MAR) -> MDR
      (MDR) -> ACC
      ```
      
    - 3）以 `LDA M` 为例
    
      ```
      ... % 取指
      
      AD(IR) -> MAR
      1 -> R
      (MAR) -> MDR
      (MDR) -> AD(IR)
      
      ... % 执行
      ```
    
  - **解析**：
  
    - 3）注意插入间址周期；
  
- 3. 图中 M 为主存，XR 为变址寄存器，EAR 为有效地址寄存器，LATCH 锁存器。各寄存器 IO 均受控制信号控制；下标 $i$ 表示寄存器输入控制信号，下标 $o$ 表示输出控制信号；设指令地址已存放至 PC；画出以下指令的指令周期信息流程图，并列出响应的控制信号序列；

  - 3.1. `ADD X, D`，其中 X 为变址寄存器 XR，D 为形式地址；
  - 3.2. `STA *D`，`*` 表示相对寻址，D 为相对位移量；
  
  ![image-20250324235545002](https://cloudflare-imgbed-ajc.pages.dev/file/1742831897407_20250324235813.png)
  
  - **答案**：
  
    - 1）ADD X, D
  
      ![image-20250325214410727](https://cloudflare-imgbed-ajc.pages.dev/file/1742910256446_image-20250325214410727.png)
  
    - 2）STA \*D
  
      ![image-20250325214855073](https://cloudflare-imgbed-ajc.pages.dev/file/1742910545454_image-20250325214855073.png)
  
  - **解析**：
  
    - 1）大体上就是**按照第 2 节内容默写**即可，然后**数据流出寄存器 R 就是对应 $R_o$，流入寄存器就是 $R_i$**。注意：
      - $PC\rightarrow BUS\rightarrow MAR$ 中的 BUS 可不写，表示数据通过总线流入寄存器；
      - PC 自增的有效控制信号是**自增控制信号 +1**；
    - 2）⚠️注意是**相对寻址**而不是间接寻址，所以应该是 $EA=(PC)+A$，而不是访问主存；
  
- 4. 机器字长 16 位，按字节编址。R1 和 R2 通用寄存器；R/W 为读/写控制标志；ALU 可以执行算术加法和加 1 两种操作。结果送入 R2；

  ![image-20250325221323171](https://cloudflare-imgbed-ajc.pages.dev/file/1742912013992_image-20250325221323171.png)

  - 4.1. 图中 X  和 Y 是什么部件？有什么作用？

  - 4.2. 若主存容量 4M 字，求 R1, Y, PC 的位数；

  - 4.3. 说明如何实现 PC+1 操作，并按顺序列出控制信号；

  - 4.4. 给出取指令过程中的操作控制信号；

  - 4.5. 指令 `ADD m` 表示将 ACC 加上存储单元 m 的值，结果送回 ACC。写出该指令执行阶段的操作控制信号；

  - **答案**：

    - 1）X 是 IR，Y 是 MAR，作用见第 2 节；

    - 2）R1 为 16 位，Y 是 MAR 23 位，PC 为 22 位；

    - 3）

      |      步骤      |   控制信号   |
      | :------------: | :----------: |
      |    PC => R1    |   PCo, R1i   |
      | (R1 + 1) => R2 | R1o, R2i, +1 |
      |    R2 => PC    |   R2o, PCi   |

    - 4）

      |     步骤     | 控制信号  |
      | :----------: | :-------: |
      | PC => (MAR)  | PCo, MARi |
      | (MAR) => MDR |  MDRi, R  |
      | (MDR) => IR  | MDRo, IRi |
      | OP(IR) => CU |    IRo    |
      | (PC+1) => PC | 第 3 小问 |

    - 5）

      |        步骤        |     控制信号      |
      | :----------------: | :---------------: |
      |   AD(IR) => MAR    |     IRo, MARi     |
      |    (MAR) => MDR    |      R, MDRi      |
      |    (MDR) => R1     |     MDRo, R1i     |
      | (R1) + (ACC) => R2 | R1o, ACCo, R2i, + |
      |    (R2) => ACC     |     R2o, ACCi     |

  - **解析**：

    - 1）X 给 CU 发数据，故应该是 IR；Y 给存储器发数据，但不接收存储器的数据，应该是 MAR；
    - 2）注意**通用寄存器字长等于机器字长**，然后 MAR 和 PC 按照第二节定义计算即可；
    - 3）根据题目给出的结构和定义来推：
      - 看图知，R1 是数据进入 ALU 的唯一通路，所以数据要先交给 R1 才能进入 ALU 运算；
      - 注意 +1 也算操作控制信号；
      - 同理，ALU 结果交给 R2 再交给 PC；
    - 4）自增 PC = PC+1 应该用第 3 问结论；
    - 5）也要注意 ALU 加法要经过 R1 和 R2；

