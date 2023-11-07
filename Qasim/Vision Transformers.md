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
- Intuition, and why they work
	- left image shows first principle componenet of the learned embedding filter (i.e. the projective filter that is applied to each patch). They resemble plausible basis functions for low-dimensional representation of fine structure
	- in the middle image we see that closer patches have more similar position embeddings. Also for larger grids, we see a sinusoidal structure. perhaps this is why 2d-aware embedding variants do not yield improvements, they 1d embedding learns the 2d position
	- in the right image, for each layer, we compute the average distance in image space across which information is integrated, based on the attention weights. This “attention distance” is analogous to receptive field size in CNNs. We see that even in the lowest layers, the ability to integrate information globally is used by the model. The attention distance increases with network depth.
![[Pasted image 20231106201229.png]]


# Comments and Links
- 
# Reference
[[Paper - An Image is Worth 16x16 Words - Transformers for Image Recognition at Scale|An Image is Worth 16x16 Words]]