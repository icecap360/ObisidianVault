---
tags:
  - "#RawInformation"
  - Video
  - "#Unread"
Link: https://www.youtube.com/watch?v=XfpMkf4rD6E&t=1249s
completed: false
---
- - Query: what are the things I am looking for
- Key: What are the things I have
- Value: What are the things I will communicate
- Two phases: Communicate and compute
- In terms of graphs
    - Loop over each node
        - q = node.query()
        - keys = key for all of the input nodes
        - scores = compatabilities = what are the important nodes that have the information I am looking for
        - weighted sum of those values of inputs
    - This message passing scheme is at the heart of the transformer
- The communication phase happens in every head in parallel, and every layer in series
- Decoder-encoder attention:
    - Each layer of the decoder computes a representation in terms of the encoder (cause V comes from encoder). It is able to do this because the encoder tells us what informaiton it has (K). All this layer has to do is find the right query (Q).
- Decoder: I have queries because I am looking for the 5th word in the sequence, the source of my informaiton that can ansewr my queries comes from encoder or previous nodes in my decoder sequence (preopopiuvs outputs).
- Cross-attention and self-attention only differ in where keys and values come from


# Details
- 

# Reference

