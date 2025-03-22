---
title: （二）UDP 协议
date: 2025-03-14T10:09:46.541879+08:00
tags: ['计网笔记', '五、传输层', '（二）UDP 协议.md']
url: /posts/rsEaBXT5oD
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

<a href="/notes408/chapters_index"> Indexes </a> > <a href="/notes408/indexes/xIRy1MDUIU"> 计网笔记 </a> > <a href="/notes408/indexes/E1vSPDp0ys"> 五、传输层 </a> > （二）UDP 协议

### 1. UDP 协议

- 基于 ip 协议数据报服务；

- **增加了较少的功能**：（1）端口，（2）差错检测；

- **缺点**：只能提供不可靠的交付；

### 2. UDP 的特点

- 1）**无连接**的服务，交换数据之前无需建立连接；

- 2）**不可靠**交付，但也**不使用拥塞控制**（适合多媒体通信）；

- 3）面向报文的服务；

	![img_qyUUSc7NW5](https://cloudflare-imgbed-ajc.pages.dev/file/1741871358701_qyUUSc7NW5.png)

	- 发送方 UDP 对于应用的报文，**添加首部后直接交付** ip 层；

	- 发送方对于应用层交付的报文，不做处理，**原样发送，一次发送一个报文**；

	- 接收方对于 ip 层的报文，**去除首部后，不做处理，原样交付给进程**，一次交付一个完整报文；

- 4）支持 1v1，1vn，nvn 和 nv1 通信；

- 5）首部开销小，仅 8 字节；

### 3. UDP 数据报

![img_dWjVfSYSkv](https://cloudflare-imgbed-ajc.pages.dev/file/1741871367427_dWjVfSYSkv.png)

#### 3.1. **UDP 首部（背）**

- 8 字节，4 字段，每个字段 2 字节；

- 1）源端口；

- 2）目的端口；

- 3）长度；

- 4）检验和；

#### 3.2. UDP 伪首部

- **作用**：**用于计算检验和**，**传输时不带入 UDP 数据报**。发送方和接收方分别**构造伪首部来计算检验和**；

- 20 字节，5 字段；

- 1）源 ip 地址：4 字节；

- 2）目的 ip 地址：4 字节；

- 3）固定字段全 0：1 字节；

- 4）固定协议类型：UDP 为 17，1 字节；

- 5）UDP 长度：2 字节；

#### 3.3. UDP 校验（计算检验和）

- 拼接：伪首部（12 字节）+ 首部（8 字节）+ 数据部分（若长度为奇数，则补一个全 0 字节）；

- 以 2 字节 16 bit 为一组，**按二进制反码运算求和**，得到 2 字节**结果，再取反码作为最终检验和**；

	- 反码求和：**先正常求和**，若溢出，则**溢出的 1 和最低位相加**（回卷到最低位）；

	- ℹ️：运算时，**检验和字段视为全 0**；

