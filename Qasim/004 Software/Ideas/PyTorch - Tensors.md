---
tags:
  - "#Ideas"
  - "#Software"
topics: "[[PyTorch]]"
---
- Tensors are similar to [[Numpy - Array]], except that tensors can run on GPUs or other hardware accelerators.
- A change in the tensor reflects in the NumPy array ONLY if both are on the CPU
- - **Don’t use inplace operations**
    - In-place operations save some memory, but can be problematic when computing derivatives because of an immediate loss of history. Hence, their use is discouraged.
- **By default, tensors are created on the CPU.**
    - Use .to() to move to a different device

# Syntax
- .cuda( n ) - `n` is the index of the GPU. If no parameter then the tensor is places on GPU 0.
- .numpy()
- .shape, 
- .dtype, 
- .device
- grad_fn() - reference to mathematical operation that created the tensor
- is_leaf() - returns whether or not tensor is a leaf 
- .item()
    - converts tensor into numerical, only for tensors with 1 element
- .to()
	- move to a defive
	- change dtype, e.g. b.to(torch.int32)
- .clone()
	- .detatch().clone() - turn off gradient tracking before cloning
- Creation
	- torch.tensor()
	- torch.from_numpy() 
	- torch.rand_like()
	- torch.ones_like()
	- torch.zeros()
	- torch.empty() - allocates memory for the tensor, but does not initialize it with any values - so what you’re seeing is whatever was in memory at the time of allocation. 
	- dtype is a field in many of these methods
- Math
	- torch.ceil()
	- torch.abs()
	- torch.floor()
	- torch.clamp(a, min, max)
	- torch.sin()
	- torch.asin()
	- torch.max()
	- torch.mean()
	- torch.std()
	- torch.prod()
	- torch.cross()
	- torch.matmul()
	- torch.svd()
	- torch.unique() - filter unique elements
	- .add_() and .mul_() - inplace versions of add and multple

# Examples
```
if torch.cuda.is_available():
    my_device = torch.device('cuda')
else:
    my_device = torch.device('cpu')

x = torch.rand(2, 2, device=my_device)
y = torch.rand(2, 2)
y = y.to(my_device)
```

# Comments and Links
- 
# Reference