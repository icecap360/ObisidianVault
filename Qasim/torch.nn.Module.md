---
tags:
  - "#Ideas"
  - "#Incomplete"
  - "#Software"
topics:
---
- Every module in PyTorch subclasses the torch.nn.Module.

# Syntax
- .training - variable
- add_module(name, module) - add child module
- apply(fn) - applies fn recursively to every submodule
- children() - returns an iterator over immediate children modules
- compile() - computes the forward
- .cpu()
- .cuda()
- double()
- .eval() - sets thte module in evaluation mode
	- This is equivalent with self.train(False).
	- Effects certain modules, e.g. Dropout, BatchNorm, etc.
- get_parameter(_target_)
	- 


# Examples
```
@torch.no_grad()
>>> def init_weights(m):
>>>     print(m)
>>>     if type(m) == nn.Linear:
>>>         m.weight.fill_(1.0)
>>>         print(m.weight)
>>> net = nn.Sequential(nn.Linear(2, 2), nn.Linear(2, 2))
>>> net.apply(init_weights)
```

# Comments and Links
- 
# Reference