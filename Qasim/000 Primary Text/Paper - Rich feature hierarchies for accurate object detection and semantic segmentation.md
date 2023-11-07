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
- Region proposal:
	- We use selective search to enable a controlled comparison with prior detection work
- CNN backbone:
    - Features are computed by forward propagating a mean-subtracted 227 × 227 RGB image through five convolutional layers and two fully connected layers.
- Regardless of the size or aspect ratio of the candidate region, we warp all pixels in a tight bounding box around it to the required size.
	- Prior to warping, we **dilate** the tight bounding box so that at the warped size there are exactly p pixels of warped image **context** around the original box (we use p = 16).
![[Pasted image 20231106183102.png]]
- SVM: Then, for each class, we score each extracted feature vector using the SVM trained for that class.
- Given all scored regions in an image, we apply a greedy non-maximum suppression (for each class independently) that rejects a region if it has an intersection-overunion (IoU) overlap with a higher scoring selected region larger than a learned threshold.
- Domain-specific fine-tuning of the Imagenet pretrained CNN: 
	- To adapt our CNN to the new task (detection) and the new domain (warped proposal windows), we continue stochastic gradient descent (SGD) training of the CNN parameters using only warped region proposals. 
	- We treat all region proposals with ≥ 0.5 IoU overlap with a ground-truth box as positives for that box’s class and the rest as negatives.
	- In each SGD iteration, we uniformly sample 32 positive windows (over all classes) and 96 background windows to construct a mini-batch of size 128. 
	- We bias the sampling towards positive windows because they are extremely rare compared to background.
- Object category classifier (SVM)
    - Once features are extracted and training labels are applied, we optimize one linear SVM per class.
    - Consider training a binary classifier to detect cars. 
    - It’s clear that an image region tightly enclosing a car should be a positive example. 
    - Less clear is how to label a region that partially overlaps a car. We resolve this issue with an IoU overlap threshold, below which regions are defined as negatives. The overlap threshold, 0.3, was selected by a grid search over {0, 0.1, . . . , 0.5} on a validation set. We found that selecting this threshold carefully is important.
    - In Appendix B we discuss why the positive and negative examples are defined differently in fine-tuning versus SVM training. We also discuss the trade-offs involved in training detection SVMs rather than simply using the outputs from the final softmax layer of the fine-tuned CNN
- Bounding box regressor:
	- We use a simple bounding-box regression stage to improve localization performance. After scoring each selective search proposal with a class-specific detection SVM, we predict a new bounding box for the detection using a class-specific bounding-box regressor
	- P’s are proposals, and G are ground truth
![[Pasted image 20231106183852.png]]
		- Transform Proposal Predictions into into predictions G_hat
![[Pasted image 20231106183915.png]]
![[Pasted image 20231106183927.png]]
![[Pasted image 20231106183938.png]]
- W_star means ridge regression

![[Pasted image 20231106183951.png]]

- - Semantic segmentation
    - O2P uses CPMC to generate 150 region proposals per image and then predicts the quality of each region, for each class, using support vector regression (SVR).
    - CNN features for segmentation. We evaluate three strategies for computing features on CPMC regions, all of which begin by warping the rectangular window around the region to 227 × 227.
        - (full) ignores the region’s shape and computes CNN features directly on the warped window, exactly as we did for detection. However, these features ignore the non-rectangular shape of the region. Two regions might have very similar bounding boxes while having very little overlap.
        - the second strategy (fg) computes CNN features only on a region’s foreground mask. We replace the background with the mean input so that background regions are zero after mean subtraction.
        - The third strategy (full+fg) simply concatenates the full and fg features; our experiments validate their complementarity (PERFORMS BEST)
![[Pasted image 20231106184516.png]]

## Results
- We propose a simple (and complementary) non-parametric method that directly shows what the network learned
    - The idea is to single out a particular unit (feature) in the network and use it as if it were an object detector in its own right. That is, we compute the unit’s activations on a large set of held-out region proposals (about 10 million), sort the proposals from highest to lowest activation, perform nonmaximum suppression, and then display the top-scoring regions.
    - Our method lets the selected unit “speak for itself” by showing exactly which inputs it fires on. We avoid averaging in order to see different visual modes and gain insight into the invariances computed by the unit.
    - We visualize units from layer pool_5, which is the maxpooled output of the network’s fifth and final convolutional layer.
![[Pasted image 20231106184350.png]]
# Comments and Implications

# Links
[@girshickRichFeatureHierarchies2014]