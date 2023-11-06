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
		- They choose to upsample higher-level feature map by nearest neightbour, use 1x1 conv, and then merge with bottom-up map by element-wise additions.
- Note that all feature maps have the same number of channels, as they want simplicity in this paper

### FPNs with RPNs
- What is RPN?
	- a small subnetwork is evaluated on dense 3×3 sliding windows,  performing object/nonobject binary classification and bounding box regression. 
	- i.e.  3×3 convolutional layer followed by two sibling 1×1 convolutions for classification and regression, which we refer to as a network head
	- Anchors are used as a set of reference boxes
	- Anchors are of multiple pre-defined scales and aspect ratios 
- We attach a head of the same design (3×3 conv and two sibling 1×1 convs) to each level on our feature pyramid
- We assign anchors of a single scale to each level. Formally, we define the anchors to have areas of {322 , 642 , 1282 , 2562 , 5122} pixels
	- we also use anchors of multiple aspect ratios {1:2, 1:1, 2:1} at each level. 
	- So in total there are 15 anchors over the pyramid
- We note that the parameters of the heads are shared across all feature pyramid levels; 
## FPNs for RoI Pooling
- RoI pooling is used to extract features
- 

## Results
- We note that the parameters of the heads are shared across all feature pyramid levels; we have also evaluated the alternative without sharing parameters and observed similar accuracy.
- we assign an RoI of width w and height h (on the input image to the network) to the level Pk of our feature pyramid by:
![[Pasted image 20231106173805.png]]
	- Here 224 is the canonical ImageNet pre-training size, and $k_0$ is the target level on which an RoI with $w * h = 224^2$ should be mapped to.
	- 
- 



# Comments and Implications

# Links
[@linFeaturePyramidNetworks2017]