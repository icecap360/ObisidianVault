---
tags:
  - "#Ideas"
  - "#Software"
topics: "[[PyTorch]]"
---
- Primitives to work with data: torch.utils.data.DataLoader and torch.utils.data.Dataset:
    - Dataset stores the samples and their corresponding labels, and DataLoader wraps an iterable around the Dataset.
	- Dataloader detailed explanation: the Dataset retrieves our dataset’s features and labels one sample at a time. While training a model, we typically want to pass samples in “minibatches”, reshuffle the data at every epoch to reduce model overfitting, and use Python’s multiprocessing to speed up data retrieval. DataLoader is an iterable that abstracts this complexity for us in an easy API.
- Lambda transforms apply any user-defined lambda function. Here, we define a function to turn the integer into a one-hot encoded tensor:
    - It first creates a zero tensor of size 10 (the number of labels in our dataset) and calls [scatter_](https://pytorch.org/docs/stable/generated/torch.Tensor.scatter_.html) which assigns a `value=1` on the index as given by the label `y`.
    `target_transform = Lambda(lambda y: torch.zeros( 10, dtype=torch.float).scatter_(dim=0, index=torch.tensor(y), value=1))`
- All TorchVision datasets have two parameters -`transform` to modify the features and `target_transform` to modify the labels - that accept callables containing the transformation logic. The [torchvision.transforms](https://pytorch.org/vision/stable/transforms.html) module offers several commonly-used transforms out of the box.
```
transforms = v2.Compose([
    v2.RandomResizedCrop(size=(224, 224), antialias=True),
    v2.RandomHorizontalFlip(p=0.5),
    v2.ToDtype(torch.float32, scale=True),
    v2.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]),
])
img = transforms(img)
```
-  [ToTensor](https://pytorch.org/vision/stable/transforms.html#torchvision.transforms.ToTensor) converts a PIL image or NumPy `ndarray` into a `FloatTensor`. and scales the image’s pixel intensity values in the range \[0,1] 
# Syntax

# Examples

# Comments and Links
- 
# Reference