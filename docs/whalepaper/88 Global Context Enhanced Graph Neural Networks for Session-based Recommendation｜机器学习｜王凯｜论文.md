

## Background

### 会话推荐
根据用户的行为序列来预测用户下一个点击的Item
![[Pasted image 20230901155909.png]]

### 基于图神经网络的会话推荐

![[Pasted image 20230901155925.png]]


## Limitation
1. 基于__图神经网络__的模型，通过计算每个物品与最后一个物品之间的Attention Score来学习整个会话的表示，但性能严重依赖于最后一个物品与当前会话的用户偏好的相关性
2. 现有的会话推荐，往往仅根据当前Session序列来提取用户特征，忽略了其他Session序列对其的影响

## Contributions
1. 提出了一个统一的模型，通过有效地利用Session Graph和Global Graph的Item转换信息，来提高当前会话的推荐性能
2. 提出了一个位置感知的注意机制，来结合物品嵌入中的反向位置信息，表现出更卓越的会话推荐性能
3. 在三个真实数据集上进行了大量实验，结果表明GCE-GNN优于包括最先进方法在内的九个基准方法

## PRELIMINARIES

### Global Graph
1. 𝜀-Neighbor Set：代表着节点𝜀跳邻居集合，用于构建global graph
2. 对于每个节点会保留他频率最高的top-N个节点用于构图
![[Pasted image 20230901160107.png]]
### Session Graph
1. 𝑟𝑖𝑛:入边
2. 𝑟𝑜𝑢𝑡:出边
3. 𝑟𝑖𝑛-𝑜𝑢𝑡:既有入边又有出边
4. 𝑟𝑠𝑒𝑙𝑓:自环

![[Pasted image 20230901160224.png]]
## Method
### Overview
![[Pasted image 20230901160302.png]]
### Global-level Item Representation Learning Layer
![[Pasted image 20230901160314.png]]
1. 边特征生产
	1. 采用了Attention的思路给每条边赋予不同的权重
2. 点特征聚合
	1. 利用attention score进行加权求和，也可以认为是直接使用sum的方式来聚合
3. $N_{v_{i}}^{g}$表示节点$v_{i}$ _在global graph下的邻居集合_
4. s表示当前session的表征
![[Pasted image 20230901160522.png]]
5. 图信息与当前节点信息融合
	1. 采用简单的全连接层
### Session-level Item Representation Learning layer
![[Pasted image 20230901160551.png]]
$a_{r_{i j}}^T$ 表示节点和 $\mathrm{j}$ 之间边的表征

### Session Representation Learning Layer
![[Pasted image 20230901160622.png]]
1. 融合Global特征和Session特征得到最终的Item表征
2. 通过Attention将Session中的Item表征聚合为Session表征
3. $h_v^{h,(k)}$ 表示v在global graph下的表征
4. $h_v^s$ 表示v在session graph下的表征
5. $P=\left[p_1, p_2, \ldots, p_l\right]$ 表示position embedding
### Prediction Layer
![[Pasted image 20230901160808.png]]
1. Session表征和所有Item表征内积得到user对所有Item的点击率
2. 通过多分类任务的做法来完成模型参数更新
## EXPERIMENTS
### Dataset
![[Pasted image 20230901161249.png]]
1. 预处理
	1. 过滤掉session长度小于2的session
	2. 过滤掉出现次数小于5的item

### Metrics
1. P@N
2. MRR@N
3. 常用Recall指标介绍

### Baselines
1. POP
2. Item-KNN
3. FPMC
4. GRU4Rec
5. NARM
6. STAMP
7. SR-GNN
8. CSRM
9. FGNN
### Performance Comparison
![[Pasted image 20230901161203.png]]

### Impact of Global Feature Encode
![[Pasted image 20230901161132.png]]
1. GCE-GNN w/o global：没有global 特征
2. GCE-GNN w/o session：没有session特征
3. GCE-GNN-1-hop：global图聚合的hop为1
4. GCE-GNN-2-hop：global图聚合的hop为2

### Impact of Position Vector
![[Pasted image 20230901161058.png]]
1. GCE-GNN-NP
	1. 使用position vector代替位置编码
2. GCE-GNN-SA
	1. 使用self-attention代替position-aware attention

### Impact of Aggregation Operations
![[Pasted image 20230901161033.png]]
![[Pasted image 20230901161037.png]]
1. Gating Mechanism:Eq(17)
2. Max Pooling:Eq(18)
3. Concatenation:Eq(19)
4. Sum Pooling
## 小结
GCE-GNN从会话图和全局图中学习两个层次的Item嵌入，并使用软注意机制聚合它们。以此增强会话推荐的效果

