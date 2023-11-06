---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Unread"
completed: false
title: Understanding the difficulty of training deep feedforward neural networks
authors: Xavier Glorot, Yoshua Bengio
year: 2010/03
url: https://proceedings.mlr.press/v9/glorot10a.html
citekey: glorotUnderstandingDifficultyTraining2010
aliases:
  - glorotUnderstandingDifficultyTraining2010
  - Xavier
  - Glorot
---

# Short Summary
* Glorot Initialization
![[Pasted image 20231106144518.png]]

## Key Points

# Abstract
```
Whereas before 2006 it appears that deep multi-layer neural networks were not successfully trained, since then several algorithms have been shown to successfully train them, with experimental results showing the superiority of deeper vs less deep architectures. All these experimental results were obtained with new initialization or training mechanisms. Our objective here is to understand better why standard gradient descent from random initialization is doing so poorly with deep neural networks, to better understand these recent relative successes and help design better algorithms in the future.  We first observe the influence of the non-linear activations functions. We find that the logistic sigmoid activation is unsuited for deep networks with random initialization because of its mean value, which can drive especially the top hidden layer into saturation. Surprisingly, we find that saturated units can move out of saturation by themselves, albeit slowly, and explaining the plateaus sometimes seen when training neural networks. We find that a new non-linearity that saturates less can often be beneficial. Finally, we study how activations and gradients vary across layers and during training, with the idea that training may be more difficult when the singular values of the Jacobian associated with each layer are far from 1.  Based on these considerations, we propose a new initialization scheme that brings substantially faster convergence.
```
# Details
## Sigmoid sucks
* By having a mean of 0.5, it subjects itself to be difficult to optmize. The sigmoid non-linearity has been already shown to slow down learning because of its non-zero mean that induces important singular values in the Hessian
* Activations seem to saturate:
  ![[Pasted image 20231106141756.png]]
- The model initially finds the biases more useful, cause they are easier to learn, so it pushes the activations to 0. Pushing the activations to 0 makes the gradient 0, so the model gets stuck in this local optimum, so it takes a long time to get out of this.
	- Note that deep networks with sigmoids but initialized from unsuper-vised pre-training (e.g. from RBMs) do not suffer from this saturation behavior. Our proposed explanation rests on the hypothesis that the transformation that the lower layer of the randomly initialized network computes initially is not useful to the classification task, unlike the transformation obtained from unsupervised pre-training. The logistic layer output softmax(b + W h) might initially rely more on its biases b (which are learned very quickly) than on the top hidden activations h derived from the input image (because h would vary in ways that are not predictive of y, maybe correlated mostly with other and possibly more dominant variations of x). Thus the error gradient would tend to push W, h towards 0, which can be achieved by pushing h towards 0.
	- Pushing the sigmoid outputs to 0 would bring them into a saturation regime which would prevent gradients to flow backward and prevent the lower layers from learning useful features. Eventually but slowly, the lower layers move toward more useful features and the top hidden layer then moves out of the saturation regime.
## Solution
- From a forward-propagation point of view, to keep infor- mation flowing we would like that variance of the activations to be the same in all layers.
  ![[Pasted image 20231106143915.png]]
- Similarly, From a backward propagation view:
![[Pasted image 20231106143933.png]]
- Bradley (2009) found that back-propagated gradients were smaller as one moves from the output layer towards the input layer, just after initialization. the variance of the back-propagated gradients decreases as we go backwards in the network.
- Equations: 
	- Consider the hypothesis that we are in a linear regime at the initialization, that the weights are initialized independently and That the inputs features variances are the same (= Var\[x\]). using symmetric activation function f with unit derivative at 0 (i.e. f ′(0) = 1),
![[Pasted image 20231106144154.png]]
![[Pasted image 20231106144217.png]]
	- Thus, given what we want in terms of forward and backpropagation, we get,
![[Pasted image 20231106144307.png]]
Or as a compromise,
![[Pasted image 20231106144402.png]]
- Uniform initialization (as shown in equation 1) the following property when all layers are of same size (z). It is bad because variance decreases as n increases
![[Pasted image 20231106144441.png]]
![[Pasted image 20231106144450.png]]
Normalized the initialization
![[Pasted image 20231106144518.png]]


# Comments and Implications

# Links
[@glorotUnderstandingDifficultyTraining2010]