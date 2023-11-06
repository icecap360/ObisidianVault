# Key Ideas
* [[NN Initialization - Xavier]] initialization is for sigmoig/tanh, He is for reli
* In HE initialization, they find that the variance of the Lth layer is a product of the previous layers, so they attempt to keep the variance=1 across all layers so that variance does not magnify or decay
* A good neural network will keep the variance of the activations constant across layers
	* [[Neural Network - Keep variance of activations constant]]
* 