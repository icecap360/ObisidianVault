---
tags:
  - "#Ideas"
  - "#Incomplete"
  - "#Software"
topics:
---
## Data Parallelism
- where we divide batches into smaller batches, and process these smaller batches in parallel on multiple GPU.
- `parallel_net = nn.DataParallel(myNet, gpu_ids = [0,1,2])`
- You can simply execute the nn.DataParallel object just like a nn.Module .
- Despite the fact our data has to be parallelised over multiple GPUs, we have to **initially** store it on a **single GPU**. We also need to make sure the DataParallel object is on that particular GPU as well. The syntax remains similar to what we did earlier with nn.Module.
![[Untitled.png]]
- One **issue** with DataParallel can be that it can put asymmetrical load on one GPU (the main node). There are generally two ways to circumvent these problem.
	- First, is to compute the loss during the forward pass. This makes sure at least the loss calculation phase is parallelised.
	- Another way is to implement a parallel loss function layer.

## Model Parallelism
- where we break the neural network into smaller sub networks and then execute these sub networks on different GPUs.
- No real benefit in speed (as too much interGPU dependence), but it does allow you to train much larger models, models that are too big to put on one gpu

# Syntax

# Examples

# Comments and Links
- 
# Reference