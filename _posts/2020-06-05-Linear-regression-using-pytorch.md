## Input Data
We are given a small dataset. The inputs represent temperature, rainfall, humidity. And output is the yield of orange and apple.

```python
# Input (temp, rainfall, humidity)
inputs = np.array([[73, 67, 43], [91, 88, 64], [87, 134, 58], 
                   [102, 43, 37], [69, 96, 70], [73, 67, 43], 
                   [91, 88, 64], [87, 134, 58], [102, 43, 37], 
                   [69, 96, 70], [73, 67, 43], [91, 88, 64], 
                   [87, 134, 58], [102, 43, 37], [69, 96, 70]], 
                  dtype='float32')

# Targets (apples, oranges)
targets = np.array([[56, 70], [81, 101], [119, 133], 
                    [22, 37], [103, 119], [56, 70], 
                    [81, 101], [119, 133], [22, 37], 
                    [103, 119], [56, 70], [81, 101], 
                    [119, 133], [22, 37], [103, 119]], 
                   dtype='float32')

inputs = torch.from_numpy(inputs)
targets = torch.from_numpy(targets)
```

The last two lines convert the inputs to tensors.

```python
inputs
```
The inputs looks like this.

```shell
tensor([[ 73.,  67.,  43.],
        [ 91.,  88.,  64.],
        [ 87., 134.,  58.],
        [102.,  43.,  37.],
        [ 69.,  96.,  70.],
        [ 73.,  67.,  43.],
        [ 91.,  88.,  64.],
        [ 87., 134.,  58.],
        [102.,  43.,  37.],
        [ 69.,  96.,  70.],
        [ 73.,  67.,  43.],
        [ 91.,  88.,  64.],
        [ 87., 134.,  58.],
        [102.,  43.,  37.],
        [ 69.,  96.,  70.]])
```
## Dataset and DataLoader 
We'll create a `TensorDataset`, which allows access to rows from `inputs` and `targets` as tuples, and provides standard APIs for working with many different types of datasets in PyTorch.

```python
from torch.utils.data import TensorDataset
# Define dataset
train_ds = TensorDataset(inputs, targets)
train_ds[0:3]
```

We can see the first three elements of the dataset.

```shell
(tensor([[ 73.,  67.,  43.],
         [ 91.,  88.,  64.],
         [ 87., 134.,  58.]]), tensor([[ 56.,  70.],
         [ 81., 101.],
         [119., 133.]]))
```

The TensorDataset allows us to access a small section of the training data using the array indexing notation ([0:3] in the above code). It returns a tuple (or pair), in which the first element contains the input variables for the selected rows, and the second contains the targets.

We'll also create a `DataLoader`, which can split the data into batches of a predefined size while training. It also provides other utilities like shuffling and random sampling of the data.

```python
from torch.utils.data import DataLoader
# Define data loader
batch_size = 5
train_dl = DataLoader(train_ds, batch_size, shuffle=True)
```
The data loader is typically used in a for-in loop. Let's look at an example.

```shell
for xb, yb in train_dl:
    print(xb)
    print(yb)
    break
tensor([[ 73.,  67.,  43.],
        [ 91.,  88.,  64.],
        [ 87., 134.,  58.],
        [ 91.,  88.,  64.],
        [ 87., 134.,  58.]])
tensor([[ 56.,  70.],
        [ 81., 101.],
        [119., 133.],
        [ 81., 101.],
        [119., 133.]])
```

In each iteration, the data loader returns one batch of data, with the given batch size. If shuffle is set to True, it shuffles the training data before creating batches. Shuffling helps randomize the input to the optimization algorithm, which can lead to faster reduction in the loss.

## Loss Function
Instead of defining a loss function manually, we can use the built-in loss function mse_loss.
The nn.functional package contains many useful loss functions and several other utilities.

```python
# Import nn.functional
import torch.nn.functional as F
# Define loss function
loss_fn = F.mse_loss
```
Let's compute the loss for the current predictions of our model.
```python
loss = loss_fn(model(inputs), targets)
print(loss)
```
The loss is calculated.
```shell
tensor(17562.2832, grad_fn=<MseLossBackward>)
```
Some examples

```python
a = torch.tensor([1,2], dtype=torch.float32)
b = torch.tensor([3,6], dtype=torch.float32)
loss_fn(a,b)
```
This will output `10` which is `mean(sum((3-1)^2,(6-2)^))`. That is `mean(sum(4,16))`. It reduces to `20/2` and it is `10` 

Another example

```python
a = torch.tensor([[1,2],[6,8]], dtype=torch.float32)
b = torch.tensor([[3,5],[3,5]], dtype=torch.float32)
loss_fn(a,b)
```
This will output `7.75` which is `mean(sum(2^2,3^2,3^2,3^2))`. It reduced to `mean(4,9,9,9)`. It is `31\4 = 7.75`

## Optimizer
Instead of manually manipulating the model's weights & biases using gradients, we can use the optimizer `optim.SGD`. SGD stands for `stochastic gradient descent`. It is called stochastic because samples are selected in `batches` (often with random shuffling) instead of as a single group.

```python
# Define optimizer
opt = torch.optim.SGD(model.parameters(), lr=1e-5)
```

Note that `model.parameters()` is passed as an argument to optim.SGD, so that the optimizer knows which matrices should be modified during the update step. Also, we can specify a learning rate which controls the amount by which the parameters are modified.

## Train the model
We are now ready to train the model. We'll follow the exact same process to implement gradient descent:

- Generate predictions

- Calculate the loss

- Compute gradients w.r.t the weights and biases

- Adjust the weights by subtracting a small quantity proportional to the gradient

- Reset the gradients to zero

Let's define a utility function fit which trains the model for a given number of epochs.

```python
# Utility function to train the model
def fit(num_epochs, model, loss_fn, opt, train_dl):
    
    # Repeat for given number of epochs
    for epoch in range(num_epochs):
        
        # Train with batches of data
        for xb,yb in train_dl:
            
            # 1. Generate predictions
            pred = model(xb)
            
            # 2. Calculate loss
            loss = loss_fn(pred, yb)
            
            # 3. Compute gradients
            loss.backward()
            
            # 4. Update parameters using gradients
            opt.step()
            
            # 5. Reset the gradients to zero
            opt.zero_grad()
        
        # Print the progress
        if (epoch+1) % 10 == 0:
            print('Epoch [{}/{}], Loss: {:.4f}'.format(epoch+1, num_epochs, loss.item()))
```

Some things to note above:

- We use the data loader defined earlier to get batches of data for every iteration.

- Instead of updating parameters (weights and biases) manually, we use opt.step to perform the update, and opt.zero_grad to reset the gradients to zero.

- We've also added a log statement which prints the loss from the last batch of data for every 10th epoch, to track the progress of training. loss.item returns the actual value stored in the loss tensor.

Let's train the model for 100 epochs.

```python
fit(100, model, loss_fn, opt)
```
