---
title: 4GWAN中的告警（alarm）分析
date: 2022-01-04 14:00:00
tag:
- note
- 5G
- 4G
---

# 告警与性能指标之间的关系

本文主要以case为单位，分析每个告警可能对性能指标造成的影响，回顾专家在判定告警对流量损失时的思路: 

1. 以expert label中标注的`Time of Interest (TOI)`确定case中的target alarm: target alarm为TOI中发生过的告警、仍未清除的告警
2. 以alarm为中心，分析与告警相关的上下游知识（如物理层、链路层等）是否发生异常（评价所用的参考指标: 环比KPI - 一天环比或一周环比）
3. 分析异常链路是否完整，包括: （1）告警所影响的KPI是否发生异常（2）目标KPI（一般为流量）是否发生异常

<!-- more -->

## case 2 (RRU级告警 接口异常 告警无损失案例)

`alarm: RF Unit CPRI Interface Error` (Minor) - 射频单元链路上承载的业务质量可能会略有下降（详细说明见下图）

![](https://raw.githubusercontent.com/KMdsy/figurebed/master/img/20220104134155.png)

**专家思路**: 

1. 告警时间内，告警小区的物理层中，上下行RSRP指标与环比参考指标相比没有明显变化，说明告警对小区无影响。
2. 进一步的比较告警小区与环比流量是否有损失。

强相关的KPI指标（告警小区）: 

- L.MeasRpts.RSRP.Index0~2 (RSRP<-110dbm)  Ratio (%)（MR测量上报RSRP<-110dbm比例(%)）
- UL PUCCH RSRP low than-120dBm ratio（上行PUCCH RSRP低于-120dBm的比例）
- UL PUSCH RSRP low than-120dBm ratio（上行PUSCH RSRP低于-120dBm的比例）


## case 3 (全站级告警 时钟类异常 告警无损失案例)

`alarm: External Clock Reference Problem`（Major）- Minor 影响切换，可能导致断站；Major 导致跨站干扰，影响速率、接入成功率、掉话率等指标，严重时会引发断站（详细说明见下图）

![](https://raw.githubusercontent.com/KMdsy/figurebed/master/img/20220104134040.png)

**专家思路**: 告警小区的物理层中的干扰指标、用户层的速率指标均无明显变化，说明告警无影响。

强相关的KPI指标（告警小区）: 

- L.UL.Interference.Avg(dBm)（上行平均干扰）
- （上行用户体验速率(Mbps)）
- LTE_User UL Average Throughput(Mbps)下行用户体验速率(Mbps)（下行用户体验速率(Mbps)）


## case 4 (小区级告警 断站异常 告警有损失案例)

`alarm: Cell Unavailable`

强相关的KPI指标（告警小区）: （未列出）
 
这个案例直接比较了告警小区的流量环比是否损失（实际情况是损失的），和邻区小区的流量累加环比是否增加（实际情况专家认为是无增加的）

## case 5/6 (RRU级告警 断站异常 告警无损失案例)

`alarm: RF Unit VSWR Threshold Crossed (Minor, Major)` - 射频单元的输出功率降低，小区覆盖缩小（详细说明见下图）
 
![](https://raw.githubusercontent.com/KMdsy/figurebed/master/img/20220104141701.png)
 
 **专家思路** : 
1. 物理层指标中，RSRP<-110dbm比例无明显变化，说明告警小区的信号强度在告警期间未发生明显变化 
2. 下行平均MCS无明显变化，说明告警小区的信道质量在告警期间未发生明显变化；下行64QAM的比例无明显变化，（QAM指相正交振幅调制，常用的QAM有16QAM、64QAM、256QAM，越大对信道质量的要求越高）
3. 同频和异频切换出成功次数无明显变化，说明告警小区的用户未因小区覆盖范围变小而切出告警小区


强相关的KPI指标（告警小区）: 
 
- L.MeasRpts.RSRP.Index0~2 (RSRP<-110dbm)  Ratio (%) （MR测量上报RSRP<-110dbm比例(%)，物理层指标，RSRP测量提供小区信号强度的测量）
- DL MCS AVG/DL64QAM RATIO （下行平均MCS/下行64QAM的比例，MAC层指标，MCS:调制编码方案，当信道质量好时采用高阶的调制方式和更高的编码效率，MCS越高，码率越大，传输效率越高）
- InterFreq HO Succ Times/IntraFreq HO Succ Times（同频切换出成功次数/异频切换出成功次数，用户层指标）

## case 13  (RRU级告警 断站异常 告警有损失案例)

TOI时间段内223196上发生过的告警，黑体为还未清除的告警: 
- **RF Unit Optical Module Fault**
- External Clock Reference Problem
- Remote Maintenance Link Failure
- **RF Unit Maintenance Link Failure**
- Cell Capability Degraded
- BBU CPRI Interface Error
- BBU CPRI Line Rate Negotiation Abnormal
- **Cell Unavailable**

告警解析如下表

|     告警名                                     |     告警类型    |     告警描述                                                              |     影响                                                                                                       |
|------------------------------------------------|-----------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
|     RF Unit Optical Module Fault               |     射频类      |     射频单元光模块故障                                                    |     光模块的收发性能下降 =>射频单元链路上承载的业务质量下降=>射频单元上承载的业务可能会中断                    |
|     External Clock Reference Problem           |     时钟类      |     时钟参考源异常                                                        |     可能导致TDD网元系统时钟不可用，可能出现小区接入失败、掉话等业务异常或无法提供业务                          |
|     Remote Maintenance   Link Failure          |     断站类      |     Operation and Maintenance CHannel (OMCH)中断超过五分钟导致无法维护    |     用户无法维护远端设备。基站只能在现场维护。                                                                 |
|     RF Unit Maintenance Link Failure           |     射频类      |     BBU与射频单元之间的维护链路故障，且故障持续一段时间                   |     射频单元上承载的业务中断。                                                                                 |
|     Cell Capability Degrade                    |     断站类      |     小区服务能力下降                                                      |     告警小区提供给客户可用的无线空口能力会下降，可能出现用户接入异常                                           |
|     BBU CPRI Interface Error                   |     射频类      |     BBU CPRI光模块接口异常告警                                            |     BBU与下级射频单元的光模块的连接链路中断，或收发性能轻微恶化，MAC层错帧率超过指定门限=>业务质量变差         |
|     BBU CPRI Line Rate Negotiation Abnormal    |     射频类      |     BBU与下级射频单元间线速率不一致                                       |     引发BBU与下级射频单元的间的带宽不足，导致小区建立失败，系统容量降低                                        |                               |

这个案例直接比较了告警小区的流量环比是否损失（实际情况是损失的），和邻区小区的流量累加环比是否增加（实际情况专家认为是无增加的）
