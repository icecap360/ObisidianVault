---
tags:
  - "#RawInformation"
  - "#Paper"
completed: true
title: Paper - Rich feature hierarchies for accurate object detection and semantic segmentation
authors: Ross Girshick, Jeff Donahue, Trevor Darrell, Jitendra Malik
year: 2014/10
url: http://arxiv.org/abs/1311.2524
citekey: girshickRichFeatureHierarchies2014
aliases:
  - girshickRichFeatureHierarchies2014
---

# Short Summary
- Combine region proposals with CNNs, we call our method R-CNN: Regions with CNN features
- At test time, our method generates around 2000 category-independent region proposals for the input image, extracts a fixed-length feature vector from each proposal using a CNN, and then classifies each region with category-specific linear SVMs. We use a simple technique (affine image warping) to compute a fixed-size CNN input from each region proposal, regardless of the region’s shape
## Key Points

# Abstract
```
Object detection performance, as measured on the canonical PASCAL VOC dataset, has plateaued in the last few years. The best-performing methods are complex ensemble systems that typically combine multiple low-level image features with high-level context. In this paper, we propose a simple and scalable detection algorithm that improves mean average precision (mAP) by more than 30% relative to the previous best result on VOC 2012---achieving a mAP of 53.3%. Our approach combines two key insights: (1) one can apply high-capacity convolutional neural networks (CNNs) to bottom-up region proposals in order to localize and segment objects and (2) when labeled training data is scarce, supervised pre-training for an auxiliary task, followed by domain-specific fine-tuning, yields a significant performance boost. Since we combine region proposals with CNNs, we call our method R-CNN: Regions with CNN features. We also compare R-CNN to OverFeat, a recently proposed sliding-window detector based on a similar CNN architecture. We find that R-CNN outperforms OverFeat by a large margin on the 200-class ILSVRC2013 detection dataset. Source code for the complete system is available at http://www.cs.berkeley.edu/~rbg/rcnn.
```
# Details
![[Pasted image 20231106181423.png]]

## Background
- To what extent do the CNN classification results on ImageNet generalize to object detection results on the PASCAL VOC Challenge? We answer this question by bridging the gap between image classification and object detection.
- One approach frames localization as a regression problem (as in paper Deep Neural Networks for Object Detection).
- An alternative is to build a sliding-window detector.
	- Problem: units high up in our network, if a network has five convolutional layers, have very large receptive fields (195 × 195 pixels) and strides (32×32 pixels) in the input image, which makes precise localization within the sliding-window paradigm an open technical challenge
	- Object features such as aspect ratio and shape vary significantly based on the angle at which image is taken. The sliding window approach is computationally very expensive when we search for multiple aspect ratios.
- 
## Solution
- At test time, our method generates around 2000 category-independent region proposals for the input image, extracts a fixed-length feature vector from each proposal using a CNN, and then classifies each region with category-specific linear SVMs. We use a simple technique (affine image warping) to compute a fixed-size CNN input from each region proposal, regardless of the region’s shape
	- Our system is also quite efficient. The only class-specific computations are a reasonably small matrix-vector product and greedy non-maximum suppression.
	- Region proposal: we use selective search to enable a controlled comparison with prior detection work
- CNN backbone:
    - Features are computed by forward propagating a mean-subtracted 227 × 227 RGB image through five convolutional layers and two fully connected layers.
- Regardless of the size or aspect ratio of the candidate region, we warp all pixels in a tight bounding box around it to the required size.
- Prior to warping, we dilate the tight bounding box so that at the warped size there are exactly p pixels of warped image context around the original box (we use p = 16).
	- 


# Comments and Implications

# Links
[@girshickRichFeatureHierarchies2014]