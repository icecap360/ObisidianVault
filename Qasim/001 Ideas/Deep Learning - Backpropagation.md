---
tags:
  - "#Ideas"
topics:
---
# Key Ideas 
- For minibatches, backpropogation is basically compputing the gradient on each sample, and then averaging it during update
    - for biases, often for the sake of computational efficiency they just do np.sum instead of np.mean (saves on a division)
    - if you look at the formula for weight update, you will see that there is a matrix multiplication instead of the average. This is because the size of the matrices is (n_weight, batchsize)\*(batchsize, n_weights), so it is really the same thing
- use the computational understanding of linear algebra when working with backprop

# Talk with Kiernan
![[Pasted image 20231106154834.png]]

![[Pasted image 20231106154900.png]]


- - In the above images, assume that the neural network is 2 inputs, 3 outpurts, no activation funciton, and no loss function (so that optimal result is for the network to predict 0s)
- Assume that the network predicts [1,1,1].T, then the loss is [1,1,1]
- The gradient of the loss with respect to the 3 neuron output layer is therefore also [1,1,1], as increasing any output neuron by 1 causes the loss function to increase by 1.
- - The forward pass has the following form:
    - z1=z0\*W, where W is 3 by 2 (3 neurons with 2 weights each)
- The gradient of the loss with respect to the 2 input neurons will be
    - dL_dZ0 = W.T\*dL_dZ1 = \[3,3].T
    - Thus increasing any of the Z0 neurons by 1 will cause the loss to increase by 3
- Notice how the gradient with respect to z0 is gradient with resepect to z1 * the weight, which is opposite of the forward propgation.
- Notice how the matrix multiplication that calculates the gradient with respect to Z0 encapsulates a summation; we take a weighted sum of the partial derivatives of the neurons in the next layer (where the weight applied to the partial derivative is the weight between neurons).
```
gradient_z0 = []
for neuron in layer_0:
    sum =0
    for next_neuron in layer_1:
        sum += weight(neuron, next_neuron)*next_neuron.gradient
    neuron.gradient = sum
    gradient_z0.append(sum)
# in our code for assignment 1, we had to sum explicitly, we had to add up these sums at the layer level because each node did not have acccess to the activations/weights of other neurons
# this gives intuitions: (1) - the larger the weight, the more the gradient of that weight is able to propagate
```

- Notice how the gradient with respect to the activtions (and pre-activation outputs) of the neurons is shape nx1, which makes sense because the outputs are also nx1. This was a misunderstanding I had, the gradients are really interpretable.
![[Pasted image 20231106155135.png]]

- In the above image we can begin to understand when a gradient will populate
    - for a particular neuron , the gradient of weights going into it is computed by: activations of previous layer (nx1 vector) * gradient of loss with respect to pre-activation of the neuron (scalar).
    In the above equation, dL_dWl = n_i x 1 * 1 x n_i-1 = n_i x n_i-1 shape matrix, so the gradient for weights comping into a neuron (each row) is the previous layers activation scaled by gradient with respect to the neuron’s pre-activation
    - gradient with respect to a given layer is computer by multiplying the forward weights by the gradient with respect to the next layer, and then scaling each neuron by derivative of activation at current pre-activations




# Comments and Links
- 
# Reference