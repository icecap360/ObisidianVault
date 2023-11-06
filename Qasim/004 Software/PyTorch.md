- *torch.nn* namespace provides all the building blocks you need to build your own neural network
- Every module in PyTorch subclasses the [[PyTorch - nn.Module]].
- Printing Information About a network  [[PyTorch - nn.Module]]:
	-  `named_parameters`
	- `named_modules`
	- `named_children`
	- `named_buffers` Return buffer tensors such as running mean average of a Batch Norm layer.
- GPUs
	- Moving objects and tensors to Cuda devices
	- .cuda( n ) - `n` is the index of the GPU. If no parameter then the tensor is places on GPU 0.
	- .to(device)  - .to on a tensor returns another tensor, but .to on a model moves the model (returns None)
	- 

# References
- Series of 5 Paperspace blogs are excellent linked on one page [https://blog.paperspace.com/pytorch-101-understanding-graphs-and-automatic-differentiation/](https://blog.paperspace.com/pytorch-101-understanding-graphs-and-automatic-differentiation/)