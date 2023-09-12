## 背景介绍
### 抛出问题
1. 过去选择哪种架构取决于下游任务。现在应该如何选择？
2. 预训练模型能否在多种任务上通用？是否存在一个最优架构。
3. 模型架构优化到底还有没有作用，是否还有研究价值？例如：Chatglm中的2D-Position Encoding；LLaMA中的SwiGLU激活函数，旋转位置编码；
### 范式回顾
1. 范式回顾：auto-encoding(Bert) vs auto-regressive(GPT)
2. 预训练目标回顾：causal LM(auto-regressive), span corruption(auto-encoder), prefixLM(Seq2seq)
3. 框架回顾：UniLM（3种Mask融合；cloze-type）, T5, GPT-3(In-context learning)

## 统一范式

1. Decoder-only vs Encoder-only vs Encoder-Decoder
	1. Encoder-only不考虑
	2. ED input/target参数不同等价于稀疏、cross attention，双倍参数
	3. DO input/target表示逐层构建
2. MoD：Mixture-of-Denoiser
![[Pasted image 20230830101738.png]]

## 模型架构

1. Model Architecture
	1. Causal Language Model (CLM): left-to-right auto-regressive
	2. Prefix LM (PLM): variation of CLM
	3. Span Corruption (SC): standard denoising objective
	4. Span Corruption + LM (SCLM): a mixture of CLM and SC
	5. UniLM (ULM): generate the masked tokens (not bert-style)
## 实验情况
![[Pasted image 20230830102326.png]]
### 实验结果
1. 结果讨论
	1. Google：无参数限制Enc-Dec；推荐至少Prefix-LM or UniLM
	2. PALM：Partial Attention > Enc-Dec > Dec
2. 苏：
	1. UniLM 相比 GPT 并无任何优势，甚至某些任务更差。
	2. 输入部分的注意力（prompt）改为双向不会带来收益，Encoder-Decoder 架构的优势很可能只是源于参数翻倍。

## 开源LLM

1. GLM
	1. Autoregressive Blank Infilling (SC+CLM)
	2. 2D Positional Encoding
![[Pasted image 20230830103614.png]]

2. LLaMA
	1. 同等size模型更多的数据训练效果更好
	2. 三个策略：
		3. Pre-normalization
		4. SwiGLU激活函数
		5. RoPE位置编码

3. InternLLM
	1. 数据准备-dataset preparation
		1. 按语言分类-Language classification
		2. 按规则和模型过滤-Rule/Model-based filtering
		3. 去重-Deduplication
	2. 多阶段预训练-multi-phase pretraining
		1. GPT-like:  82layers, 80heads, 128d_head, 10240d_model
		2. 分阶段训练和存储模型参数-Each stage with its optimization objective defined by controlling various proportions of data, resume the training the stage if performance doesnot meet expaction
		3. 句子处理为固定长度-packaged sentences of varying lengths into fixed-length sequences, using special symbols
	3. 对齐-alignment: InstructGPT, 5M(prompt)+3H+PPO
	4. 为什么好？
		1. 架构比较统一
		2. 更好的数据和与训练？

## 讨论记录

## 参考文献
1. UL2: Unifying Language Learning Paradigms
2. Decoder-Only or Encoder-Decoder? Interpreting Language Model as a Regularized Encoder-Decoder
3. LLaMA: Open and Efficient Foundation Language Models
4. GLM: General Language Model Pretraining with Autoregressive Blank Infilling
5. InternLM: A Multilingual Language Model with Progressively Enhanced Capabilities
6. Unified Language Model Pre-training for Natural Language Understanding and Generation
7. Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer
8. Language Models are Few-Shot Learners
9. A Comprehensive Analysis of Adapter Efficiency
10. 为什么现在的 LLM 都是 Decoder-only 的架构？https://kexue.fm/archives/9529