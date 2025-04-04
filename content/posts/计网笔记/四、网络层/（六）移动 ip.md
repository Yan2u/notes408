---
title: （六）移动 ip
date: 2025-03-14T10:09:47.845326+08:00
tags: ['计网笔记', '四、网络层', '（六）移动 ip.md']
url: /posts/URXAs4M3xK
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

<a href="/notes408/chapters_index"> Indexes </a> > <a href="/notes408/indexes/xIRy1MDUIU"> 计网笔记 </a> > <a href="/notes408/indexes/KBqa1knYFw"> 四、网络层 </a> > （六）移动 ip

### 1. 概念

- 为了满足**移动节点在移动中保持其连接性**而设计的 ip 地址；

- 移动 ip 技术：**指移动用户可跨网络随意移动和漫游，而不用修改计算机原来的 ip 地址**，同时，继续享有原网络中一切权限。

- 简单地说，移动 ip 就是实现网络全方位的移动或者漫游；

- **移动 ip 不等于 4G，5G**

	- 移动 ip 的**底层通信链路不同**于 4G 或 5G 移动通信；

	- 移动 ip 的**要点：跨网络**（较大范围的局域网如校园网不属于移动 ip）；

- 移动节点拥有两个 ip 地址

	- **归属地址（Home Address）**：与归属网（本地网）连接时使用的地址，移动过程中不变；

	- **转交地址**：临时分配给移动节点的地址，一般是移动节点达到新网络时，通过 DHCP 获取；

		- 转交地址是隧道转发的终点地址；

		- 分为**外区代理的转交地址**（大多数）和驻留本地网的转交地址（配置转交地址）；

		- 外区代理转交地址：可以让多个移动结点共享一个转交地址，一般是外区代理路由器持有；

		- 驻留本地转交地址：**仅供一个移动节点使用**，移动节点自身作为隧道重点；

		- 外区代理转交地址向外区代理申请，驻留本地转交地址向归属代理申请；

	- **代理发现**：移动节点必须实现代理发现，以实时获得代理节点的地址；

		- **被动发现**：等待代理节点周期广播；

		- **主动发现**：广播一条请求代理报文，由代理节点进行回复；

			- 进行主动发现的条件：无法通过被动方式、链路层协议或其他方式获得代理节点地址；

- **隧道技术（Tunneling）**

	- **归属代理将 ip 数据报通过隧道技术转发给外区代理**，再转发给外区的移动节点；

	- **隧道**：原始的 ip 数据报被**完整封装**在转发的 ip 数据报中，原封不动地转发给隧道终点的结点；

	- 隧道终点的结点收到数据报后，**解除封装，取出原始数据报**再进行转发；

### 2. 通信过程

- 移动 ip 系统中的归属代理和外区代理不停地向网上发送代理通告（Agent Advertisement） 消息；

- 移动结点收到消息，知道环境中有归属代理和外区代理的存在，并确定自己是在归属网还是在外区网上；

- 如果移动结点收到的是归属代理发来的消息，则说明自己仍在归属网上，此时，不启动移动功能；

- 当移动结点检测到它移到外区网，从外区网处获取一个临时的 ip 地址，即转交地址；

- **移动结点向归属代理注册**，表示已离开归属网，**把所获的关联地址通知归属代理**。归属代理可以随时获取移动结点的当前位置信息；

- 注册完毕后，当通信端要向移动结点发送报文时，**仍然使用移动结点的固定 ip 地址（原地址）**；

	- 报文将被路由到移动结点的归属网，并被归属代理截获；

	- 归属代理将该报文封装，通过隧道将报文发送到移动结点所在的外区网；

### 3. 实例解析

![img_aVoQucHJ52](https://cloudflare-imgbed-ajc.pages.dev/file/1741871981503_aVoQucHJ52.png)

- 主机 X 的归属网为 A 202.119.80.0，初始拥有一个 ip 地址 202.119.80.16；

- 主机 X 移动到了外网 B 202.119.81.0 覆盖的区域，则若要继续访问互联网：

	- 主机 X 向 B 的外区代理发送相关消息，通知外区代理；

	- 外区代理记录主机 X 的归属网和原 ip 地址等信息，同时**为 X 分配新的转交地址**（属于 B）；

	- 外区代理向网 A 的归属代理通知主机 X 的移动，A 的归属代理记录相关信息；

- 互联网对主机 X 的访问请求到达时：

	- 使用的 X 的原地址，所以先到达 A 的归属代理；

	- A 识别到主机 X，用隧道机制将数据转发给 B 的外区代理；

	- B 再将数据根据 X 的转交地址转发给 X；

- 当 X 向互联网发送数据时：

	- 先到达 B 的外区代理；

	- B 的外区代理识别到 X，直接进行转发（一般就不经过 A 代理了）；

### 4. 特点

- **对通信端和上层应用透明**：对于这两者来说，ip 地址始终固定，不用进行额外的处理；

- 移动 ip 是网络层的移动解决方案；

- 移动 ip 实现的**复杂度高**；

