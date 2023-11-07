## TODO

- Forget about freezing the backbone, nobody does that
- Freeze the backbone by loading the pretrained weights in the init
    - In this way you have two configs, the pretrained backbone, and the full model
- Another option to unfreeze the layers before saving the model

## Thigs I have learnt
- the prefix of load_state_dict depends on the name of the object you try to load it into
```
#This is some bad code for k, v in _state_dict.items():
    
state_dict['backbone.'+k] = v
# load state_dict
self.load_state_dict(state_dict, False)
    
#This can be replaced with self.backbone.load_state_dict(state_dict, True)
```
## TurboViT Architecture

- 2 Parallel backbones, one for processing depth, another for processing rgb
    
- The two backbones are fused together using a fusion layer, which is just convolutions
    
    - The outputs are the same shape as the aactivations of a single bacbone
- That feeds into an FPN
    
    - Apples 1x1 conv to each of the scales, so that they are all the same channel (48)
    - For the scales that have a feature map above them (higher spatial resolution), interpolate and add the feature map at lower resolution
    - Applied convolution to each of the 4 fmaps
    - Now the 5th feature map is max pool of the smallest spatial resolution feature map
- They feed into an RPN
    
    - It applies the following to each of the 5 scales
    
![[Pasted image 20231106210126.png]]
    - The RPN outputs are then reshaped into 4 for bounding boxes and 2 for classification
    
![[Pasted image 20231106210149.png]]
    - I.e. the feature maps are predictions
- The RPN outputs are somehow turned into bbox feature maps, or roi’s
    
    - This along with the anchors is confusing
- ROI Head
    
    - Given the bounding box features, the following happens
    - 2 FC layers are applied
    - A FC is applied to create class predictions
    - A FC is applied to create bbox predictions