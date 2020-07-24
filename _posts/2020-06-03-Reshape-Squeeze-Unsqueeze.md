## Reshape
There are many ways to change the shape of tensor. One way to do that is by using a reshape function. Lets create a tensor of shape 4x3

```python
t = torch.rand(4,3)
t
```

This will generate a 4x3 tensor with random values.

```shell
tensor([[0.9748, 0.9240, 0.0625],
        [0.4856, 0.3671, 0.2967],
        [0.6812, 0.4086, 0.7151],
        [0.0572, 0.5907, 0.1290]])
```

Now lets reshape it to a tensor having one row and 12 elements in each row. 

```python
t.reshape(1,12)
```
The result is

```shell
tensor([[0.9748, 0.9240, 0.0625, 0.4856, 0.3671, 0.2967, 0.6812, 0.4086, 0.7151,
         0.0572, 0.5907, 0.1290]])
```

Lets try to reshape to 3 rows with each row having 4 elements

```python
t.reshape(3,4)
```

The result is 

```shell
tensor([[0.9748, 0.9240, 0.0625, 0.4856],
        [0.3671, 0.2967, 0.6812, 0.4086],
        [0.7151, 0.0572, 0.5907, 0.1290]])
```

Here only the shape is changing the underlying data remains the same.

Lets try some more reshapes.

```python
t.reshape(2,6)
```
```shell
tensor([[0.9748, 0.9240, 0.0625, 0.4856, 0.3671, 0.2967],
        [0.6812, 0.4086, 0.7151, 0.0572, 0.5907, 0.1290]])
```

```python
t.reshape(1,2,6)
```
```shell
tensor([[[0.9748, 0.9240, 0.0625, 0.4856, 0.3671, 0.2967],
         [0.6812, 0.4086, 0.7151, 0.0572, 0.5907, 0.1290]]])
```

```python
t.reshape(3,2,2)
```
```shell
tensor([[[0.9748, 0.9240],
         [0.0625, 0.4856]],

        [[0.3671, 0.2967],
         [0.6812, 0.4086]],

        [[0.7151, 0.0572],
         [0.5907, 0.1290]]])
```

## Squeeze

Squeezing a tensor removes the dimensions or axes that have a length of one.

```python
u = t.reshape(3,4,1)
print(u.shape)
v= u.squeeze()
print(v.shape)
```
Here we can see that the third axis has a length of 1 and its removed.
```shell
torch.Size([3, 4, 1])
torch.Size([3, 4])
```

Now lets see what happens if the dimension is in the middle has a length 1.

```python
u = t.reshape(3,1,4)
print(u.shape)
v= u.squeeze()
print(v.shape)
```
This time also the axis with legth 1 is removed
```shell
torch.Size([3, 1, 4])
torch.Size([3, 4])
```

This happends if the axis is at the begining.
```python
u = t.reshape(1,3,4)
print(u.shape)
v= u.squeeze()
print(v.shape)
```
```shell
torch.Size([1, 3, 4])
torch.Size([3, 4])
```

Lets see what happens if there are multiple axis with length 1.

```python
u = t.reshape(1,1,3,4)
print(u.shape)
v= u.squeeze()
print(v.shape)
```
We gets the same result.

```shell
torch.Size([1, 1, 3, 4])
torch.Size([3, 4])
```
This happens irrespective of the number of axis with length one.

```python
u = t.reshape(1,3,1,4)
print(u.shape)
v= u.squeeze()
print(v.shape)
```
```shell
torch.Size([1, 3, 1, 4])
torch.Size([3, 4])
```

```python
u = t.reshape(1,3,1,4,1,1,1)
print(u.shape)
v= u.squeeze()
print(v.shape)
```
```shell
torch.Size([1, 3, 1, 4, 1, 1, 1])
torch.Size([3, 4])
```

```python
```
```shell
```

```python
```
```shell
```

```python
```
```shell
```

```python
```
```shell
```
