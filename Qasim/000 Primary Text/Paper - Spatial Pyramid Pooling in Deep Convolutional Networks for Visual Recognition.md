---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Unread"
completed: false
title: "Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition"
authors: "Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun"
year: '2014/01'
url: "http://arxiv.org/abs/1406.4729"
citekey: "heSpatialPyramidPooling2014"
aliases:
  - ""
  - "heSpatialPyramidPooling2014"
---

# Short Summary

## Key Points

# Abstract
```
Existing deep convolutional neural networks (CNNs) require a fixed-size (e.g., 224x224) input image. This requirement is "artificial" and may reduce the recognition accuracy for the images or sub-images of an arbitrary size/scale. In this work, we equip the networks with another pooling strategy, "spatial pyramid pooling", to eliminate the above requirement. The new network structure, called SPP-net, can generate a fixed-length representation regardless of image size/scale. Pyramid pooling is also robust to object deformations. With these advantages, SPP-net should in general improve all CNN-based image classification methods. On the ImageNet 2012 dataset, we demonstrate that SPP-net boosts the accuracy of a variety of CNN architectures despite their different designs. On the Pascal VOC 2007 and Caltech101 datasets, SPP-net achieves state-of-the-art classification results using a single full-image representation and no fine-tuning. The power of SPP-net is also significant in object detection. Using SPP-net, we compute the feature maps from the entire image only once, and then pool features in arbitrary regions (sub-images) to generate fixed-length representations for training the detectors. This method avoids repeatedly computing the convolutional features. In processing test images, our method is 24-102x faster than the R-CNN method, while achieving better or comparable accuracy on Pascal VOC 2007. In ImageNet Large Scale Visual Recognition Challenge (ILSVRC) 2014, our methods rank #2 in object detection and #3 in image classification among all 38 teams. This manuscript also introduces the improvement made for this competition.
```
# Details
## Problem
- there is a technical issue in the training and testing of the CNNs: the prevalent CNNs require a fixed input image size (e.g., 224×224), which limits both the aspect ratio and the scale of the input image. When applied to images of arbitrary sizes, current methods mostly fit the input image to the fixed size, either via cropping [3], [4] or via warping [13], [7], as shown in Figure 1 (top). But the cropped region may not contain the entire object, while the warped content may result in unwanted geometric distortion.

## Background
- Spatial pyramid pooling [14], [15] (popularly known as spatial pyramid matching ) is famous
	-  an extension of the Bag-of-Words (BoW) model
- It partitions the image into divisions from finer to coarser levels, and aggregates local features in them.
- SPP properties
	1) SPP is able to generate a fixedlength output regardless of the input size, while the sliding window pooling used in the previous deep networks [3] cannot; 	
	2) SPP uses multi-level spatial bins, while the sliding window pooling uses only a single window size. Multi-level pooling has been shown to be robust to object deformations [15]; 
	3) SPP can pool features extracted at variable scales thanks to the flexibility of input scales.
### From original SPP paper
- Informally, pyramid matching works by placing a sequence of increasingly coarser grids over the feature space and taking a weighted sum of the number of matches that occur at each level of resolution.
- At any fixed resolution, two points are said to match if they fall into the same cell of the grid; matches found at finer resolutions are weighted more highly than matches found at coarser resolutions.
- 
## Methodology
- We add an SPP layer on top of the last convolutional layers
- The SPP layer pools the features and generates fixed length outputs, which are then fed into the fully connected layers (or other classifiers). 
	- In other words, we perform some **information** “**aggregation**” at a **deeper stage** of the network hierarchy (between convolutional layers and fully-connected layers) to avoid the need for cropping or warping at the beginning. 
- Training with variable-size images **increases scale-invariance** and **reduces over-fitting**.
	- We develop a simple multi-size training method.
	- In each epoch we train the network with a given input size, and switch to another input size for the next epoch. Experiments show that this multi-size training converges just as the traditional single-size training, and leads to better testing accuracy.
- 

## Results

# Comments and Implications

# Links
[@heSpatialPyramidPooling2014]