---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Incomplete"
completed: false
title: Fast R-CNN
authors: Ross Girshick
year: 2015/09
url: http://arxiv.org/abs/1504.08083
citekey: girshickFastRCNN2015
aliases:
  - girshickFastRCNN2015
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
	- Features are also written to disk. But unlike R-CNN, the fine-tuning algorithm proposed in SPPNet cannot update the convolutional layers that precede the spatial pyramid pooling. Unsurprisingly, this limitation (fixed convolutional layers) limits the accuracy of very deep networks
## Methodology
- We propose a single-stage training algorithm that jointly learns to classify object proposals and refine their spatial locations
	1. Higher detection quality (mAP) than R-CNN, SPPnet 
	2. Training is single-stage, using a multi-task loss 
	3. Training can update all network layers 
	4. No disk storage is required for feature caching
### Architecture
![[Pasted image 20231030192023.png]]
- Input: image and object proposals
1. Process the image using a Convnet
2. For each proposal, a RoI pooling layer extracts a fixed-length feature vector from the feature map
3. Each feature vector is fed into a sequence of FC layers, that finally branch into 2 sibling output layers: 
	- Cls: one that produces softmax probability estimates over K object classes plus a catch-all “background” class 
	- BBox Reg: another layer that outputs four real-valued numbers for each of the K object classes
### ROI Pooling
- The RoI pooling layer uses max pooling to convert the features inside any valid region of interest into a small feature map
	- The feature maps has a fixed spatial extent of H × W (e.g., 7 × 7), where H and W are layer hyper-parameters that are independent of any particular RoI
- RoI: 
	- a rectangular window within a convolutional feature map
	- Each RoI is defined by a four-tuple *(r, c, h, w)* that specifies its top-left corner *(r, c)* and its height and width *(h, w)*
- RoI max pooling:
	- Works by dividing the *h × w* RoI window into an *H × W* grid of sub-windows of approximate size *h/H × w/W* and then max-pooling the values in each sub-window into the corresponding output grid cell
	- Pooling is applied independently to each feature map channel, as in standard max pooling.
	- The RoI layer is simply the special-case of the spatial pyramid pooling layer used in SPPnets in which there is only one pyramid level. We use the pooling sub-window calculation given in SPPnet.


### Fine-tuning for detection
#### Adapting a pre-trained ImageNet network
* Use 2 data inputs: RoI and raw Image
* Replace the last FC layer with a class + bbox_reg head
* Replace last max pooling layer with an RoI pooling layer, make sure to pick H and W to be compatible with the net's first FC layer

- SPPNet is unable to update weights below the spatial pyramid pooling layer

Problem
- Back-prop thourgh SPP is inefficient when each training sample (i.e. RoI) comes from a different image, which is how R-CNN and SPPnet are trained
	- Inefficiency comes from the fact that each RoI may have a very large receptive field, comparable to the entire input image.
	- Thus forward pass must process the entire receptive field
Solution
- Take advantage of feature sharing during training
- SGD mini-batches are sampled hierarchically, first by sampling N images, then by sampling *R/N* RoIs from each image. Critically, RoIs from the same image share computation and memory in the forward and backward passes.
- For example, when using N = 2 and R = 128, the proposed training scheme is roughly 64× faster than sampling one RoI from 128 different images (i.e., the R-CNN and SPPnet strategy)
- One concern over this strategy is it may cause slow training convergence because RoIs from the same image are correlated. This concern does not appear to be a practical issue and we achieve good results with N = 2 and R = 128 using fewer SGD iterations than R-CNN.

#### Multi-task loss
- Classification head: discrete probability distribution (per RoI), *p = ($p_0$, . . . , $p_K$)*, over *K + 1* categories
- Regression head: offsets, *$t_k$ = ($t_k^x$ , $t_k^y$ , $t_k^w$, $t_k^h$)* for each of the *K* object classes. $t_k$ specifies a scale-invariant translation and log-space height/width shift relative to an object proposal
- Each training RoI is labeled with a ground-truth class *u* and a ground-truth bounding-box regression target *v*
![[Pasted image 20231030200658.png]]
![[Pasted image 20231030200916.png]]
![[Pasted image 20231030201149.png]]
- *L2* loss used in R-CNN and SPPnet, is bad because. When the regression targets are unbounded (large error), training with *L2* loss can require careful tuning of learning rates in order to prevent exploding gradients. 
- In this paper, they use $\lambda = 1$ (equal weight to both tasks)


## Results
- 9x faster then VGG16
- 9x faster then SPPnet
- top accuracy on PASCAL VOC 2012
- 
# Comments and Implications

# Links
[@girshickFastRCNN2015]