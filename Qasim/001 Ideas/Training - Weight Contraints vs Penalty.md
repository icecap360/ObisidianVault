---
tags:
  - "#Ideas"
  - "#Incomplete"
topics: "[[Training]]"
---
- - Using a constraint rather than a penalty prevents weights from growing very large no matter how large the proposed weight-update is. This makes it possible to start with a very large learning rate which decays during learning, thus allowing a far more thorough search of the weight-space than methods that start with small weights and use a small learning rate.
- How to do gradient clip:
	- If a weight-update violates this constraint, we renormalize the weights of the hidden unit by division
	- 
# Comments and Links
- 
# Reference