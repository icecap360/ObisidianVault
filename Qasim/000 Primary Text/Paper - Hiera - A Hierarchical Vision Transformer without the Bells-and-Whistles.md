---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Incomplete"
completed: false
title: "Hiera: A Hierarchical Vision Transformer without the Bells-and-Whistles"
authors: Chaitanya Ryali, Yuan-Ting Hu, Daniel Bolya, Chen Wei, Haoqi Fan, Po-Yao Huang, Vaibhav Aggarwal, Arkabandhu Chowdhury, Omid Poursaeed, Judy Hoffman, Jitendra Malik, Yanghao Li, Christoph Feichtenhofer
year: 2023/06
url: http://arxiv.org/abs/2306.00989
citekey: ryaliHieraHierarchicalVision2023
aliases:
  - Hiera
  - ryaliHieraHierarchicalVision2023
---

# Short Summary

- strip bells-and-whistles from SOTA vision transformer, without losing accuracy, via pretraining with a strong visual pretext task (such as masked autoencoder)
- HIera: simple hierarchical ViT, more accurate while being faster

# Abstract
```
Modern hierarchical vision transformers have added several vision-specific components in the pursuit of supervised classification performance. While these components lead to effective accuracies and attractive FLOP counts, the added complexity actually makes these transformers slower than their vanilla ViT counterparts. In this paper, we argue that this additional bulk is unnecessary. By pretraining with a strong visual pretext task (MAE), we can strip out all the bells-and-whistles from a state-of-the-art multi-stage vision transformer without losing accuracy. In the process, we create Hiera, an extremely simple hierarchical vision transformer that is more accurate than previous models while being significantly faster both at inference and during training. We evaluate Hiera on a variety of tasks for image and video recognition. Our code and models are available at https://github.com/facebookresearch/hiera.
```
# Details
## Problem
model hierarchical vision transformers are complex, which makes them slow on actual hardware
## Background
- - What are hierarchical transformers, e.g. Swin and MViT
    - ViTs are simple but inefficient. They use the same spatial resolution and number of channels throughout the network
        - Think of how alexnet and resnet use fewer channels but higher spatial resolution in early stages with simpler features, and more channels but lower spatial resolution later in the model with more complex features.
    - To get ViTs to perform well during “fully supervised training on ImageNet-1K”, specialized complicated modules were added:
        - examples: cross-shapped windows in CSWin, decomposed relative position embeddings in MViTv2
        - These techniques tried to add spatial bias into ViT which lack it
        - 
## Methodology
- IDEA: Why should we slow down our architecture to add these biases, if we could just train the model to learn them instead?
- MAE pretraining has shown to be a very effective tool to teach ViTs spatial reasoning, which was a task previously dominated by models like Swin or MViT.
- MAE pretraining is sparse, can be 4-10x as fast as normal supervised training
- Combining ViT with convolutions increases performance, but is slower and cannot be used with MAE like self-supervised tasks
- Adapting sparse training to hierarchical models is nontrivial, attempts include:
    - replace masked patches with [mask] tokens, meaning most computation is wasted on non-visible tokens and training is incredibly slow.
    - special masking strategy that significantly hurts accuracy
    - MCMAE uses masked conolution in first couple of stages, reduces efficieny
    - Hiera: Designing our architecture specifically for sparse MAE pretraining,
- - Approach
    - simple experiment: take an existing hierarchical vision transformer and ablate its bells-and-whistles while training with a strong pretext task
    - MAE breaks the 2D grid hierarchical ViT’s rely on
    - Terminology:
        - mask units: resolution we apply MAE masking
        - tokens: internal resolution of the model
    - We use patch sizes equivalent to what is typically used by other hierarchical models: 4x4
    - Idea: treat mask units as contiguous, seprate from tokens
    - In our case, we mask 32×32 pixel regions, meaning one mask unit is 8 × 8 tokens at the start of the network.
![[Pasted image 20231106204333.png]]
Caption: While MAE masks individual tokens, tokens in multi- stage transformers start very small (e.g., 4 × 4 pixels), doubling size in each stage. (a) Thus, we mask coarser “mask units” (32×32 pixels) instead of tokens directly. (b) For efficiency, MAE is sparse, meaning it deletes what it masks (a problem for spatial modules like convs). (c) Keeping masked tokens fixes this, but gives up the potential 4 − 10× training speed-up of MAE. (d) As a baseline, we introduce a trick that treats mask units as a separate entities for convs, solving the issue but requiring undesirable padding. (e) In Hiera, we side-step the problem entirely by changing the architecture so the kernels can’t overlap between mask unit

## Using MViT as our Architecture
- We choose MViTv2 as our base architecture, as its small 3 × 3 kernels are affected the least by the separate-and-pad trick described in Fig. 4d
- - About MViT2
    - 4 stages
        - starts by modelling low level features with small channel capacity but high spatial resolution
        - each subsequent stage trades channel capacity for spatial resolution
    - pooling attention: features are locally aggregated using 3x3 convolution before computing self-attention
        - K and V pooled to decrease computation in the first two stages, while Q is pooled to transition from one stage to the next by reducing spatial resolution
    - decomposed relative position embeddings used instead of absolute ones
    - residual pooling connection to skip between pooled Q tokens inside the attention block
- - Applying MAE
    - MViT2 downsamples by 2x2 a total of 3 times across 4 stages, as it uses a token size of 4x4 pixels, thus mask unit size=32x32, which ensures that each mask unit corresponds to $8^2,4^2,2^2,1^2$ tokens in stages 1,2,3,4
    - As in 4d, mask units are shifted to the batch dimension to seperate them for pooling, treating each mask unit as an “image”, but undo shift afterward so that self-attention is still global
 
## Results

# Comments and Implications

# Links
[@ryaliHieraHierarchicalVision2023]