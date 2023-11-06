---
tags:
  - "#RawInformation"
  - Article
completed: false
url: https://colah.github.io/posts/2015-09-NN-Types-FP/
year: "2015"
authors: Christopher Olah
---

# 3-Sentence Summary
Deep learning studies a connection between optimization and functional programming.
- Long form: It feels like a new kind of programming altogether, a kind of differentiable functional programming. One writes a very rough functional program, with these flexible, learnable pieces, and defines the correct behavior of the program with lots of data. Then you apply gradient descent, or some other optimization algorithm. The result is a program capable of doing remarkable things that we have no idea how to create directly, like generating captions describing images.


# Details
- At present, three narratives are competing to be the way we understand deep learning. 
	- There’s the neuroscience narrative, drawing analogies to biology. 
	- There’s the representations narrative, centered on transformations of data and the manifold hypothesis. 
- Finally, there’s a probabilistic narrative, which interprets neural networks as finding latent variables. These narratives aren’t mutually exclusive, but they do present very different ways of thinking about deep learning.
- This papers answer
	- Deep learning studies a connection between optimization and functional programming.
- With every layer, neural networks transform data, molding it into a form that makes their task easier to do.
	- We call these transformed versions of data “representations.”
	- **Representations correspond to types**.
- At their crudest, **types** in computer science are a way of embedding some kind of data in n bits. Similarly, representations in deep learning are a way to embed a data manifold in n dimensions.
- Just as two functions can only be composed together if their types agree, two layers can only be composed together when their representations agree. Data in the wrong representation is nonsensical to a neural network.
- Multiple input layers mapping into one representation, and multiple outputs mapping from the same representation, thus input 1+2 are related in the representation rep2, which is related to rep3 and output 1+2
 ![[Pasted image 20231106121051.png]]
- In programming, the abstraction of functions is essential. we can write code once and use it as we need it.
    - Using multiple copies of a neuron in different places is the neural network equivalent of using functions. Because there is less to learn, the model learns more quickly and learns a better model.
    - This technique – the technical name for it is ‘weight tying’ – is essential to the phenomenal results we’ve recently seen from deep learning.
    - For the model to work, you need to do it in a principled way, exploiting some structure in your data. In practice, there are a handful of patterns that are widely used, such as recurrent layers and convolutional layers

![[Pasted image 20231106121313.png]]
![[Pasted image 20231106121326.png]]
![[Pasted image 20231106121342.png]]
![[Pasted image 20231106121401.png]]