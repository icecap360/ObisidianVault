---
tags:
  - "#RawInformation"
  - Article
completed: false
url: https://jalammar.github.io/illustrated-transformer/
year: "{{year}}"
authors: Jay Allamar
---

# Details
- The problem is translation, again we have a decoder and encoder layer
- Both the encoder and the decoder contain 6 layers,
    - the architecture of each layer within the encoder and decoder are the same
![[Pasted image 20231106151427.png]]
- The input is an array of word embeddings, where the length of the array is the same as the length of the sentence. In practice we set the length of the array to the longest possible length of a sentence

## Architecture of encoder layer

Simple View
![[Pasted image 20231106151551.png]]
Complex View (residuals, shown in dotted line, and normalization)
![[Pasted image 20231106151624.png]]
- - Each of the 6 layers of the encoder maps a word embedding (vector size 512) to a vector of size 512
- Within a layer, the feedforward weights are shared between embedding/vectors
    - Thus the feedforward network is not able to model dependencies between words
    - This stage can be parallelized
## Self attention
- Look to other words to help get a better encoding for the current word
- With RNNs, we would have one hidden state.
	- This hidden state would allow it to understand previous words
- Steps: For each word vector ….
    - Calculate a query vector, a key vector and a value vector . They are calculated through weight matrices Wq, Wk, and Wv. These matrices are shared between words though.
    - Next calculate a score to assign each of the other words
        - Score for the ith word and jth word = $\frac{(q_i \dot k_j)}{\sqrt(dim(k))}$
            - Division by the square root was added to stablize gradients
    - Normalize each of the scores using the softmax
    - For the ith word, the output will be the weighted sum of all value vectors with the score $z_i= sum(score_j * v_j)$ where the summation is over all words
    - In the matrix fomula below, for X and Z are n x m, where n = number of rows = number of examples
![[Pasted image 20231106151848.png]]

![[Pasted image 20231106152143.png]]

## Multi-Headed Attention
* Give the attention layer multiple “representation subspaces”
	* This can help the model focus on different positions, as with one attention head the model will likely learn to associate a given word with itself (Score between word_i and word_i is high)
![[Pasted image 20231106152234.png]]

* In the end, concatenate the encodings of the different heads
![[Pasted image 20231106152259.png]]

## Representing order using positional encoding
- We want a way to encode the order of words , so that the model can determine if
	- 1. among two words, which one came first
	- 2. among two words, what is the distance between them
- In the original paper, they add a second vector to each embedding
![[Pasted image 20231106152350.png]]

## Decoder
- Also has 6 layers
![[Pasted image 20231106152443.png]]

Simple view of decoder layer
![[Pasted image 20231106152532.png]]

Complex view of 2 layer decoder/encoder (with residual, and layer norms)
![[Pasted image 20231106152600.png]]

* The encoder-decoder attention sublayer:
    - the self-attention values (z) of the last encoder are used to calculate the self attention matrices K and V. The matrices are shared among all 6 layers of the decoder,
    - Thus at each layer, it creates its Queries matrix from the layer below it, but takes the Keys and Values matrix from the output of the encoder stack.
- Self-Attention in the decoder
	- To ensure that the decoder does not use future/unpredicted words, we set future words to -inf before the softmax that calculates the score
	- The decoder of course has feedback
![[Pasted image 20231106152710.png]]

## FInal linear and softmax layer
- - the decoder stack (in the image above) outputs floats, which need to be turned into words
- Linear layer is a simple fully connected neural network that projects into larger vector of logits
- The final vector is usually has size equal to the vocabulary (e.g. 10000 words)
- 

# Reference

