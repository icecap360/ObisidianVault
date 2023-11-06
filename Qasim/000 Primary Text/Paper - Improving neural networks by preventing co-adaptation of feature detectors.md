---
tags:
  - "#RawInformation"
  - "#Paper"
  - "#Unread"
completed: false
title: Paper - Improving neural networks by preventing co-adaptation of feature detectors
authors: "Geoffrey E. Hinton, Nitish Srivastava, Alex Krizhevsky, Ilya Sutskever, Ruslan R. Salakhutdinov"
year: '2012/07'
url: "http://arxiv.org/abs/1207.0580"
citekey: "hintonImprovingNeuralNetworks2012"
aliases:
  - ""
  - "hintonImprovingNeuralNetworks2012"
---

# Short Summary

## Key Points

# Abstract
```
When a large feedforward neural network is trained on a small training set, it typically performs poorly on held-out test data. This "overfitting" is greatly reduced by randomly omitting half of the feature detectors on each training case. This prevents complex co-adaptations in which a feature detector is only helpful in the context of several other specific feature detectors. Instead, each neuron learns to detect a feature that is generally helpful for producing the correct answer given the combinatorially large variety of internal contexts in which it must operate. Random "dropout" gives big improvements on many benchmark tasks and sets new records for speech and object recognition.
```
# Details
Improving neural networks by preventing co-adaptation of feature detectors
	- Often, overfitting can happen because the feature detector learns features that are only helpful in the context of other specific features
	- We want each neuron to learn something generally helpful, that is useful with many of the previous internal contexts
	- There are many possible neural networks/weight settings that give the same predictions, sometimes perfect predictions on the training set
- Another way to view the dropout procedure is as a very efficient way of performing model averaging with neural networks. A good way to reduce the error on the test set is to average the predictions produced by a very large number of different networks.
	- Random dropout makes it possible to train a huge number of different networks in a reasonable time.
	- At test time, we use the “mean network” that contains all of the hidden units but with their outgoing weights halved to compensate for the fact that twice as many of them are active
- Theoretically, for single hidden layer networks, the mean network will always be better than any individual dropout network. Also, the search space of dropout (how many models in esemble) is exponential.
	- - In networks with a single hidden layer of N units and a “softmax” output layer for computing the probabilities of the class labels, using the mean network is exactly equivalent to taking the geometric mean of the probability distributions over labels predicted by all 2 N possible networks. Assuming the dropout networks do not all make identical predictions, the prediction of the mean network is guaranteed to assign a higher log probability to the correct answer than the mean of the log probabilities assigned by the individual dropout networks (2).
	- Similarly, for regression with linear output units, the squared error of the mean network is always better than the average of the squared errors of the dropout networks.
- Dropout as a “mixture of experts”
    - It is also possible to adapt the individual dropout probability of each hidden or input unit by comparing the average performance on a validation set with the average performance when the unit is present. This makes the method work slightly better. For datasets in which the required input-output mapping has a number of fairly different regimes, performance can probably be further improved by making the dropout probabilities be a learned function of the input, thus creating a statistically efficient “mixture of experts” (13) in which there are combinatorially many experts, but each parameter gets adapted on a large fraction of the training data
- how Dropout compares to other classical baysian models
    - Dropout is considerably simpler to implement than Bayesian model averaging which weights each model by its posterior probability given the training data. For complicated model classes, like feedforward neural networks, Bayesian methods typically use a Markov chain Monte Carlo method to sample models from the posterior distribution (14). By contrast, dropout with a probability of 0.5 assumes that all the models will eventually be given equal importance in the combination but the learning of the shared weights takes this into account.
- It is a better regukarizer then l2 norms
    - Dropout can be seen as an extreme form of bagging in which each model is trained on a single case and each parameter of the model is very strongly regularized by sharing it with the corresponding parameter in all the other models. This is a much better regularizer than the standard method of shrinking parameters towards zero.

# Comments and Implications

# Links
[@hintonImprovingNeuralNetworks2012]