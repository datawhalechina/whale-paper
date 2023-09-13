跨模态训练模型

## Abstract
IMAGEBIND是一种新颖的方法，能够将六种不同模态（图像、文本、音频、深度、热度和IMU数据）的联合嵌入进行学习。利用大规模视觉-语言模型的强大能力，IMAGEBIND通过自然地与图像进行匹配，将其零样本能力扩展到新的模态。这使得IMAGEBIND能够轻松整合和探索多种模态，产生各种新颖的应用。

1. Zero-shot capabilities: Extending to new modalities via natural pairing with images
2. Leveraging: Large-scale vision-language models
3. Applications: Cross-modal retrieval,Composing modalities with arithmetic, Cross-modal detection, and generation
4. Evaluation: IMAGEBIND as a new framework for vision models in visual and non-visual tasks

## Introduction

### CLIP(Contrastive Language-Image Pretraining)
1. CLIP is a neural network model developed by OpenAI.
2. It associates images and their textual descriptions by learning a joint embedding space.
3. CLIP represents a significant advancement in multimodal AI research, bridging the gap between vision and language understanding.
![[Pasted image 20230830171324.png]]

### IMAGEBIND
1. The paper proposes a method called IMAGEBIND, which enables learning a joint embedding across six modalities: images, text, audio, depth, thermal, and IMU data.
2. Challenge：
	1. Difficulty of acquiring paired data for all modalities
	2. Heterogeneity of modalities
	3. Lack of a common embedding space for different modalities
3. Main contributions
	1. Propose a joint embedding space for different modalities
	2. Cross-modal Retrieval
	3. Embedding-space Arithmetic

## Method
模型方法-技术细节
### System model
1. Aligning specific pairs of modalities
2. Zero-shot image classification using text prompts
3. Binding modalities with images
![[Pasted image 20230830171526.png]]
### Feature Alignment
1. Feature alignment is crucial for multimodal learning
2. CLIP requires high-quality and aligned datasets for depth and thermal modes.
3. Audio and IMU modes have dense semantic information, making mapping to the same space easier.
### Binding modalities with images
使用图像和另一种模态数据进行配对，例如文本、音频、深度、热力、IMU等。训练的目标是通过最大化它们在共享嵌入空间中的相似性来实现。具体方法是将图像和另一种模态的观测编码为规范化的嵌入向量，然后通过InfoNCE损失函数来提高嵌入向量和编码器的一致性。有趣的发现是，即使只对其中一种模态进行训练，未见过的其他模态也能在嵌入空间中对齐。这使得可以进行各种零样本和跨模态的检索任务，无需进行专门的训练。该方法在零样本文本-音频分类任务上取得了最先进的结果，而无需配对的（音频，文本）样本。

1. InfoNCE  loss
	1. ![[Pasted image 20230830171627.png]]
	2. ∗Given an image $I_I$  and other modality $M_i$, we get $q_i=f(I_i)$ and $k_i=g(M_i)$ where $f$, $g$ are deep network.
	3. *$τ$ is a scalar temperature which controls the smoothness of the softmax distribution, and $j$ denotes negatives.
2. In fact, we use symmetric loss $L_{I,M}+L_{M,I}$  as final loss function!

## Experiment
###  Dataset
在IMAGEBIND中使用了六种模态数据：图像/视频、文本、音频、深度、热像和IMU。他们利用Audioset数据集中的（视频，音频）配对，SUN RGB-D数据集中的（图像，深度）配对，以及Ego4D数据集中的（视频，IMU）配对作为自然配对数据。对于这些模态的配对，他们没有使用额外的监督信号，如类标签或文本。为了训练，他们对SUN RGB-D和LLVIP数据集进行了50倍的复制，并按照之前的方法进行处理。
![[Pasted image 20230830171929.png]]

### IMPLEMENTATION RESULT

1. 每个任务评估了IMAGEBIND在未同时观察其他模态的情况下，将文本嵌入与其他模态关联的能力。
2. 报告了每个基准任务中已知的最佳监督上限。
3. IMAGEBIND在紧急零样本分类方面表现出很高的性能。
4. IMAGEBIND在每个基准测试中都显著提升，甚至比针对特定模态和任务进行训练的监督专家模型表现更好。这表明IMAGEBIND能够对齐不同的模态，并将与图像相关的文本监督隐式地转移到其他模态，如音频。
5. IMAGEBIND在非视觉模态（如音频和IMU）方面展现出强大的对齐能力，表明它们与图像的自然配对是一种有效的监督来源。
6. 还报告了标准的零样本图像（ImageNet - IN1K，Places-365 - P365）和视频（Kinetics400 - K400，MSR-VTT 1kA - MSR-VTT）任务。由于图像和文本编码器使用OpenCLIP进行初始化并冻结，因此这些结果与OpenCLIP的结果相匹配。

![[Pasted image 20230830171944.png]]


1. 零样本的文本到音频检索和分类。
2. 先前的工作使用配对数据进行训练，如AudioCLIP使用（音频，文本）监督，AVFIC使用自动挖掘的（音频，文本）对。比较了它们与IMAGEBIND的零样本文本到音频检索和分类性能。
3. IMAGEBIND在音频文本检索基准测试中明显优于先前的工作。在Clotho数据集上，IMAGEBIND的性能是AVFIC的两倍，尽管在训练过程中没有使用任何音频的文本配对。
4. IMAGEBIND在ESC上实现了可比的音频分类性能，相较于监督的AudioCLIP模型。需要注意的是，AudioCLIP在音频-文本训练中使用的是AudioSet的类名称作为文本目标，因此被称为“监督”。IMAGEBIND在所有三个基准测试中的出色表现验证了它使用图像作为桥梁来对齐音频和文本模态的能力。

![[截屏2023-08-30 18.04.01.png]]

1. 使用MSR-VTT 1k-A基准测试评估了Table 4中的文本到音频和视频检索性能。IMAGEBIND在视频检索方面表现出强大的紧急检索性能，相比于先前的工作（如MIL-NCE），仅使用音频。
2. IMAGEBIND在文本到视频的性能也非常出色（Table 2中的36.1% R@1），因为它结合了OpenCLIP的视觉和文本编码器，优于许多先前的方法。
3. 结合音频和视频模态进一步提升了性能，展示了IMAGEBIND特征在已有的强大检索模型上的实用性。
![[Pasted image 20230830171951.png]]

## Conclusion

### Contibutions
1. IMAGEBIND uses paired relevant modalities $(I,M)$ for training joint embeddings.
2. This joint embedding allows for zero-shot classification and cross-modal retrieval

### Future work
1. Expanding image alignment loss and adapting universal embeddings for structured prediction tasks, such as detection.
2. Alignment loss can be improved by augmenting data with other aligned sources.
3. sCLIP's performance lags behind specialized models due to the absence of specific downstream task training for embeddings.

 ## Q&A