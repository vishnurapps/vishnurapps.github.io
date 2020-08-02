## Simple neural network to predict house price

Imagine if house pricing was as easy as a house costs 50k + 50k per bedroom, so that a 1 bedroom house costs 100k, a 2 bedroom house costs 150k etc.

In this way a house with 7 bedrooms will cost 350k + 50k = 400K

Lets make a neural network for that purpose.

```python
import tensorflow as tf
import numpy as np
from tensorflow import keras
```

We have imported the required libraries. Now we need to create a neural network. Here we are creating a neural network having a single layer.
Also we specified that the input is a tensor of shape 1. After creating the neural network we need to make an optimier and loss function.
We are using stocastic gradient descent as optimizer and mean square error as the loss function.

```python
model = tf.keras.Sequential([keras.layers.Dense(units = 1, input_shape = [1])])
model.compile(optimizer = 'sgd', loss = 'mean_squared_error')
```
We are generating our inputs and outputs. To converge faster, we are dividing the output by 100 while feeding to the network. We are telling the model to train on this data 1000 times.

```python
xs = np.array([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], dtype = float)
ys = np.array([100, 150, 200, 250, 300, 350], dtype = float)
model.fit(xs, ys/100, epochs = 1000)
print(model.predict([7.0]))
```

Here the neural network trains for 1000 epochs and we will get the below output.

```shell
Epoch 1/1000
6/6 [==============================] - 0s 17ms/sample - loss: 2.7517
Epoch 2/1000
6/6 [==============================] - 0s 308us/sample - loss: 1.2904
Epoch 3/1000
6/6 [==============================] - 0s 631us/sample - loss: 0.6139
Epoch 4/1000
6/6 [==============================] - 0s 442us/sample - loss: 0.3007
Epoch 5/1000
6/6 [==============================] - 0s 454us/sample - loss: 0.1556
....
Epoch 996/1000
6/6 [==============================] - 0s 323us/sample - loss: 2.1974e-05
Epoch 997/1000
6/6 [==============================] - 0s 421us/sample - loss: 2.1814e-05
Epoch 998/1000
6/6 [==============================] - 0s 433us/sample - loss: 2.1655e-05
Epoch 999/1000
6/6 [==============================] - 0s 520us/sample - loss: 2.1497e-05
Epoch 1000/1000
6/6 [==============================] - 0s 435us/sample - loss: 2.1340e-05
[[4.0066633]]
```

We have given scaled input to the network to train faster. As a result we get the output as `4.0066633`
