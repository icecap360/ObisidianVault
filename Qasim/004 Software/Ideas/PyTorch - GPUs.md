---
tags:
  - "#Ideas"
  - "#Software"
topics: "[[PyTorch]]"
---
-  Moving objects and tensors to Cuda devices
	- .cuda( n ) - `n` is the index of the GPU. If no parameter then the tensor is places on GPU 0. You can change the default `n` by calling 
	  `torch.cuda.set_device(0) # or 1,2,3`
	- .to(device)  - .to on a tensor returns another tensor, but .to on a model moves the model (returns None)
## Memory Management
### Training Loop
- Tensors may not be freed even once you are out of the trainiing loop, e.g.
	```
	for x in range(10): 
		i = x 
	print(i) # 9 is printed
	```

- Pytorch does have strong garbage collection
- python doesn’t enforce scoping rules, so a variable is freed when no pointers to it exist
- In fact, as a **general rule** of thumb, if you are done with a tensor, you should del as it won't be garbage collected unless there is no reference to it left.
	- To free up space, use `del pred, loss`, after the training loop is complete 
- **In the training loop, use python datatypes**, not tensors. Example shown below
	- Why is `iter_loss` not freed in this example
```
total_loss = 0

for x in range(10):
  # assume loss is computed
  iter_loss = torch.randn(3,4).mean()
  iter_loss.requires_grad = True     # losses are supposed to differentiable
  total_loss += iter_loss            # use total_loss += iter_loss.item) instead
```
Everytime you add iter_loss, a computation graph with addbackward function node is added. A computational node cannot be freed unless backward is called on it, but there is no scope calling backward
Solution: We merely replace the line `total_loss += iter_loss` with `total_loss += iter_loss.item()`, as item() returns the python data type from a tensor containing single values.
### Emptying the cuda cache
- `torch.cuda.empty_cache()`
- Pytorch may not give memory back after using `del`
	- memory can become cached so that PyTorch can allocate future tensors without requesting the OS
	- this can be problematic if you are using more then 2 processes
### OOM Errors Library - GPUtil
- Use GPUtil library
- Place the `GPUtil.showUtilization()`  at different places in the code to figure out exactly what is causing the network to go OOM

## Importance of torch.no_grad()
- Computational graphs are created during the forward pass
- As variables are allocated, buffers for their gradients and intermediate values are also allocated
- These buffers are freed during backward pass, with the exception of those allocated for leaf variables
- If backward pass does not happen, the buffers aren't freed leading to memory pile
- Thus conduct inference with the torch.no_grad() context manager

## Using `cudnn` backend for benchmarking
- Provides optimisation especially when the input to your network is fixed size
```
torch.backends.cudnn.benchmark = True
torch.backends.cudnn.enabled = True
```

## Using 16 bit Floats
```
model = model.half()     # convert a model to 16-bit
input = input.half()     # convert a model to 16-bit
```
- Cuts GPU by almost half
- Drawbacks:
	- You can have overflow issues with 16-bit float. e.g. overflow while trying to store the Union area of two bounding boxes (for computation of IoUs)  in a float16.  So make sure you have a realistic bound on the value you are trying to save in a float16. 
	  
	  Nvidia has recently released a PyTorch extension called Apex, that facilitates numerically safe mixed precision training in PyTorch.
	- In PyTorch, batch-norm layers have convergence issues with half precision floats. If that's the case with you, make sure that batch norm layers are float32.
```
model.half()  # convert to half precision
for layer in model.modules():
  if isinstance(layer, nn.BatchNorm2d):
    layer.float()
```

Also, you need to make sure when the output is passed through different layers in the forward function, the input to the batch norm layer is converted from float16 to float32 and then the output needs to be converted back to float16


	- 
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

[[GPUs - Model Memory Footprint]]