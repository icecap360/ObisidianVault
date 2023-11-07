---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Unread"
completed: false
title: Paper - Non-local Neural Networks
authors: "Xiaolong Wang, Ross Girshick, Abhinav Gupta, Kaiming He"
year: '2018/04'
url: "http://arxiv.org/abs/1711.07971"
citekey: "wangNonlocalNeuralNetworks2018"
aliases:
  - ""
  - "wangNonlocalNeuralNetworks2018"
---

# Short Summary

## Key Points

# Abstract
```
Both convolutional and recurrent operations are building blocks that process one local neighborhood at a time. In this paper, we present non-local operations as a generic family of building blocks for capturing long-range dependencies. Inspired by the classical non-local means method in computer vision, our non-local operation computes the response at a position as a weighted sum of the features at all positions. This building block can be plugged into many computer vision architectures. On the task of video classification, even without any bells and whistles, our non-local models can compete or outperform current competition winners on both Kinetics and Charades datasets. In static image recognition, our non-local models improve object detection/segmentation and pose estimation on the COCO suite of tasks. Code is available at https://github.com/facebookresearch/video-nonlocal-net .
```
# Details
## Background
- We want to capture long range dependencies and information. Local operations applies repeatedly are not sufficient, due to vanishing/exploding gradient, other difficulties in optimization
- a non-local operation computes the response at a position as a weighted sum of the features at all positions in the in- put feature maps (Figure 1). The set of positions can be in space, time, or spacetime, implying that our operations are applicable for image, sequence, and video problems
- As such, our work provides insight by relating this recent self-attention model to the classic computer vision method of non-local means [ 4 ], and extends the sequential self-attention network in [ 49 ] to a generic space/spacetime non-local network for image/video recognition in computer vision.

## Solution
![[Pasted image 20231106193940.png]]
* Let g(x_j) = W_j\*X_j 
* Possibilities of f:
	- gaussian,: ![[Pasted image 20231106194127.png]]
	- embedded gaussian ![[Pasted image 20231106194148.png]]
	- dot product: ![[Pasted image 20231106194256.png]]
	- Concatenation ![[Pasted image 20231106194319.png]]
	- embedded gaussian version of f, with softmax along dimension j ![[Pasted image 20231106194350.png]]
This is Self -Attention!
# Comments and Implications

# Links
[@wangNonlocalNeuralNetworks2018]