---
tags:
  - "#Ideas"
  - "#Incomplete"
  - "#Software"
topics: "[[numpy.Array]]"
---
The rules for broadcasting are:
- Each tensor must have at least one dimension - no empty tensors.
- Comparing the dimension sizes of the two tensors, _going from last to first:_
    - Each dimension must be equal, _or_
    - One of the dimensions must be of size 1, _or_
    - The dimension does not exist in one of the tensors


# Examples
Success, examples from [[PyTorch]]:
```
a =     torch.ones(4, 3, 2)
b = a * torch.rand(   3, 2) # 3rd & 2nd dims identical to a, dim 1 absent
c = a * torch.rand(   3, 1) # 3rd dim = 1, 2nd dim identical to a
d = a * torch.rand(   1, 2) # 3rd dim identical to a, 2nd dim = 1
```
Fails:
```
a =     torch.ones(4, 3, 2)
b = a * torch.rand(4, 3)    # dimensions must match last-to-first
c = a * torch.rand(   2, 3) # both 3rd & 2nd dims different
d = a * torch.rand((0, ))   # can't broadcast with an empty tensor
```


# Comments and Links
- 
# Reference