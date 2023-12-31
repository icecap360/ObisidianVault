---
tags:
  - "#Ideas"
  - "#Software"
topics: "[[PyTorch]]"
---
- All optimization logic is encapsulated in the optimizer.
    - requires registering the model parameters and learning rate
    - `optimizer = torch.optim.SGD(model.parameters(), lr=learning_rate)`
    - Inside the training loop, optimization happens in three steps:
	- Make sure you call `scheduler.step` at start of the epoch loop so your learning rate is updated. Be careful not to write it in the batch loop, otherwise your learning rate may be updated at the 10th batch rather than 10th epoch.
        - Call `optimizer.zero_grad()` to reset the gradients of model parameters. Gradients by default add up; to prevent double-counting, we explicitly zero them at each iteration.
        - Backpropagate the prediction loss with a call to `loss.backward()`. PyTorch deposits the gradients of the loss w.r.t. each parameter.
        - Once we have our gradients, we call `optimizer.step()` to adjust the parameters by the gradients collected in the backward pass
    - 

# Syntax
```
def train_loop(dataloader, model,loss_fn,optimizer):
    size = len(dataloader.dataset)
    # Set the model to training mode - important for batch normalization and dropout layers
    # Unnecessary in this situation but added for best practices
	model.train()
    for batch, (X, y) in enumerate(dataloader):
        # Compute prediction and loss
        pred = model(X)
        loss =loss_fn(pred, y)

        # Backpropagation
        loss.backward()
		optimizer.step()
		optimizer.zero_grad()

        if batch % 100 == 0:
            loss, current = loss.item(), (batch + 1) * len(X)
            print(f"loss: {loss:>7f}  [{current:>5d}/{size:>5d}]")


def test_loop(dataloader, model,loss_fn):
    # Set the model to evaluation mode - important for batch normalization and dropout layers
    # Unnecessary in this situation but added for best practices
	model.eval()
    size = len(dataloader.dataset)
    num_batches = len(dataloader)
    test_loss, correct = 0, 0

    # Evaluating the model with torch.no_grad() ensures that no gradients are computed during test mode
    # also serves to reduce unnecessary gradient computations and memory usage for tensors with requires_grad=True
    withtorch.no_grad():
	for X, y in dataloader:
		pred = model(X)
		test_loss +=loss_fn(pred, y).item()
		correct += (pred.argmax(1) == y).type(torch.float).sum().item()
    test_loss /= num_batches
    correct /= size
    print(f"Test Error: \n Accuracy: {(100*correct):>0.1f}%, Avg loss: {test_loss:>8f} \n")
```

# Examples
- Different learning rates for different layers
```
optimiser = torch.optim.SGD([{"params": Net.fc1.parameters(), 'lr' : 0.001, "momentum" : 0.99}, {"params": Net.fc2.parameters()}], lr = 0.01, momentum = 0.9)
```

# Comments and Links
- 
# Reference