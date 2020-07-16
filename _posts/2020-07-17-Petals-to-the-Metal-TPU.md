## Distribution Strategy
- A TPU has eight different cores and each of these cores acts as its own accelerator. (A TPU is sort of like having eight GPUs in one machine.) 
- We tell TensorFlow how to make use of all these cores at once through a distribution strategy. 
- Run the following cell to create the distribution strategy that we'll later apply to our model.

```python
# Detect TPU, return appropriate distribution strategy
try:
    tpu = tf.distribute.cluster_resolver.TPUClusterResolver() 
    print('Running on TPU ', tpu.master())
except ValueError:
    tpu = None

if tpu:
    tf.config.experimental_connect_to_cluster(tpu)
    tf.tpu.experimental.initialize_tpu_system(tpu)
    strategy = tf.distribute.experimental.TPUStrategy(tpu)
else:
    strategy = tf.distribute.get_strategy() 

print("REPLICAS: ", strategy.num_replicas_in_sync)
```
- TensorFlow will distribute training among the eight TPU cores by creating eight different replicas of your model.

## Loading the Competition Data
- When used with TPUs, datasets are often serialized into TFRecords. 
- This is a format convenient for distributing data to each of the TPUs cores.
```python
ds_train = get_training_dataset()
ds_valid = get_validation_dataset()
ds_test = get_test_dataset()

print("Training:", ds_train)
print ("Validation:", ds_valid)
print("Test:", ds_test)
```
These are tf.data.Dataset objects. You can think about a dataset in TensorFlow as a stream of data records.

```shell
Training: <PrefetchDataset shapes: ((None, 512, 512, 3), (None,)), types: (tf.float32, tf.int32)>
Validation: <PrefetchDataset shapes: ((None, 512, 512, 3), (None,)), types: (tf.float32, tf.int32)>
Test: <PrefetchDataset shapes: ((None, 512, 512, 3), (None,)), types: (tf.float32, tf.string)>
```

## Define Model

- The distribution strategy we created earlier contains a context manager, strategy.scope. 
- This context manager tells TensorFlow how to divide the work of training among the eight TPU cores. 
- When using TensorFlow with a TPU, it's important to define your model in a strategy.scope() context.

```python
with strategy.scope():
    pretrained_model = tf.keras.applications.VGG16(
        weights='imagenet',
        include_top=False ,
        input_shape=[*IMAGE_SIZE, 3]
    )
    pretrained_model.trainable = False
    
    model = tf.keras.Sequential([
        # To a base pretrained on ImageNet to extract features from images...
        pretrained_model,
        # ... attach a new head to act as a classifier.
        tf.keras.layers.GlobalAveragePooling2D(),
        tf.keras.layers.Dense(len(CLASSES), activation='softmax')
    ])
    model.compile(
        optimizer='adam',
        loss = 'sparse_categorical_crossentropy',
        metrics=['sparse_categorical_accuracy'],
    )

model.summary()
```

The summary looke like below

```shell
Downloading data from https://storage.googleapis.com/tensorflow/keras-applications/vgg16/vgg16_weights_tf_dim_ordering_tf_kernels_notop.h5
58892288/58889256 [==============================] - 1s 0us/step
Model: "sequential"
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
vgg16 (Model)                (None, 16, 16, 512)       14714688  
_________________________________________________________________
global_average_pooling2d (Gl (None, 512)               0         
_________________________________________________________________
dense (Dense)                (None, 104)               53352     
=================================================================
Total params: 14,768,040
Trainable params: 53,352
Non-trainable params: 14,714,688
_________________________________________________________________
```

## Training

After defining a few parameters, we're good to go.

```python
# Define the batch size. This will be 16 with TPU off and 128 (=16*8) with TPU on
BATCH_SIZE = 16 * strategy.num_replicas_in_sync

# Define training epochs
EPOCHS = 12
STEPS_PER_EPOCH = NUM_TRAINING_IMAGES // BATCH_SIZE

history = model.fit(
    ds_train,
    validation_data=ds_valid,
    epochs=EPOCHS,
    steps_per_epoch=STEPS_PER_EPOCH,
)
```

We train for 12 epochs

```shell
Epoch 1/12
99/99 [==============================] - 27s 269ms/step - sparse_categorical_accuracy: 0.0889 - loss: 4.1105 - val_sparse_categorical_accuracy: 0.1334 - val_loss: 3.8827
Epoch 2/12
99/99 [==============================] - 18s 177ms/step - sparse_categorical_accuracy: 0.1736 - loss: 3.7529 - val_sparse_categorical_accuracy: 0.1915 - val_loss: 3.6397
Epoch 3/12
99/99 [==============================] - 17s 174ms/step - sparse_categorical_accuracy: 0.2095 - loss: 3.5472 - val_sparse_categorical_accuracy: 0.2333 - val_loss: 3.4456
Epoch 4/12
99/99 [==============================] - 16s 166ms/step - sparse_categorical_accuracy: 0.2438 - loss: 3.3630 - val_sparse_categorical_accuracy: 0.2581 - val_loss: 3.2910
Epoch 5/12
99/99 [==============================] - 17s 168ms/step - sparse_categorical_accuracy: 0.2703 - loss: 3.2099 - val_sparse_categorical_accuracy: 0.2799 - val_loss: 3.1603
Epoch 6/12
99/99 [==============================] - 16s 165ms/step - sparse_categorical_accuracy: 0.2911 - loss: 3.0828 - val_sparse_categorical_accuracy: 0.2985 - val_loss: 3.0463
Epoch 7/12
99/99 [==============================] - 16s 166ms/step - sparse_categorical_accuracy: 0.3198 - loss: 2.9652 - val_sparse_categorical_accuracy: 0.3281 - val_loss: 2.9469
Epoch 8/12
99/99 [==============================] - 16s 165ms/step - sparse_categorical_accuracy: 0.3370 - loss: 2.8811 - val_sparse_categorical_accuracy: 0.3413 - val_loss: 2.8547
Epoch 9/12
99/99 [==============================] - 17s 168ms/step - sparse_categorical_accuracy: 0.3636 - loss: 2.7800 - val_sparse_categorical_accuracy: 0.3677 - val_loss: 2.7770
Epoch 10/12
99/99 [==============================] - 16s 164ms/step - sparse_categorical_accuracy: 0.3785 - loss: 2.7033 - val_sparse_categorical_accuracy: 0.3772 - val_loss: 2.7042
Epoch 11/12
99/99 [==============================] - 16s 165ms/step - sparse_categorical_accuracy: 0.3997 - loss: 2.6282 - val_sparse_categorical_accuracy: 0.3944 - val_loss: 2.6368
Epoch 12/12
99/99 [==============================] - 16s 165ms/step - sparse_categorical_accuracy: 0.4165 - loss: 2.5623 - val_sparse_categorical_accuracy: 0.4071 - val_loss: 2.5748
```

## Plotting the loss

```python
display_training_curves(
    history.history['loss'],
    history.history['val_loss'],
    'loss',
    211,
)
display_training_curves(
    history.history['sparse_categorical_accuracy'],
    history.history['val_sparse_categorical_accuracy'],
    'accuracy',
    212,
)
```

## Predict output

```python
test_ds = get_test_dataset(ordered=True)

print('Computing predictions...')
test_images_ds = test_ds.map(lambda image, idnum: image)
probabilities = model.predict(test_images_ds)
predictions = np.argmax(probabilities, axis=-1)
print(predictions)
```

This will produce the predictions

```shell
Computing predictions...
[ 67  28 103 ...  48 102  48]
```

## Create submission file

```python
print('Generating submission.csv file...')

# Get image ids from test set and convert to unicode
test_ids_ds = test_ds.map(lambda image, idnum: idnum).unbatch()
test_ids = next(iter(test_ids_ds.batch(NUM_TEST_IMAGES))).numpy().astype('U')

# Write the submission file
np.savetxt(
    'submission.csv',
    np.rec.fromarrays([test_ids, predictions]),
    fmt=['%s', '%d'],
    delimiter=',',
    header='id,label',
    comments='',
)

# Look at the first few predictions
!head submission.csv
```

This will generate the submission file

```shell
Generating submission.csv file...
id,label
252d840db,67
1c4736dea,28
c37a6f3e9,103
00e4f514e,103
59d1b6146,53
8d808a07b,53
aeb67eefb,103
53cfc6586,48
aaa580243,67
```
