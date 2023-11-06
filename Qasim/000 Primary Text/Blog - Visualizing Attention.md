---
tags:
  - "#RawInformation"
  - Article
completed: true
url: https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/
year: "{{year}}"
authors: Jay Allamar
---

# 3-Sentence Summary



# Details
- Consider the problem of language translation
- Traditionally, we would use 2 RNNs (an encoder and a decoder). The encoder would iteratively calculate a single hidden state, and the final hidden state calculated once all the input sequence had been fed in (also called the context vector) would be passed to the decoder
- A single context vector was insufficient
- So they invented attention.
- First, instead of passing the last hidden state of the encoding stage, the encoder passes all the hidden states to the decoder. Second, an attention decoder does an extra step before producing its output. In order to focus on the parts of the input that are relevant to this decoding time step, the decoder does the following:
	- Look at the set of encoder hidden states it received – each encoder hidden state is most associated with a certain word in the input sentence
	- assign a weight to each of the encoder hidden state (of all past time steps)
	- softmax the scores
	- multiply the respective hidden state by the weight.
	    - amplifying hidden states with high scores, and drowning out hidden states with low scores
	- feed in all these weighted hidden states of the encoder to the decoder
	- Note that the weights are different at each timestep
- 1. The attention decoder RNN takes in the embedding of the \<END> token, and an initial decoder hidden state.
2. The RNN processes its inputs, producing an output and a new hidden state vector (h4). The output is discarded.
3. Attention Step: We use the encoder hidden states and the h4 vector to calculate a context vector (C4) for this time step.
4. We concatenate h4 and C4 into one vector.
5. We pass this vector through a feedforward neural network (one trained jointly with the model).
6. The output of the feedforward neural networks indicates the output word of this time step.
7. Repeat for the next time steps



