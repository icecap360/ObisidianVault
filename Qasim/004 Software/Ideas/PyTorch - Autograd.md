---
tags:
  - "#Ideas"
  - "#Incomplete"
  - "#Software"
topics:
---
- PyTorch has a built-in differentiation engine called torch.autograd. It supports automatic computation of gradient for any computational graph.
- You can set the value of *requires_grad* when creating a tensor, or later by using *x.requires_grad_(True)* method.
- A function that we apply to tensors to construct computational graph is in fact an object of class [Function](https://pytorch.org/docs/stable/autograd.html#torch.autograd.Function). This object must know how to compute the function in the forward direction, and also how to compute its derivative during the backward propagation step.
    - A reference to the backward propagation function is stored in `grad_fn` property of a tensor.

- **grad_fn** refers to the mathematical operator that created the variable. Every tensor has one.
	- it is set during the forward pass but is used during the backward pass
	- set only once when the tensor is created
	- If `requires_grad` is set to False, `grad_fn` would be `None`
	- If Tensor is a leaf node (i.e. `is_leaf` is set to True), then `grad_fn` is also None

- We can only obtain the `grad` properties for the leaf nodes of the computational graph, which have `requires_grad` property set to `True`. For all other nodes in our graph, gradients will not be available.
    ![[Pasted image 20231031133319.png]]

- We can only perform gradient calculations using **backward once on a given graph**, for performance reasons. If we need to do several backward calls on the same graph, we need to pass retain_graph=True to the backward call.
- - If you want to use backward twice, clear the gradients:
	- Notice that when we call `backward` for the second time with the same argument, the value of the gradient is different. This happens because when doing `backward` propagation, PyTorch **accumulates the gradients**, i.e. the value of computed gradients is added to the `grad` property of all leaf nodes of computational graph. If you want to compute the proper gradients, you need to zero out the `grad` property before. In real-life training an _optimizer_ helps us to do this.
    
- Conceptually, autograd keeps a **record of data** (tensors) and **all executed operations** (along with the resulting new **tensors**) in a DAG consisting of [Function](https://pytorch.org/docs/stable/autograd.html#torch.autograd.Function) objects.
    - In this DAG, leaves are the input tensors, roots are the output tensors.
    - By tracing this graph from roots to leaves, you can automatically compute the gradients using the chain rule.
- The backward pass kicks off when `.backward()` is called on the DAG root. `autograd` then:
    - computes the gradients from each `.grad_fn`,
    - accumulates them in the respective tensor’s `.grad` attribute
    - using the chain rule, propagates all the way to the leaf tensors
- How torch allows you to change shape size and operations at each iteration
    - **DAGs are dynamic in PyTorch** An important thing to note is that the graph is recreated from scratch; after each `.backward()` call, autograd starts populating a new graph. This is exactly what allows you to use control flow statements in your model; you can change the shape, size and operations at every iteration if needed.

- Jacobian products
    - let the loss function be a scalar
    - pytorch does not support calculating J
    - PyTorch allows you to compute **Jacobian Product** vT⋅J for a given input vector v=(v1…vm). This is achieved by calling `backward` with v as an argument. The size ofvv should be the same as the size of the original tensor, with respect to which we want to compute the product:
# Syntax

- To compute derivatives, we call `loss.backward()`, and then retrieve the values from `w.grad` and `b.grad`. The *backward()* function can be called with no arguments, or a scalar/1d tensor argument:
```
import torch
x = torch.ones(5) # input tensor 
y = torch.zeros(3) # expected output 
w = torch.randn(5, 3, requires_grad=True) 
b = torch.randn(3, requires_grad=True) 
z = torch.matmul(x, w)+b 
loss = torch.nn.functional.binary_cross_entropy_with_logits(z, y)
loss.backward() 
print(\[w.grad, b.grad])
```
    

- **Disabling gradient tracking** to speed up computation or **freeze parameters**:
    - By default, all tensors with `requires_grad=True` are tracking their computational history and support gradient computation. We can stop tracking computations by surrounding our computation code with `torch.no_grad()` block or detatch :
```python
with torch.no_grad():
    print((x ** 2).requires_grad)

 z =torch.matmul(x,w)+bz_det =z.detach()
    print(z_det.requires_grad)
```
  
# Examples

# Comments and Links
- 
# Reference