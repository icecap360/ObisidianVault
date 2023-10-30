---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Unread"
completed: false
title: "Fast R-CNN"
authors: "Ross Girshick"
year: '2015/09'
url: "http://arxiv.org/abs/1504.08083"
citekey: "girshickFastRCNN2015"
aliases:
  - ""
  - "girshickFastRCNN2015"
---

# Short Summary

## Key Points

# Abstract
```
This paper proposes a Fast Region-based Convolutional Network method (Fast R-CNN) for object detection. Fast R-CNN builds on previous work to efficiently classify object proposals using deep convolutional networks. Compared to previous work, Fast R-CNN employs several innovations to improve training and testing speed while also increasing detection accuracy. Fast R-CNN trains the very deep VGG16 network 9x faster than R-CNN, is 213x faster at test-time, and achieves a higher mAP on PASCAL VOC 2012. Compared to SPPnet, Fast R-CNN trains VGG16 3x faster, tests 10x faster, and is more accurate. Fast R-CNN is implemented in Python and C++ (using Caffe) and is available under the open-source MIT License at https://github.com/rbgirshick/fast-rcnn.
```
# Details
## Problem
- Modern CNNs are slow and inelegent
	- (1) Numerous candidate object locations must be processed
	- (2) Candidates provide only rough localization that must be refined through precise localization
- Other solutions may resolve these issues at the cost of speed accuracy or simplicity
- R-CNN drawbacks:
	- Training is multi-stage : train convnets, SVM, and finally bounding box regressors
	- Expensive in compute and space: Convnet runs many times (number of proposals * number of images), and all of the extracted features need to be written to disk
	- 48 sec per image
## Related works
- Why is R-CNN slow?
	- Convnet does not share computation: 1 Convnet forward pass for each object proposal
- SPPNet tried to address
		- It runs CNN on whole image, it then extracts proposals from the shared feature space
		- Features are extracted for a proposal by maxpooling the portion of the feature map inside the proposal into a fixed-size output (e.g., 6 × 6). 
		- Multiple output sizes are pooled and then concatenated as in spatial pyramid pooling 
		- It reduces training time by 3x (cause of shared features), accelerate by 10-100x
- SPPNet drawbacks:
	- Training is a multi-stage pipeline extracting features, fine-tuning a network with log loss, training SVMs, and finally fitting bounding-box regressors. 
	- Features are also written to disk. But unlike R-CNN, the fine-tuning algorithm proposed in [11] cannot update the convolutional layers that precede the spatial pyramid pooling. Unsurprisingly, this limitation (fixed convolutional layers) limits the accuracy of very deep networks
## Methodology
- We propose a single-stage training algorithm that jointly learns to classify object proposals and refine their spatial locations
## Results
- 9x faster then VGG16
- 9x faster then SPPnet
- top accuracy on PASCAL VOC 2012
- 
# Comments and Implications

# Links
[@girshickFastRCNN2015]