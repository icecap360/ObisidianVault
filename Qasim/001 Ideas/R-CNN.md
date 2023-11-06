---
tags:
  - "#Ideas"
topics: "[[Object Detection]]"
---
- Combine region proposals with CNNs, we call our method R-CNN: Regions with CNN features
- At test time, our method generates around 2000 category-independent region proposals for the input image, extracts a fixed-length feature vector from each proposal using a CNN, and then classifies each region with category-specific linear SVMs. We use a simple technique (affine image warping) to compute a fixed-size CNN input from each region proposal, regardless of the region’s shape
# Comments and Links
- 
# Reference
[[CV Architecture]]
[[Paper - Rich feature hierarchies for accurate object detection and semantic segmentation]]