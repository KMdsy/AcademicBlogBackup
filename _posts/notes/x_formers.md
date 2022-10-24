---
title: Transformers - Survey
date: 2022-01-20 14:00:00
tag: 
- note
- transformers
---

A Survey of Transformers

TIANYANG LIN, YUXIN WANG, XIANGYANG LIU, and XIPENG QIU

School of Computer Science, Fudan University, China and Shanghai Key Laboratory of Intelligent Information Processing, Fudan
University, China

<!-- more -->

## Motivations

Vanilla Transformer的主要弊端与改进方向

+ **Model Efficiency**：Transforme处理长序列时效率低下，这主要是由于self-attention的计算和内存复杂性造成的。改进方法包括轻量级attention，例如sparse attention
variants、和分治方法（Divide-and-conquer），例如recurrent and hierarchical mechanism。
+ **Model Generalization**：Transformer的结构从理论上来说是非常灵活的，几乎不对输入数据的结构性偏差进行假设，因此很难对**小规模数据**进行训练。
改进方法包括引入结构性偏差（structural bias）或正则化（regularization,）、对大规模未标记数据进行预训练等。
+ **Model Adaptation**：这类工作旨在使Transformer适应特定的下游任务和应用。


这篇文章主要根据改进vanilla Transformer的方式来组织相关的工作，即：**架构修改**、**预训练**、**应用**。且本文主要关注架构变体，并简要讨论预训练和面向应用的变体。


## Background

### Vanilla Transformer

<div align="center">
    <img src="https://raw.githubusercontent.com/KMdsy/figurebed/master/img/20220120210749.png" width = "70%" />
</div>

首先，transformer遵循seq2seq结构，其中encoder decoder都由$L$个单独的模块堆叠而成。要点包括：

+ **encoder**: multi-head self-attention, position-wise feed-forward network (FFN), residual connection, Layer Normalization.
+ **decoder**: 上述模块 + cross-attention (between the multi-head self-attention modules and the position-wise FFNs), decoder中的attention matrix计算是有位置限制的
（考虑到后续时刻输出不能为前序时刻的输出提供参考）

#### Multi-head attention 

<div align="center">
  $\operatorname{Attention}(\mathrm{Q}, \mathrm{K}, \mathrm{V})=\operatorname{softmax}\left(\frac{\mathrm{QK}^{\top}}{\sqrt{D_{k}}}\right) \mathrm{V}=\mathrm{AV}$
</div>

上式是attention的基本原理，其中query $Q \in \mathbb{R}^{N \times D_{k}}$，key $\mathrm{K} \in \mathbb{R}^{M \times D_{k}}$，value $\mathbf{V} \in \mathbb{R}^{M \times D_{v}}$。$N,M$分别为query和key（value）的长度，$D_k, D_v$为key（query）与value的维度。$\mathrm{A}=\operatorname{softmax}\left(\frac{\mathrm{QK}^{\top}}{\sqrt{D_{k}}}\right)$也被称为attention matrix。除以$\sqrt{D_{k}}$是为了缓解梯度消失。

将数据维度压缩为1，则上述三个对象可以理解为，$\mathbf{v}=[v_1, \cdots, v_N]$代表module在未经过筛选时要输出的值，我们期望的输出是$\mathbf{w} * \mathbf{v}$，其中权重$\mathbf{w}$的计算即为上式中$softmax(\cdot)$的结果。

  **与软寻址之间的联系（如下图）**：令Source $\mathbf{S}=[<k_1, v_1>, \cdots, <k_n, v_n>]$视为存储器中的全部内容，当前有一个查询$q=k_i$，目的是取出source中匹配键值的值$v_i$。
  我们记$\mathbf{k} = [k_1, \cdots, k_n]$，我们通过Query $q$和存储器内元素的地址$\mathbf{k}$进行相似性比较来寻址，之所以说是软寻址，指的不像一般寻址只从存储内容里面找出一条内容$k_i$，
而是可能从$\mathbf{k}$中的每一项都会取出内容，取出内容的重要性根据$q$和$\mathbf{k}$的相似性来决定，相似性记为$\mathbf{w} = [w_1, \cdots, w_n]$，
  之后对存储器中的每一项对应的值进行加权求和，即$v = w_1 v_1 + \cdots + w_n v_n$，得到最终的Value值，也即Attention的结果值。
  所以不少研究人员将Attention机制看作软寻址的一种特例，这也是非常有道理的。
  
<div align="center">
    <img src="https://raw.githubusercontent.com/KMdsy/figurebed/master/img/20220120203102.png" width = "50%" />
</div>


<div align="center">
    \begin{aligned}
        \text { MultiHeadAttn }(Q, K, V) &=\text { Concat }\left(\text { head }_{1}, \cdots, \text { head }_{H}\right) \mathrm{W}^{O}, \\
        \text { where head }_{i} &=\operatorname{Attention}\left(Q W_{i}^{Q}, K W_{i}^{K}, V W_{i}^{V}\right) .
    \end{aligned}
</div>

上式是multi-head attention的基本表达式，其中$Q, K, V$的维度均为$D_m$，他们分别由几个线性映射（$W_{i}^{Q}, W_{i}^{K} W_{i}^{V}$）投影到维度为$D_k, D_k, D_v$的空间中，并进行attention计算，最后模型将所有输出连接并将其投影到$D_m$维空间。

+ **Self-attention**：$Q=K=V=X$，$X$是前一层的输出
+ **Masked Self-attention**：在Transformer解码器中，self-attention生成的weight受到位置限制，其生成的attention matrix只度量某个位置i和j之间的权重，且$i>=j$。具体地，其实现过程是为attention matrix的某些位置赋予mask。$\hat{A}=\exp \left(\frac{Q K^{\top}}{\sqrt{D_{k}}}\right)$, $\hat{A}_{i j}=-\infty \text { if } i<j$。这种自我注意通常被称为自回归注意或因果注意。
+ **Cross-attention**：query由上一层decoder的输出投影而来，key/value又encoder的输出投影而来

#### Position-wise FFN

基于位置的FFN是一个全连接的前馈网络，它在每一个位置上进行独立运算，注意：前向网络的参数在不同位置上是共享的，因此Position-wise FFN也可以理解为两层kernel size为1的卷积层。

<div align="center">
    $\operatorname{FFN}\left(\mathbf{H}^{\prime}\right)=\operatorname{ReLU}\left(\mathbf{H}^{\prime} \mathbf{W}^{1}+\mathbf{b}^{1}\right) \mathbf{W}^{2}+\mathbf{b}^{2}$
</div>

其中$\mathbf{H}^{\prime}$为上一层的输出，$\mathbf{W}^{1} \in \mathbb{R}^{D_{m} \times D_{f}}, \mathbf{W}^{2} \in \mathbb{R}^{D_{f} \times D_{m}}, \mathbf{b}^{1} \in \mathbb{R}^{D_{f}}, \mathbf{b}^{2} \in \mathbb{R}^{D_{m}}$，一般来讲FFN的维度参数设置为$D_f > D_m$

#### Residual connection and normalization

<div align="center">
    \begin{aligned}
        \mathrm{H}^{\prime} &=\text { LayerNorm }(\text { SelfAttention }(\mathrm{X})+\mathrm{X}) \\
        \mathrm{H} &=\text { LayerNorm }\left(\mathrm{FFN}\left(\mathrm{H}^{\prime}\right)+\mathrm{H}^{\prime}\right)
    \end{aligned}
</div>

#### Position Encodings

因为Transformer没有引入递归结构或卷积操作，所以对于每个attention来说，它们不知道数据的前后位置信息（特别是对于编码器来说）。因此需要对数据的位置做额外的表征

### 模型的拆解用法

+ **encoder-decoder**：用于seq2seq modeling
+ **encoder only**：representation learning，用于支持classification，sequence labeling
+ **decoder only**：（此时encoder-decoder cross-attention module也被移除），sequence generation，用于支持language modeling

（后续更新：主要transformer的总结，以及亮点结构）
