---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Unread"
completed: false
title: Paper - End-to-End Object Detection with Transformers
authors: "Nicolas Carion, Francisco Massa, Gabriel Synnaeve, Nicolas Usunier, Alexander Kirillov, Sergey Zagoruyko"
year: '2020/05'
url: "http://arxiv.org/abs/2005.12872"
citekey: "carionEndtoEndObjectDetection2020"
aliases:
  - ""
  - "carionEndtoEndObjectDetection2020"
---

# Short Summary

## Key Points

# Abstract
```
We present a new method that views object detection as a direct set prediction problem. Our approach streamlines the detection pipeline, effectively removing the need for many hand-designed components like a non-maximum suppression procedure or anchor generation that explicitly encode our prior knowledge about the task. The main ingredients of the new framework, called DEtection TRansformer or DETR, are a set-based global loss that forces unique predictions via bipartite matching, and a transformer encoder-decoder architecture. Given a fixed small set of learned object queries, DETR reasons about the relations of the objects and the global image context to directly output the final set of predictions in parallel. The new model is conceptually simple and does not require a specialized library, unlike many other modern detectors. DETR demonstrates accuracy and run-time performance on par with the well-established and highly-optimized Faster RCNN baseline on the challenging COCO object detection dataset. Moreover, DETR can be easily generalized to produce panoptic segmentation in a unified manner. We show that it significantly outperforms competitive baselines. Training code and pretrained models are available at https://github.com/facebookresearch/detr.
```
# Details
## Bi-partite Loss
- The problem of directly predicting sets:
    - In order for the model to make non-duplicate predictions, it needs to be aware of the other predictions it has made. Two solutions come to mind
        - Auto-regressive sequence models, where the decoder uses its previous prediction to make a new prediction
        - An alternative is fully connected models. These are computationally expensive, and can only be used for constant size set prediction
        - In this paper they use transformers
    - In all cases, the loss-function should be invariant to a permutation of the predictions
        - e.g. hungarian, a bipartite matching between ground-truth and prediction.
        - They use a bipartite loss approach
- Object detection set prediction loss
    - The output will be a set of N predictions, where N>> typical # of objects
    - “Our loss produces an optimal bipartite matching between predicted and ground truth objects, and then optimize object-specific (bounding box) losses.
    - y_hat denotes the set of N predictions ![[Pasted image 20231106195644.png]]
- We can consider y as a set composed of a series of objects, and padded with no objects. (cause N >> typical # of objects)
- Finding the optimal assignment of predictions to y can be done with the hungarian:
![[Pasted image 20231106195739.png]]
* Definition of $L_{match}$ and notation for predictions,: notice how they don’t use log probabilities for the class predictions so that the bounding box loss is comparable to the class loss:
  ![[Pasted image 20231106195815.png]]
* The key difference with anchors and match proposals is that the the authors find a one-to-one matching for direct set prediction without duplicates
* The Hungarian loss function is used for the matching found in f the previous step (done for each pair)
![[Pasted image 20231106195837.png]]
- In practice, we down-weight the log-probability term when ci = ∅ by a factor 10 to account for class imbalances
- Bounding box loss: L1 loss will have different scales for small and large boxes even if the relative errors are similar, so they use a linear combination of l1 with generalized IOU loss

## Architecture
- - CNN Backbone creates C=2048 and H,W=H0/32,W0/32
- 1x1 convolution to decrease channel size to d
- flatten each channel to 1 dimension, to get dxHW
- Feed into transformer encoder
- The difference with the original transformer is that our model decodes the N objects in parallel at each decoder layer, while Vaswani et al. \[47] use an autoregressive model that predicts the output sequence one element at a time.
- They use positional embedding, to encode that where a feature occurred spatially (within a given channel). However their positional embedding also satisfy translational equivariance, (unique contribution)
    - Solution: Relative positional embeddings, which encode relative position rather then absolute position, see the following for more [Attention Augmented Convolutional Networks]
- Since the decoder is also permutation-invariant, the N input embeddings (or object queries) from the encoder must be different to produce different results.
- A feed forward network decodes the N decoder embeddings into a N boxes and classes
    - Using self- and encoder-decoder attention over these embeddings, the model globally reasons about all objects together using pair-wise relations between them, while being able to use the whole image as context.
    - The FFN predicts the normalized center coordinates, height and width of the box w.r.t. the input image, and the linear layer predicts the class label using a softmax function
- They found it helpful to add auxiliary loss so the model outputs the correct number of objects in each class.
    - “We add prediction FFNs and Hungarian loss after each decoder layer. All predictions FFNs share their parameters. We use an additional shared layer-norm to normalize the input to the prediction FFNs from different decoder layers.”
		

# Comments and Implications

# Links
[@carionEndtoEndObjectDetection2020]