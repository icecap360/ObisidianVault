---
tags:
  - "#Ideas"
  - "#Incomplete"
topics:
---
- The mAP is calculated by finding Average Precision(AP) for each class and then average over a number of classes.
## Steps
Here is a summary of the steps to calculate the AP:

1. Generate the prediction scores using the model.
2. Convert the prediction scores to class labels.
3. Calculate the confusion matrix—TP, FP, TN, FN.
4. Calculate the precision and recall metrics.
5. Calculate the area under the precision-recall curve.Precision-Recall curve is obtained by plotting the model's precision and recall values as a function of the model's confidence score threshold.
    1. A better model will give predictions with confidence scores that allow you to have both high precisoin and high recall
![[Pasted image 20231106191646.png]]
6. Measure the average precision. Average Precision is calculated as the weighted mean of precision's at each threshold; the weight is the increase in recall from the prior threshold.
    1. Average Precision is a way to summarize the PR curve into a single value.

## Intuition
- AUC and AP are better at describing the PR curve, as F1 is just a single number
![[Pasted image 20231106191752.png]]
- As you can see, AP is a deiscrete version of AUC. It can be calculated by a weighted mean
- For object detection tasks, precision is calculated based on the IoU threshold.The precision value differs based w.r.t IoU threshold.This shows that the AP metric is dependent on the IoU threshold. **Choosing the IoU threshold is an arbitrary process**. Hence, to avoid this ambiguity use mAP.
- Summary: choose a set of IOU thresholds, calculate AP for each threshold, average the AP across IOU thresholds, now average across classes
- - Notice how when the IOU is low, the model gets better predictions (wider precision recall curves)
![[Pasted image 20231106191912.png]]
- To remove the effect of IOU and of performance on a given class, average across IOU’s, and then average across classes.
# Comments and Links
- 
# Reference