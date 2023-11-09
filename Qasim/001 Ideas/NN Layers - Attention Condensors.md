---
tags:
  - "#Ideas"
  - "#Incomplete"
topics: "[[Transformers]]"
---
![[Pasted image 20231109152736.png]]


- Motivation:
	- create a self-contained, stand-alone self-attention mechanism that facilitate for sparser use of larger stand-alone convolution modules to reduce overall network complexity, we designed the proposed attention condensers in a way that **jointly models both local and cross-channel activation relationships within a unified embedding**.
- Alternatives:
	- Existing self-attention mechanisms for deep convolutional neural network architectures in literature have focused on the decoupling of attention into channel-wise attention [20] and local attention [21], where channel-wise attention mechanisms project input activations along the non-channel dimensions and model cross-channel activation relationships while local attention mechanisms project input activations along the channel dimension and model local activation relationships. Furthermore, existing self-attention mechanisms for deep convolutional neural network architectures are designed to depend on existing convolution modules within the network architecture.
- How it works
	- To greatly reduce the computational complexity of such a joint modeling, we condense the input activations to a reduced dimension to strike a balance between modeling capability and computational efficiency.
	- A condensation layer C(V ), an embedding structure E(Q), an expansion layer X(K), and a selective attention mechanism F(V, A, S). The condensation layer (i.e., Q = C(V )) condenses the input activations V for reduced dimensionality to Q in a way that emphasizes activations in close proximity to strong activations.
	- An embedding structure (i.e., K = E(Q)) then learns and produces a condensed embedding K from Q that characterizes joint local and cross-channel activation relationships. 
	- An expansion layer (i.e., A = X(K)) projects the condensed embedding K for increased dimensionality to produce self-attention values A to be used for selective attention (i.e., V 0 = F(V, A, S)). 
	- The output V 0 are a product of the input activations V , self-attention values A, and scale S via selective attention (i.e., V 0 = F(V, A, S)). 
		- The introduction of scale S to the selective attention mechanism enables greater flexibility for the degree of selective attention to be determined. More specifically, the degree of selective attention increases as S decreases such that as S → 0, V 0 → A. 
	- Overall, by jointly modeling local and cross-channel activation relationships within a unified condensed embedding, attention condensers can act as self-contained, stand-alone modules and facilitate for efficient deep neural networks with much sparser use of larger stand-alone convolution modules and more frequent use of attention condensers.


# Comments and Links
- 
# Reference