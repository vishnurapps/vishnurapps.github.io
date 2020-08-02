Sometimes while training a neural network, we need to stop the training when certain conditions are met. An example to that is achieving a particular accuracy.

Lets see how to do that.

```python
import tensorflow as tf

mnist = tf.keras.datasets.mnist
(x_train, y_train),(x_test, y_test) = mnist.load_data()
x_train = x_train/255.0
y_train = y_train/255.0

class myCallback(tf.keras.callbacks.Callback):
    def on_epoch_end(self, epoch, logs={}):
        if(logs.get('accuracy') > 0.09):
            print("Reached 9% accuracy so cancelling trining!")
            self.model.stop_training = True

callbacks = myCallback()
```
The first step to do this is creating a callback. A callback is a function that do some operation. It will be passed to the fit function of the model.
Here we have created a callback. Out callback will get executed on the end of all epochs. Tenserflow will check the value of the accuracy of the last epoch.
If the accuracy is greater that the specified limit, then the `stop_training` is set to true.

We have created an object of the callback. Next step is to pass it. Lets see how to pass it.

```python
model = tf.keras.models.Sequential([tf.keras.layers.Flatten(),
                                    tf.keras.layers.Dense(1024, activation = tf.nn.relu),
                                    tf.keras.layers.Dense(10, activation = tf.nn.softmax)
                                   ])

model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])

model.fit(x_train, y_train, epochs = 10, callbacks = [callbacks])
```

We have passed it in the argument called c callbacks of the fit functions. We can pass multiple callbacks like this to the fit function as a list.

When the condition we specified met, we got the below message.

```shell
Epoch 1/10
1872/1875 [============================>.] - ETA: 0s - loss: 0.0019 - accuracy: 0.0986Reached 9% accuracy so cancelling trining!
1875/1875 [==============================] - 9s 5ms/step - loss: 0.0018 - accuracy: 0.0987
```

There are some more things that we need to note here. These are not related to callbacks. One thing is the activation of the last layer.
We have used the `tf.nn.softmax` as activation. This is because we need a probablity as the output of the last layer.

Also the loss function used is `sparse_categorical_crossentropy` which is a version of `categorical_crossentropy`. This loss function is used as we are dealing with a classification problem.
