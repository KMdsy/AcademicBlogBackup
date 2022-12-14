---
title: 微服务框架下的多源异构数据异常检测 - 调研
date: 2021-04-13 12:00:00
updated: 2022-04-18 13:07:00
tag: 
- survey
- AIOps
- micro-service
- cloud native
- dataset
---

2021-04-13: 整理了微服务框架下，做异常检测的大体思路，分别调研“多源异构数据融合”，以及分别基于“时间序列”“日志”“调用链”数据的异常检测方法。

2022-04-18: 补充了“异常根因分析”相关的工作，这些工作大多是基于图的，可以为4G LTE种的根因分析提供思路。

<!-- more -->

[裴丹团队的主页](https://netman.aiops.org/publications/)

[2020 AIOPS workshop](https://aiopsworkshop.github.io/accepted_papers/index.html)

[Train Ticket -- Test Bed](https://github.com/FudanSELab/train-ticket/)

[Sock shop -- Test bed](https://microservices-demo.github.io/)



## 一些笔记

在相关工作<a href="#lundetecting2021">[1]</a>中：

1. 所要检测的异常如何定义，是一个关键的问题。本文提到"error messages", "performance degradations"与"trace structure and response time anomalies"本身就是几种不同的异常类型，融合多种数据将有助于我们探测更多种类的异常。

2. 本文所建立的调用树中包含了两种调用关系——本地调用（两个微服务位于同一个主机，彼此之间进行调用）、远程调用（两个微服务不在同一个主机，远程调用），但好像在分析的时候没什么区别。

在相关工作<a href="#multimodalsasho2019">[2]</a>中：

1. trace数据具有多模态，即meta action之间的结构（因果关系模态，或sequential nature模态）、服务响应时间（实值序列）。

2. trace数据的一个有趣的特性在于，它是一种能够良好的反映服务层状态的数据，同时还包含有大量的底层信息。

3. tree结构的调用在实际调用的时候，存在高并发的特性，即：两个被同一组件同时调用的服务，在响应的时候可能出现先后的差异，这在使用序列作为trace表征方式中是非常常见的，但这种差异不代表异常。

## 相关论文



#### 根因分析

##### 基于图的方法

1. **Graph-based root cause analysis for service-oriented and microservice architectures**

    ÁlvaroBrandó, Marc Solé, Alberto Huélamo, David Solans, María S.Pérez, VictorMuntés-Mulero

    Journal of Systems and Software, Volume 159, January 2020, 110432

2. **GRANO: interactive graph-based root cause analysis for cloud-native distributed data platform**

    Hanzhang Wang, Phuong Nguyen, Jun Li, Selcuk Kopru, Gene Zhang, Sanjeev Katariya, Sami Ben-Romdhane

    Proceedings of the VLDB EndowmentVolume 12Issue 12August 2019

我们通过提供系统组件拓扑、警报和应用程序事件的整体视图，展示了 Grano，这是一个用于云原生分布式数据平台的端到端异常检测和根本原因分析（或简称 RCA）系统。Grano 提供： 一个*检测层*，用于处理大量时间序列监控数据，以检测逻辑和物理系统组件的异常情况；*Anomaly Graph Layer*具有新颖的图形建模和算法，用于利用系统拓扑数据和检测结果来识别系统组件级别的根本原因相关性；和*应用层*自动通知待命人员，并通过交互式图形界面提供实时和按需 RCA 支持。该系统使用 eBay 的生产数据进行部署和评估，以帮助值班人员将根本原因的识别时间从几小时缩短到几分钟。

3. **A Causality Mining and Knowledge Graph Based Method of Root Cause Diagnosis for Performance Anomaly in Cloud Application**s

    Juan Qiu, Qingfeng Du, Kanglin Yin, Shuang-Li Zhang, and Chongshu Qian

    Applied Sciences, Volume 10, Issue 6

随着云计算技术的发展，微服务架构（MSA）已经成为云原生应用中流行的应用架构。很多面向用户的服务由很多微服务支持，服务之间的依赖比传统的单体架构应用更加复杂。在这种情况下，如果一个微服务的性能指标发生异常变化，就会导致其他相关服务降级甚至出现故障，这可能会给依赖的业务带来很大的损失。因此，在云应用的运维工作中，挖掘问题的因果关系，尽快找到根源至关重要。在本文中，我们提出了一种挖掘因果关系和诊断根本原因的方法，该方法使用知识图谱技术和因果搜索算法。我们在一个经典的云原生应用上验证了所提出的方法，发现该方法是有效的。将我们的方法应用于云原生应用程序的大部分服务后，准确率和召回率均超过 80%。

4. **Groot: An Event-graph-based Approach for Root Cause Analysis in Industrial Settings**

    Hanzhang Wang; Zhengkai Wu; Huai Jiang; Yichao Huang; Jiamu Wang; Selcuk Kopru; Tao Xie

    2021 36th IEEE/ACM International Conference on Automated Software Engineering (ASE)

对于大型分布式系统，有效诊断事件的根本原因以保持系统的高可用性至关重要。微服务架构的最新发展为工业环境中的根本原因分析 (RCA) 带来了三大挑战（即操作的复杂性、系统规模和监控）。为了应对这些挑战，在本文中，我们提出了 Groot，一种基于事件图的 RCA 方法。 Groot 基于事件构建实时因果关系图，这些事件汇总了被分析系统中的各种类型的指标、日志和活动。此外，为了整合来自站点可靠性工程 (SRE) 工程师的领域知识，Groot 可以使用用户定义的事件和特定领域的规则进行定制。目前，Groot 在 5,000 项实际生产服务中支持 RCA，并被 eBay 的 SRE 团队积极使用，eBay 是一个全球电子商务系统，每年为超过 1.59 亿活跃买家提供服务。在 15 个月内，我们收集了一个数据集，其中包含 952 个实际生产事件的标记根本原因以进行评估。评估结果表明，Groot 能够达到 95% 的 top-3 准确率和 78% 的 top-1 准确率。为了分享我们在工业环境中部署和采用 RCA 的经验，我们进行了一项调查以表明 Groot 的用户发现它有用且易于使用。我们还分享了部署和采用 Groot 解决生产环境中的 RCA 问题的经验教训。

5. **Graph Based Root Cause Analysis in Cloud Data Center**

    Divyaansh Dandona; Mevlut Demir; John J. Prevost

    2020 IEEE 15th International Conference of System of Systems Engineering (SoSE)

低成本计算的吸引力和云技术的按需扩展已导致许多软件应用程序迁移到云。这种对云的依赖增加转化为对云数据中心的直接依赖，这些云数据中心形成了现代云。这些数据中心是由许多系统组成的复杂建筑物，这些系统相互交互以托管最终应用程序。在这个系统系统中检测异常事件，然后及时确定其根本原因是一项艰巨的任务。在本文中，我们提出了一个图形模型来封装系统的云数据中心系统，并分享一种减少根本原因分析的搜索空间的方法。



### 关于多源数据融合的文章

1.  **Multi-Source Anomaly Detection in Distributed IT Systems**
	

Jasmin Bogatinovski, Sasho Nedelkoski

知乎解析：[link](https://zhuanlan.zhihu.com/p/347051870)

2020 AIOPS workshop
	
+ 本工作联合考虑了日志数据与trace数据，开发了一种span2vec的方法，用于将trace数据像log数据一样表示为一系列的模板数据，进一步的便于与日志数据进行融合。
+ 异常检测任务在本文中被转化为了一个“下一步模板预测”的**有监督任务**，可以分别对下一步可能出现的日志与模板进行预测，则偏离预测的trace/log模板即为异常。
+ 在本工作中，log与trace数据提供了不同角度的系统状态信息——日志数据可以在服务级别上有更丰富的描述，可以被视为服务运行的的指纹。Trace数据中则没有太多上述系统级的信息，但包含执行一次用户清请求的总流程图。但是从结果上来看，日志数据的加入，提升了预测trace异常的recall，但反之，trace数据的的加入并没有显著提升log数据的异常检测性能。可能的原因在于log的数据粒度比较大。

> One explanation of this behaviour is that the granularity of the information from the logs is truncated on the level of the data source with a lower frequency of generation -- the trace is harder for the information in the trace to be transferred to the logs. The information that the multimodal method is receiving from the logs when it is aiming to predict the next relevant span complements the information as obtained just from the sequence of spans individually.


​	
2. **Multi-source Distributed System Data for AI-Powered Analytics**

	Sasho Nedelkoski, Jasmin Bogatinovski, Ajay Kumar Mandapati, Soeren Becker, Jorge Cardoso, Odej Kao
	
	European Conference on Service-Oriented and Cloud Computing, ESOCC 2020: Service-Oriented and Cloud Computing
	
	开发了一个用于捕捉多源（三种）数据的一个分析平台，对这个test bed做了描述
	
	

3. **An Intelligent Anomaly Detection Scheme for Micro-Services Architectures With Temporal and Spatial Data Analysis**

	Yuan Zuo; Yulei Wu; Geyong Min; Chengqiang Huang; Ke Pei
	
	IEEE Transactions on Cognitive Communications and Networking ( Volume: 6, Issue: 2, June 2020)
	
	联合使用日志（时间数据）和调用链数据query trace（空间数据）做特征提取与融合，并构建one-class classifier
	
	

### 微服务架构下的异常检测

#### 基于调用链数据

1. **Observing and Controlling Performance in Microservices**

	学术毕业论文：André Pascoal Bento
	
	讲使用trace数据，建模系统中微服务之间的依赖关系，并建立为图模型，然后计算出每个依赖关系之间的应答响应时间
	
2. **Unsupervised Detection of Microservice Trace Anomalies through Service-Level Deep Bayesian Networks**

	Ping Liu; Haowen Xu; Qianyu Ouyang; Rui Jiao; Zhekang Chen; Shenglin Zhang; ...
	
	2020 IEEE 31st International Symposium on Software Reliability Engineering (ISSRE)
	
	**基于微服务trace数据，检测意外的调用关系或者意外的调用响应时间**
	
- 本文与其他工作有显著的不同，别的工作的异常检测对象一般都是针对单个的调用meta action，而本工作是针对一整条tree状的trace。

- 本文使用精心设计的手工特征来表征trace数据，向量化的trace数据用一个容量很大的概率模型来学习他们的正常模式。其中之所以要用到容量大的模型，是因为要学习的模式是一整个application的trace，而非一个trace。

- 手工设计的trace数据使得异常检测后的结果可以快速定位异常类型与异常根因，自带可解释性。

	

3.  <a name="multimodalsasho2019"><sup>[2]</sup></a> **Anomaly Detection from System Tracing Data Using Multimodal Deep Learning**

Sasho Nedelkoski; Jorge Cardoso; Odej Kao

2019 IEEE 12th International Conference on Cloud Computing (CLOUD)

本文将trace数据视为了**多模态数据**，第一种模态为由事件序列组成的类似于NLP的序列数据（其实与日志模板序列是相似的），第二种模态是每个事件对应的响应时间。

- 第一步将trace数据异常检测视为了单模态数据的异常检测。其本质是，将上述两种“文本序列”“实值序列”送进LSTM网络中，进行预测，当真实值不在TopK label（文本序列预测）或不在95%置信区间（实值序列预测）时，即为异常。

- 本文提到的多模态融合，其具体方案是，两种数据均通过单层的LSTM网络，然后在第二个隐层中进行融合（concat）。

本文的最大缺陷在于：
	
- 虽然理论上event间的调用因果关系被视为了文本序列中的语法，交由LSTM学习，但是这种拓扑关系本身是已知的，而没有被利用，LSTM学习到的关系是什么，也是不可解释的
- 本文实验中的异常全部都是人工生成的，而且有对照网络输出构造异常样本的嫌疑，实验结果存疑。
- 本文在还提到，本文通过所学习的预测模型，对调用链中的并发与依赖事件进行了重建与识别，但是识别这些事件对异常检测的作用在哪，并发与否在trace数据的JSON文件中不应该是可以直接被解析的吗 
	



4. **Self-Supervised Anomaly Detection from Distributed Traces**

Jasmin Bogatinovski; Sasho Nedelkoski; Jorge Cardoso; Odej Kao

2020 IEEE/ACM 13th International Conference on Utility and Cloud Computing (UCC)
	
- 这篇论文仍然将trace数据建模为**模板序列**，将异常检测问题视为下一模板预测问题。

- 不同之处在于引入了self-supervised learning，具体的，在训练的时候，遮蔽trace中的随机一个span，训练网络预测被遮蔽位置的span是什么。
具体的，网络学习到的是：trace中每一个位置的预测可能性列表

- 在异常检测的时候，统计待测样本中，span不在预测出饿top-k list中的比例，记为anomaly score
	


5. **Anomaly Detection and Classification using Distributed Tracing and Deep Learning**

Sasho Nedelkoski; Jorge Cardoso; Odej Kao

2019 19th IEEE/ACM International Symposium on Cluster, Cloud and Grid Computing (CCGRID)
	
- 本文本质上是对于时间序列提出了一种异常检测的方法，只是根据本文的检测目标——响应时间，将特定一组微服务的响应时间序列作为了研究对象。然而所提出的方法其实可以在广义的时间序列上进行评估。

- 本文着重说明了在正常序列具有多种模式的时候，我们该如何去建模正常，这里提出使用VAE的多维高斯分布来建模这种复杂分布，再利用重建误差作为异常得分。

- 后续，他还通过训练了一个基于1D-CNN的网路，做了异常分类，首先由用户预定义几种异常，然后训练一个有监督的分类器，以帮助将那些检测到的异常归类到已知的异常类型中去。

- 本文比较有用的点在于：在处理运维时间序列上，其实存在很多难点，需要我们使用一些预处理方法，如去噪、平滑、去离群值等方法来处理，以保证模型性能，且具备可解释性。

> However, the large amount of events in the time series and the fact that proper training of neural networks **requires normalization**, leads to obligation of having an outlier removal technique. **The presence of a strong outlier, will lead to values clamped to zero after the normalization**. Therefore, events having response time greater than three standard deviations from the mean are removed from the training batch.
> 
> Next, we normalize the values by using min-max scaling (0, 1) to ensure that we stay in the positive range of values for the response time. In contrast, **using standardization might produce negative values that do not have natural meaning when we deal with response time (no negative time)**. Normalization is required and makes the optimization function of the neural network well-conditioned, which is key for convergence.
> 
> we apply **smoothing for noise removal and robustness to small deviations**. The time series is convolved with Hamming smoothing filter defined with its optimal parameters [35] and size M as...
> 



6. **MicroRAS: Automatic Recovery in the Absence of Historical Failure Data for Microservice Systems**

Li Wu; Johan Tordsson; Alexander Acker; Odej Kao

2020 IEEE/ACM 13th International Conference on Utility and Cloud Computing (UCC)

- 本文致力于提出一个故障自动恢复方法，指的是当系统已经异常后，我们如何评估某些操作所带来的正面/负面影响，并在恢复效果与恢复时间之间做权衡，选择有较高收益的动作。

- 本文将trace数据建模为一个属性图，属性图用于构建系统状态模型，分析动作的传播影响等。
	
	
7. **MicroRCA: Root Cause Localization of Performance Issues in Microservices**

Li Wu; Johan Tordsson; Erik Elmroth; Odej Kao

NOMS 2020 - 2020 IEEE/IFIP Network Operations and Management Symposium

- 我们提出了一种新的与应用无关的系统MicroRCA，用于定位基于容器的微服务中性能异常的根本原因。该方法构造了一个属性图模型，将服务异常性能症状与相应的资源利用率相关联，从而推断出异常微服务。
	



8. <a name="lundetecting2021"><sup>[1]</sup></a> **Detecting anomalies in microservices with execution trace comparison**

Lun Meng, Feng Ji, Yao Sun, TaoWang

Future Generation Computer Systems, Volume 116, March 2021, Pages 291-301

本文提出了一种基于调用链数据的异常检测方法，拟检测的异常分为两种：1) 调用关系异常：调用关系本身并不是一成不变的，调用关系会因为调用时的参数而发生动态变化，但是某些异常会导致微服务调用不常见的异常的调用总是与已有的调用关系不同，因此可以被检测出来；2) 调用响应异常：这种异常不会破坏调用关系，但是会直接影响服务迟延，因此也是异常。

为解决调用关系异常，本文**首先**收集软件测试期间的调用链数据用于合成应用运行时的tarce tree，注意由于trace tree会因为调用携带的参数不同而不同，因此在软件测试阶段收集（几乎）所有情况的调用关系数据是有必要的。**然后**将实时的调用链数据与刚才的baseline之间计算最小编辑距离（baseline中有多种调用baseline，因此需要与每一个baseline计算他们之间的距离，取距离最小的baseline作为正常模板，并计算anomaly score），以作为anomaly score。
	
>  To construct a baseline execution trace set S, for every arrival execution trace T_i, if we
>  cannot match T_i with an execution trace C_i in S, we add T_i in S.

调用时间在物理资源充分的情况下，一般是不会有大的波动的，因此，调用时间的激增就可以被视为一个调用时间异常。为了识别_激增_，本文使用coefficient of variation（CV）来表示一次请求的响应时间异常程度。此时的trace数据被用一个$M \times N$的metrix表示，其中第m行第n列表示，在第i次用户请求时，第j个组件（微服务）的响应情况。然后借助PCA对矩阵进行分解，用来识别导致异常的微服务。这里没太看懂。

![image](https://user-images.githubusercontent.com/16149619/115550685-b9e17f00-a2dc-11eb-9094-980b220a86f3.png)
![image](https://user-images.githubusercontent.com/16149619/115550693-bea63300-a2dc-11eb-8f08-d44732ca1b41.png)


​	
9. **Midiag: A Sequential Trace-Based Fault Diagnosis Framework for Microservices**
	
	Lun Meng, Yao Sun, Shudong Zhang
	
	International Conference on Services Computing, SCC 2020: Services Computing – SCC 2020 pp 137-144
	
- 本文还是将异常检测问题转化为下一system call预测问题，这里的system call也是用模板来代替，与之前的不同的是，这里的模板是通过K-means+最长公共子序列搜索来得到的，这与日志相关的工作十分相似。



------------

#### 基于日志数据

- 基于日志解析的大规模微服务架构软件系统异常检测

	Anomaly Detection of Large Scale Microservice Architecture Software System Based on Log Parsing

	邰丽媛, 田春岐 ：同济大学计算机科学与技术系，上海;
	王 伟 ：华东师范大学数据科学学院，上海;
	
- Root-Cause Metric Location for Microservice Systems via Log Anomaly Detection

	Lingzhi Wang; Nengwen Zhao; Junjie Chen; Pinnong Li; Wenchi Zhang; Kaixin Sui
	
	2020 IEEE International Conference on Web Services (ICWS)	
	
- Anomaly Detection of Large Scale Microservice Architecture Software System Based on Log Parsing

	Liyuan Tai, Chun-qi Tian, W. Wang


------------

#### 基于时间序列数据

- Localizing Failure Root Causes in a Microservice through Causality Inference 
	
	Yuan Meng; Shenglin Zhang; Yongqian Sun; Ruru Zhang; Zhilong Hu; Yiyin Zhang, ...
	
	2020 IEEE/ACM 28th International Symposium on Quality of Service (IWQoS)
	
	**基于微服务KPI数据的关联推断方法**
	
	我们设计了一种新的PCTS(路径条件时间序列)算法，在充分利用传播延迟的情况下学习监控指标的依赖图。在PCTS中，我们首先采用改进的PC[10]学习时间序列中每个点的因果图。然后生成两个时间序列之间的边，生成失效因果图。
	
	我们提出了一种新的基于时间原因的随机漫步(TCORW)方法。在TCORW中，我们成功地整合了三种类型的信息:(1)监测指标的因果关系;(2)度量的异常信息，包括发生时间和异常程度;(3)基于领域知识获得的度量优先级
	
	结合PCTS和TCORW，我们提出了一个新的框架——微原因，来推断微服务失败的前N个根本原因。据我们所知，这是在微服务中定位故障根源的第一个工作。


- Performance Diagnosis in Cloud Microservices using Deep Learning
	
	Li Wu, Jasmin Bogatinovski, Sasho Nedelkoski, Johan Tordsson and Odej Kao
	
	2020 AIOPS workshop
	
	**多源时间序列的异常检测与根因定位**——我们从多个数据源收集数据，包括应用程序、操作系统和网络，以提供由不同根源(如软件bug、硬件问题、资源争用等)引起的性能问题的罪魁祸首。我们的系统被设计成**与应用程序无关的**，不需要应用程序使用仪器来获取数据。相反，我们收集应用程序和运行时系统本身报告的指标。
	
	本文提出了一种应用不可知系统，以细粒度定位微服务性能下降的罪魁祸首，不仅包括产生性能问题的异常服务，还包括与服务异常相关的罪魁祸首指标。我们的方法首先通过构建服务依赖图来发现潜在的罪魁祸首服务，然后应用自动编码器根据重构错误的排序列表来识别异常服务指标。
	
	我们采用两阶段方法进行异常检测和根本原因分析(系统概述在第3节中描述)。在第一阶段，我们根据基于图的方法[16]对导致故障的服务进行建模。这使我们能够通过识别导致错误服务性能下降的根本原因(异常度量)来查明引发性能下降的潜在错误服务。第二阶段，对潜在故障的推断，是基于以下假设:故障行为的最重要症状与正常运行时的值存在显著偏差。在任何时间点测量每个症状的个体贡献，从而导致观察到的行为与正常行为之间的差异，从而可以定位最可能反映故障的症状。有了这个假设，我们的目标是在正常系统行为下模拟症状值
	


- TELESTO: A Graph Neural Network Model for Anomaly Classification in Cloud Services
	
	Dominik Scheinert and Alexander Acker
	
	2020 AIOPS workshop
	
	**多维时间序列的异常分类任务**，即不仅只识别正异常，还是被异常的种类

	我们提出了一种通过训练分类模型来识别再次出现的异常的方法。利用系统度量数据，如CPU利用率、已分配内存或磁盘I/O统计数据，并将这些数据建模为多元时间序列，我们的模型能够识别异常类型特定的模式，并为它们分配各自的异常标签。

	我们提出的模型架构TELESTO利用一种新颖的图神经网络架构，在空间和时间维度上利用多变量时间序列建模为图。它不受维度变化的影响，优于其他两种常用的图神经网络方法。

--------------

## Dataset

1. ToN IoT-The role of heterogeneity and the need for standardization of features and attack types in IoT network intrusion datasets


2. https://zenodo.org/record/3549604#.YEGfWI5LiUk
	
	一个公开数据集，里面包括AIOPS常见的三种数据
	
	
	
3. test bed搭建：

	framework：OpenStack, Kolla-Ansible(dockerized environment)/k8s
	For the **metrics** collection across the physical nodes in the infrastructure, we utilize [Glances](20),
	
	OpenStack introduces a small but powerful library called [*osprofiler*](21) that is used by all OpenStack projects and their Python clients to generate **traces**.
	
	The **log** files are distributed over the infrastructure and they are grouped in directories by the OpenStack projects (e.g., nova, neutron, glance, etc.) at the wally nodes.
	
	anomaly injection: (ref) Multi-source Distributed System Data for AI-Powered Analytics
	
	To generate workloads and inject faults into the infrastructure we used [Rally](25)	

### Trace data

1. Azure Public dataset: composes of two datasets representing two representative traces of the virtual machine of Microsoft Azure
	
	[link](5)
	
2. Alibaba’s cluster data is a collection of two datasets from real-world production

	[link](2, 14, 28)
	
3. Google’s collection of two tracing datasets originates from parts of Google cluster management software and systems

	[link](10)
	
### metric data

1. A plethora of available collections of datasets containing metric data can be found in Stonybrook

	[link](31)

2. [Numenta](1) predominantly contains datasets
from streaming and real-time applications, while [Harvard](9), [ELKI](8), [LMU](15) store network intrusion data.


### log data

1. The [CFDR resource](3) stores links or 19 log datasets grouped in 11 data collections.

2. The second resource is the [loghub data resource](35).
	

------------------

## 没啥用的文章

- **MicroMon: A Monitoring Framework for Tackling Distributed Heterogeneity**

	Babar Khalid, Nolan Rudolph, Ramakrishnan Durairajan, Sudarsun Kannan
	
	12th {USENIX} Workshop on Hot Topics in Storage and File Systems (HotStorage 20).
	
	没啥用，描述了一种在微服务上运行的监视系统，主要应对的是微服务系统的软件/硬件异质性问题，提高监视系统的吞吐量
	
- **Advancing Monitoring in Microservices Systems**

	Marcello Cinque; Raffaele Della Corte; Antonio Pecchia
	
	2019 IEEE International Symposium on Software Reliability Engineering Workshops (ISSREW)
	
	没啥用，描述了一种在微服务上运行的监视系统
	
- **学位论文：ANOMALY DETECTION IN CLOUD-NATIVE SYSTEMS**

	Surace, Pino' (2019)
	
	没啥用，讲了一些云原生应用中的组件综述，以及如何用简单的ML方法利用微服务中的时间序列做异常检测
	
- Artificial Intelligence for IT Operations (AIOPS) Workshop White Paper

	Jasmin Bogatinovski, Sasho Nedelkoski, Alexander Acker, Florian Schmidt, Thorsten Wittkopp, Soeren Becker, Jorge Cardoso, Odej Kao

	white paper for the AIOPS 2020 workshop at ICSOC 2020
