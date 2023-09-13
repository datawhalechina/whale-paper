Code: https://codi-gen.github.io


### Abstract
1. A novel generative model: CoDi
2. Multimodal Generation:CoDi's input is not restricted to certain modalities like text or image.
3. Training Data Flexibility
4. Composable Generation Strategy
5. Customizability and Performance

## Introduction
### CoDi
The paper proposes a model called CoDi, which enable generating any combination of output modalities, including but not limited to language, image, video, and audio.
### Challenge
1. Limited Real-World Applicability of Existing Models
2. Heavily Computational
3. Lack of consistency and alignment across modalities
4. Need for Comprehensive Model Design and Diverse Data

## Related Work
### Diffusion model
1. Diffusion models (DMs) are a class of generative models.
2. Deep Diffusion Process (DDP) adopts a sequence of reversible diffusion steps to model image probability distribution.
![[Pasted image 20230830213155.png]]
3. Denoising diffusion probabilistic model(DDPM) are generative models mainly used to remove noise from visual or audio data.
![[Pasted image 20230830213210.png]]


## Method
### System model
1. Preliminary: Latent Diffusion Model
2. Composable Multimodal Conditioning
3. Composable Diffusion
4. Joint Multimodal Generation by Latent Alignment
### Latent Diffusion Model
The architecture of latent diffusion model：
![[Pasted image 20230830213330.png]]
1. The step t are controlled by a variance schedule$\beta_1,...,\beta_t$ ：![[Pasted image 20230830213408.png]]
2. The denoising training objective can be exprssed as(MSE)：![[Pasted image 20230830213529.png]]
	1. Given the random sample of  Z_t=α_t z+σ_t ε,where ε~N(0,I), where C is the prompt encode and ε is noise,ε_θ  is denoising model, y represents the conditional variable
3. The denoising process can be realized through reparameterized Gaussian sampling:![[Pasted image 20230830213610.png]]
#### CoDi model architecture:
![[Pasted image 20230830213634.png]]

### Composable Conditioning
Step 1:
![[Pasted image 20230830213701.png]]
1. It maps the embeddings of each modality to a shared space.
2. Multimodal conditioning is achieved through interpolation of unimodal embeddings. The multimodal conditional interpolation formula is as follows:![[Pasted image 20230830213720.png]]
Step 2:
![[Pasted image 20230830213734.png]]
1. For the diffusion model of modality A, the training objective is:![[Pasted image 20230830213748.png]]where θc denotes the weights of cross-attention modules in the UNet and V_B (Z_t^A) is the representation of the variable Z_t^A in the environment encoder V.
Step 3:
1. Cross-attention weights are trained in image and text diffusers using text-image paired data.
2. The text diffuser weights are frozen, and the environment encoder and cross-attention weights of the audio diffuser are trained using text-audio paired data.
3. The audio diffuser and its environment encoder are then frozen, and joint generation of the video modality is trained using audio-video paired data.
## Experiment
### Dataset
![[Pasted image 20230830213842.png]]
### IMPLEMENTATION RESULT
1. ![[Pasted image 20230830213855.png]]
2. CoDi achieves competitive performance with state-of-the-art techniques in image and video generation, as indicated by Table 2 and Table 3.
	1. ![[Pasted image 20230830213924.png]]
3. CoDi has achieved new state-of-the-art (SOTA) performance in audio captioning and audio generation, as demonstrated in Table 4 and Table 6!
	1. ![[Pasted image 20230830213945.png]]
	2. ![[Pasted image 20230830213948.png]]
4. CoDi achieves high-quality image generation within specific input modalities groups, as shown in Table 8. Table 9 indicates that CoDi closely resembles the ground truth across various input modalities groups.
	1. ![[Pasted image 20230830214016.png]]

## Conclusion
### Contributions
1. CoDi enables the generation of high-quality and coherent outputs across different modalities.
2. Composable diffusion models can be combined in different ways to achieve diverse generation objectives, making them a versatile tool for generative modeling.
### Future work
1. Model Improvements: Explore techniques for improving the performance and efficiency of composable diffusion models.
2. Domain Adaptation:Investigate the applicability of composable diffusion to other domains, such as audio synthesis, video generation, or 3D object generation.
