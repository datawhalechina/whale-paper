# 1. 背景
基于文本协同过滤（TCF）已成为文本和新闻推荐的主流方法，利用文本编码器来表示推荐项。然而，现有的TCF模型主要关注使用小型或中型LM。
# 2. 动机
目前还不确定将推荐项编码器替换为最大且最强大的LM之一，比如拥有1750亿参数的GPT-3模型对推荐性能会产生怎样的影响。作者能期待前所未有的结果吗？因此，作者进行了一系列广泛的实验，旨在探索TCF范式的性能极限。
# 3. 方法
具体来说，作者将推荐项编码器的规模从1亿增加到1000亿，以揭示TCF范式的扩展限制。然后，作者检查这些极大型LM是否能实现通用的推荐项表示。此外，作者将利用最强大的LM对TCF范式的性能与当前主导的基于ID嵌入的方法进行比较，并探究TCF范式的可转移性。最后，作者还将TCF与最近流行的基于ChatGPT1的提示式推荐进行比较。研究发现不仅获得了积极的结果，还揭示了一些令人惊讶且之前未知的负面结果，这可能会激发对基于文本推荐系统的深入思考和创新。

# 4. 实验
## 4.1 数据集
本文在三个真实世界的文本数据集上评估了TCF和LLM作为文本编码器：MIND 微软发布的新闻点击推荐数据集，来自H&M平台的HM服装购买数据集，以及来自在线视频推荐的Bili评论数据集站台。

## 4.2 对比模型
- 以双塔DSSM模型为例的CTR（click-through rate）预测模型
- 作为Transformer model式序列模型示例的SASRec模型
## 4.3 实验结果
本文研究了基于文本的协同过滤（TCF）技术在推荐系统中的性能和限制。它将TCF与主导的基于ID的协同过滤（IDCF）范式进行了比较，用于热门或流行项目的推荐，并发现在以文本为中心的推荐系统中，使用冻结的文本编码器的TCF可以达到与IDCF相似的性能。然而，使用DSSM骨干的TCF几乎没有与IDCF竞争的机会。本文还研究了TCF是否能够实现通用的推荐模型，这需要在不同领域和平台之间的可转移性。研究得出结论，虽然具有大型语言模型（LMs）的TCF模型表现出一定程度的迁移学习能力，但它们仍然远远不足以成为通用的推荐模型。本文还将TCF与使用ChatGPT4Rec的基于提示的技术进行了比较，并发现在典型的推荐设置中，ChatGPT的性能不及TCF。本文预计随着自然语言处理大型模型的进一步发展，TCF可能会取得更好的性能，但构建大型基础推荐模型可能比自然语言处理和计算机视觉领域更具挑战性。

# 5. 总结
本文探讨了使用大型语言模型进行基于文本的协同过滤的上限。作者使用具有数十亿参数的大型LM作为文本编码器，比较了TCF与当前基于ID嵌入的范式的性能限制。他们探讨了TCF模型是否能够为推荐任务生成通用的项目表示，研究了它们在跨平台推荐中的可转移性，并将TCF与最近流行的基于提示的ChatGPT推荐进行了比较。作者进行了一系列实验来回答文献中鲜有关注的问题。研究结果表明，TCF是一种有前景的方法，可以与基于ID的推荐竞争，但也突出了使用大型LM进行推荐任务的一些限制。

[comment]: <> (# 6. 视频)
[comment]: <> ( [链接]&#40;TODO&#41;)