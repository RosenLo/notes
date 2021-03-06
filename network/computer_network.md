<!-- vim-markdown-toc GFM -->

* [一、概述](#一概述)
    * [1.1 计算机通信模型](#11-计算机通信模型)
    * [1.2 计算机网络的传输技术](#12-计算机网络的传输技术)
        * [1.2.1 广播式链接](#121-广播式链接)
        * [1.2.2 点到点链接](#122-点到点链接)
    * [1.3 交换技术](#13-交换技术)
        * [1.3.1 电路交换](#131-电路交换)
        * [1.3.2 存储-转发交换](#132-存储-转发交换)
    * [1.4 计算机网络参考模型](#14-计算机网络参考模型)
        * [1.4.1 网络体系结构](#141-网络体系结构)
        * [1.4.2 五层协议参考模型](#142-五层协议参考模型)
        * [1.4.3 ISO/OSI七层协议参考模型](#143-isoosi七层协议参考模型)
        * [1.4.4 TCP/IP参考模型](#144-tcpip参考模型)
        * [1.4.5 数据处理流程](#145-数据处理流程)
* [二、物理层](#二物理层)
    * [2.1 数据通信的基本概念](#21-数据通信的基本概念)
        * [2.1.1 数据与信息](#211-数据与信息)
        * [2.1.2 信号](#212-信号)
        * [2.1.3 信道工作方式](#213-信道工作方式)
        * [2.1.4 同步与异步通信](#214-同步与异步通信)
            * [同步通信](#同步通信)
            * [异步通信](#异步通信)
        * [2.1.5 串行通信和并行通信](#215-串行通信和并行通信)
    * [2.2 波特率与比特率*](#22-波特率与比特率)
        * [2.2.1 波特率与比特率的关系*](#221-波特率与比特率的关系)
    * [2.3 数字信号的Fourier分析](#23-数字信号的fourier分析)
    * [2.4 最大数据传输率与带宽的关系](#24-最大数据传输率与带宽的关系)
        * [2.4.1 Nyquist定理*](#241-nyquist定理)
        * [2.4.2 Shannon定理](#242-shannon定理)
        * [2.4.3 噪声信道中的传输速率](#243-噪声信道中的传输速率)
    * [2.5 数据传输](#25-数据传输)
        * [2.5.1 数字数据用模拟信号传输](#251-数字数据用模拟信号传输)
            * [调幅ASK(Amplitude Shift Keying)](#调幅askamplitude-shift-keying)
            * [调频FSK(Frequency Shift Keying)](#调频fskfrequency-shift-keying)
            * [调相PSK(Phase Shift Keying) ](#调相pskphase-shift-keying)
            * [调制举例](#调制举例)
        * [2.5.2 数字数据用数字信号传输](#252-数字数据用数字信号传输)
            * [不归零编码](#不归零编码)
            * [曼切斯特编码*](#曼切斯特编码)
        * [2.5.3 模拟数据在数字信道上传输](#253-模拟数据在数字信道上传输)
        * [2.5.4 数字数据传输的专用术语](#254-数字数据传输的专用术语)
            * [基带传输](#基带传输)
            * [宽带传输](#宽带传输)
    * [2.6 多路复用](#26-多路复用)
        * [2.6.1 频分多路复用FDM（Frequency Division Multiplexing）](#261-频分多路复用fdmfrequency-division-multiplexing)
        * [2.6.2 时分多路复用TDM（Time division Multiplexing）](#262-时分多路复用tdmtime-division-multiplexing)
            * [同步TDM](#同步tdm)
            * [异步TDM](#异步tdm)
    * [2.7 交换技术](#27-交换技术)
        * [2.7.1 电路交换（电话）](#271-电路交换电话)
        * [2.7.2 报文交换（电报）](#272-报文交换电报)
        * [2.7.3 分组交换](#273-分组交换)
        * [2.7.4 虚电路交换](#274-虚电路交换)
        * [2.7.5 信元交换](#275-信元交换)

<!-- vim-markdown-toc -->


# 一、概述

## 1.1 计算机通信模型

- 客户-服务器模型（C/S）：客户号-服务器之间是请求-应答过程。例如：FTP文件下载，HTTP网页浏览。
- 对等通信：每个人可以与其他一个或多个人通信，不存在固定的客户号和服务器角色。例如：迅雷下载。

## 1.2 计算机网络的传输技术

### 1.2.1 广播式链接
- 特点
    - 网络上所有机器共享一个信道；
    - 任何机器发送的数据都可以被其他机器搜到。
- 分类：广播、多播
- 地理位置局部化的网络采用此模式。如局域网：
    - 公共（广播）信道的分配
        - 静态分配
            - 信道预先分配，不随数据的时候及流量等参数而变化
        - 动态分配
            - 信道按照实际需求而动态的分配
            - 包括集中式和非集中式两种
                - 集中式：有一个独立的实体决定信道的下一个使用者（发送者）是谁。
                - 非集中式：每台机器自己决定是否传送数据。

### 1.2.2 点到点链接
- 特点
    - 网络由多个连接构成，一个连接对应一对机器；
    - 数据从源端到目的端可能要经过多台中间机器。
- 别名：单播
- 大型网络通常采用此模式。如广域网：
    - 构成
        - 通信子网和主机
        - 计算机网络=通信子网+资源子网

## 1.3 交换技术

### 1.3.1 电路交换
- 双方建立通信电路连接，并独占该线路的所有数据通信，中间节点不存储任何数据。
- 例如：电话系统

### 1.3.2 存储-转发交换
- 数据分块，每个中间节点完整接收数据块后再发送给下一个节点，在发送之前需要存储接收的数据块。
- 报文交换
    - 例子：电报系统
- 分组交换
    - 和报文交换一样都是先存储再转发，但对数据块的大小有严格限制，使用比报文更小的数据块、更低的延时，更高的系统吞吐量。
    - 计算机网络广泛采用。

## 1.4 计算机网络参考模型

### 1.4.1 网络体系结构

<div> <img src="../assets/network/20180508083811597.png" /> </div><br>

### 1.4.2 五层协议参考模型
五层协议的体系结构只是为介绍网络原理而设计的，实际应用还是TCP/IP四层体系结构

- 应用层：为特定应用程序提供数据传输服务，例如 HTTP、DNS 等。数据单位为报文。
- 运输层：提供的是进程间的通用数据传输服务。由于应用层协议很多，定义通用的运输层协议就可以支持不断增多的应用层协议。运输层包括两种协议：传输控制协议 TCP，提供面向连接、可靠的数据传输服务，数据单位为报文段；用户数据报协议 UDP，提供无连接、尽最大努力的数据传输服务，数据单位为用户数据报。TCP 主要提供完整性服务，UDP 主要提供及时性服务。
- 网络层：为主机之间提供数据传输服务，而运输层协议是为主机中的进程提供服务。网络层把运输层传递下来的报文段或者用户数据报封装成分组。
- 数据链路层：网络层针对的还是主机之间的数据传输服务，而主机之间可以有很多链路，链路层协议就是为同一链路的结点提供服务。数据链路层把网络层传来的分组封装成帧。
- 物理层：考虑的是怎样在传输媒体上传输数据比特流，而不是指具体的传输媒体。物理层的作用是尽可能屏蔽传输媒体和通信手段的差异，使数据链路层感觉不到这些差异。


### 1.4.3 ISO/OSI七层协议参考模型
比五层协议多两层

<div> <img src="../assets/network/osireferencemodel.png" /> </div><br>

- 表示层：数据压缩、加密以及数据描述。这使得应用程序不必担心在各台主机中表示/存储的内部格式不同的问题。
- 会话层：建立及管理会话。

### 1.4.4 TCP/IP参考模型

它只有简单的四层，上层所有的协议都向下汇聚到一个IP协议中。
TCP/IP协议族可以为各式各样的应用服务提供服务（everything over IP）

<div> <img src="../assets/network/2018-11-20_22-38-41.jpg" /> </div><br>

### 1.4.5 数据处理流程



# 二、物理层
物理层考虑的是怎样才能在连接各种计算机的传输媒体上传输数据比特流，而不是指具体的传输媒体。

## 2.1 数据通信的基本概念

### 2.1.1 数据与信息
数据是有意义的实体，涉及的是事物的形式。信息是数据的内容和解释。

数据有模拟数据和数字数据两种形式：

- 模拟数据是指在某个时间区间产生的连续的值。例如，声音和视频、温度和压力等都是连续变化的值
- 数字数据是指产生的离散的值。例如，文本信息和整数

### 2.1.2 信号
信号是数据的表示形式，或称数据的电磁或电子编码。它使数据能以适当的形式在介质上传输。

信号有模拟信号和数字信号两种基本形式：

- 模拟传输：不考虑传输的内容。长距离传输时，采用信号放大器放大衰减的信号。
- 数字传输：关心信号的内容，不论传输的数字信号还是模拟信号。长距离传输时，采用转发器，消除噪声的累积。

长距离传输时，通常采用的是数字传输。
模拟信号在传输中会产生波形失真，难以还原。
数字传输只要能正确区分0/1就足够了，这样使得数字传输比模拟传输更可靠。

### 2.1.3 信道工作方式

- 单工通信：单向传输。如广播、电视
- 半双工：双方都可以发送或接收，但不能同时，即当一方发送时，另一方接收
- 全双工：双方同时可以发送和接收信息，需要两条信道

### 2.1.4 同步与异步通信

#### 同步通信
同步通信是指发送方和接收方的采样时钟是同一个。通常发送方在发送数据的编码中包含时钟，而接收方则从数据流中提取时钟用以采样。所以说双方所用的时钟是同一个。

根据同步通信规程，同步通信又分为面向字符的同步通信和面向bit流的同步通信 
#### 异步通信
异步通信是指发送方和接收方的采样时钟不是同一个。是以字符为单位的数据传输。数据块以字符为单位并以特殊的位作标志，每个字符都要附加1位起始位和1位停止位，以标记字符的开始和结束。此外，还要附加1位奇偶校验位

异步通信必须指定的四个参数：

- 波特率
- 字符长度
- 奇偶校验
- 停止位长度

### 2.1.5 串行通信和并行通信

- 串行通信：数据按位为单位以时间为序
- 并行通信：数据按字符为单位（多位同时）以时间为序

## 2.2 波特率与比特率*

- 波特率：信号变化次数（每秒采样的次数），单位是baud(波特)
- 比特率：数据传输速率，单位是bps、b/s等。

两者之间的差别在于每次采样的量化值

### 2.2.1 波特率与比特率的关系*
如信号分为V级，则比特率 = (log2V) ×波特率

这里V表示每个波特的信号调制等级，代表调制技术(每次采样的位数)的高低

## 2.3 数字信号的Fourier分析

## 2.4 最大数据传输率与带宽的关系
带宽决定了最大数据传输率的上限。

它们之间数学关系
- 无噪声条件下： Nyquist定理
- 有噪声条件下： Shannon定理

### 2.4.1 Nyquist定理*
在无噪声信道中，当带宽为H Hz，信号电平为V级，则：

数据传输速率 = 2Hlog2V b/s（2为底数）

（信号电平为V级，在二进制中，仅为0、1两级）

即：以每秒高于2H次的速率对线路采样是无意义的，因为高频分量已被滤波器滤掉无法再恢复。

### 2.4.2 Shannon定理
在噪声信道中，当带宽为H Hz，信噪比为S/N，则：最大数据传输速率(b/s) = Hlog2(1+S/N)（2为底数）

很多情况下噪声用分贝(dB) 表示：噪声（dB）= 10log10S/N（10为底数）

如：噪声为30dB，则信噪比为S/N=1000

### 2.4.3 噪声信道中的传输速率
在噪声信道（话音信道）中，当带宽为3k Hz，信噪比为30dB（较为真实的电话信道），则：

最大数据传输速率(b/s) = Hlog2(1+S/N)

= 3000log2(1+1000)

= 30000 (b/s)

最大数据传输速率为30k b/s，这是在噪声信道中的传输速率极限，实际上是难以达到的

## 2.5 数据传输

### 2.5.1 数字数据用模拟信号传输
将数字数据调制成模拟信号进行传输。通常有三种方式：

#### 调幅ASK(Amplitude Shift Keying)
用载波的两个不同的振幅来表示两个二进制值。如用无信号表示0，有信号表示1。

<div> <img src="../assets/network/2018-11-27_22-12-40.jpg" /> </div><br>

#### 调频FSK(Frequency Shift Keying)
用载波附近的两个不同的频率来表示两个二进制值。如用信号频率为2f表示0，信号频率为f表示1。
<div> <img src="../assets/network/2018-11-27_22-15-15.jpg" /> </div><br>

#### 调相PSK(Phase Shift Keying) 
用载波的相位移动来表示二进制值。如用信号相位角为0表示0，相位角为π表示1。
<div> <img src="../assets/network/2018-11-27_22-16-17.jpg" /> </div><br>

#### 调制举例
<div> <img src="../assets/network/2018-11-27_22-23-40.jpg" /> </div><br>

### 2.5.2 数字数据用数字信号传输
最普通的方法是用两个不同的电压值来表示两个二进制值0和1。
常用的数字信号编码有：

#### 不归零编码
正电平表示1，0电平表示0，并且在表示完一个码元后，电压毋需回到0。缺点是存在发送方和接收方的同步问题。

#### 曼切斯特编码*
- bit中间有信号低-高跳变为0
- bit中间有信号高-低跳变为1

**特点：每一位数据需要两个时钟周期，因此信号的频率是数据率的2倍(例如10Mbps需要20MHz信号频率)**

### 2.5.3 模拟数据在数字信道上传输
采样、量化和编码

### 2.5.4 数字数据传输的专用术语
#### 基带传输
由信源产生的原始电信号称为基带信号。如将数字信号0、1直接用两种不同的电压表示，然后送到线路上去传输。

用于数字传输：局域网，50 Ω，通常传输距离为185m（细缆）、500m（粗缆）

#### 宽带传输
将基带信号进行调制后形成模拟信号，然后采用频分复用技术实现宽带传输

有线电视网：带宽可达300MHz ~ 450MHz，由于以模拟信号传输，所以传输距离可达100km

宽带系统可分为多个信道，所以模拟和数字信号可混合使用。但通常需解决数据双向传输的问题

在混合光缆HFC（Hybrid Fiber Coax）中，450 ~ 550MHZ是电视信号。550-750MHZ是数字信号

## 2.6 多路复用
无论是广域网还是局域网，都存在这样一个事实，即传输介质的带宽大于传输单一信号所需的带宽。为了有效地利用传输系统，通常采用多路复用（Multiplexing）技术以同时携带多路信号来高效率地使用传输介质。多路复用主要有两种：

### 2.6.1 频分多路复用FDM（Frequency Division Multiplexing）
FDM是基于这样的前提：传输介质的可用带宽必须超过各路给定信号所需带宽的总和。如果将这几路信号中的每路信号都以不同的载波频率进行调制，而且各路载波频率之间留有一定的间隔以使各路信号带宽不相互重叠，那么这些信号就可同时在介质上传输。

<div> <img src="../assets/network/2018-11-29_22-55-32.jpg" /> </div><br>


### 2.6.2 时分多路复用TDM（Time division Multiplexing）
每个信号按时间先后轮流交替地使用单一信道，那么，多个数字信号的传输便可在宏观上同时进行。对单一信道的交替使用可以按位、字节或块等为单位来进行。

<div> <img src="../assets/network/2018-11-29_23-06-23.jpg" /> </div><br>

#### 同步TDM
时间片与输入装置一一对应，即同步

如某个时间片对应的装置无数据发送，则该时间片空闲

传输介质的传输速率不能低于各个输入信号的数据速率之和

#### 异步TDM
时间片是按需动态分配的

时间片与输入装置之间没有对应关系，任何一个时间片都可以被用于传输任何一路输入信号

在传输的数据单元中必须包含有地址信息，以便寻址目的节点

传输介质的传输速率不低于各输入信号的平均数据速率之和

异步TDM又称为统计TDM（STDM）

## 2.7 交换技术
### 2.7.1 电路交换（电话）
- 在数据传输前，必须建立端到端的连接，称为连接。其中可能穿越多个交换局，每个交换局都必须提供连接
- 一旦某个节点故障，必须重新建立连接
- 连接建立后，整个通路将被独占，数据的传输没有额外的延时，数据中不必包含地址域
- 数据按序传输，但信道的使用率较低
- 适合传输大批量的数据，如流数据

### 2.7.2 报文交换（电报）
- 无论数据传输过程要跨越多少个交换局（通常是路由器），只要下一站不忙，该数据即送至下一站
- 数据的传输毋需建立连接，数据的传输是一站一站往下送，所以数据中必须包含目的地址，采用存储-转发（store-forward）机制
- 线路的利用率较高
- 数据传输过程中，可能延时较大，且不可估计
- 由于报文大小不定，所以每个中间站点都必须有足够的缓存，通常采用硬盘作缓存

### 2.7.3 分组交换
- 与报文交换相似，只是将报文分为若干个定长的分组，每个分组为一个子报文
- 数据中必须包含目的地址或虚电路号
- 采用存储-转发机制，延时不可估计
- 线路的利用率较高
- 由于报文大小固定，所以每个中间站点的缓存通常是内存
- 接收分组和发送分组的顺序可能不一致 ，并且可能还需要重组
- 适合传输文本型数据

<div> <img src="../assets/network/2018-11-29_23-26-53.jpg" /> </div><br>

### 2.7.4 虚电路交换
- 将电路交换的概念引入到分组传输
- 虚电路连接的建立
	- 传输方发起连接请求，中间节点根据路径信息建立交换表。在交换表内，节点为连接建立一个虚电路号，并与端口号相关联，表示用户信息从该端口输入，以及从那个端口输出到下一节点。
- 虚电路连接的传输
	- 分组中没有目的地址，只有虚电路号。接收分组时只检查其头部，一旦得到其虚电路号，则立即交换表，转发至适当的端口
- 虚电路连接的拆除

### 2.7.5 信元交换
- 将分组分成固定长的单元 — 信元，并用虚电路方式交换。
- ATM网用这种交换方式。


# 三、数据链路层

## 3.1 数据链路层的定义与功能

- 为网络层提供服务
	- 无确认无连接
	- 有确认无连接
	- 有确认有连接
- 保证直接相连的两台主机的可靠传输
	- 将传输的信息组合成帧
	- 校验和重发
	- 流量控制

## 3.2 数据帧的组成
数据帧的组成必须保证能识别一个完整的帧，并保证一旦出现传输差错导致前一个帧丢失，必须能识别下一个帧，即再同步。


## 3.3 纠错码和检错码
- 纠错码：
	- 海明（Hamming） 码
- 检错码：
	- 奇偶校验码
	- 校验和（Check Sum）
	- 块校验码（Block Check Code）
	- 循环冗余检错码 CRC（Cyclic Redundancy Check）

### 3.3.1 海明码
海明距离（Hamming distance）

两个码字中不相同的位的个数称为海明距离

只要对两个码字做异或(XOR)运算，然后计算结果中1的个数即为海明距离。

例如：

```
		10001001
	XOR 10110001
	————————————
		00111000    —> 海明距离为3

```
- 校验理论
	- 为了检验d位的错误，需要一个距离为d+1的编码方案。
	- 为了纠正d个错误，需要一个距离为2d+1的编码方案。

### 3.3.2 奇偶校验码
- 原理
	- 在数据后加一个奇偶(parity)位，奇偶位设置标准是保证码字中“1”位的数目是偶数(或奇数)。
- 例子：1011010
	- 偶校验：10110100
	- 奇校验：10110101
- 校验效果
	- 由于奇偶校验码的Hamming distance是2，因此它只能检验一位的错误。


### 3.3.3 校验和(CheckSum) 
算法简单、实现容易，但检错率不高。此方法在IP头部校验和TCP全段的校验都采用此方法。

将发送的数据看成是二进制整数序列，并划分成一段段规定的长度（如8位、16位、32位等），累加他们的和，校验和是此和的补码。将校验和与数据一起发送。在接收端，所有数据与校验和之和＝0。如要传输”Hello world.” 
