---
tags:
  - "#Ideas"
  - "#Incomplete"
topics: "[[Region Proposals]]"
---

- An important property of a region proposal method is to have a very high recall.
- Based on computing hierarchical grouping of similar regions based on color, texture, size and shape compatibility.
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
- Inution

# Comments and Links
- 
# Reference

- I looked up selective search in a blog: [https://learnopencv.com/selective-search-for-object-detection-cpp-python/](https://learnopencv.com/selective-search-for-object-detection-cpp-python/)