---
title: Sim2Real Transfer
date: 2023-02-06 16:10:00
tag: 
- survey
- sim2real
- transfer learning
---



sim2real的全称是simulation to reality，是强化学习的一个分支，同时也属于transfer learning的一种。主要解决的问题是机器人领域中，直接让机器人或者机械臂在现实环境中与环境进行交互、采样时，会出现以下两个比较严重的问题：

- 采样效率太低（在用强化学习算法解决机器人相关问题时，所需要的样本量一般会达到上千万，在现实环境中采集如此数量级的样本要耗费几个月的时间）
- 安全问题 （由于强化学习需要通过智能体在环境中进行大范围的随机采样来进行试错，因而在某些时刻其做出的行为可能会损伤机器人自身，例如手臂转动角度过大或者避障任务中由于碰撞造成的不可逆损伤等等；也可能会损害周围的环境甚至生物）

但是如果我们在模拟器中进行强化学习算法的训练，以上两个问题均可迎刃而解。但是，这里同样会存在一个问题，由于模拟器对于物理环境的建模都是存在误差的，因而在模拟环境中学习到的最优策略是否可以直接在现实环境中应用呢？答案往往是否定的，我们把这个问题称为 “reality gap”。而sim2real的工作就是去尝试解决这个问题。

这里值得注意的一点是，虽然这个方向叫做sim2real，其实其中的所有的算法都可以直接应用在sim2sim，real2real等的任务中。

[引自：https://zhuanlan.zhihu.com/p/106216904]

本文找了一篇survey，对其中的内容作出整理，意图对整个sim2real领域有一个大致的了解。

## 分类

以下分类引自

> W. Zhao, J. P. Queralta and T. Westerlund, "Sim-to-Real Transfer in Deep Reinforcement Learning for Robotics: a Survey," 2020 IEEE Symposium Series on Computational Intelligence (SSCI), Canberra, ACT, Australia, 2020, pp. 737-744, doi: 10.1109/SSCI47803.2020.9308468.

### Zero-shot Transfer

建立一个逼真的模拟器，或者有足够的模拟经验，这样模型就可以直接应用到现实环境中。这种策略通常称为zero-shot transfer或direct transfer。建立真实世界精确模型的系统识别（System Identification）和域随机化（Domain Randomization Methods）是可以被视为一次性迁移的技术。我们在第 III-B 节和第III-C节中分别讨论了这两个问题。

### System Identification

值得注意的是，模拟器不是真实世界的忠实代表。**系统识别[51]正是为了建立物理系统的精确数学模型，并使模拟器更真实，需要仔细校准**。尽管如此，获得足够逼真的模拟器的挑战仍然存在。例如，很难构建高质量的渲染图像来模拟真实的视觉。此外，同一机器人的许多物理参数可能会因温度、湿度、定位或其磨损而发生显著变化，这给系统识别带来了更大的困难。

### Domain Randomization Methods

领域随机化是这样一种想法[52]，我们可以高度随机化模拟，而不是仔细建模真实世界的所有参数，以覆盖真实世界数据的真实分布，尽管模型和真实世界之间存在偏差。图3a显示了域随机化的范例。

### Domain Adaptation Methods

域自适应方法使用来自源域的数据来提高学习模型在数据总是较少可用的不同目标域上的性能。由于源域和目标域之间通常存在不同的特征空间，为了更好地迁移源数据中的知识，我们应该尝试使这两个特征空间统一。这是域自适应的主要精神，可以用图3b中的图来描述。

![image-20230206202202709](https://raw.githubusercontent.com/KMdsy/figurebed/master/img/image-20230206202202709.png)

### Learning with Disturbances (干扰)

Domain randomization 和 dynamics randomization侧重于在模拟环境中引入扰动，目的是使代理不易受模拟与现实之间不匹配的影响[30]、[38]、[40]。在其他作品中也扩展了相同的概念，其中引入了扰动以获得更鲁棒的代理。例如，在[72]中，作者考虑了嘈杂的奖励。虽然与模拟到真实的迁移没有直接关系，但嘈杂的奖励可以更好地模拟真实世界的代理训练。此外，在我们最近的一些作品中[8]，[73]，我们考虑了影响并行学习的不同代理的环境扰动。当要使用通用策略部署或训练多个真实代理时，这是需要考虑的一个方面。

### Simulation Environments

sim2real的一个关键方面是**模拟器**的选择。独立于用于有效地将知识转移到真实机器人的技术，模拟器越逼真，预期的结果就越好。文献中使用最广泛的模拟器是 Gazebo [74]、Unity3D 和 PyBullet [75]或 MuJoCo [17]。Gazebo 具有与机器人操作系统 (ROS) 中间件广泛集成的优势，因此可以与真实机器人中存在的部分机器人堆栈一起使用。另一方面，PyBullet 和 MuJoCo 与 DL 和 RL 库以及体育馆环境进行了更广泛的集成。一般来说，Gazebo 适合更复杂的场景，而 PyBullet 和 MuJoCo 提供更快的训练。

在以一次性传输的系统识别为目标的情况下，研究人员通常会构建或定制满足特定问题要求和约束的特定模拟[32]、[36]、[41]。
