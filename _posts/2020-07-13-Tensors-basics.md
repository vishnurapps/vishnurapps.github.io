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
