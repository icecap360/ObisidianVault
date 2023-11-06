---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Unread"
completed: false
title: Paper - Feature Pyramid Networks for Object Detection
authors: "Tsung-Yi Lin, Piotr Dollár, Ross Girshick, Kaiming He, Bharath Hariharan, Serge Belongie"
year: '2017/04'
url: "http://arxiv.org/abs/1612.03144"
citekey: "linFeaturePyramidNetworks2017"
aliases:
  - ""
  - "linFeaturePyramidNetworks2017"
---

# Short Summary

## Key Points

# Abstract
```
Feature pyramids are a basic component in recognition systems for detecting objects at different scales. But recent deep learning object detectors have avoided pyramid representations, in part because they are compute and memory intensive. In this paper, we exploit the inherent multi-scale, pyramidal hierarchy of deep convolutional networks to construct feature pyramids with marginal extra cost. A top-down architecture with lateral connections is developed for building high-level semantic feature maps at all scales. This architecture, called a Feature Pyramid Network (FPN), shows significant improvement as a generic feature extractor in several applications. Using FPN in a basic Faster R-CNN system, our method achieves state-of-the-art single-model results on the COCO detection benchmark without bells and whistles, surpassing all existing single-model entries including those from the COCO 2016 challenge winners. In addition, our method can run at 5 FPS on a GPU and thus is a practical and accurate solution to multi-scale object detection. Code will be made publicly available.
```
# Details
## Background
- These pyramids are scale-invariant in the sense that an object’s scale change is offset by shifting its level in the pyramid.
- Featurized image pyramids were heavily used in the era of hand-engineered features
    - ConvNets are also more robust to variance in scale and thus facilitate recognition from features computed on a single input scale \[15, 11, 29] (Fig. 1(b)).
    - But even with this robustness, pyramids are still needed to get the most accurate results. e.g. All recent top entries in the ImageNet use multi-scale testing on featurized image pyramids.
    - The principle advantage of featurizing each level of an image pyramid is that it produces a multi-scale feature representation in which all levels are semantically strong, including the high-resolution levels.
- Running a CNN on each level of features is inefficient computationally, plus all four feature levels cannot be trained without OOM
- So at the time of writing this paper, image pyramids were used mostly at test time only, which creates inconsistency between training and testing
-
## Solution
- CNNs are also hierarchical
    - This in-network feature hierarchy produces feature maps of different spatial resolutions, but introduces large semantic gaps caused by different depths.
    - The high-resolution maps have low-level features that harm their representational capacity for object recognition.
- The Single Shot Detector (SSD) \[22] is one of the first attempts at using a ConvNet’s pyramidal feature hierarchy as if it were a featurized image pyramid (Fig. 1(c)).
    - It misses the oppurtunity to reuse the feature maps, why?
    - to avoid using low-level features SSD , it builds the pyramid starting from high up in the network (e.g., conv4_3 of VGG nets \[36]) and then by adding several new layers.
![[Pasted image 20231106170250.png]]
- Our model echoes a featurized image pyramid, which has not been explored in these works.
![[Pasted image 20231106170509.png]]
- The bottom-up pathway is the feedforward computation of the backbone ConvNet, which computes a feature hierarchy consisting of feature maps at several scales with a scaling step of 2.
- 
-The construction of our pyramid involves a bottom-up pathway, a top-down pathway, and lateral connections, as introduced in the following.
	- **Bottom-up pathway**: The bottom-up pathway is the feedforward computation of the backbone ConvNet, which computes a feature hierarchy consisting of feature maps at several scales with a scaling step of 2.
	  Use Feature Maps from the last layer in each stage. This choice is natural since the deepest layer of each stage should have the strongest features.
	- **Top-down pathway**:
	  The topdown pathway hallucinates higher resolution features by upsampling spatially coarser, but semantically stronger, feature maps from higher pyramid levels. 
	- **Lateral connection**: Lateral connections are then used to enhance features from the bottom-up pathway. Each lateral connection merges feature maps of the same spatial size from the bottom-up pathway and the top-down pathway. The bottom-up feature map is of lower-level semantics, but its activations are more accurately localized as it was subsampled fewer times
		- They choose to upsample by nearest neightbour, and merge with bottom-up map by element-wise additions.

## Results
- The resulting Feature Pyramid Network is general purpose and in this paper we focus on sliding window proposers (Region Proposal Network, RPN for short) \[29] and region-based detectors (Fast R-CNN) \[11]. in this paper we present results using ResNets \[16].



# Comments and Implications

# Links
[@linFeaturePyramidNetworks2017]