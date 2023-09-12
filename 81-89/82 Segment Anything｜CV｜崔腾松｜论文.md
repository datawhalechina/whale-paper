### 摘要
我们介绍了 Segment Anything (SA) 项目：用于图像分割的新任务、模型和数据集。在数据收集循环中使用我们的高效模型，我们构建了迄今为止（迄今为止）最大的分割数据集，在 1100 万张许可和尊重隐私的图像上有超过 10 亿个掩码。

该模型被设计和训练为可提示的，因此它可以将零样本转移到新的图像分布和任务中。我们评估了它在众多任务上的能力，发现它的零样本性能令人印象深刻——通常与之前的完全监督结果具有竞争力，甚至优于之前的结果。我们在 https://segment-anything.com 上发布了 Segment Anything Model (SAM)和相应的 1B 掩模和 11M 图像数据集 (SA-1B)，以促进对计算机视觉基础模型的研究。

## 背景介绍

### 目标
目标是建立一个图像分割的基础模型

提出问题：
1.什么任务可以实现 zero-shot 泛化？
2.对应的模型架构是什么？
3.哪些数据可以支持这个任务和模型？

解决方法：

1. 使用prompt工程解决新数据分布上的一系列下游分割问题
2. 一个可提示的模型
3. 能实现强大泛化任务的数据集
### 任务Task
•promptable segmentation任务，即通过提示来实现图像分割的任务。
•该任务旨在实现零样本迁移，并且可以通过prompt engineering方法来实现。
![[Pasted image 20230830202848.png]]

## 模型架构
1. 支持promptable segmentation任务，并且可以通过prompt engineering方法实现零样本迁移。
2. 即SAM模型，包括一个强大的图像编码器、一个prompt编码器和一个轻量级的掩码解码器。
### SAM overview
SAM有三个组件，图像编码器，灵活的提示编码器和快速掩码解码器
![[Pasted image 20230830203152.png]]

## 数据构建
### 数据Data
1. 提出了一个新的数据集SA-1B，它包含10亿个掩模，并且可以用于训练和评估SAM模型。
2. 使用了多个数据集进行训练和评估SAM模型（包括COCO、ADE20K、Cityscapes、LVIS等）
![[Pasted image 20230830203044.png]]
### 数据集掩码属性
图例引用每个数据集中的图像和掩码的数量。与现有的最大的分割数据集Open Images相比，SA-1B的图像增加了11倍，掩码增加了400倍。
![[Pasted image 20230830203217.png]]

### 数据标注
1. Data engine ：a data collection loop
2. 三个阶段：辅助人工阶段，半自动阶段，全自动阶段
3. 注释员使用 SAM 交互式地注释图像，然后新注释的数据反过来用于更新 SAM，彼此相互作用，重复执行此循环来改善模型和数据集。
### SA-1B数据集
![[Pasted image 20230830203317.png]]

## 实验分析

### 分割效果
1. 每列显示了来自单个模糊点提示（绿色圆圈）的 SAM 生成的3 个有效掩码（大中小）。
2. 即使提示是模糊的，可能指向多个对象。输出也应该是这些对象中的至少一个的合理掩模。
![[Pasted image 20230830203123.png]]

## 评价谈论
### 贡献
1. 提出了一种新的图像分割模型SAM，它支持promptable segmentation任务，并且可以通过prompt engineering实现零样本迁移。
2. 提出了一种基于prompt engineering的零样本迁移方法，可以将SAM模型应用于其他图像分割任务中。
3. 在多个数据集上评估了SAM模型的性能，并与其他图像分割方法进行比较。
4. 设计了一个人类研究实验来验证SAM模型是否能够有效地支持promptable segmentation任务。
5. 提出了一个新的数据集SA-1B，它包含10亿个掩模，并且可以用于训练和评估SAM模型。

### 评价
![[Pasted image 20230830181416.png]]

### 放眼当下
CV还有很长的路要走。
1. 交互性强，上手即用，非常方便。
	1. 图像语义可以无限细分，且逻辑关系非常复杂（识别的粒度和确定性之间）。
2. SAM其实缩小了大厂和小厂的差距，可以使用SAM的特征做各种下游任务。
	1. 但下游任务所面临的困难或许比预训练还要更多！
3. SAM作为分割领域基础模型的潜力（如CLIP），未来将为大量的视觉任务提供基础能力。
	1. 与ChatGPT不同，能解决你NLP几乎所有的任务。

### 展望未来

新数据集+新范式+超强零样本泛化能力。
1. 研究和数据集共享，加速对图像分割以及更通用图像与视频理解的研究。
2. 将 NLP 的 prompt 范式引入了 CV 领域，这类范式可能在学术界将迎来一次爆发。
3. SAM的出现统一了分割这个任务的下游应用，说明了CV大模型是可能存在的。
4. SAM作为大系统的一个组件，执行分割任务。
5. 基于 prompt 工程等技术的可组合系统设计将支持更广泛的应用。
6. 可以成为 AR、VR、内容创建、科学领域和更通用 AI 系统的强大组件。


## 其它

### 相关技术/论文
1. Transformer（一种基于自注意力机制的神经网络结构）
2. MaskFormer（图像分割方法，它能够将任何现有的像素分类模型
3. 无缝转换为掩码分类方法，采用Transformer解码器来计算一组成对的预测，
4. 每个预测由一个类别预测和一个掩码嵌入向量组成）  
5. MAE（可扩展自监督学习，对输入图像的随机块进行mask并对遗失像素进行重建。）
6. VIT（一种基于Transformer的视觉注意力模型，使用Transformer编码器来提取图像特征，并使用Transformer解码器来生成图像）
7. CLIP（图像文本多模态，使用了一个基于Transformer的编码器来提取特征，并使用这些特征来计算图像和文本之间的相似度）
8. DETR（目标检测方法，它使用Transformer编码器来提取特征，并使用Transformer解码器来生成目标检测结果）
### 网站推荐
1. [https://huggingface.co/ybelkada/segment-anything](https://huggingface.co/ybelkada/segment-anything%0d)
2. [https://paperswithcode.com/](https://paperswithcode.com/)
3. [https://readpaper.com/](https://readpaper.com/%0d
4. [https://www.chatpdf.com/](https://www.chatpdf.com/%0d)
### 开源项目推荐

https://github.com/IDEA-Research/Grounded-Segment-Anything
通过结合Grounding DINO和Segment Anything来创建一个非常有趣的演demo

项目特点：
1. Segment Anything是一个强大的分割模型。但是，它需要提示（如框/点）来生成遮罩。
2. Grounding DINO是一个强大的零样本检测器，可以通过自由形式的文本生成高质量的边框和标签。这两个模型的结合使得能够仅通过文本输入检测和分割所有物体！
3. BLIP+GroundingDINO+SAM组合用于自动标注！
4. GroundingDINO+SAM+Stable-diffusion组合用于数据工厂，生成新数据！