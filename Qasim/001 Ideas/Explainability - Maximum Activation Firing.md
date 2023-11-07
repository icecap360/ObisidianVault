---
tags:
  - "#Ideas"
  - "#Incomplete"
topics: "[[Explainability]]"
---
- a simple (and complementary) non-parametric method that directly shows what the network learned
    - The idea is to single out a particular unit (feature) in the network and use it as if it were an object detector in its own right. That is, we compute the unit’s activations on a large set of held-out region proposals (about 10 million), sort the proposals from highest to lowest activation, perform nonmaximum suppression, and then display the top-scoring regions.
    - Our method lets the selected unit “speak for itself” by showing exactly which inputs it fires on. We avoid averaging in order to see different visual modes and gain insight into the invariances computed by the unit.
    - We visualize units from layer pool_5, which is the maxpooled output of the network’s fifth and final convolutional layer
# Comments and Links
- 
# Reference.