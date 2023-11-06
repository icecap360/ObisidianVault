---
tags:
  - "#Ideas"
  - "#Software"
topics: "[[PyTorch]]"
---
- pyTorch models store the learned parameters in an internal state dictionary, called state_dict
- be sure to call model.eval() method before inferencing to set the dropout and batch normalization  layers to evaluation mode. Failing to do this will yield inconsistent inference results.
# Syntax
```
model =models.vgg16(weights='IMAGENET1K_V1') 
torch.save(model.state_dict(), 'model_weights.pth')
```

```
model =models.vgg16() # we do not specify ``weights``, i.e. create untrained model model.load_state_dict(torch.load('model_weights.pth')) 
model.eval()
```
- **Saving the shape and the weights together**, requires the class definition to be available
	- torch.save(model, 'model.pth')
# Examples

# Comments and Links
- 
# Reference