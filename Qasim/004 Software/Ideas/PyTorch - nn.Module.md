---
tags:
  - "#Ideas"
  - "#Software"
topics: "[[PyTorch]]"
---
- Every module in PyTorch subclasses the torch.nn.Module.
- By subclassing nn.module, all parameters will be tracked
-  What's more, you may require different [[Weight Initialisation]] schemes for different sort of layers. This can be accomplished by the `modules` and `apply`  functions. `modules()` is a member function of nn.Module class which returns an iterator containing all the member `nn.Module` members objects of an `nn.Module` function. Then use the `apply` function can be called on each nn.Module to set it's initialisation.
# Syntax
- .training - variable
- add_module(name, module) - add child module
- **apply**(fn) - applies fn recursively to every submodule
- compile() - computes the forward
- .cpu()
- .cuda( n ) - `n` is the index of the GPU. If no parameter then the tensor is places on GPU 0.
- double()
- .eval() - sets thte module in evaluation mode
	- This is equivalent with self.train(False).
	- Effects certain modules, e.g. Dropout, BatchNorm, etc.
- .train() - sets training mode
- .zero_grad() - resets gradients
- Parameters
	- **get_parameter**(_target_) - Returns the parameter given by target if it exists, otherwise throws an error.
	- register_parameter() - adds parameter
- Iterator
	- children() - returns an iterator over immediate children modules. 
	- **modules**() - iterator of all modules in a netowrk, duplicates are returned once 
	- named_modules() - Returns an iterator which gives a tuple containing name of the parameters (if a convolutional layer is assigned as `self.conv1`, then it's parameters would be `conv1.weight` and `conv1.bias`) and the value returned by the `__repr__` function of the `nn.Parameter`
	- named_children() - returns iterator of immediate children
	- named_parameters() - returns iterator over module parameters, yielding both name of pareter and parameter itself
	- parameters() - returns iterator over parameters
	- named_buffers() - Return buffer tensors such as running mean average of a Batch Norm layer
	
	- 
- Hooks
	- register_full_backward_hook()
		- Modifying inputs or outputs inplace is not allowed when using backward hooks and will raise an error.
		- register_backward_hook is depreciated
	- register_forward_hook()
	- register_forward_pre_hook()
	- register_full_backward_pre_hook()
		- Modifying inputs inplace is not allowed when using backward hooks and will raise an error.
- state_dict: Maps nn.Parameter objects to values
	- state_dict() - gets state dict. 
	- load_state_dict(state_dict, strict=True, assign=False) - If strict is True, then the keys of state_dict must exactly match the keys returned by this module’s state_dict() function
		- assign (bool, optional) – whether to assign items in the state dictionary to their corresponding keys in the module instead of copying them inplace into the module’s current parameters and buffers. When False, the properties of the tensors in the current module are preserved while when True, the properties of the Tensors in the state dict are preserved. Default: False
		- If assign is True the optimizer must be created after the call to load_state_dict.
		- Returns
			- missing_keys is a list of str containing the missing keys
			- unexpected_keys is a list of str containing the unexpected keys


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