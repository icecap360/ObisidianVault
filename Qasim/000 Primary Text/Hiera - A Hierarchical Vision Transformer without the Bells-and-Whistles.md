---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Unread"
completed: false
title: "Hiera: A Hierarchical Vision Transformer without the Bells-and-Whistles"
authors: "Chaitanya Ryali, Yuan-Ting Hu, Daniel Bolya, Chen Wei, Haoqi Fan, Po-Yao Huang, Vaibhav Aggarwal, Arkabandhu Chowdhury, Omid Poursaeed, Judy Hoffman, Jitendra Malik, Yanghao Li, Christoph Feichtenhofer"
year: '2023/06'
url: "http://arxiv.org/abs/2306.00989"
citekey: "ryaliHieraHierarchicalVision2023"
aliases:
  - "Hiera"
  - "ryaliHieraHierarchicalVision2023"
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
## Results

# Comments and Implications

# Links
[@ryaliHieraHierarchicalVision2023]