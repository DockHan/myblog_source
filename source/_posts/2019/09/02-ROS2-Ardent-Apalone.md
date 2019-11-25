---
title: 02 ROS2 Ardent Apalone
categories:
  - ROS
tags:
  - ROS2
type: home
comments: true
date: 2019-09-08 16:09:51
---

>引用： 
- [古月居](http://www.guyuehome.com/column/ros2%e6%8e%a2%e7%b4%a2%e6%80%bb%e7%bb%93)
- [ros1 wiki](http://wiki.ros.org)
- [ros2 wiki](http://github.com/ros2/ros2/wiki)
- [ROS2安装说明](https://blog.csdn.net/zhangrelay/article/details/78778590)

## ROS2的介绍,与ROS1的区别
---
- ROS (Robot Operating System)机器人操作系统，主要分为两代release:
- **ROS1 (代号 Kinetic/indigo)**: http://wiki.ros.org
- **ROS2 (代号 Ardent Apolane)**: http://github.com/ros2/ros2/wiki

### 相比ROS1，ROS2的涉及目标
1. 支持多机器人系统
   ROS2增强了对多机器人系统的支持，提高了多机器人之间网络通讯的网络性能，更多多机器人系统及应用将出现在ROS社区中。
2. 铲除原型与产品之间的鸿沟
  ROS2不仅针对科研领域，还关注机器人从研究到应用之间的过渡，可以让更多机器人直接搭载ROS2系统走向市场
3. 支持微控制器
  ROS2不仅可以运行在现有的X86和ARM系统上，还将支持MCU等嵌入式微控制器，比如常用的ARM-M4、M7内核。
4. 支持实时控制
  ROS2还加入了实时控制的支持，可以提高控制的时效性和整体机器人的性能。
5. 跨系统平台支持
  ROS2不止能运行在Linux系统之上，还增加了对Windows和MACOS等系统的支持，让开发者的选择更加自由。

### ROS2的架构与ROS1的区别
![ROS2架构](/../../../images/01&#32;ros2archtecture.png)
1. OS层
  ROS1主要构建于Linux系统之上，ROS2带来了改变，支持构建的系统包括Linux、Windows、Mac、RTOS，甚至没有操作系统的裸机。
2. 中间件
  ROS中最重要的一个概念就是基于发布/订阅模型的“节点”，可以让开发者并行开发低耦合的功能模块，并且便于二次复用。ROS1的通讯系统基于TCPROS/UDPROS，而ROS2的通讯系统基于DDS。DDS是一种分布式实时系统中数据发布/订阅的标准解决方案，下一小节会具体讲解。ROS2内部提供了DDS的抽象层实现，用户不需要关注底层DDS的提供厂家。

  在ROS1的架构中Nodelet和TCPROS/UDPROS是并列的层次，为同一个进程中的多个节点提供一种更优化的数据传输方式。ROS2中也保留了这种数据传输方式，只不过换了一个名字，叫做“Intra-process”，同样也是独立于DDS。
3.  应用层
  ROS1强依赖于ROS Master，可以想像一旦Master宕机，整个系统会面临如何的窘境。但是从右边ROS2的架构中我们可以发现，之前让人耿耿于怀的Master终于消失了，节点之间使用一种称为“Discovery”的发现机制来获取彼此的信息。
3.1 ROS2的核心——DDS
  DDS，Data Distribution Service，即数据分发服务，2004年由对象管理组织OMG（Object Management Group）发布，是一种专门为实时系统设计的数据分发/订阅标准。

`DDS最早应用于美国海军， 解决舰船复杂网络环境中大量软件升级的兼容性问题，目前已经成为美国国防部的强制标准，同时广泛应用于国防、民航、工业控制等领域，成为分布式实时系统中数据发布/订阅的标准解决方案。其技术核心是以数据为核心的发布/订阅模型（Data-Centric Publish-Subscribe ，DCPS），这种DCPS模型创建了一个“全局数据空间”（global data space）的概念，所有独立的应用都可以访问
在DDS中，每一个发布者或者订阅者都称为参与者（participant），类似于ROS中节点的概念。每一个参与者都可以使用某种定义好的数据类型来读写全局数据空间。`
4 **ROS2的通讯模型**
ROS1 主要是基于topic service等通讯机制，ROS2主要是基于DDS(Data Distribution Service), 所以通讯模型相对会复杂一些：
![通讯模型](/../../../images/02&#32;ros2communication.png)
ROS2的通讯模型有以下几个关键概念：

4.1. **参与者（Domain Participant）**
  一个参与者Participant就是一个容器，对应于一个使用DDS的用户，任何DDS的用户都必须通过Participant来访问全局数据空间。

4.2. **发布者（Publisher）**
  数据发布的执行者，支持多种数据类型的发布，可以与多个数据写入器（DataWriter）相联，发布一种或多种主题（Topic）的消息。

4.3. **订阅者（Subscriber）**
  数据订阅的执行者，支持多种数据类型的订阅，可以与多个数据读取器（DataReader）相联，订阅一种或多种主题（Topic）的消息。

4.4. **数据写入器（DataWriter）**
  应用向发布者更新数据的对象，每个数据写入器对应一个特定的Topic，类似于ROS1中的一个消息发布者。

4.5. **数据读取器（DataReader）**
  应用从订阅者读取数据的对象，每个数据读取器对应一个特定的Topic，类似于ROS1中的一个消息订阅者。

4.6. **主题（Topic）**
  这个和ROS1中的Topic概念一致，一个Topic包含一个名称和一种数据结构。

4.7. **Quality of Service**
  质量服务原则，简称QoS Policy，这是ROS2中新增的、也是非常重要的一个原则，控制各方面与底层的通讯机制，主要从时间限制、可靠性、持续性、历史记录这几个方面，满足用户针对不同场景的数据需求。



## Ubuntu18:04 安装ROS2
---


> 引用： [ROS2探索总结](http://www.guyuehome.com/1228)

