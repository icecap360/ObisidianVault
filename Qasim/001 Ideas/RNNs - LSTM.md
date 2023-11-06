---
tags:
  - "#Ideas"
  - "#Incomplete"
topics: "[[RNNs]]"
---
- RNNs suffer from vanishing/exploding gradients
- The is because there is a shared feedback weight, so the hidden state either magnifies or amplifies older input states
- Solution: Create 2 paths, one for long term memory and one for short term memories
![[Pasted image 20231106153215.png]]
![[Pasted image 20231106153231.png]]

- IN the above diagram,
    - the green line represents long term memory. It has no weights or biases that can modify it DIRECTLY, allowing the hidden states to propogate
    - The pink line represents short term memories, there are weight and biases that modify it directly
- The first stage (forget stage), shown in blue, reduces the long-term memory
    - e.g. plug in input=1, short term memory=1, longterm memory=2, the new long term memory is 1.99
    - think of it as calculating the percent of long term memory that should be remembered
- ## The yellow stage, called the “potential long-term memory”, calculates the long-term value of the short-term memory and input
    
    - its output can be positive or negative (because the activation function is a tanh). note however that a negative potential memory does not necessarly end up subtreacting from the long term memory, as a negative potential memory may occur sumultatnosly with near 0 % potential memory to remember (green stagE)
- The green stage, called % potential memory to remember,
    - works in the same way as the blue stage
- Red and purple stages update determine how much of the long term memory should be added to the short term memory
    - The purple stage is the % potential memory to remember, works same as the blue and green stages
    - The red stage sets the short term memory as tanh(long term memory) * % (determined by purple stage)





# Comments and Links
- 
# Reference
[https://www.youtube.com/watch?v=YCzL96nL7j0](https://www.youtube.com/watch?v=YCzL96nL7j0)