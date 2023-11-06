---
tags:
  - "#Ideas"
  - "#Incomplete"
topics: "[[GPUs - Model Memory Footprint]]"
---
* 3 main groups of operation in transformers, ranked by compute intensity
    - tensor contraction
    - statistical normalization (softmax, layer norms, anything with reduciton operations that can be applied via a map)
    - Element wise operators: biases, dropout, activations, residual connections
- Within training a model, the following items will be in GPU
    - Kernel (about 1-2 Gb)
    - model weights
    - optimizer states
    - gradients
    - forward activations saved for gradient computation
    - temporary buffers
    - functionality-specific memory
- How many bytes per parameter with AdamW
	- A typical model trained in mixed precision with AdamW requires 18 bytes per model parameter plus activation memory. For inference there are no optimizer states and gradients, so we can subtract those. And thus we end up with 6 bytes per model parameter for mixed precision inference, plus activation memory.
	- Model Weights:
	    - 4 bytes * number of parameters for fp32 training
	    - 6 bytes * number of parameters for mixed precision training
	- Optimizer states
	    - 8 bytes * number of parameters for AdamW (maintains 2 states)
	    - 2 bytes * number of parameters for 8-bit AdamW optimizers like bits and bytes
	    - 4 bytes * number of parameters for optimizers like SGD with momentum (maintains only 1 state)
	    - 4 bytes * number of parameters for either fp32 or mixed precision training (gradients are always kept in fp32)
	    - Size depends on many factors, the key ones being sequence length, hidden size and batch size.
- - Temprary variables
    - when coding it’s crucial to think strategically about such temporary variables and sometimes to explicitly free those as soon as they are no longer needed.

# References
- [https://huggingface.co/docs/transformers/model_memory_anatomy](https://huggingface.co/docs/transformers/model_memory_anatomy)