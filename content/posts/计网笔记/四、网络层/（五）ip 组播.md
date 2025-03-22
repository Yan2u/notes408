---
title: （五）ip 组播
date: 2025-03-14T10:09:48.094275+08:00
tags: ['计网笔记', '四、网络层', '（五）ip 组播.md']
url: /posts/QhzyQ2Eg5i
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

<a href="/notes408/chapters_index"> Indexes </a> > <a href="/notes408/indexes/xIRy1MDUIU"> 计网笔记 </a> > <a href="/notes408/indexes/KBqa1knYFw"> 四、网络层 </a> > （五）ip 组播

### 1. 概念

![img_hee0ewktnd](https://cloudflare-imgbed-ajc.pages.dev/file/1741871371796_hee0ewktnd.png)

### 2. 特点

- **使用组地址**：即 D 类地址，**只能作为目的地址**；

- **永久性**：组播地址为永久地址，由 Internet 号码指派管理局 IANA 负责指派；

- 组播成员可以动态更新；

- 需要硬件支持；

#### 3. ip 组播地址

- **MAC 组播地址块**的范围：`01-00-5E-00-00-00` 到 `01-00-5E-7F-FF-FF`；

- D 类 ip 地址可供分配的有 28 位，前 5 位不能用来构成以太网硬件地址；

- D 类 ip 地址和组播 MAC 地址的关系

	![img_27x0N0dCg6](https://cloudflare-imgbed-ajc.pages.dev/file/1741871381323_27x0N0dCg6.png)

	- 组播 MAC 地址的开头为固定的 `01-00-5E`；

