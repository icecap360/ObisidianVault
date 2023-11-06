---
tags:
  - "#RawInformation"
  - "#Paper"
completed: true
title: "Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification"
authors: Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun
year: 2015/02
url: http://arxiv.org/abs/1502.01852
citekey: heDelvingDeepRectifiers2015
aliases:
  - Delving Deep into Rectifiers
  - heDelvingDeepRectifiers2015
---

# Short Summary

## Key Points

# Abstract
```
Rectified activation units (rectifiers) are essential for state-of-the-art neural networks. In this work, we study rectifier neural networks for image classification from two aspects. First, we propose a Parametric Rectified Linear Unit (PReLU) that generalizes the traditional rectified unit. PReLU improves model fitting with nearly zero extra computational cost and little overfitting risk. Second, we derive a robust initialization method that particularly considers the rectifier nonlinearities. This method enables us to train extremely deep rectified models directly from scratch and to investigate deeper or wider network architectures. Based on our PReLU networks (PReLU-nets), we achieve 4.94% top-5 test error on the ImageNet 2012 classification dataset. This is a 26% relative improvement over the ILSVRC 2014 winner (GoogLeNet, 6.66%). To our knowledge, our result is the first to surpass human-level performance (5.1%, Russakovsky et al.) on this visual recognition challenge.
```
# Details
- Recent deep CNNs are mostly initialized by random weights drawn from Gaussian distributions \[16]. With fixe standard deviations (e.g., 0.01 in \[16]), very deep model (e.g., >8 conv layers) have difficulties to converge, as reported by the VGG team \[25] and also observed in our experiments. To address this issue, in \[25] they pre-train a model with 8 conv layers to initialize deeper models. But this strategy requires more training time, and may also lead to a poorer local optimum. In \[29, 18], auxiliary classifiers are added to intermediate layers to help with convergence.
- Glorot and Bengio \[7] proposed to adopt a properly scaled uniform distribution for initialization. This is called “Xavier” initialization in \[14]. Its derivation is based on the assumption that the activations are linear. This assumption is invalid for ReLU and PReLU
- A proper initialization method should avoid reducing or magnifying the magnitudes of input signals exponentially. So we expect the above product to take a proper scalar (e.g., 1).
- Ignoring the derivation,
![[Pasted image 20231106145448.png]]
a sufficient condition is either of the following,
![[Pasted image 20231106145510.png]]

- Such an initializaion assumes several things, but ignore it
	- If we let w_l−1 have a symmetric distribution around zero and bl−1 = 0, then yl_−1 has zero mean and has a symmetric distribution around zero
	- It is worth noticing that E[$(x_l)^2$] ≠ Var[xl] unless x_l has zero mean. For the ReLU activation, xl = max(0, yl−1) and thus it does not have zero mean
	- You can also use the following equation
	- IN actuality, the range for uniform sampling is different in each layer
	- The following is a passage detailing the comparison with Xavier
![[Pasted image 20231106145611.png]]

# Comments and Implications

# Links
[@heDelvingDeepRectifiers2015]