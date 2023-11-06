- Allow you to do things during backpropagation
- register a hook on a Tensor or nn.Module. A hook is basically a function that is executed when the either `forward` or `backward` is called.
	- When I say forward, I don't mean the forward of a nn.Module . forward function here means the forward  function of the torch.Autograd.Function object that is the grad_fn of a Tensor. For example, if a tensor is created by tens = tens1 + tens2, it's grad_fn is AddBackward. Still doesn't make sense? You should definitely go back and read this article.
	- Notice, that a nn.Module like a nn.Linear has multiple forward invocations. It's output is created by two operations, (Y = W * X + B), addition and multiplication and thus there will be two forward calls. This can mess things up, and can lead to multiple outputs. We will touch this in more detail later in this article.
- Types of hooks:
	- Forward
	- Backward
- Use case: printing and logging, changing gradient
## Proper way of using Hooks
- Attach hooks to the tensors within a module rather then an entire module (see nn.module subsection as to why hooks on nn.module are bad)
	- `named_parameters` allows us much much more control over which gradients to tinker with.
- Notwithstanding the issues I already highlighted with attaching hooks to PyTorch, I've seen many people use forward hooks to save intermediate feature maps by saving the feature maps to a python variable external to the hook function
- 
- 
## Hooks on Tensors
- No Forward hook for a tensor
- Signature for the backward hook
	- `hook(grad) -> Tensor or None`, where grad is basically the value contained in the grad attribute of the tensor after backward is called.
	- DO NOT modify the arg `grad`
	- Must return None or a Tensor which will be used in place of `grad`
- Registering
	- `Tensor.register_hook(fn: Callable[Tensor, Optional[Tensor]])`. The given function is called every time a gradient for this Tensor is computed. Can optionally return a new value for the gradient that will be used in the autograd instead of the current value.
### Node Hooks
- Autograd Node gradient hooks via Node.register_hook(fn: Callable[Tuple[Tensor, ...], Tuple[Tensor, ...]]). The given function is called every time this node is executed and return the gradient wrt the inputs. These hooks must always return the new gradient values.

## Hooks on NN.Module
- Bad practice:
	- break abstraction: nn.Module is supposed to be a modularised object representing a layer. However, a hook is subjected a forward and a backward, of which there can be an arbitrary number in a nn.Module object. This requires me to know the internal structure of the modularised object. For example, a nn.Linear involves two forward calls during it's execution. Multiplication and Addition ( y = w * x + b). This is why the input to the hook function can be a tuple containing the inputs to two different forward calls and output s the output of the forward call. 
	- grad_input and grad_output can be pretty ambiguous for the reason of multiple calls inside a nn.Module object.
- Backward hook: `hook(module, grad_input, grad_output) -> Tensor or None`
	- grad_input is the gradient of the input of nn.Module object w.r.t to the loss ( dL / dx, dL / dw, dL / b).
	- grad_output is the gradient of the output of the nn.Module object w.r.t to the gradient.
	- You can optionally return a grad_input (notice how the grad_input is the output becuase in backward pass we go backwards across the layers)
- Forward Hook
	- `hook(module, input, output) -> None`
	- Can be used to get the input value just before or after the evaluation of the Module.forward method.
- Registering
	- Module pre-forward hook via `Module.register_forward_pre_hook(fn: Callable[Tuple[Module, Any, ...], Optional[Tuple[Any, ...]]])`
		- Can be used to get value of input just before evaluation of Module.forward
	- Module forward hook via `Module.register_forward_hook(fn: Callable[Tuple[Module, Any, ...], Optional[Tuple[Any, ...]]])`
		-  Can be used to get value of input just after evaluation of Module.forward
	- Module backward hook via `Module.register_full_backward_hook(fn: Callable[Tuple[Module, Tuple[Any, ...], Tuple[Any, ...]], Optional[Tuple[Any, ...]]])`
		- Can be used to get the value of the gradients wrt to all inputs and output of the Module.
	- The old Module.register_backward_hook() that are deprecated and will be removed soon ™
	
## Example
- I want to :
	- Turn gradients of linear biases to zero while backpropagating
	- Make sure the no gradients going to conv layer is less than 0
```python
import torch
import torch.nn as nn

class myNet(nn.Module):
  def __init__(self):
    super().__init__()
    self.conv = nn.Conv2d(3,10,2, stride = 2)
    self.relu = nn.ReLU()
    self.flatten = lambda x: x.view(-1)
    self.fc1 = nn.Linear(160,5)


  def forward(self, x):
    x = self.relu(self.conv(x))
    x.register_hook(lambda grad : torch.clamp(grad, min = 0))     #No gradient shall be backpropagated
                                                                  #conv outside less than 0

    # print whether there is any negative grad
    x.register_hook(lambda grad: print("Gradients less than zero:", bool((grad < 0).any())))
    return self.fc1(self.flatten(x))


net = myNet()

for name, param in net.named_parameters():
  # if the param is from a linear and is a bias
  if "fc" in name and "bias" in name:
    param.register_hook(lambda grad: torch.zeros(grad.shape))


out = net(torch.randn(1,3,8,8))

(1 - out).mean().backward()

print("The biases are", net.fc1.bias.grad)     #bias grads are zero
```

Output
```python
Gradients less than zero: False
The biases are tensor([0., 0., 0., 0., 0.])
```