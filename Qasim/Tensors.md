---
tags:
  - "#Ideas"
  - "#Incomplete"
  - "#Software"
topics: "[[PyTorch]]"
---
- Tensors are similar to [[Numpy Array]], except that tensors can run on GPUs or other hardware accelerators.
- A change in the tensor reflects in the NumPy array ONLY if both are on the CPU
- - **Don’t use inplace operations**
    - In-place operations save some memory, but can be problematic when computing derivatives because of an immediate loss of history. Hence, their use is discouraged.
- **By default, tensors are created on the CPU.**
    - Use .to() to move to a different device

# Syntax
- numpy()
- .shape, 
- .dtype, 
- .device
- .item()
    - converts tensor into numerical, only for tensors with 1 element
- .to
	- move to a defive
	- change dtype, e.g. b.to(torch.int32)
- Creation
	- torch.tensor()
	- from_numpy() 
	- rand_like()
	- ones_like()
	- zeros()
	- .empty() - allocates memory for the tensor, but does not initialize it with any values - so what you’re seeing is whatever was in memory at the time of allocation. 
	- dtype is a field in many of these methods
- Math
	- 

# Examples


# Comments and Links
- 
# Reference