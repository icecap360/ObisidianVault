---
tags:
  - "#RawInformation"
  - Article
topics: "[[Deep Learning]]"
completed: false
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
![[Pasted image 20231106123009.png]]
- Newtonian Approach
	- - A gif is shown showing the true shape of the loss function given velocity/angle parameters derived from physics equations
	- True loss function has the same trend but is not identical to the loss function after part one of the neural network training (where we have trained a network to predict distance from velocity/angle).
	- The true loss function, based on physics, is smoother, has robust descent, and fewer GD iterations. No ill-conditioning,
# 3-Sentence Summary



# Details
- 

# Reference

