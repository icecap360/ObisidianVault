---
tags:
  - "#Ideas"
  - "#Incomplete"
  - "#Software"
topics:
---
-  Moving objects and tensors to Cuda devices
	- .cuda( n ) - `n` is the index of the GPU. If no parameter then the tensor is places on GPU 0. You can change the default `n` by calling 
	  `torch.cuda.set_device(0) # or 1,2,3`
	- .to(device)  - .to on a tensor returns another tensor, but .to on a model moves the model (returns None)
- Memory Management
	- Tensors may not be freed even once you are out of the trainiing loop, e.g.
	`for x in range(10): i = x print(i)`

	- Pytorch does have strong garbage collection
	- python doesn’t enforce scoping rules, so a variable is freed when no pointers to it exist
	- 
- OOM Errors Library - GPUtil
	- Use GPUtil library
	- Place the `GPUtil.showUtilization()`  at different places in the code to figure out exactly what is causing the network to go OOM
## Multiple GPUs
### Data Parallelism
- where we divide batches into smaller batches, and process these smaller batches in parallel on multiple GPU.
- `parallel_net = nn.DataParallel(myNet, gpu_ids = [0,1,2])`
- You can simply execute the nn.DataParallel object just like a nn.Module .
- Despite the fact our data has to be parallelised over multiple GPUs, we have to **initially** store it on a **single GPU**. We also need to make sure the DataParallel object is on that particular GPU as well. The syntax remains similar to what we did earlier with nn.Module.
![[Untitled.png]]
- One **issue** with DataParallel can be that it can put asymmetrical load on one GPU (the main node). There are generally two ways to circumvent these problem.
	- First, is to compute the loss during the forward pass. This makes sure at least the loss calculation phase is parallelised.
	- Another way is to implement a parallel loss function layer.

### Model Parallelism
- where we break the neural network into smaller sub networks and then execute these sub networks on different GPUs.
- No real benefit in speed (as too much interGPU dependence), but it does allow you to train much larger models, models that are too big to put on one gpu
- Implementing Model parallelism is PyTorch is pretty easy as long as you remember 2 things.
	- The input and the network should always be on the same device.
	- `to` and `cuda` functions have autograd support, so your gradients can be copied from one GPU to another during backward pass.

```
class model_parallel(nn.Module):
	def __init__(self):
		super().__init__()
		self.sub_network1 = ...
		self.sub_network2 = ...

		self.sub_network1.cuda(0)
		self.sub_network2.cuda(1)

	def forward(x):
		x = x.cuda(0)
		x = self.sub_network1(x)
		x = x.cuda(1)
		x = self.sub_network2(x)
		return x
```
Notice in the forward function, we transfer the intermediate output from sub_network1 to GPU 1 before feeding it to sub_network2. Since cuda has autograd support, the loss backpropagated from sub_network2 will be copied to buffers of sub_network1 for further backpropagation.

# Syntax

# Examples

# Comments and Links
- 
# Reference