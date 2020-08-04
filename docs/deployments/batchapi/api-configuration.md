# API configuration

_WARNING: you are on the master branch, please refer to the docs on the branch that matches your `cortex version`_

Once your model is [exported](exporting.md) and you've implemented a [Predictor](predictors.md), you can configure your API via a yaml file (typically named `cortex.yaml`).

Reference the section below which corresponds to your Predictor type: [Python](#python-predictor), [TensorFlow](#tensorflow-predictor), or [ONNX](#onnx-predictor).

## Python Predictor

```yaml
- name: <string>  # API name (required)
  kind: BatchAPI
  predictor:
    type: python
    path: <string>  # path to a python file with a PythonPredictor class definition, relative to the Cortex root (required)
    processes_per_replica: <int>  # the number of parallel serving processes to run on each replica (default: 1)
    threads_per_process: <int>  # the number of threads per process (default: 1)
    config: <string: value>  # arbitrary dictionary passed to the constructor of the Predictor (optional)
    python_path: <string>  # path to the root of your Python folder that will be appended to PYTHONPATH (default: folder containing cortex.yaml)
    image: <string> # docker image to use for the Predictor (default: cortexlabs/python-predictor-cpu or cortexlabs/python-predictor-gpu based on compute)
    env: <string: string>  # dictionary of environment variables
  networking:
    endpoint: <string>  # the endpoint for the API (aws only) (default: <api_name>)
    api_gateway: public | none  # whether to create a public API Gateway endpoint for this API (if not, the load balancer will be accessed directly) (default: public)
  compute:
    cpu: <string | int | float>  # CPU request per replica, e.g. 200m or 1 (200m is equivalent to 0.2) (default: 200m)
    gpu: <int>  # GPU request per replica (default: 0)
    inf: <int> # Inferentia ASIC request per replica (default: 0)
    mem: <string>  # memory request per replica, e.g. 200Mi or 1Gi (default: Null)
```

See additional documentation for [parallelism](parallelism.md), [compute](compute.md), [networking](networking.md), [prediction monitoring](prediction-monitoring.md), and [overriding API images](system-packages.md).

## TensorFlow Predictor

```yaml
- name: <string>  # API name (required)
  kind: BatchAPI
  predictor:
    type: tensorflow
    path: <string>  # path to a python file with a TensorFlowPredictor class definition, relative to the Cortex root (required)
    model_path: <string>  # S3 path to an exported model (e.g. s3://my-bucket/exported_model) (either this or 'models' must be provided)
    signature_key: <string>  # name of the signature def to use for prediction (required if your model has more than one signature def)
    models:  # use this when multiple models per API are desired (either this or 'model_path' must be provided)
      - name: <string> # unique name for the model (e.g. iris-classifier) (required)
        model_path: <string>  # S3 path to an exported model (e.g. s3://my-bucket/exported_model) (required)
        signature_key: <string>  # name of the signature def to use for prediction (required if your model has more than one signature def)
      ...
    processes_per_replica: <int>  # the number of parallel serving processes to run on each replica (default: 1)
    threads_per_process: <int>  # the number of threads per process (default: 1)
    config: <string: value>  # arbitrary dictionary passed to the constructor of the Predictor (optional)
    python_path: <string>  # path to the root of your Python folder that will be appended to PYTHONPATH (default: folder containing cortex.yaml)
    image: <string> # docker image to use for the Predictor (default: cortexlabs/tensorflow-predictor)
    tensorflow_serving_image: <string> # docker image to use for the TensorFlow Serving container (default: cortexlabs/tensorflow-serving-gpu or cortexlabs/tensorflow-serving-cpu based on compute)
    env: <string: string>  # dictionary of environment variables
  networking:
    endpoint: <string>  # the endpoint for the API (aws only) (default: <api_name>)
    api_gateway: public | none  # whether to create a public API Gateway endpoint for this API (if not, the load balancer will be accessed directly) (default: public)
  compute:
    cpu: <string | int | float>  # CPU request per replica, e.g. 200m or 1 (200m is equivalent to 0.2) (default: 200m)
    gpu: <int>  # GPU request per replica (default: 0)
    inf: <int> # Inferentia ASIC request per replica (default: 0)
    mem: <string>  # memory request per replica, e.g. 200Mi or 1Gi (default: Null)
```

See additional documentation for [parallelism](parallelism.md), [compute](compute.md), [networking](networking.md), [prediction monitoring](prediction-monitoring.md), and [overriding API images](system-packages.md).

## ONNX Predictor

```yaml
- name: <string>  # API name (required)
  kind: BatchAPI
  predictor:
    type: onnx
    path: <string>  # path to a python file with an ONNXPredictor class definition, relative to the Cortex root (required)
    model_path: <string>  # S3 path to an exported model (e.g. s3://my-bucket/exported_model.onnx) (either this or 'models' must be provided)
    models:  # use this when multiple models per API are desired (either this or 'model_path' must be provided)
      - name: <string> # unique name for the model (e.g. iris-classifier) (required)
        model_path: <string>  # S3 path to an exported model (e.g. s3://my-bucket/exported_model.onnx) (required)
        signature_key: <string>  # name of the signature def to use for prediction (required if your model has more than one signature def)
      ...
    processes_per_replica: <int>  # the number of parallel serving processes to run on each replica (default: 1)
    threads_per_process: <int>  # the number of threads per process (default: 1)
    config: <string: value>  # arbitrary dictionary passed to the constructor of the Predictor (optional)
    python_path: <string>  # path to the root of your Python folder that will be appended to PYTHONPATH (default: folder containing cortex.yaml)
    image: <string> # docker image to use for the Predictor (default: cortexlabs/onnx-predictor-gpu or cortexlabs/onnx-predictor-cpu based on compute)
    env: <string: string>  # dictionary of environment variables
  networking:
    endpoint: <string>  # the endpoint for the API (aws only) (default: <api_name>)
    api_gateway: public | none  # whether to create a public API Gateway endpoint for this API (if not, the load balancer will be accessed directly) (default: public)
  compute:
    cpu: <string | int | float>  # CPU request per replica, e.g. 200m or 1 (200m is equivalent to 0.2) (default: 200m)
    gpu: <int>  # GPU request per replica (default: 0)
    mem: <string>  # memory request per replica, e.g. 200Mi or 1Gi (default: Null)
```

See additional documentation for [parallelism](parallelism.md), [compute](compute.md), [networking](networking.md), [prediction monitoring](prediction-monitoring.md), and [overriding API images](system-packages.md).