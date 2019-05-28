# 计算机网络知识学习前言

> 计算机网络属于web开发工程师需要掌握的计算机基础知识之一，目前很多大公司在面试应届毕业生的时候都会问一些计网相关知识，其中典型的有“TCP三次握手和四次挥手”等经典面试题。因此掌握计算机网络知识是必不可少的。但是我们并不需要过于深入的了解计算机网络知识，更深层次的理论知识是网络工程师需要学习了解的。作为web开发工程师，计算机网络中我们主要需要掌握网络层、传输层以及应用层相关知识，而数据链路层和物理层并不需要太深入了解。

# 计算机网络概述

## 网络的网络

网络把主机连接起来，而互联网是把多种不同的网络连接起来，因此互联网是网络的网络。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/network-of-networks.gif"/>
</div>

## ISP

互联网服务提供商 ISP 可以从互联网管理机构获得许多 IP 地址，同时拥有通信线路以及路由器等联网设备，个人或机构向 ISP 缴纳一定的费用就可以接入互联网。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/72be01cd-41ae-45f7-99b9-a8d284e44dd4.png"/  width="500">
</div>

目前的互联网是一种多层次 ISP 结构，ISP 根据覆盖面积的大小分为第一层 ISP、区域 ISP 和接入 ISP。互联网交换点 IXP 允许两个 ISP 直接相连而不用经过第三个 ISP。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/3be42601-9d33-4d29-8358-a9d16453af93.png"/ width="600">
</div>

## 主机之间的通信方式

客户-服务器（C/S）：客户是服务的请求方，服务器是服务的提供方。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/914894c2-0bc4-46b5-bef9-0316a69ef521.jpg"/ width="300">
</div>

对等（P2P）：不区分客户和服务器。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/42430e94-3137-48c0-bdb6-3cebaf9102e3.jpg"/>
</div>

电路交换与分组交换
1. 电路交换
电路交换用于电话通信系统，两个用户要通信之前需要建立一条专用的物理链路，并且在整个通信过程中始终占用该链路。由于通信的过程中不可能一直在使用传输线路，因此电路交换对线路的利用率很低，往往不到 10%。

2. 分组交换
每个分组都有首部和尾部，包含了源地址和目的地址等控制信息，在同一个传输线路上同时传输多个分组互相不会影响，因此在同一条传输线路上允许同时传输多个分组，也就是说分组交换不需要占用传输线路。

在一个邮局通信系统中，邮局收到一份邮件之后，先存储下来，然后把相同目的地的邮件一起转发到下一个目的地，这个过程就是存储转发过程，分组交换也使用了存储转发过程。

## 时延

总时延 = 排队时延 + 处理时延 + 传输时延 + 传播时延
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/4b2ae78c-e254-44df-9e37-578e2f2bef52.jpg"/ width="500">
</div>

1. 排队时延
分组在路由器的输入队列和输出队列中排队等待的时间，取决于网络当前的通信量。

2. 处理时延
主机或路由器收到分组时进行处理所需要的时间，例如分析首部、从分组中提取数据、进行差错检验或查找适当的路由等。

3. 传输时延
主机或路由器传输数据帧所需要的时间。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/dcdbb96c-9077-4121-aeb8-743e54ac02a4.png"/ width="150">
</div>
其中 l 表示数据帧的长度，v 表示传输速率。

4. 传播时延
电磁波在信道中传播所需要花费的时间，电磁波传播的速度接近光速。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/a1616dac-0e12-40b2-827d-9e3f7f0b940d.png"/ width="150">
</div>
其中 l 表示信道长度，v 表示电磁波在信道上的传播速度。

## 计算机网络体系结构

### 五层协议

- 应用层 ：为特定应用程序提供数据传输服务，例如 HTTP、DNS 等协议。数据单位为报文。

- 传输层 ：为进程提供通用数据传输服务。由于应用层协议很多，定义通用的传输层协议就可以支持不断增多的应用层协议。运输层包括两种协议：传输控制协议 TCP，提供面向连接、可靠的数据传输服务，数据单位为报文段；用户数据报协议 UDP，提供无连接、尽最大努力的数据传输服务，数据单位为用户数据报。TCP 主要提供完整性服务，UDP 主要提供及时性服务。

- 网络层 ：为主机提供数据传输服务。而传输层协议是为主机中的进程提供数据传输服务。网络层把传输层传递下来的报文段或者用户数据报封装成分组。

- 数据链路层 ：网络层针对的还是主机之间的数据传输服务，而主机之间可以有很多链路，链路层协议就是为同一链路的主机提供数据传输服务。数据链路层把网络层传下来的分组封装成帧。

- 物理层 ：考虑的是怎样在传输媒体上传输数据比特流，而不是指具体的传输媒体。物理层的作用是尽可能屏蔽传输媒体和通信手段的差异，使数据链路层感觉不到这些差异。

### OSI

其中表示层和会话层用途如下：

- 表示层 ：数据压缩、加密以及数据描述，这使得应用程序不必关心在各台主机中数据内部格式不同的问题。
- 会话层 ：建立及管理会话。

五层协议没有表示层和会话层，而是将这些功能留给应用程序开发者处理。

### TCP/IP

它只有四层，相当于五层协议中数据链路层和物理层合并为网络接口层。

TCP/IP 体系结构不严格遵循 OSI 分层概念，应用层可能会直接使用 IP 层或者网络接口层。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/0fa6c237-a909-4e2a-a771-2c5485cd8ce0.png"/ width="500">
</div>

### 数据在各层之间的传递过程

在向下的过程中，需要添加下层协议所需要的首部或者尾部，而在向上的过程中不断拆开首部和尾部。

路由器只有下面三层协议，因为路由器位于网络核心中，不需要为进程或者应用程序提供服务，因此也就不需要传输层和应用层。

# 物理层

## 通信方式

根据信息在传输线上的传送方向，分为以下三种通信方式：

- 单工通信：单向传输
- 半双工通信：双向交替传输
- 全双工通信：双向同时传输

## 带通调制

模拟信号是连续的信号，数字信号是离散的信号。带通调制把数字信号转换为模拟信号。 
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/c34f4503-f62c-4043-9dc6-3e03288657df.jpg"/ width="500">
</div>


# 链路层

## 基本问题

### 1.封装成帧

将网络层传下来的分组添加首部和尾部，用于标记帧的开始和结束。 
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/29a14735-e154-4f60-9a04-c9628e5d09f4.png"/ width="300">
</div>

### 2.透明传输

透明表示一个实际存在的事物看起来好像不存在一样。

帧使用首部和尾部进行定界，如果帧的数据部分含有和首部尾部相同的内容，那么帧的开始和结束位置就会被错误的判定。需要在数据部分出现首部尾部相同的内容前面插入转义字符。如果数据部分出现转义字符，那么就在转义字符前面再加个转义字符。在接收端进行处理之后可以还原出原始数据。这个过程透明传输的内容是转义字符，用户察觉不到转义字符的存在。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/e738a3d2-f42e-4755-ae13-ca23497e7a97.png"/ width="700">
</div>

### 3.差错校验

目前数据链路层广泛使用了循环冗余检验（CRC）来检查比特差错。 以FCS作为帧校验数据位。

## 信道分类

### 1.广播信道

一对多通信，一个节点发送的数据能够被广播信道上所有的节点接收到。

所有的节点都在同一个广播信道上发送数据，因此需要有专门的控制方法进行协调，避免发生冲突（冲突也叫碰撞）。

主要有两种控制方法进行协调，一个是使用信道复用技术，一是使用 CSMA/CD 协议。

### 2.点对点信道

一对一通信。

因为不会发生碰撞，因此也比较简单，使用 PPP 协议进行控制。

## 信道复用技术

### 1.频分复用技术

频分复用的所有主机在相同的时间占用不同的频率带宽资源。 
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/4aa5e057-bc57-4719-ab57-c6fbc861c505.png"/ width="300">
</div>

### 2.时分复用技术

时分复用的所有主机在不同的时间占用相同的频率带宽资源。 
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/67582ade-d44a-46a6-8757-3c1296cc1ef9.png"/ width="300">
</div>
使用频分复用和时分复用进行通信，在通信的过程中主机会一直占用一部分信道资源。但是由于计算机数据的突发性质，通信过程没必要一直占用信道资源而不让出给其它用户使用，因此这两种方式对信道的利用率都不高。 

### 3.统计时分复用

是对时分复用的一种改进，不固定每个用户在时分复用帧中的位置，只要有数据就集中起来组成统计时分复用帧然后发送。 
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/6283be2a-814a-4a10-84bf-9592533fe6bc.png"/ width="300">
</div>

### 4.波分复用

光的频分复用。由于光的频率很高，因此习惯上用波长而不是频率来表示所使用的光载波。 

### 5.码分复用

为每个用户分配 m bit 的码片，并且所有的码片正交，对于任意两个码片 ![img](https://latex.codecogs.com/gif.latex?\vec{S}) 和 ![img](https://latex.codecogs.com/gif.latex?\vec{T}) 有 

<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/308a02e9-3346-4251-8c41-bd5536dab491.png"/ width="150">
</div>

为了讨论方便，取 m=8，设码片 ![img](https://latex.codecogs.com/gif.latex?\vec{S}) 为 00011011。在拥有该码片的用户发送比特 1 时就发送该码片，发送比特 0 时就发送该码片的反码 11100100。

在计算时将 00011011 记作 (-1 -1 -1 +1 +1 -1 +1 +1)，可以得到
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/6fda1dc7-5c74-49c1-bb79-237a77e43a43.png"/ width="150">
</div>

<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/e325a903-f0b1-4fbd-82bf-88913dc2f290.png"/ width="150">
</div>

其中 ![img](https://latex.codecogs.com/gif.latex?\vec{S%27}) 为 ![img](https://latex.codecogs.com/gif.latex?\vec{S}) 的反码。 

利用上面的式子我们知道，当接收端使用码片 ![img](https://latex.codecogs.com/gif.latex?\vec{S}) 对接收到的数据进行内积运算时，结果为 0 的是其它用户发送的数据，结果为 1 的是用户发送的比特 1，结果为 -1 的是用户发送的比特 0。

码分复用需要发送的数据量为原先的 m 倍。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/99b6060e-099d-4201-8e86-f8ab3768a7cf.png"/ width="500">
</div>

## CSMA/CD协议

CSMA/CD 表示载波监听多点接入 / 碰撞检测。

- **多点接入** ：说明这是总线型网络，许多主机以多点的方式连接到总线上。
- **载波监听** ：每个主机都必须不停地监听信道。在发送前，如果监听到信道正在使用，就必须等待。
- **碰撞检测** ：在发送中，如果监听到信道已有其它主机正在发送数据，就表示发生了碰撞。虽然每个主机在发送数据之前都已经监听到信道为空闲，但是由于电磁波的传播时延的存在，还是有可能会发生碰撞。

记端到端的传播时延为 τ，最先发送的站点最多经过 2τ 就可以知道是否发生了碰撞，称 2τ 为 **争用期** 。只有经过争用期之后还没有检测到碰撞，才能肯定这次发送不会发生碰撞。

当发生碰撞时，站点要停止发送，等待一段时间再发送。这个时间采用 **截断二进制指数退避算法** 来确定。从离散的整数集合 {0, 1, .., (2k-1)} 中随机取出一个数，记作 r，然后取 r 倍的争用期作为重传等待时间。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/19d423e9-74f7-4c2b-9b97-55890e0d5193.png"/ width="500">
</div>

## PPP协议

互联网用户通常需要连接到某个 ISP 之后才能接入到互联网，PPP 协议是用户计算机和 ISP 进行通信时所使用的数据链路层协议。

<div align="center">
 <img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/e1ab9f28-cb15-4178-84b2-98aad87f9bc8.jpg"/ width="200">
</div>
PPP 的帧格式：

- F 字段为帧的定界符
- A 和 C 字段暂时没有意义
- FCS 字段是使用 CRC 的检验序列
- 信息部分的长度不超过 1500

<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/759013d7-61d8-4509-897a-d75af598a236.png"/ width="600">
</div>

## MAC地址

MAC 地址是链路层地址，长度为 6 字节（48 位），用于唯一标识网络适配器（网卡）。

一台主机拥有多少个网络适配器就有多少个 MAC 地址。例如笔记本电脑普遍存在无线网络适配器和有线网络适配器，因此就有两个 MAC 地址。

## 局域网

局域网是一种典型的广播信道，主要特点是网络为一个单位所拥有，且地理范围和站点数目均有限。

主要有以太网、令牌环网、FDDI 和 ATM 等局域网技术，目前以太网占领着有线局域网市场。

可以按照网络拓扑结构对局域网进行分类：
<div align="center">
 <img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/807f4258-dba8-4c54-9c3c-a707c7ccffa2.jpg"/>
</div>

## 以太网

以太网是一种星型拓扑结构局域网。

早期使用集线器进行连接，集线器是一种物理层设备， 作用于比特而不是帧，当一个比特到达接口时，集线器重新生成这个比特，并将其能量强度放大，从而扩大网络的传输距离，之后再将这个比特发送到其它所有接口。如果集线器同时收到两个不同接口的帧，那么就发生了碰撞。

目前以太网使用交换机替代了集线器，交换机是一种链路层设备，它不会发生碰撞，能根据 MAC 地址进行存储转发。

以太网帧格式：

- **类型** ：标记上层使用的协议；
- **数据** ：长度在 46-1500 之间，如果太小则需要填充；
- **FCS** ：帧检验序列，使用的是 CRC 检验方法；

<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/164944d3-bbd2-4bb2-924b-e62199c51b90.png"/ width="700">
</div>

## 交换机

交换机具有自学习能力，学习的是交换表的内容，交换表中存储着 MAC 地址到接口的映射。

正是由于这种自学习能力，因此交换机是一种即插即用设备，不需要网络管理员手动配置交换表内容。

下图中，交换机有 4 个接口，主机 A 向主机 B 发送数据帧时，交换机把主机 A 到接口 1 的映射写入交换表中。为了发送数据帧到 B，先查交换表，此时没有主机 B 的表项，那么主机 A 就发送广播帧，主机 C 和主机 D 会丢弃该帧，主机 B 回应该帧向主机 A 发送数据包时，交换机查找交换表得到主机 A 映射的接口为 1，就发送数据帧到接口 1，同时交换机添加主机 B 到接口 2 的映射。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/a4444545-0d68-4015-9a3d-19209dc436b3.png"/>
</div>

## 虚拟局域网

虚拟局域网可以建立与物理位置无关的逻辑组，只有在同一个虚拟局域网中的成员才会收到链路层广播信息。

例如下图中 (A1, A2, A3, A4) 属于一个虚拟局域网，A1 发送的广播会被 A2、A3、A4 收到，而其它站点收不到。

使用 VLAN 干线连接来建立虚拟局域网，每台交换机上的一个特殊接口被设置为干线接口，以互连 VLAN 交换机。IEEE 定义了一种扩展的以太网帧格式 802.1Q，它在标准以太网帧上加进了 4 字节首部 VLAN 标签，用于表示该帧属于哪一个虚拟局域网。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/e98e9d20-206b-4533-bacf-3448d0096f38.png"/ width="500">
</div>

# 网络层

## 概述

因为网络层是整个互联网的核心，因此应当让网络层尽可能简单。网络层向上只提供简单灵活的、无连接的、尽最大努力交互的数据报服务。

使用 IP 协议，可以把异构的物理网络连接起来，使得在网络层看起来好像是一个统一的网络。

<div align="center"></div>
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/8d779ab7-ffcc-47c6-90ec-ede8260b2368.png"/>
</div>

与 IP 协议配套使用的还有三个协议：

- 地址解析协议 ARP（Address Resolution Protocol）
- 网际控制报文协议 ICMP（Internet Control Message Protocol）
- 网际组管理协议 IGMP（Internet Group Management Protocol）

## IP数据报文格式
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/85c05fb1-5546-4c50-9221-21f231cdc8c5.jpg"/>
</div>

- **版本** : 有 4（IPv4）和 6（IPv6）两个值；
- **首部长度** : 占 4 位，因此最大值为 15。值为 1 表示的是 1 个 32 位字的长度，也就是 4 字节。因为固定部分长度为 20 字节，因此该值最小为 5。如果可选字段的长度不是 4 字节的整数倍，就用尾部的填充部分来填充。
- **区分服务** : 用来获得更好的服务，一般情况下不使用。
- **总长度** : 包括首部长度和数据部分长度。
- **生存时间** ：TTL，它的存在是为了防止无法交付的数据报在互联网中不断兜圈子。以路由器跳数为单位，当 TTL 为 0 时就丢弃数据报。
- **协议** ：指出携带的数据应该上交给哪个协议进行处理，例如 ICMP、TCP、UDP 等。
- **首部检验和** ：因为数据报每经过一个路由器，都要重新计算检验和，因此检验和不包含数据部分可以减少计算的工作量。
- **标识** : 在数据报长度过长从而发生分片的情况下，相同数据报的不同分片具有相同的标识符。
- **片偏移** : 和标识符一起，用于发生分片的情况。片偏移的单位为 8 字节。


<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/23ba890e-e11c-45e2-a20c-64d217f83430.png"/>
</div>

## IP地址编址方式

IP 地址的编址方式经历了三个历史阶段：

- 分类
- 子网划分
- 无分类

### 1.分类

由两部分组成，网络号和主机号，其中不同分类具有不同的网络号长度，并且是固定的。

IP 地址 ::= {< 网络号 >, < 主机号 >}
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/cbf50eb8-22b4-4528-a2e7-d187143d57f7.png"/ width="500">
</div>

### 2.子网划分

通过在主机号字段中拿一部分作为子网号，把两级 IP 地址划分为三级 IP 地址。

IP 地址 ::= {< 网络号 >, < 子网号 >, < 主机号 >}

要使用子网，必须配置子网掩码。一个 B 类地址的默认子网掩码为 255.255.0.0，如果 B 类地址的子网占两个比特，那么子网掩码为 11111111 11111111 11000000 00000000，也就是 255.255.192.0。

注意，外部网络看不到子网的存在。

### 3.无分类

无分类编址 CIDR 消除了传统 A 类、B 类和 C 类地址以及划分子网的概念，使用网络前缀和主机号来对 IP 地址进行编码，网络前缀的长度可以根据需要变化。

IP 地址 ::= {< 网络前缀号 >, < 主机号 >}

CIDR 的记法上采用在 IP 地址后面加上网络前缀长度的方法，例如 128.14.35.7/20 表示前 20 位为网络前缀。

CIDR 的地址掩码可以继续称为子网掩码，子网掩码首 1 长度为网络前缀的长度。

一个 CIDR 地址块中有很多地址，一个 CIDR 表示的网络就可以表示原来的很多个网络，并且在路由表中只需要一个路由就可以代替原来的多个路由，减少了路由表项的数量。把这种通过使用网络前缀来减少路由表项的方式称为路由聚合，也称为 **构成超网** 。

在路由表中的项目由“网络前缀”和“下一跳地址”组成，在查找时可能会得到不止一个匹配结果，应当采用最长前缀匹配来确定应该匹配哪一个。

## 地址解析协议ARP

网络层实现主机之间的通信，而链路层实现具体每段链路之间的通信。因此在通信过程中，IP 数据报的源地址和目的地址始终不变，而 MAC 地址随着链路的改变而改变。 

<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/66192382-558b-4b05-a35d-ac4a2b1a9811.jpg"/ width="600">
</div>

ARP 实现由 IP 地址得到 MAC 地址。 
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/b9d79a5a-e7af-499b-b989-f10483e71b8b.jpg"/ width="600">
</div>

每个主机都有一个 ARP 高速缓存，里面有本局域网上的各主机和路由器的 IP 地址到 MAC 地址的映射表。

如果主机 A 知道主机 B 的 IP 地址，但是 ARP 高速缓存中没有该 IP 地址到 MAC 地址的映射，此时主机 A 通过广播的方式发送 ARP 请求分组，主机 B 收到该请求后会发送 ARP 响应分组给主机 A 告知其 MAC 地址，随后主机 A 向其高速缓存中写入主机 B 的 IP 地址到 MAC 地址的映射。

<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/8006a450-6c2f-498c-a928-c927f758b1d0.png"/ width="700">
</div>

## 国际控制报文协议ICMP

ICMP 是为了更有效地转发 IP 数据报和提高交付成功的机会。它封装在 IP 数据报中，但是不属于高层协议。 

<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/e3124763-f75e-46c3-ba82-341e6c98d862.jpg"/ width="600">
</div>

ICMP 报文分为差错报告报文和询问报文。 

<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/aa29cc88-7256-4399-8c7f-3cf4a6489559.png"/ width="600">
</div>

### 1.Ping

Ping 是 ICMP 的一个重要应用，主要用来测试两台主机之间的连通性。

Ping 的原理是通过向目的主机发送 ICMP Echo 请求报文，目的主机收到之后会发送 Echo 回答报文。Ping 会根据时间和成功响应的次数估算出数据包往返时间以及丢包率。

### 2.Traceroute

Traceroute 是 ICMP 的另一个应用，用来跟踪一个分组从源点到终点的路径。

Traceroute 发送的 IP 数据报封装的是无法交付的 UDP 用户数据报，并由目的主机发送终点不可达差错报告报文。

- 源主机向目的主机发送一连串的 IP 数据报。第一个数据报 P1 的生存时间 TTL 设置为 1，当 P1 到达路径上的第一个路由器 R1 时，R1 收下它并把 TTL 减 1，此时 TTL 等于 0，R1 就把 P1 丢弃，并向源主机发送一个 ICMP 时间超过差错报告报文；
- 源主机接着发送第二个数据报 P2，并把 TTL 设置为 2。P2 先到达 R1，R1 收下后把 TTL 减 1 再转发给 R2，R2 收下后也把 TTL 减 1，由于此时 TTL 等于 0，R2 就丢弃 P2，并向源主机发送一个 ICMP 时间超过差错报文。
- 不断执行这样的步骤，直到最后一个数据报刚刚到达目的主机，主机不转发数据报，也不把 TTL 值减 1。但是因为数据报封装的是无法交付的 UDP，因此目的主机要向源主机发送 ICMP 终点不可达差错报告报文。
- 之后源主机知道了到达目的主机所经过的路由器 IP 地址以及到达每个路由器的往返时间。

## 虚拟专用网VPN

由于 IP 地址的紧缺，一个机构能申请到的 IP 地址数往往远小于本机构所拥有的主机数。并且一个机构并不需要把所有的主机接入到外部的互联网中，机构内的计算机可以使用仅在本机构有效的 IP 地址（专用地址）。

有三个专用地址块：

- 10.0.0.0 ~ 10.255.255.255
- 172.16.0.0 ~ 172.31.255.255
- 192.168.0.0 ~ 192.168.255.255

VPN 使用公用的互联网作为本机构各专用网之间的通信载体。专用指机构内的主机只与本机构内的其它主机通信；虚拟指好像是，而实际上并不是，它有经过公用的互联网。

下图中，场所 A 和 B 的通信经过互联网，如果场所 A 的主机 X 要和另一个场所 B 的主机 Y 通信，IP 数据报的源地址是 10.1.0.1，目的地址是 10.2.0.3。数据报先发送到与互联网相连的路由器 R1，R1 对内部数据进行加密，然后重新加上数据报的首部，源地址是路由器 R1 的全球地址 125.1.2.3，目的地址是路由器 R2 的全球地址 194.4.5.6。路由器 R2 收到数据报后将数据部分进行解密，恢复原来的数据报，此时目的地址为 10.2.0.3，就交付给 Y。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/1556770b-8c01-4681-af10-46f1df69202c.jpg"/>
</div>


## 网络地址转换NAT

专用网内部的主机使用本地 IP 地址又想和互联网上的主机通信时，可以使用 NAT 来将本地 IP 转换为全球 IP。

在以前，NAT 将本地 IP 和全球 IP 一一对应，这种方式下拥有 n 个全球 IP 地址的专用网内最多只可以同时有 n 台主机接入互联网。为了更有效地利用全球 IP 地址，现在常用的 NAT 转换表把传输层的端口号也用上了，使得多个专用网内部的主机共用一个全球 IP 地址。使用端口号的 NAT 也叫做网络地址与端口转换 NAPT。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/2719067e-b299-4639-9065-bed6729dbf0b.png"/>
</div>

## 路由器的结构

路由器从功能上可以划分为：路由选择和分组转发。

分组转发结构由三个部分组成：交换结构、一组输入端口和一组输出端口。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/c3369072-c740-43b0-b276-202bd1d3960d.jpg"/>
</div>

## 路由器分组转发流程

- 从数据报的首部提取目的主机的 IP 地址 D，得到目的网络地址 N。
- 若 N 就是与此路由器直接相连的某个网络地址，则进行直接交付；
- 若路由表中有目的地址为 D 的特定主机路由，则把数据报传送给表中所指明的下一跳路由器；
- 若路由表中有到达网络 N 的路由，则把数据报传送给路由表中所指明的下一跳路由器；
- 若路由表中有一个默认路由，则把数据报传送给路由表中所指明的默认路由器；
- 报告转发分组出错。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/1ab49e39-012b-4383-8284-26570987e3c4.jpg"/>
</div>

## 路由选择协议

路由选择协议都是自适应的，能随着网络通信量和拓扑结构的变化而自适应地进行调整。

互联网可以划分为许多较小的自治系统 AS，一个 AS 可以使用一种和别的 AS 不同的路由选择协议。

可以把路由选择协议划分为两大类：

- 自治系统内部的路由选择：RIP 和 OSPF
- 自治系统间的路由选择：BGP

### 1.内部网关协议RIP

RIP 是一种基于距离向量的路由选择协议。距离是指跳数，直接相连的路由器跳数为 1。跳数最多为 15，超过 15 表示不可达。

RIP 按固定的时间间隔仅和相邻路由器交换自己的路由表，经过若干次交换之后，所有路由器最终会知道到达本自治系统中任何一个网络的最短距离和下一跳路由器地址。

距离向量算法：

- 对地址为 X 的相邻路由器发来的 RIP 报文，先修改报文中的所有项目，把下一跳字段中的地址改为 X，并把所有的距离字段加 1；
- 对修改后的 RIP 报文中的每一个项目，进行以下步骤：若原来的路由表中没有目的网络 N，则把该项目添加到路由表中；否则：若下一跳路由器地址是 X，则把收到的项目替换原来路由表中的项目；否则：若收到的项目中的距离 d 小于路由表中的距离，则进行更新（例如原始路由表项为 Net2, 5, P，新表项为 Net2, 4, X，则更新）；否则什么也不做。
- 若 3 分钟还没有收到相邻路由器的更新路由表，则把该相邻路由器标为不可达，即把距离置为 16。

RIP 协议实现简单，开销小。但是 RIP 能使用的最大距离为 15，限制了网络的规模。并且当网络出现故障时，要经过比较长的时间才能将此消息传送到所有路由器。

### 2.内部网关协议OSPF

开放最短路径优先 OSPF，是为了克服 RIP 的缺点而开发出来的。

开放表示 OSPF 不受某一家厂商控制，而是公开发表的；最短路径优先表示使用了 Dijkstra 提出的最短路径算法 SPF。

OSPF 具有以下特点：

- 向本自治系统中的所有路由器发送信息，这种方法是洪泛法。
- 发送的信息就是与相邻路由器的链路状态，链路状态包括与哪些路由器相连以及链路的度量，度量用费用、距离、时延、带宽等来表示。
- 只有当链路状态发生变化时，路由器才会发送信息。

所有路由器都具有全网的拓扑结构图，并且是一致的。相比于 RIP，OSPF 的更新过程收敛的很快。

### 3.外部网关协议BGP

BGP（Border Gateway Protocol，边界网关协议）

AS 之间的路由选择很困难，主要是由于：

- 互联网规模很大；
- 各个 AS 内部使用不同的路由选择协议，无法准确定义路径的度量；
- AS 之间的路由选择必须考虑有关的策略，比如有些 AS 不愿意让其它 AS 经过。

BGP 只能寻找一条比较好的路由，而不是最佳路由。

每个 AS 都必须配置 BGP 发言人，通过在两个相邻 BGP 发言人之间建立 TCP 连接来交换路由信息。
<div align="center">
<img src="https://docsify-1258928558.cos.ap-guangzhou.myqcloud.com/computer-network/9cd0ae20-4fb5-4017-a000-f7d3a0eb3529.png"/>
</div>
