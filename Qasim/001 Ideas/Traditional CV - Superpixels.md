---
tags:
  - "#Ideas"
  - "#Incomplete"
topics: "[[Traditional Computer Vision]]"
---
- - a single pixel, standing alone by itself, is not a natural representation of an image.
- Intuitively, it would make more sense to explore not only perceptual, but semantic meanings of an image formed by locally grouping pixels as well.
- These superpixels carry more perceptual and semantic meaning than their simple pixel grid counterparts.
- it allows us to reduce the complexity of the images themselves from hundreds of thousands of pixels to only a few hundred superpixels.

### Computing Super Pixels
- N = number of pixels
- K = number of super pixels
- S = distance between center of super pixels = $\sqrt{\dfrac{N}{K}}$​
- Guess an initial number of super pixels, and distribute them evenly across the image
	- Move each of the initial super pixel coordinates in the direction with least gradient. This is because we don’t want to initialize superpixels at edges or boundaries of two regions
- Definition of a superpixel and distance between pixels

![[Pasted image 20231106191013.png]]

- - Iterate until the superpixel locations stabalize
    - For each super pixel SP
        - Add pixels in the 2Sx2S neightbourhood around the center of superpixel SP if the distance between them is small enough
        - Compute the new center of superpixel SP


# Comments and Links
- 
# Reference