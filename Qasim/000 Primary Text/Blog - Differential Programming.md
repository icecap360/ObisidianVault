---
tags:
  - "#RawInformation"
  - Article
topics: "[[Topics to Study Deep Learning]]"
completed: true
url: "{{https://www.assemblyai.com/blog/differentiable-programming-a-simple-introduction/#:~:text=Deep%20Learning%20builds%20on%20top,requirements%20%2D%20training%20via%20automatic%20differentiation.}}"
year: "{{2022}}"
---
- **Differentiable Programming** is a relatively new term that is often conflated with Deep Learning. While Deep Learning indeed overlaps with Differentiable Programming, Deep Learning is a _subset_ of Differentiable Programming.
- Requires only three things:
	- 1. A **parameterized function** / method / model to be optimized
	2. A **loss** that is suitable to measure performance, and
	3. (Automatic) **differentiability** of the object to be optimized
- Many Machine Learning techniques at their core boil down to minimizing some loss function to learn a model that is well-suited to solve some problem. 
	- Deep Learning builds on top of this central idea by imposing two **requirements** on the model itself - a **network architecture** and training via **automatic differentiation.** 
	- In contrast, **Differentiable Programming** demands only _one_ of these requirements - training via automatic differentiation.

### Example
- **Question**:  Given a target at a known distance, how can we adjust a cannon's angle and ejection velocity in order to hit the target?
- With neural networks, we have a complicated two step training process to learn the mapping from control params (theta and velocity_init) to distance, then learn the inputs/parameters that give us the desired distance in the simulator. 
	- Note that we cannot predict control parameters directly because we don't know a good loss function between estimated control parameters and the ground-truth best control parameters.
	- Given  NN that approximates the mapping from control parameters to resulting distance, how are we to get suitable control parameters for a given target distance
		- Once we have a loss surface which is a function of the control parameters and whose shape is parameterized by our target distance; we can simply use **gradient descent**. While we first used gradient descent in order to _learn_ an approximation of the control parameters to distances mapping, we are now using gradient descent to _minimize_ the corresponding loss surface of this mapping as parameterized by our input target distance.
	- Using this  pproach, the model's control parameters can be way off, (error is technically unbounded), this is because error between approximate and tru model is unbounded. 
	- To bound the errors: In order to bound the error, we would need to input a target, use gradient descent (on the fixed neural model) to learn control parameters, use these control parameters to run experiments and collect the _true_ resulting distance, and then compare this distance to the input target distance, performing some statistical analysis over large amounts of data.
![[Pasted image 20231106123009.png]]
- Newtonian Approach - use physics
	- Use the loss function created from physics equations, then use gradient descent to find the optimal control params
	- - A gif is shown showing the true shape of the loss function given velocity/angle parameters derived from physics equations, overlayyed with our loss function from the previous approach
	- You'll notice that, despite the fact that the surfaces trend the same way, the **parabola which defines the minimum curve in the real model** (blue) **is not identical to the minimum curve of the neural model** (green). That is, even if we properly minimize on the empirical loss surface, the resulting control parameters may not lie on the true solution curve.
	- True loss function has the same trend but is not identical to the loss function of our previous approach after part one of the neural network training (where we have trained a network to predict distance from velocity/angle).
	- The true loss function, based on physics, is smoother, has robust descent, and fewer GD iterations. No ill-conditioning,
- Differentiable Programming Approach (Combine the approaches)
    - Remember this whole issue started because we were unable to measure loss on the parameters alone
![[Pasted image 20231106125359.png]]
	- use the physical model to generate a sensible loss function.
	- The result is effectively an autoencoding network in which the neural network learns the "inverse" of the physical model which maps from control parameters to resulting distances:
![[Pasted image 20231106125509.png]]
	- Since our physical model constitutes a composition of differentiable functions, we can backpropagate through the network and update the parameters of the neural network to learn.


# 3-Sentence Summary



# Details
- 

# Reference

