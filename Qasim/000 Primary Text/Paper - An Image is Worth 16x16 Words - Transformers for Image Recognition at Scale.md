---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Unread"
completed: false
title: "An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale"
authors: "Alexey Dosovitskiy, Lucas Beyer, Alexander Kolesnikov, Dirk Weissenborn, Xiaohua Zhai, Thomas Unterthiner, Mostafa Dehghani, Matthias Minderer, Georg Heigold, Sylvain Gelly, Jakob Uszkoreit, Neil Houlsby"
year: '2021/06'
url: "http://arxiv.org/abs/2010.11929"
citekey: "dosovitskiyImageWorth16x162021"
aliases:
  - "An Image is Worth 16x16 Words"
  - "dosovitskiyImageWorth16x162021"
---

# Short Summary

## Key Points

# Abstract
```
While the Transformer architecture has become the de-facto standard for natural language processing tasks, its applications to computer vision remain limited. In vision, attention is either applied in conjunction with convolutional networks, or used to replace certain components of convolutional networks while keeping their overall structure in place. We show that this reliance on CNNs is not necessary and a pure transformer applied directly to sequences of image patches can perform very well on image classification tasks. When pre-trained on large amounts of data and transferred to multiple mid-sized or small image recognition benchmarks (ImageNet, CIFAR-100, VTAB, etc.), Vision Transformer (ViT) attains excellent results compared to state-of-the-art convolutional networks while requiring substantially fewer computational resources to train.
```
# Details
## Background
- Keeps overall structure of CNN
- We experiment with applying a standard Transformer directly to images, with the fewest possible modifications
- Idea: Splitting an image into patches and provide the sequence of linear embeddings of these patches as an input to a Transformer
    - It did not work well on medium size datasets: When trained on mid-sized datasets such as ImageNet without strong regularization, these models yield modest accuracies of a few percentage points below ResNets of comparable size.
        
        Cause of problem: lack inductive biases inherent to CNNs, like translation equivariance and locality
        
    - It works with larger datasets: excellent results when pre-trained on 14M-300M images, dollowed by fine tuning
        
- Past transformer+image approaches did not perform well on hardware
    - Naive application, each pixel attends to every other pixel, too expensive
    - Parmar et al. (2018) applied the self-attention only in local neighborhoods for each query pixel instead of globally. These multi-head dot-product self attention blockks can replace convolutions
    - Sparse Transformers: employ scalable approximations to global self-attention to be applicable to images
    - Apply attention in blocks of varying size, such as along individual axes
- iGPT got 72% on imagenet: which applies Transformers to image pixels after reducing image resolution and color space
## ViT
- Notation:
	- (P, P ) is the resolution of each image patch
	- N=HW/P^2
	- Notation for patches
![[Pasted image 20231106200506.png]]
- Computing patch embeddings:
    - The Transformer uses constant latent vector size D through all of its layers, so we flatten the patches and map to D dimensions with a trainable linear projection
    - Note: this is different from DETR, they flattened each channel of the feature map, here they flatten (across channels) and project each patch
- - Similar to BERT’s [class] token, we prepend a learnable embedding to the sequence of embedded patches ($z_0^0$ = x_class), whose state at the output of the Transformer encoder ($z_L^0$) serves as the image representation y (Eq. 4).
    - During pretraining, the head, attatched to ($z_L^0$), is an MLP with 1 hidden layer, but a single linear layer at fine-tuning time
- Position embeddings are added. they using standard learnable 1D position embeddings, as 2d-aware embeddings did not work well
![[Pasted image 20231106200734.png]]
- Transformer encoder: Layer norm is applied before every block and residual connections after every block

## Inductive Bias
- In ViT, only MLP layers are local and translationally equivariant, while the self-attention layers are global
- There is very little 2D structure usage, e.g. cutting image into patches, fine-tuning time for adjusting position embeddings for images of different resolutions
- spatial relations need to be learned from scratch

## Hybrid architecture
- - Divide CNN feature maps into patches rather then the raw image.
- As a special case, the patches can have spatial size 1x1, which means that the input sequence is obtained by simply flattening the spatial dimensions of the feature map and projecting to the Transformer dimension.

## Fine-tuning
- For this, we remove the pre-trained prediction head and attach a zero-initialized D × K feedforward layer, where K is the number of downstream classes.
- It is often beneficial to fine-tune at higher resolution than pre-training.
	- When feeding images of higher resolution, we keep the patch size the same, which results in a larger effective sequence length.
	- The Vision Transformer can handle arbitrary sequence lengths (up to memory constraints), however, the pre-trained position embeddings may no longer be meaningful. We therefore perform 2D interpolation of the pre-trained position embeddings, according to their location in the original image. Note that this resolution adjustment and patch extraction are the only points at which an inductive bias about the 2D structure of the images is manually injected into the Vision Transformer
## Experiments
- When considering the computational cost of pre-training the model, ViT performs very favourably, attaining state of the art on most recognition benchmarks at a lower pre-training cost.
- However, we note that pre-training efficiency may be affected not only by the architecture choice, but also other parameters, such as training schedule, optimizer, weight decay, et
## Intuition, and why they work
- left image shows first principle componenet of the learned embedding filter (i.e. the projective filter that is applied to each patch). They resemble plausible basis functions for low-dimensional representation of fine structure
- in the middle image we see that closer patches have more similar position embeddings. Also for larger grids, we see a sinusoidal structure. perhaps this is why 2d-aware embedding variants do not yield improvements, they 1d embedding learns the 2d position
- in the right image, for each layer, we compute the average distance in image space across which information is integrated, based on the attention weights. This “attention distance” is analogous to receptive field size in CNNs. We see that even in the lowest layers, the ability to integrate information globally is used by the model. The attention distance increases with network depth.
![[Pasted image 20231106201229.png]]



# Comments and Implications

# Links
[@dosovitskiyImageWorth16x162021]