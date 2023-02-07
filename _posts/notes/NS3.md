---
title: NS3无线网络仿真器
date: 2023-02-07 19:51:00
Updated: 2023-02-07 21:00:00
tag: 
- note
- 5G
- 4G
- simulator
- ns3

---

本文整理了在用NS3做无线网络仿真的时候，需要掌握的一些基础知识

<!-- more -->

## 关键名词

- Node：应该将 `Node`视为您将向其添加application的计算机。一个是添加诸如应用程序、协议栈和外围卡及其相关驱动程序之类的东西，以使计算机能够执行有用的工作。*我们在ns-3*中使用相同的基本模型。
- Application：*ns-3* `application`在 *ns-3* `Nodes`上运行，以驱动模拟世界中的模拟。
- Channel：在*ns-3*的模拟世界中，将`Node`连接到表示通信信道的对象。在这里，基本的通信子网络抽象被称为`Channel`，并在C++中由Channel类表示。Channel类提供了管理通信子网络对象和将节点连接到这些对象的方法。channel的实体可以建模像导线这样简单的东西，还可以模拟像大型以太网交换机或无线网络中充满障碍物的三维空间这样复杂的事物。在本教程中，我们将使用名为`CsmaChannel`、`PointToPointChannel`和`WifiChannel`的Channel的实体。
    - `CsmaChannel`为实现载波侦听多址通信介质的通信子网络的一个版本建模。这为我们提供了类似以太网的功能。
- Net Device：*ns-3*中的`net device`抽象类包含了软件驱动与模拟的硬件设备，一个`net device`被“安装”在`Node`中，用于使能`Node`与其他`Node`之间经由`Channel`的通信。正如一个真实的计算机，一个`Node`可以通过多个`net device`链接多个`channel`。
- Topology Helpers：在*ns-3*中，连接`Net Devices`到`Node`，连接`NetDevice`到`channel`，分配IP地址等都是常见的任务，因此我们提供了我们称之为Topology Helpers的功能，以尽可能简化这一过程。
    - 创建网络设备、添加MAC地址、在节点上安装该网络设备、配置节点的协议堆栈，然后将网络设备连接到信道，可能需要许多不同的ns-3核心操作。甚至需要更多的操作来将多个设备连接到多点信道上，然后将单个网络连接到互联网上。我们提供了拓扑帮助器对象，这些对象将这些不同的操作组合成一个易于使用的模型。

