---
tags:
  - "#Incomplete"
  - "#Relationships"
topics:
---
* Inductive Bias
	- In ViT, only MLP layers are local and translationally equivariant, while the self-attention layers are global
	- There is very little 2D structure usage, e.g. cutting image into patches, fine-tuning time for adjusting position embeddings for images of different resolutions
	- spatial relations need to be learned from scratch
- Hybrid architecture
	- - Divide CNN feature maps into patches rather then the raw image.
	- As a special case, the patches can have spatial size 1x1, which means that the input sequence is obtained by simply flattening the spatial dimensions of the feature map and projecting to the Transformer dimension.
- 
# Comments and Links
- 
# Reference