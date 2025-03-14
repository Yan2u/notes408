---
title: （三）万维网 www
date: 2025-03-14T10:09:46.969943+08:00
tags: ['计网笔记', '六、应用层']
author: "Yan2u"
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
showToc: true
TocOpen: false
ShowWordCount: true
UseHugoToc: true
---

<a href="/notes408/chapters_index"> Indexes </a> > <a href="/notes408/indexes/计网笔记_index"> 计网笔记 </a> > <a href="/notes408/indexes/计网笔记/六应用层_index"> 六、应用层 </a> > （三）万维网 www

### 1. 概念

#### 1.1. 概念

- 万维网 WWW (World Wide Web) 是一个**大规模的、联机式**的信息储藏所；

- 万维网用链接的方法能非常方便地从因特网上的一个站点访问另一个站点，从而主动地按需获取丰富的信息；

#### 1.2. 万维网的工作方式

- 工作模式：客户服务器模式（C/S）；

- 客户程序：运行在用户计算机上的浏览器；

- 服务器：保存万维网文档的计算机运行服务器程序；

#### 1.3. 万维网要解决的问题

- **怎样标志分布在整个因特网上的万维网文档**：URL（统一资源定位符）；

	![img_s1y4j3hQng](https://cloudflare-imgbed-ajc.pages.dev/file/1741871154171_s1y4j3hQng.png)

- **客户端和服务器交互**所用的协议：HTTP 传输协议；

- **显示文档以及超链接**：HTML 标记语言；

### 2. HTTP 协议

#### 2.1. 概念

- 面向事务（transaction）的应用层协议；

- 是万维网上能够**可靠地交换文件**（包括文本、声音、图像等各种多媒体文件）的重要基础；

#### 2.2. 工作原理

![img_OF1eQrxcVX](https://cloudflare-imgbed-ajc.pages.dev/file/1741871165276_OF1eQrxcVX.png)

- 实例分析：点击超链 http://www.tsinghua.edu.cn/chn/yxsz/index.htm

	- 1）浏览器分析超链指向的 URL；

	- 2）浏览器向 **DNS 请求域名 **www.tsinghua.edu.cn** 的 ip 地址**；

	- 3）DNS 给出对应的 ip 地址；

	- 4）浏览器与服务器**建立 TCP 连接**；

	- 5）浏览器发出 **GET 命令，路径为 `/chn/yxsz/index.htm`**；

	- 6）服务器响应，将 index.htm 文件发送给浏览器；

	- 7）**释放 TCP 连接**；

	- 8）浏览器渲染并显示 index.htm 的所有内容；

- **主要特点**

	- 面向事务的客户—服务器模式协议；

	- **HTTP 1.0 协议是无状态**的（stateless）；

	- **HTTP 本身是无连接的协议**，而使用了面向连接的 TCP 提供的服务；

- 请求一个文档一般需要 $2\times\mathrm{RTT}$ 的时间：先用一个 RTT 建立连接，再用一个发送请求和响应报文；

#### 2.3. 持续连接

- **HTTP1.1 协议**使用持续连接（HTTP 1.0 没有此特性）；

- 含义：**服务器在发送响应后仍然在一段时间内保持这条连接**，使同一个客户（浏览器）和该服务器可以继续在这条连接上传送后续的 HTTP 请求和响应报文；

	- 对比 HTTP 1.0：1.0 协议在响应一次之后就释放 TCP 连接；

- 两种工作方式

	- **非流水式**：客户在**收到前一个响应后才能发出下一个**请求（易考）；

		- 优点：相比于非持续连接（两倍 RTT 开销），**节省了建立 TCP 连接所需的一个 RTT 时间**；

		- 缺点：服务器在发送完一次后，其 TCP 连接就处于空闲状态，浪费了服务器资源；

	- **流水式**：客户可以**连续发送请求**，而不必等到响应之后。服务器也可以**连续响应**；

		- 使用流水线方式时，客户访问所有的对象只需花费一个 RTT 时间，使 TCP 连接中的空闲时间减少，提高了下载文档效率；

#### 2.4. HTTP 报文

- 两类报文：1）请求报文，2）响应报文；

-  结构（了解）

	![img_vD1v9zSqBA](https://cloudflare-imgbed-ajc.pages.dev/file/1741871165325_vD1v9zSqBA.png)

	- 请求报文中，开始行也称为请求行；

	- **HTTP 方法**

		- GET：请求读取；

		- HEAD：请求读取首部；

		- POST：请求添加信息；

		- PUT：在指定的 URL 下存放一个文档；

		- DELETE：删除 URL 所表示的资源；

		- TRACE：环回测试用；

		- CONNECT：用于代理服务器；

		- OPTION：请求一些选项的信息；

	- HTTP 状态码

		- 1xx：表示通知信息的，如请求收到了或正在进行处理；

		- 2xx：表示成功，如接受或知道了；

		- 3xx：表示重定向，表示要完成请求还必须采取进一步的行动；

		- 4xx：表示客户的差错，如请求中有错误的语法或不能完成；

		- 5xx：表示服务器的差错，如服务器失效无法完成请求；

#### 2.5. Cookie

- 表示服务器和客户端交互的状态信息的数据；

- 网站使用 Cookie 来跟踪用户：使用 Cookie 的网站服务器为用户产生一个**唯一的识别码**。利用此识别码，网站就能够跟踪该用户在该网站的活动；

