- Allow you to do things during backpropagation
- register a hook on a Tensor or nn.Module. A hook is basically a function that is executed when the either `forward` or `backward` is called.
	- When I say forward, I don't mean the forward of a nn.Module . forward function here means the forward  function of the torch.Autograd.Function object that is the grad_fn of a Tensor. For example, if a tensor is created by tens = tens1 + tens2, it's grad_fn is AddBackward. Still doesn't make sense? You should definitely go back and read this article.
	- Notice, that a nn.Module like a nn.Linear has multiple forward invocations. It's output is created by two operations, (Y = W * X + B), addition and multiplication and thus there will be two forward calls. This can mess things up, and can lead to multiple outputs. We will touch this in more detail later in this article.
- Types of hooks:
	- Forward
	- Backward
- Use case: printing and logging, changing gradient

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
- Backward hook: `hook(module, grad_input, grad_output) -> Tensor or None`
	- grad_input is the gradient of the input of nn.Module object w.r.t to the loss ( dL / dx, dL / dw, dL / b).
	- grad_output is the gradient of the output of the nn.Module object w.r.t to the gradient.
	- You can optionally return a grad_input (notice how the grad_input is the output becuase in backward pass we go backwards across the layers)
- Forward Hook
	- `hook(module, input, output) -> None`
	- Can be used to get the input value just before the evaluation of the Module.forward method.
- Registering
	- Module pre-forward hook via `Module.register_forward_pre_hook(fn: Callable[Tuple[Module, Any, ...], Optional[Tuple[Any, ...]]])`
	- 