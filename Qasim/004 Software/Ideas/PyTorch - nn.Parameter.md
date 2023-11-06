---
tags:
  - "#Ideas"
  - "#Software"
topics: "[[PyTorch]]"
---
- Internally, nn.Parameter facilitates registering or keeping track of parameters in a given network
	- Notice one thing, that when we defined net, we didn't need to add the parameters of nn.Conv2d to parameters of net. It happened implicitly by virtue of setting nn.Conv2d object as a member of the net object. This is internally facilitated by the nn.Parameter class, which subclasses the Tensor class.
- Notice one thing, that when we defined net, we didn't need to add the parameters of nn.Conv2d to parameters of net. It happened implicitly by virtue of setting nn.Conv2d object as a member of the net object. This is internally facilitated by the nn.Parameter class, which subclasses the Tensor class.
	- 

# Syntax
```
self.tens = nn.Parameter(torch.ones(3,4)) # This will show up in a parameter list
```

# Examples

# Comments and Links
- 
# Reference