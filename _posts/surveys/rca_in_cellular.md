---
title: Root Cause Analysis and Diagnosis in Cellular Network
date: 2022-04-18 11:05:00
updated: 2022-04-18 16:08:00
tag: 
- survey
- root cause analysis
- diagnosis
---

本文主要调研了，LTE场景下的异常根因分析（RCA）方法，用于支持在4G LTE/5G场景下的用户投诉分析。

2022-04-18: 以“root cause analysis, cellular network”为关键词，调研了google scholar中top 2 pages的相关工作，TODO: 完善某些论文的可借鉴点总结。

<!-- more -->

## 任务目标

- **总体目标**：发现用户的投诉根因，**解释**：网络发生了什么问题，这些问题如何一步一步影响用户（在网络机理方面构成完整链条），最终导致用户投诉。

- **主要发现几类问题**：

    - 网络侧问题（重点）：将用户投诉视为网络出现问题的指示剂，发现网络中出现了什么问题

    - 用户侧问题：上一个问题假设用户的投诉都是公平的，还有一种可能，用户所在的位置有不可避免的环境因素，导致服务体验差

- **下一步工作方向**：将发掘到的<u>异常根因</u>从“<u>单维KPI异常</u>”拓展到“<u>带网络机理链条的多维KPI异常pattern</u>”上

![网络机理图-KPI指标关联关系](https://raw.githubusercontent.com/KMdsy/figurebed/master/img/FM%26PM%20logic%20related%20graph.png)

## 相关工作调研

1. **Root Cause Analysis in 5G/6G Networks**

    Dinis Canastro; Ricardo Rocha; Mário Antunes; Diogo Gomes; Rui L. Aguiar

    2021 8th International Conference on Future Internet of Things and Cloud (FiCloud)

    文章比较新，会议不知名，但可以参考related work部分，以及其中提到的其他工作。

2. 🌟**Automatic root cause analysis based on traces for LTE self-organizing networks**

    Ana Gomez-Andrades; Raquel Barco; Immaculada Serrano; Patricia Delgado; Patricia Caro-Oliver; Pablo Munoz

    IEEE Wireless Communications (Volume: 23, Issue: 3, June 2016)

    本文的研究重点在于，基于用户级别的数据，分析用户连接被释放的原因，本文包含大量有关蜂窝网络的<u>背景知识</u>，很值得借鉴

    - 用户连接被释放的原因可分为：Normal Release, Access Failure, Dropped Connection
    - RF层面上实现自动诊断所需要关注的指标信息（Indicators and Measurements）
    - RF层面上的大体根因包括：Coverage Hole (CH), Lack of Dominant Cells (LD), Cell Edge (CE), Mobility Problems (MP), Interference (I)
        - 注：其他层面的异常原因还包括：excessive antenna downtilt (EAD), too small antenna downtilt (TSAD), coverage hole (CH), too late handover (TLH), inter-system interference (ISI), excessive reduction of transmit power (ERTP), and normal cell (Normal) [link](https://link.springer.com/article/10.1007/s11036-020-01589-1)
    - 本文还包含一些在蜂窝网络中做RCA的必要背景知识（LTE Traces and Events），以及一个user case

    本文所提出的框架大体为：“首先，它根据释放的类型对连接进行分类，随后，根据异常释放的连接的事件信息，确定具体的故障原因。”

3. 🌟**Root Cause Analysis Based on Temporal Analysis ofMetrics Toward Self-Organizing 5G Networks**

    Pablo Muñoz, Isabel de la Bandera, Emil J. Khatib, Ana Gómez-Andrades, Inmaculada Serrano, and Raquel Barco

    IEEE Transactions on Vehicular Technology ( Volume: 66, Issue: 3, March 2017)

    本文提出的自动RCA方法是基于小区级别的KPI的。与现有技术相比，所提出的方法考虑了网络度量的<u>时间依赖性</u>和<u>故障对相邻小区的影响</u>，以实现更好的诊断准确性。

    - 算法的输入包括<u>告警小区以及周围小区的大量KPI</u>。
    - 关联性度量方法被用于计算这些指标之间的相关性以表征网络的状态。
    - 通过计算加权相关性将该状态与存储的故障模式进行比较以提供诊断。
    - 此相关性由根据先前计算的度量相关性和专家知识构建的有效权重进行调制。

4. **Root Cause Analysis of Reduced Accessibility in 4G Networks**

    Diogo Ferreira, Carlos Senna, Paulo Salvador, Luís Cortesão, Cristina Pires, Rui Pedro & Susana Sargento 

    International Conference on Machine Learning for Networking, MLN 2019: Machine Learning for Networking pp 117–133

    本文算是一个案例研究，分析了 4G 网络可访问性降低的可能根本原因，可参考性在于<u>分析结论</u>，以及L<u>TE数据中通用的数据（预）处理思路</u>。

    - 本文分析了 4G 网络可访问性降低的可能根本原因，同时考虑了重要关键绩效指标 (KPI) 的信息，并考虑了它们在以前时间框架中的演变。

    - 结果表明，网络可访问性降低的主要原因与故障切换次数、网络中的电话和短信数量、整体下载量和小区可用性有关。然而，每个小区的可访问性降低的主要原因更多地与每个小区的用户数量及其产生的下载量有关。

5. 🌟**Automatic Root Cause Analysis for LTE Networks Based on Unsupervised Techniques**

    Ana Gómez-Andrades; Pablo Muñoz; Inmaculada Serrano; Raquel Barco

    IEEE Transactions on Vehicular Technology ( Volume: 65, Issue: 4, April 2016)

    【待补充细节】

    
