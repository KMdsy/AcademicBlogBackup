---
title: 关联分析与因果分析调研
date: 2021-12-25 12:00:00
tag: 
- survey
- causal discovery
- correlation analysis
- paper list
---


本文旨在调研在智能运维领域中的关联分析与因果分析方法，本文的组织如下：

1. 首先我们将给出一个源自真实业界需求的案例场景。
2. 围绕该上述场景，我们将介绍该场景中可用的关联分析、因果分析方法，并给出相关的方法分类
3. 对于经典方法（基于频繁项挖掘）和与我们的研究相关的方法（基于图嵌入），本文也将列出相关的综述与论文列表。
4. 在本文的研究范围之外，本文也列出了“事件序列-事件序列”关联分析、“事件序列-时间序列”关联分析的相关方法。

<!-- more -->

## 案例场景与问题转化

该场景源于移动通信网络中的网络优化任务。在现网网络优化的过程中，往往需要进行**网络参数**的调整，参数调整后将收集网络中的**性能指标数据**（KPI）用于验证参数调整的效果。实际上，网络KPI受多种配置参数的联合影响，且针对某个小区的参数调整也会影响该小区的邻区，进而造成邻区网络性能的变化。因此，我们希望了解：

1. 给定需要优化的网络性能指标，确定有哪些参数（组）将影响该指标；
2. 给定要调整的网络参数，分析该参数将影响哪些指标。

综上所述，本场景的本质是要建立“参数”到“网络性能指标”之间的关联关系（较低阶）或因果关系（较高阶）。其中KPI数据本质上是**时间序列**，而参数调整可以视为**时间序列**或**事件序列**。因此本任务对应的科学问题被简化为

> 给定一对时间序列（或一个时间序列与一个事件序列），如何建模二者之间的关联关系（或因果关系）

**在后面的章节中，我们主要调研“时间序列-时间序列”之间的关联分析方法。**


一些背景知识：

+ 移动网络的建设过程分为：规划、建设、维护、优化，四个步骤，我们的研究属于优化环节。
+ 网络配置参数分为：非协同类参数（参数调整后只影响本小区的网络性能）、协同类参数（参数调整后会影响邻区网络性能）
+ 调整协同类参数的具体场景包括：切换类与负载均衡类的网络性能优化，RF参数优化


## 分类

<div align="center">
    <img src="https://raw.githubusercontent.com/KMdsy/figurebed/master/img/20220216193335.png" width = "90%" />
</div>

**注：**

1. 上述分类是针对本文的研究目标进行的分类，事实上“基于相似度”、“基于相关性”、“基于图模型 - 基于回归模型”的方法属于属于关联分析，“基于图模型”分类下的“基于条件约束”、“基于得分”、“基于函数式模型”的方法属于因果分析。
2. 因果分析的分类依据、以及介绍可参照[[12]](#refer12)[[13]](#refer13)[[14]](#refer13)

    
### Reference 
> <div></div>
> [1] Su, Y., Zhao, Y., Xia, W., Liu, R., Bu, J., Zhu, J., ... & Pei, D. (2019, June). CoFlux: robustly correlating KPIs by fluctuations for service troubleshooting. In Proceedings of the International Symposium on Quality of Service (pp. 1-10).
> 
> <div></div>
> [2] Niennattrakul, V., & Ratanamahatana, C. A. (2007, April). On clustering multimedia time series data using k-means and dynamic time warping. In 2007 International Conference on Multimedia and Ubiquitous Engineering (MUE'07) (pp. 733-738). IEEE.
>
> <div></div>
> [3] 贾海涛. (2018). 基于数据挖掘的动环监控系统告警相关性研究 (Doctoral dissertation, 北京: 北京交通大学).
> 
> <div></div>
> [4] Luo, C., Lou, J. G., Lin, Q., Fu, Q., Ding, R., Zhang, D., & Wang, Z. (2014, August). Correlating events with time series for incident diagnosis. In Proceedings of the 20th ACM SIGKDD international conference on Knowledge discovery and data mining (pp. 1583-1592).
>
> <div></div>
> [5] Jiang, G., Chen, H., & Yoshihira, K. (2006, June). Discovering likely invariants of distributed transaction systems for autonomic system management. In 2006 IEEE International Conference on Autonomic Computing (pp. 199-208). IEEE.
> 
> <div></div>
> [6] Gerhardus, A., & Runge, J. (2020). High-recall causal discovery for autocorrelated time series with latent confounders. Advances in Neural Information Processing Systems, 33, 12615-12625.
>
> <div></div>
> [7] Nauta, M., Bucur, D., & Seifert, C. (2019). Causal discovery with attention-based convolutional neural networks. Machine Learning and Knowledge Extraction, 1(1), 312-340.
>
> <div></div>
> [8] Chu, Y., Wang, X., Ma, J., Jia, K., Zhou, J., & Yang, H. (2020, November). Inductive Granger Causal Modeling for Multivariate Time Series. In 2020 IEEE International Conference on Data Mining (ICDM) (pp. 972-977). IEEE.
>
> <div></div>
> [9] Brouillard, P., Lachapelle, S., Lacoste, A., Lacoste-Julien, S., & Drouin, A. (2020). Differentiable causal discovery from interventional data. Advances in Neural Information Processing Systems, 33, 21865-21877.
>
> <div></div>
> [10] Ogarrio, J. M., Spirtes, P., & Ramsey, J. (2016, August). A hybrid causal search algorithm for latent variable models. In Conference on probabilistic graphical models (pp. 368-379). PMLR.
>
> <div></div>
> [11] Ding, C., Gong, M., Zhang, K., & Tao, D. (2019). Likelihood-free overcomplete ICA and applications in causal discovery. Advances in Neural Information Processing Systems, 32.
>
> <div id="refer12">
> [12] Glymour, C., Zhang, K., & Spirtes, P. (2019). Review of causal discovery methods based on graphical models. Frontiers in genetics, 10, 524.
> </div> 
> 
> <div id="refer13">
> [13] Yao, L., Chu, Z., Li, S., Li, Y., Gao, J., & Zhang, A. (2021). A survey on causal inference. ACM Transactions on Knowledge Discovery from Data (TKDD), 15(5), 1-46.
> </div> 
> 
> <div id="refer14">
> [14] Malinsky, D., & Danks, D. (2018). Causal discovery algorithms: A practical guide. Philosophy Compass, 13(1), e12470.
> </div> 

## 基于频繁项挖掘的工作调研

### Apriori

**Apriori**最早由 Agrawal 提出，通过多次迭代建立候选集查找频繁项。在[[15]](#refer15)中，作者对大规模操作系统中事件序列间的相关性进行了研究，为了方便对事件序列的相关性进行研究，作者首先将冗长的事件转化为不同的事件类型，并根据事件类型序列数据定义了“episode”。记在时间窗口 TW 内发生的所有事件类型即为一个 episode，并记为$E_{eA}, T_{eA}$，其中$E_{eA}$为与事件𝑒A相邻的所有事件的集合，$T_{eA}$为对应的时间窗口。然后，文中用频繁项查找算法 Apriori 对 episode 序列中频繁项集进行搜索，而这种频繁项集的形式就被认定为事件之间的相关性。文中利用不同 h-置信度与 Apriori 中修剪压缩比的变化曲线关系，自动求出最适宜的最小支持度，从而对频繁项集进行修剪。

HJ Lu[[16]](#refer16)认为经典的关联规则挖掘忽略了事物发生的语境，如时间、地点等。作者认为项目关联有两种：
1）事物内的频繁项关联（如，同一天内，两个股票一起涨）；
2）不同事物间的频繁项关联（如 A 股票第一天涨了后，B 股票在第四天有较大概率也涨）。
而传统的频繁项关联算法只局限于查找关联1。因此文中对Apriori算法进行了扩展，以提出一种Extension-Apriori算法，将关联规则挖掘的范围从传统的单维度关联扩展到多维的事物间关联，并研究了多维事件关联规则中支持度与置信度的计算方式。同时算法利用哈希技术过滤掉不必要的候选二项集，将所有可能的二项集散列到一个哈希表中，减少了数据库扫描的复杂度。

但由于 Apriori 算法每一次增加频繁项集大小时都需要重新扫描整个数据集，所以当数据集很大时，算法效率较低，因此有许多研究是针对如何将Apriori 算法进行优化提速。

如[[17]](#refer17)研究了频繁项挖掘算法在 MapReduce 框架中的实现。文献[[17]](#refer17)将串行的Apriori算法转化为分布式的MapReduce版本，在每一次查找频繁项集时，使用map生成候选支持，并用reduce收集全局的候选支持。并且算法可以根据候选对象的数量与前一个MapReduce阶段的执行时间，动态的收集可变长度的候选对象，极大地缩短了Apriori生成频繁项集的时间。 

### FP-growth

频繁模式生长算法（**FP-growth**）是最早由韩家炜等人提出的利用频繁模式树进行频繁项挖掘的算法。相比Apriori，FP-growth只用遍历两遍数据，且不需要产生候选序列，极大提高了挖掘效率。因此也有许多研究人员通过 FP-growth 来挖掘序列中的频繁项。

文献[[18]](#refer18)中，作者提出了一种并行化FP-Growth算法的MapReduce方法，将大规模的频繁项挖掘任务自动分割成独立的计算任务，并将其映射到MapReduce框架中。作者用提出的方法对包含网页与标签的事件序列数据库进行网页间相关度的计算，从而实现有效的相关网页推荐。

而传统的Apriori和FP-growth 算法都是基于最小支持度的频繁项搜索算法，因此存在以下两个问题：
1）当最小支持度设置较大时，包含稀有项的频繁项会被过滤掉；
2）当最小支持度设置较小时，则会生成过多的频繁项导致计算量爆炸。
对于 FP-Growth 算法，由于每个项目都有最低支持度，因此用户很难一次为所有的项目设置适当的阈值，所以用户通常需要多次优化算法的参数，直到达到满意的结果。

基于上述问题，文献[[19]](#refer19)提出一种MIS-Tree结构和名为CFP-Growth的算法，与FP-growth不同，CFP-Growth中所有的输入项目以最小支持度进行降序排列，然后将所有的项目输入到树结构中，构建一个类似FP-tree的树状结构称为MIS-Tree，同时测量树中各项目的支持度。然后对树中所有支持度小于全部项目中最小支持度的项进行修剪，并对含有相同父节点的项进行合并，以保证树结构的紧凑性，得到的即为最小支持度项目树(MIS-Tree)。
最后只需要将MIS-Tree中每个项目作为后缀项并进行扫描就可以发现完整的频繁模式集。文中作者分别在合成数据集和真实数据集上进行测试，结果表明该算法更加有效与快速。


### Reference

> <div id="refer15">
> [15] Gupta, C. (2012, September). Event correlation for operations management of largescale it systems. In Proceedings of the 9th international conference on Autonomic computing (pp. 91-96).
> </div>
>
> <div id="refer16">
> [16] Lu, H., Feng, L., & Han, J. (2000). Beyond intratransaction association analysis: mining multidimensional intertransaction association rules. ACM Transactions on Information Systems (TOIS), 18(4), 423-454.
> </div>
>
> <div id="refer17">
> [17] Lin, M. Y., Lee, P. Y., & Hsueh, S. C. (2012, February). Apriori-based frequent itemset mining algorithms on MapReduce. In Proceedings of the 6th international conference on ubiquitous information management and communication (pp. 1-8).
> </div>
>
> <div id="refer18">
> [18] Li, H., Wang, Y., Zhang, D., Zhang, M., & Chang, E. Y. (2008, October). Pfp: parallel fp-growth for query recommendation. In Proceedings of the 2008 ACM conference on Recommender systems (pp. 107-114).
> </div>
>
> <div id="refer19">
> [19] Hu, Y. H., & Chen, Y. L. (2006). Mining association rules with multiple minimum supports: a new mining algorithm and a support tuning mechanism. Decision support systems, 42(1), 1-24.
> </div>


## 基于图的方法

[[20]](#refer20)描述了在一个分布式系统中构建的异常检测系统及其原因分析平台，在图结构部分，本文构建图的方式是值得借鉴的。
首先，本文的系统图结构是已知的**系统架构图**，针对探测到的异常，本文提出方法用于在图上分析异常所造成的影响，并定位根因，图的构造方式：
    
1. 发生异常时的图anomaly graph使用$G=(V, E)$来表示，其中$C \cup A$。
2. $C$是系统的组件构成的集合，包含系统中的逻辑组件、物理组件等，$A$是底层异常检测结果报告的异常（包括由规则定义的和由实时监控系统得到的），$E$是anomaly graph中的边集合，
3. 图中存在两种边：1）连接组件的边，代表组件之间的从属关系；2）连接组件与异常的边，代表某个组件产生了某个异常
4. 本文还为alarm edge设置了分数，代表了某个组件发生某个异常的严重程度，这个分数由：1）某个时间段内该异常每次发生的严重程度；2）该异常的发生频率，共同决定。

*本文在异常发生的时候，建立一个异常时的状态图，然后在图上针对异常及其连接的边计算异常严重程度分数，分数高的边所连接的组件可能就是异常。*


[[21]](#refer21)从云服务设施提供商的角度，建立了一种异常定位的方法，其中使用了有向无环图$G=(V,E)$来建模异常的传播，其中：
    
1. 节点表示虚拟机VM
2. 边表示节点之间的两种关系
    1. 由service call引起的业务关联
    2. 由于在同一个物理主机上而可能产生资源竞争的

该文章假设对于某个异常$a$，它发生的时候（一般指这个异常是某个服务的等待时间过长），某个VM上的一组相关指标一定也是繁忙的（如CPU等，意思是这个指标的繁忙导致了异常的发生）。
    
因此，为了找到这样的一组指标作为异常的原因，作者提出在$G$上进行随机游走，游走的过程中来计算每个vm的指标与异常服务的响应时间之间的关联关系（利用similarity，其中similarity的计算为：提高某个VM的物理指标占用，如cpu，然后测量service的响应时间，计算二者在这段时间内的相关系数），游走规则如下：
    
1. walker总是更倾向于往具有更高similarity的节点去游走
2. walker游走到低similarity的节点的时候，可以选择返回
3. walker的领域均为低similarity的节点时，可以选择待着不动
    
综上所述，在游走的过程中，被访问最多的节点将被视为是高可能性的异常根因。*本文在异常发生的时候，建立一个包含了物理关系与服务调用关系的网络拓扑图，然后在图上以随机游走的方式计算节点的异常程度分数，游走完毕后，即可识别出异常的根因。*


[[22]](#refer22)利用**多维时序指标**来动态生成服务关联，诊断根因。针对多维时间序列，该工作分析时序之间的异常关联，推断**异常行为图**来描述不同服务之间的相关性。根据行为图，该工作使用前向、自向和后向随机游走算法设计启发式模型，用以识别服务故障的根本原因。解析可以看：[link](https://blog.csdn.net/weixin_53741275/article/details/111973738)

[[23]](#refer23)这项工作提出了一个框架，MicroCause，可以准确地定位导致微服务故障的根本原因的**监控指标（时间序列）**。MicroCause结合了简单而有效的路径条件时间序列（PCTS）算法以准确地捕获时间序列之间的**因果关系**，以及面向时间因果的新型随机游走方法（TCORW）


[[24]](#refer24)中提到，**不变网络**已被证明是表征复杂系统行为的有效方法。在不变网络中，节点表示系统组件，边缘表示两个组件之间的稳定，重要的交互作用。不变性网络的结构和演化，尤其是**消失的相关性**，可以为定位因果异常和执行诊断提供重要的启示。然而，现有的利用不变网络检测因果异常的方法通常使用消失的相关性百分比来对可能的偶然分量进行排名，这有几个局限性：1）网络中的故障传播被忽略；2）根源偶然异常可能并不总是那些消失率很高的节点；3）消失的相关性的时间模式未用于鲁棒检测。为了解决这些局限性，在本文中，我们提出了一个**基于网络扩散的框架**，以识别重大的因果异常并对它们进行排名。我们的方法可以有效地对整个不变网络上的故障传播建模，并且可以对结构和随时间变化的破碎不变模式进行联合推断。
    
### Reference

> <div id="refer20">
> [20] Wang, H., Nguyen, P., Li, J., Kopru, S., Zhang, G., Katariya, S., & Ben-Romdhane, S. (2019). GRANO: Interactive graph-based root cause analysis for cloud-native distributed data platform. Proceedings of the VLDB Endowment, 12(12), 1942-1945.
> </div>
>
> <div id="refer21">
> [21] Weng, J., Wang, J. H., Yang, J., & Yang, Y. (2018). Root cause analysis of anomalies of multitier services in public clouds. IEEE/ACM Transactions on Networking, 26(4), 1646-1659.
> </div>
>
> <div id="refer22">
> [22] Ma, M., Xu, J., Wang, Y., Chen, P., Zhang, Z., & Wang, P. (2020, April). Automap: Diagnose your microservice-based web applications automatically. In Proceedings of The Web Conference 2020(pp. 246-258).
> </div>
>
> <div id="refer23">
> [23] Meng, Y., Zhang, S., Sun, Y., Zhang, R., Hu, Z., Zhang, Y., ... & Pei, D. (2020, June). Localizing failure root causes in a microservice through causality inference. In 2020 IEEE/ACM 28th International Symposium on Quality of Service (IWQoS) (pp. 1-10). IEEE.
> </div>
>
> <div id="refer24">
> [24] Cheng, W., Zhang, K., Chen, H., Jiang, G., Chen, Z., & Wang, W. (2016, August). Ranking causal anomalies via temporal and dynamical analysis on vanishing correlations. In Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining (pp. 805-814).
> </div>


## "事件序列-时间序列"关联分析论文列表

- Luo, C., Lou, J. G., Lin, Q., Fu, Q., Ding, R., Zhang, D., & Wang, Z. (2014, August). Correlating events with time series for incident diagnosis. In Proceedings of the 20th ACM SIGKDD international conference on Knowledge discovery and data mining (pp. 1583-1592).

- Chi, L., Sathe, S., Han, B., & Wang, Y. (2016, December). A Novel Method for Assessing Event Impacts on Event-Driven Time Series. In 2016 IEEE 16th International Conference on Data Mining Workshops (ICDMW) (pp. 519-526). IEEE.

    **梗概**：本文提出使用自适应窗口的$t_score$来动态的度量事件对时间序列的影响，并提出了$A_score$，用于定量评估事件对时间序列的影响程度。

- Wu, C., Zhao, N., Wang, L., Yang, X., Li, S., Zhang, M., ... & Pei, D. Identifying Root-Cause Metrics for Incident Diagnosis in Online Service Systems. In The 32nd International Symposium on Software Reliability Engineering (ISSRE 2021). IEEE.

    **梗概**：本文贡献在于三个方面，首先根据假设检验，识别发生异常时有哪些指标被影响。然后对异常进行分类，去除那些运维人员不会考虑的异常模式。最后根据上述两个输出，输出一个经过排序的可疑指标列表，以辅助工程师识别异常根因。

- Xun, P., Zhu, P. D., Li, C. L., & Zhu, H. Y. (2016, December). Discovering multi-type correlated events with time series for exception detection of complex systems. In 2016 IEEE 16th International Conference on Data Mining Workshops (ICDMW) (pp. 21-28). IEEE.

- Krosman, K., & Sosnowski, J. (2021). Correlating Time Series Signals and Event Logs in Embedded Systems. Sensors, 21(21), 7128.

- Minaei-Bidgoli, B., & Lajevardi, S. B. (2008, September). Correlation mining between time series stream and event stream. In 2008 Fourth International Conference on Networked Computing and Advanced Information Management (Vol. 2, pp. 333-338). IEEE.

### 不具备很强参考意义但很有趣的论文

- Chi, L., Han, B., & Wang, Y. (2016). Open Problem: Accurately Measuring Event Impacts on Time Series. In KDD MiLeTS Workshop.
    
    KDD workshop中提出的open problem，有指导性意见
    
- Van Dortmont, M. A. M. M., van den Elzen, S., & van Wijk, J. J. (2019, June). ChronoCorrelator: Enriching events with time series. In Computer Graphics Forum (Vol. 38, No. 3, pp. 387-399).

    一种可视化的方法，用于将事件关联到时间序列的变化上，也是采用了类似two sample test的方法
    
- Xiao, S., Yan, J., Farajtabar, M., Song, L., Yang, X., & Zha, H. (2019). Learning time series associated event sequences with recurrent point process networks. IEEE transactions on neural networks and learning systems, 30(10), 3124-3136.

    这篇揭示了多种事件类型之间的相关关系（基于attention），但没有说明事件对时间序列的影响程度，本文的主要目标是事件预测



## "事件序列-事件序列"关联分析论文列表（待筛选）

- Noel, S., Robertson, E., & Jajodia, S. (2004, December). Correlating intrusion events and building attack scenarios through attack graph distances. In 20th Annual Computer Security Applications Conference (pp. 350-359). IEEE.

- Bayomie, D., Awad, A., & Ezat, E. (2016, June). Correlating unlabeled events from cyclic business processes execution. In International Conference on Advanced Information Systems Engineering (pp. 274-289). Springer, Cham.

- Dindar, N., Fischer, P. M., Soner, M., & Tatbul, N. (2011, July). Efficiently correlating complex events over live and archived data streams. In Proceedings of the 5th ACM international conference on Distributed event-based system (pp. 243-254).

- Steiger, E., Resch, B., de Albuquerque, J. P., & Zipf, A. (2016). Mining and correlating traffic events from human sensor observations with official transport data using self-organizing-maps. Transportation Research Part C: Emerging Technologies, 73, 91-104.

- Vlachos, M., Wu, K. L., Chen, S. K., & Philip, S. Y. (2008). Correlating burst events on streaming stock market data. Data Mining and Knowledge Discovery, 16(1), 109-133.

- Koch, G. G., Koldehofe, B., & Rothermel, K. (2010, July). Cordies: Expressive event correlation in distributed systems. In Proceedings of the Fourth ACM International Conference on Distributed Event-Based Systems (pp. 26-37).

- Cheng, L., Van Dongen, B. F., & Van Der Aalst, W. M. (2017, May). Efficient event correlation over distributed systems. In 2017 17th IEEE/ACM International Symposium on Cluster, Cloud and Grid Computing (CCGRID) (pp. 1-10). IEEE.

- Gruschke, B. (1998, October). Integrated event management: Event correlation using dependency graphs. In Proceedings of the 9th IFIP/IEEE International Workshop on Distributed Systems: Operations & Management (DSOM 98) (pp. 130-141).

- Jiang, G., & Cybenko, G. (2004, June). Temporal and spatial distributed event correlation for network security. In Proceedings of the 2004 American Control Conference (Vol. 2, pp. 996-1001). IEEE.

- Liu, G., Mok, A. K., & Yang, E. J. (1999, May). Composite events for network event correlation. In Integrated Network Management VI. Distributed Management for the Networked Millennium. Proceedings of the Sixth IFIP/IEEE International Symposium on Integrated Network Management.(Cat. No. 99EX302) (pp. 247-260). IEEE.

- Rozsnyai, S., Slominski, A., & Lakshmanan, G. T. (2011, July). Discovering event correlation rules for semi-structured business processes. In Proceedings of the 5th ACM international conference on Distributed event-based system (pp. 75-86).

- Wu, G., Zhang, H., Qiu, M., Ming, Z., Li, J., & Qin, X. (2013). A decentralized approach for mining event correlations in distributed system monitoring. Journal of parallel and Distributed Computing, 73(3), 330-340.

- Kotenko, I., Fedorchenko, A., Saenko, I., & Kushnerevich, A. (2018, March). Parallelization of security event correlation based on accounting of event type links. In 2018 26th Euromicro International Conference on Parallel, Distributed and Network-based Processing (PDP) (pp. 462-469). IEEE.

- Xuewei, F., Dongxia, W., Jiemei, Z., Guoqing, M., & Jin, L. (2010, June). Analyzing and correlating security events using state machine. In 2010 10th IEEE International Conference on Computer and Information Technology (pp. 2849-2854). IEEE.

- Hasan, M., Sugla, B., & Viswanathan, R. (1999, May). A conceptual framework for network management event correlation and filtering systems. In Integrated Network Management VI. Distributed Management for the Networked Millennium. Proceedings of the Sixth IFIP/IEEE International Symposium on Integrated Network Management.(Cat. No. 99EX302) (pp. 233-246). IEEE.

- Skopik, F., & Fiedler, R. (2013). Intrusion detection in distributed systems using fingerprinting and massive event correlation. INFORMATIK 2013–Informatik angepasst an Mensch, Organisation und Umwelt.

- Flammini, F., Mazzocca, N., Pappalardo, A., Pragliola, C., & Vittorini, V. (2011, August). Augmenting surveillance system capabilities by exploiting event correlation and distributed attack detection. In International Conference on Availability, Reliability, and Security (pp. 191-204). Springer, Berlin, Heidelberg.

- Martin-Flatin, J. P. (2004, June). Distributed event correlation and self-managed systems. In Proceedings of the 1st International Workshop on Self-* Properties in Complex Information Systems (Self-Star 2004) (pp. 61-64).

- Griffith, R., Hellerstein, J., Kaiser, G., & Diao, Y. (2006, June). Dynamic adaptation of temporal event correlation for qos management in distributed systems. In 200614th IEEE International Workshop on Quality of Service (pp. 290-294). IEEE.

- Steinert, R., Gestrelius, S., & Gillblad, D. (2011, December). A distributed spatio-temporal event correlation protocol for multi-layer virtual networks. In 2011 IEEE Global Telecommunications Conference-GLOBECOM 2011 (pp. 1-5). IEEE.

- Griffith, R., Hellerstein, J. L., Diao, Y., & Kaiser, G. E. (2005). Dynamic Adaptation of Rules for Temporal Event Correlation in Distributed Systems.

- Fu, X., Ren, R., Zhan, J., Zhou, W., Jia, Z., & Lu, G. (2012, October). Logmaster: Mining event correlations in logs of large-scale cluster systems. In 2012 IEEE 31st Symposium on Reliable Distributed Systems (pp. 71-80). IEEE.

- Myers, J., Grimaila, M., & Mills, R. (2010, April). Insider threat detection using distributed event correlation of web server logs. In International Conference on Cyber Warfare and Security (p. 251). Academic Conferences International Limited.

- Myers, J., Grimaila, M., & Mills, R. (2010, April). Insider threat detection using distributed event correlation of web server logs. In International Conference on Cyber Warfare and Security (p. 251). Academic Conferences International Limited.

- Myers, J., Grimaila, M. R., & Mills, R. F. (2011, January). Log-based distributed security event detection using simple event correlator. In 2011 44th Hawaii International Conference on System Sciences (pp. 1-7). IEEE.

- Parekh, J. J. (2005). Privacy-Preserving Distributed Event Correlation.

- Kato, N., Ohta, K., Ika, T., Mansfield, G., & Nemoto, Y. (1999). A proposal of event correlation for distributed network fault management and its evaluation. IEICE Transactions on Communications, 82(6), 859-867.

- Cerullo, G., Coppolino, L., D’Antonio, S., Formicola, V., Papale, G., & Ragucci, B. (2016). Enabling convergence of physical and logical security through intelligent event correlation. In Intelligent Distributed Computing IX (pp. 427-437). Springer, Cham.

- Alves, P. G. (2014). A Distributed Security Event Correlation Platform for SCADA (Doctoral dissertation, Universidade de Coimbra).

- Zhang, B., & Al-Shaer, E. (2007, October). Self-organizing monitoring agents for hierarchical event correlation. In International Workshop on Distributed Systems: Operations and Management (pp. 13-24). Springer, Berlin, Heidelberg.

- Katker, S. (1996, March). A modeling framework for integrated distributed systems fault management. In Proceedings of IFIP/IEEE International Conference on Distributed Platforms (pp. 186-198). IEEE.

- Teufl, P., Payer, U., & Fellner, R. (2010, February). Event correlation on the basis of activation patterns. In 2010 18th Euromicro Conference on Parallel, Distributed and Network-based Processing (pp. 631-640). IEEE.

- Yoneki, E. (2010). Time/space aware event correlation. In Principles and Applications of Distributed Event-Based Systems (pp. 43-74). IGI Global.

- Guo, N., Gao, T., Zhang, B., & Zhao, H. (2007, October). Distributed and scalable event correlation based on causality graph. In Asia-Pacific Network Operations and Management Symposium (pp. 567-570). Springer, Berlin, Heidelberg.

- Tai, W., OSullivan, D., & Keeney, J. (2008, April). Distributed fault correlation scheme using a semantic publish/subscribe system. In NOMS 2008-2008 IEEE Network Operations and Management Symposium (pp. 835-838). IEEE.

- Marwede, N., Rohr, M., van Hoorn, A., & Hasselbring, W. (2009, March). Automatic failure diagnosis support in distributed large-scale software systems based on timing behavior anomaly correlation. In 2009 13th European Conference on Software Maintenance and Reengineering (pp. 47-58). IEEE.

- Fu, S., & Xu, C. Z. (2007, October). Quantifying temporal and spatial correlation of failure events for proactive management. In 2007 26th IEEE International Symposium on Reliable Distributed Systems (SRDS 2007) (pp. 175-184). IEEE.
    


-------------

## 其他工作

这部分记录在调研过程中，实现关联分析的其他工作，尤其在运维领域

- Liu, P., Chen, Y., Nie, X., Zhu, J., Zhang, S., Sui, K., ... & Pei, D. (2019, October). FluxRank: A Widely-Deployable Framework to Automatically Localizing Root Cause Machines for Software Service Failure Mitigation. In 2019 IEEE 30th International Symposium on Software Reliability Engineering (ISSRE) (pp. 35-46). IEEE.

    用于已经确定了异常的发生后，快速的定位异常的根本原因，即定位到导致异常的服务器（而非导致异常的代码段或者更具体的原因）

    FluxRank的设计思路在于：在系统发生故障的时候，通常需要先确认故障，然后在尽可能短的时间内将业务转移到其他不受异常影响的服务器上以便尽快恢复任务。最后将使用很长的时间来分析导致异常的原因：如代码错误等。本文旨在定位异常到服务器级别，专注于异常的快速缓解，而非找到根本原因。

- Jayathilaka, H., Krintz, C., & Wolski, R. (2017, April). Performance monitoring and root cause analysis for cloud-hosted web applications. In Proceedings of the 26th International Conference on World Wide Web (pp. 469-478).

    这个工作与关联分析完全无关，主要是开发了一种在PaaS平台上的监控系统，用于快速异常定位于根因分析
    
- Arzani, B., Ciraci, S., Loo, B. T., Schuster, A., & Outhred, G. (2016, August). Taking the blame game out of data centers operations with netpoirot. In Proceedings of the 2016 ACM SIGCOMM Conference (pp. 440-453).

    这个工作涉及到了关联分析，但与序列数据完全无关
    
- Gao, J., Yaseen, N., MacDavid, R., Frujeri, F. V., Liu, V., Bianchini, R., ... & Arzani, B. (2020, July). Scouts: Improving the Diagnosis Process Through Domain-customized Incident Routing. In Proceedings of the Annual conference of the ACM Special Interest Group on Data Communication on the applications, technologies, architectures, and protocols for computer communication (pp. 253-269).

    完全无关，主要做异常定位的，也不是序列数据
    

    


