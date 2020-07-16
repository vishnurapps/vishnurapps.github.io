To use TPU we need to check the hardware and define the distribution strategy.

```python
# Detect hardware, return appropriate distribution strategy
try:
    # TPU detection. No parameters necessary if TPU_NAME environment variable is
    # set: this is always the case on Kaggle.
    tpu = tf.distribute.cluster_resolver.TPUClusterResolver()
    print('Running on TPU ', tpu.master())
except ValueError:
    tpu = None

if tpu:
    tf.config.experimental_connect_to_cluster(tpu)
    tf.tpu.experimental.initialize_tpu_system(tpu)
    strategy = tf.distribute.experimental.TPUStrategy(tpu)
else:
    # Default distribution strategy in Tensorflow. Works on CPU and single GPU.
    strategy = tf.distribute.get_strategy()

print("REPLICAS: ", strategy.num_replicas_in_sync)
```

This will return the master tpu and replicas

```shell
Running on TPU  grpc://10.0.0.2:8470
REPLICAS:  8
```

The AUTOTUNE is for optimization, BATCH_SIZE is determined from  

```python
# For tf.dataset
AUTO = tf.data.experimental.AUTOTUNE

# Data access

GCS_DS_PATH = KaggleDatasets().get_gcs_path('alaska2-image-steganalysis')


# Configuration
EPOCHS = 30
BATCH_SIZE = 32 * strategy.num_replicas_in_sync
```

