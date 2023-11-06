---
tags:
  - "#Ideas"
topics: "[[Deep Learning Perspectives]]"
---
- Ways to interpret matrix multiplication with a vector



- - Linear Transformation of vector: Each column of the matrix defines where the corresponding unit vector in the input space is mapped in the output space.
    - Each layer can be interpreted as geometrically transforming the previous layer activations
- - Composition of column vectors: A vector takes a linear combination of the Layer n takes combinations of the column vector Wn to create the activations of layer n+1.
    - Layer n can thus be seen as a coordinate vector of a subspace defined by matrix Wn
    - In this way a neural network can be seen as changing the basis into a nonlinear subspace
    - Each neuron in layer n+1 assigns weights/importance for each of the previous neurons
- - Systems of Linear Equations: Each of the elements (a_i) in layer l, are summed together, each a_i being weighted by ith column in w_i.
    - Each weight is a importance assigned to the ith neuron
- - Change of basis: matrices are used to change the basis of vectors
    - Each weight matrix transforms the previous layer into a new basis. Thus trainng can be seen as finding the optimal basis that will represent/transform the input vector space into an output vector space.
    - Statistical:
        - A neural network is about transforming the input subspace in a space where linear regression or classificartion is doable
        - Each layers transforms into a different feature space, making it easier for subsequent layers to learn higher-level representations and patterns
    - Remember how to read a column of change of basis vector: Let matrix P transform a coordinate vector in B space to C space (s_B * P_(B → C) = s_C). For example basis vector of B, b_1, is mapped to basis bector of C by c_1
![[Pasted image 20231106154355.png]]
- - Row-Column Dot Products:
    - The activations of neurons in a hidden layer can be seen as dot products between the weights (rows of the weight matrix) and the input data (the vector).
    - Each neuron focuses on specific aspects or features of the input data, and the dot products determine their importance.
    - The activation can be seen as the dot product or similarity between the weights and the input vector. The more similarity between the weights and the input vector greater the activation, the more orthogonality the lower the activation.
- - Projection: In some applications, matrix-vector multiplication can be used to project one vector onto another, especially when dealing with orthogonal matrices or orthogonal projections.
    - Project the input data into a different space
    - The choice of activation functions (e.g., ReLU, sigmoid) can influence the nature of this projection.
- Eigenvalues
    - Analyzing the eigenvalues and eigenvectors of weight matrices can provide insights into how information is transformed in hidden layers.
    - Larger eigenvalues tell you which directions the matrix enhances or which direction contains most of the variance in the matrix.
    - help understand which directions the matrix scales

### Reading matrices
- Weigth matrix
    - Each neuron in a given layer is a column in the weight matrix (contains a weight for each of the previuos layers)
# Comments and Links
- 
# Reference