---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Unread"
completed: false
title: Paper - Deep Neural Networks for Object Detection
authors: "Christian Szegedy, Alexander Toshev, Dumitru Erhan"
year: '2013/01'
url: "https://proceedings.neurips.cc/paper_files/paper/2013/hash/f7cade80b7cc92b991cf4d2806d6bd78-Abstract.html"
citekey: "szegedyDeepNeuralNetworks2013"
aliases:
  - ""
  - "szegedyDeepNeuralNetworks2013"
---

# Short Summary

## Key Points

# Abstract
```
Deep Neural Networks (DNNs) have recently shown outstanding performance on the task of whole image classification. In this paper we go one step further and address the problem of object detection -- not only classifying but also precisely localizing objects of various classes using DNNs. We present a simple and yet powerful formulation of object detection as a regression to object masks. We define a multi-scale inference procedure which is able to produce a high-resolution object detection at a low cost by a few network applications. The approach achieves state-of-the-art performance on Pascal 2007 VOC.
```
# Details
![[Pasted image 20231106180105.png]]

- Model predicts a floating point mask of size=N=dxd =e.g. 24x24 . It should have value 1 at particular pixel if this pixel lies within the bounding box of an object of a given class and 0 otherwise
- $m$ is the ground truth mask in the loss function below
![[Pasted image 20231106180207.png]]
- Regularizing loss: Most of the objects are small relative to the image size and the network can be easily trapped by the trivial solution of assigning a zero value to every output.
	- To avoid this undesirable behavior, it is helpful to increase the weight of the outputs corresponding to non-zero values in the ground truth mask by a parameter λ ∈ R+. If λ is chosen small, then the errors on the output with ground truth value 0 are penalized significantly less than those with 1 and therefore encouraging the network to predict nonzero values even if the signals are weak.
- To deal with multiple touching objects, we generate not one but several masks, each representing either the full object or part of it. Since our end goal is to produce a bounding box, we use one network to predict the object box mask and four additional networks to predict four halves of the box: bottom, top, left and right halves, thus these 5 predictions overparamaterize the system. if two objects of the same type are placed next to each other, then at least two of the produced five masks would not have the objects merged.
    - How exactly we project ground truth boxes of higher dimension (i.e. on 512x512 input image) onto smaller dxd outputs of the model is a little complicated, but basically we assign value of IOU(ground truth bbox in 512x512 coordinates, bbox of output mask in inputs coordinates)
- 

# Comments and Implications

# Links
[@szegedyDeepNeuralNetworks2013]