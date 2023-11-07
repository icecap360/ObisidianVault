---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Unread"
completed: false
title: "Deep Residual Learning for Image Recognition"
authors: "Kaiming He, Xiangyu Zhang, Shaoqing Ren, Jian Sun"
year: '2016/06'
url: "http://ieeexplore.ieee.org/document/7780459/"
citekey: "heDeepResidualLearning2016"
aliases:
  - ""
  - "heDeepResidualLearning2016"
---

# Short Summary

## Key Points

# Abstract
```
Deeper neural networks are more difﬁcult to train. We present a residual learning framework to ease the training of networks that are substantially deeper than those used previously. We explicitly reformulate the layers as learning residual functions with reference to the layer inputs, instead of learning unreferenced functions. We provide comprehensive empirical evidence showing that these residual networks are easier to optimize, and can gain accuracy from considerably increased depth. On the ImageNet dataset we evaluate residual nets with a depth of up to 152 layers—8× deeper than VGG nets [41] but still having lower complexity. An ensemble of these residual nets achieves 3.57% error on the ImageNet test set. This result won the 1st place on the ILSVRC 2015 classiﬁcation task. We also present analysis on CIFAR-10 with 100 and 1000 layers.
```
# Details
Deep Neural networks were struggling in 2015.

E.g. if you take a deep network which is composed of few normal layers + later layers being identity mapping, the deep network should perform atleast as good as the smaller network (cause it can just put the latter layers as identity mappings). But this doesn’t always happen

e.g. The degredation problem: if you trained deep networks for long enough, they were known to eventually have their training accuracy increase randomly

Idea: instead of forcing the later layers to fit a desired underlying mapping, let these later layers fit a residual mapping

H(x): desired underlying mapping

Fit the later layers on F(x)=H(x)-x, and the original mapping H(x) is recast to F(x)+x

It is easier for a stack of nonlinear layers to push a residual to zero than to fit an identity mapping

- “We hypothesize that it is easier to optimize the residual mapping than to optimize the original, unreferenced mapping. To the extreme, if an identity mapping were optimal, it would be easier to push the residual to zero than to fit an identity mapping by a stack of nonlinear layers.”

F(x)+x can be realized by shortcut connections (see figure 2),

The shortcut connections simply perform identity mapping,
![[Pasted image 20231106204713.png]]
Prior work, highway networks, perform shortcut connections but their gates are data-dependent and have parameters, while in resnet the shortcut connection is never closed, it always allows 100% of the information to flow

Notice that you are solving the same problem, you are just reorganizing it so that it is easier for the model to solve
- ““If the optimal function is closer to an identity mapping than to a zero mapping, it should be easier for the solver to find the perturbations with reference to an identity mapping, than to learn the function as a new one.”
- With the residual learning re- formulation, if identity mappings are optimal, the solvers may simply drive the weights of the multiple nonlinear layers toward zero to approach identity mappings
- Biases are ignored in this paper for simplicity, by a residual applieed to a stack of a few layers
![[Pasted image 20231106204758.png]]
- Linear projection when F and x are not the same dimension
![[Pasted image 20231106204821.png]]
- The also show that using an identity mapping is superior to learning Ws
- The residual block should be used with 2+ layer blocks, as one layer skip connection is not difficult for the model to model
# Comments and Implications

# Links
[@heDeepResidualLearning2016]