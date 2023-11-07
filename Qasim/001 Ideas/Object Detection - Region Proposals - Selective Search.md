---
tags:
  - "#Ideas"
  - "#Incomplete"
topics: "[[Region Proposals]]"
---
- Based on computing hierarchical grouping of similar regions based on color, texture, size and shape compatibility.

## Details
- Selective Search starts by over-segmenting the image based on intensity of the pixels using a graph-based segmentation method by Felzenszwalb and Huttenlocher
    - The outputs cannot be used as Region Proposals because
        - Most of the actual objects in the original image contain 2 or more segmented parts
        - Region proposals for occluded objects such as the plate covered by the cup or the cup filled with coffee cannot be generated using this method
![[Pasted image 20231106190314.png]]
	- If we try to address the first problem by further merging the adjacent regions similar to each other we will end up with one segmented region covering two objects.
- - Take these oversegments as initial input and performs the following steps
    - Add all bounding boxes corresponding to segmented parts to the list of regional proposals
    - Group adjacent segments based on similarity
    - Go to step 1
### Intuition
- We create region proposals from smaller segments to larger segments in a bottom-up approach.
- An important property of a region proposal method is to have a very high recall, no precision!
### Similarity Measure
![[Pasted image 20231106190609.png]]

- - Color
    - Descriptor: A color histogram of 25 bins is calculated for each channel of the image and histograms for all channels are concatenated to obtain a 25×3 = 75-dimensional color descriptor.
    - Similarity measure:
![[Pasted image 20231106190632.png]]

- Texture
	- Descriptor: Extracting Gaussian derivatives at 8 orientations for each channel. For each orientation and for each color channel, a 10-bin histogram is computed resulting into a 10x8x3 = 240-dimensional feature descriptor.
	- Similariy measre: histogram intersection again
![[Pasted image 20231106190718.png]]
- - Size Similarity:
    - encourages smaller regions to merge early.
    - Ensures that region proposals at all scales are formed at all parts of the image.
    - If this similarity measure is not taken into consideration a single region will keep gobbling up all the smaller adjacent regions one by one and hence region proposals at multiple scales will be generated at this location only.
![[Pasted image 20231106190738.png]]

- - Shape Compatibility
    - If r_i fits into r_j we would like to merge them in order to fill gaps and if they are not even touching each other they should not be merged.
![[Pasted image 20231106190756.png]]





# Comments and Links
- 
# Reference

- I looked up selective search in a blog: [https://learnopencv.com/selective-search-for-object-detection-cpp-python/](https://learnopencv.com/selective-search-for-object-detection-cpp-python/)