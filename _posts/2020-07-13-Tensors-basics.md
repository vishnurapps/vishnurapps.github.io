## Basics of tensor

A `torch.Tensor` is a multi-dimensional matrix containing elements of a single data type.
```python
import torch
a = torch.ones(3)
a
```
This produces a tensor with three elements.

```shell
tensor([1., 1., 1.])
```

- Although on the surface this example doesn’t differ much from a list of number objects, under the hood things are completely different.
- Python lists or tuples of numbers are collections of Python objects that are individually allocated in memory
- PyTorch tensors or NumPy arrays, on the other hand, are views over (typically) contiguous memory blocks containing unboxed C numeric types rather than Python objects.
- Each element is a 32-bit (4-byte) float in this case
- This means storing a 1D tensor of 1,000,000 float numbers will require exactly 4,000,000 contiguous bytes, plus a small overhead for the metadata (such as dimensions and numeric type)

```python
points = torch.tensor([4.0, 1.0, 5.0, 3.0, 2.0, 1.0])
points
```
This produces a two dimentional tensor.

```shell
tensor([[4., 1.],
        [5., 3.],
        [2., 1.]])
```

The shape of the tensor can be identified using the `shape` attribute

```python
points.shape
```
```shell
torch.Size([3, 2])
```
## Slicing

Like numpy the slicing can be done here as well.
```python
points[1:]
```
Here we wants from first row till end and all columns
```shell
tensor([[5., 3.],
        [2., 1.]])
```
The operation `points[1:,:]` also produce same result. This also means we want rows from 1 till end and all columns.

```python
points[1:,0]
```

This selects from first row till end and zeroth column.

```shell
tensor([5., 2.])
```

To get the second column we can use the below command.

```python
points[1:,1]
```

Here we can see that the last column was selected.

```shell
tensor([3., 1.])
```

If we want to add an extra dimension, we can use the index `None`

```python
points[None]
```

This is similar to unsqueeze

```shell
tensor([[[4., 1.],
         [5., 3.],
         [2., 1.]]])
```

## Unsqueeze

Unsqueeze is used to add one dimension.

```python
a = torch.tensor(range(10))
a, a.shape
```

We get a tensor with a single dimension

```shell
(tensor([0, 1, 2, 3, 4, 5, 6, 7, 8, 9]), torch.Size([10]))
```

To add a dimension in row we can use like below.

```python
b = a.unsqueeze(0)
b, b.shape
```
This will produce a row tensor with 10 columns

```shell
(tensor([[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]]), torch.Size([1, 10]))
```
To add a new dimension along column we can use like below.

```python
c = a.unsqueeze(1)
c, c.shape
```

This will produce a tensor with 10 rows and one column

```shell
(tensor([[0],
         [1],
         [2],
         [3],
         [4],
         [5],
         [6],
         [7],
         [8],
         [9]]),
 torch.Size([10, 1]))
```
