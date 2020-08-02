In the last blog we have seen that the accuracy was not getting more than 10%. This was happenning because the neural network we choosed for the process was not powerful enough.
To process the problems related to images, its better to use the CNN (Conventional Neural Networks).

As seen in last blog we are creating a callback class to stop training when accuracy reach 99.8%.

```python
import tensorflow as tf

class myCallback(tf.keras.callbacks.Callback):
    def on_epoc_end(self, epoch, logs={}):
        if(logs.get('accuracy') > 0.95):
            print("Reached accuracy 95% accuracyc so cancelling training!")
            self.model.stop_training = True
callbacks = myCallback()

mnist = tf.keras.datasets.mnist
(training_images, training_labels), (test_images, test_labels) = mnist.load_data()
```

Here we can see that we are reshaping the input to the neural network. Last time we just flattened and gave to network.
The first dimension is the number of images, second and third are the shape of the image and last one is the number of color channels.

```python
training_images = training_images.reshape(60000, 28, 28, 1)
training_images = training_images/255.0
test_images = test_images.reshape(10000, 28, 28, 1)
test_images = test_images/255.0
```

We can see that there are some special types of networks inside the Sequential. Conv2D is 2d convolutional layer. We need 32 channels in the output of this layer.
We want to use 3x3 kernels to make these channels. MaxPooling2D is used to select the maximum value from a 2x2 matrix. 

```python
model = tf.keras.models.Sequential([
    tf.keras.layers.Conv2D(32, (3,3), activation = 'relu', input_shape = (28, 28, 1)),
    tf.keras.layers.MaxPooling2D(2,2),
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dense(128, activation = 'relu'),
    tf.keras.layers.Dense(10, activation = 'softmax')
])

model.compile(optimizer = 'adam', loss = 'sparse_categorical_crossentropy', metrics = ['accuracy'])
model.fit(training_images, training_labels, epochs = 15, callbacks = [callbacks])
```
