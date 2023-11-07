---
tags:
  - "#Ideas"
  - "#Incomplete"
topics: "[[Object Detection - Anchors]]"
---
-_Anchor boxes_ are a set of predefined bounding boxes of a certain height and width.
- During detection, the predefined anchor boxes are tiled across the image.
    - The network predicts the probability and other attributes, such as background, intersection over union (IoU) and offsets for every tiled anchor box.
- enables a network to detect multiple objects, objects of different scales, and overlapping objects.
- allows for parallelization
- Stride between prediction heads is a hyperparameter

![[Pasted image 20231106193508.png]]
![[Pasted image 20231106193543.png]]

- This approach is old, and was solved by [[CornerNet]] and [[ExtremeNet]]
# Comments and Links
- 
# Reference