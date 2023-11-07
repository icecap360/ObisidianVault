---
tags:
  - "#Ideas"
topics: "[[Traditional Computer Vision]]"
---
![[Pasted image 20231107162427.png]]

1. Image Subdivision: The image is divided into a grid of cells, creating a coarse level of spatial subdivision. This grid typically starts with one cell that covers the entire image.
2. Feature Extraction: Visual features are extracted from each of the cells. These features can be local descriptors like SIFT (Scale-Invariant Feature Transform) or SURF (Speeded-Up Robust Features) descriptors. For each cell, you calculate a feature histogram representing the distribution of visual features within that cell.
3. Aggregation: The histograms from all cells at the current level of the pyramid are concatenated or combined to create a single, more comprehensive histogram representation for that level.
	1. often done by taking a weighted sum of the number of matches that occur at each level of resolution
	2. At any fixed resolution, two points are said to match if they fall into the same cell of the grid; matches found at finer resolutions are weighted more highly than matches found at coarser resolutions.
4. Refinement: Steps 1 to 3 are repeated for multiple levels of the pyramid, each time dividing the image into smaller and more numerous subregions. This hierarchical structure allows the model to capture information at different spatial scales.
5. Classification or Matching: The concatenated histograms from all levels of the pyramid are used as the feature representation for the image. This representation can be fed into a classifier or used for matching tasks, such as image retrieval or object recognition.


#### How does spatial pyramid matching help BoW
- If you use a Bag of Words ([[Traditional CV - Bag of Visual Words]]) alone, it doesn't discriminate if a patch was obtained from the top or bottom of the image since it doesn't save any spatial information, but if you divide the image in several regions (in the spatial pyramid way) and use a BoW on every region, you will know that if the BoWs of the upper part of the image contain "sky visual words", the BoWs in the middle "sea and sand visual words" and the BoWs at the bottom "sand visual words" it is very probable that the image scene category is 'beach'.


# Comments and Links
- 
# Reference