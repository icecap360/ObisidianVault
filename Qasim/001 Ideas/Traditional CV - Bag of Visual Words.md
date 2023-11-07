---
tags:
  - "#Ideas"
  - "#Incomplete"
topics:
---
We use the keypoints and descriptors of those key points to construct vocabularies. Then we can represent a given image as a frequency histogram of features that are in the image.
![[Pasted image 20231107161537.png]]

#### ChatGPT
- Features consist of  keypoints and descriptors of those keypoints.
1. Feature Extraction: The first step is to extract local visual features from the images. These features are often scale-invariant and robust to transformations, like Scale-Invariant Feature Transform (SIFT), Speeded-Up Robust Features (SURF), or Oriented FAST and Rotated BRIEF (ORB) descriptors. These features are called "keypoints" and describe local patterns or structures within the image.
2. Feature Descriptor Quantization: The extracted keypoints are then described using feature descriptors, typically represented as vectors. These descriptors capture the appearance and characteristics of the keypoints. The next step is to quantize these descriptors into a fixed number of visual words or "codebook" based on clustering techniques like k-means clustering. Each visual word represents a cluster of similar descriptors.
3. Histogram Generation: For each image, a histogram is created to represent the distribution of visual words within the image. To do this, you assign each keypoint descriptor to the nearest visual word (cluster) in the codebook. The histogram records how many keypoints belong to each visual word in the codebook. The order of keypoints doesn't matter; it's a bag-like representation, just like in Bag of Words for text.
4. Image Representation: The histograms generated in step 3 serve as the image representation. Each image is now represented as a histogram where each bin corresponds to a different visual word. This representation encodes the frequency of each visual word in the image.
5. Classification or Retrieval: The resulting histograms can be used for various tasks. In image classification, you can train a classifier, such as a Support Vector Machine (SVM) or a k-nearest neighbors (k-NN) algorithm, to categorize images based on their histogram representations. In image retrieval, you can compare the histograms of query images with those of a database to find the most similar images.

# Comments and Links
- 
# Reference
[[Paper - Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition|heSpatialPyramidPooling2014]]