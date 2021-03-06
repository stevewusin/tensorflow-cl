# Tensorflow-cl

Run Tensorflow on OpenCL™ 1.2 devices.  UNDER CONSTRUCTION!!!

## Summary

This repo was created from the original Tensorflow repository at:

- https://github.com/tensorflow/tensorflow

Please see the main repository for full Tensorflow documentation.  This readme will only focus on the OpenCL porting aspects of Tensorflow.

## Test results, on v0.13.0 wheel

| test | Intel HD5500 | NVIDIA K520 |
|----- |-------|-----|
| [test_tf.py](https://github.com/hughperkins/tensorflow-cl/blob/v0.13.0/tensorflow/stream_executor/cl/test/test_tf.py) | ok | ok |
| [test_tf2.py](https://github.com/hughperkins/tensorflow-cl/blob/v0.13.0/tensorflow/stream_executor/cl/test/test_tf2.py) | ok | ok |
| [test_tf3.py](https://github.com/hughperkins/tensorflow-cl/blob/v0.13.0/tensorflow/stream_executor/cl/test/test_tf3.py) | ok | ok |
| [test_tf4.py](https://github.com/hughperkins/tensorflow-cl/blob/v0.13.0/tensorflow/stream_executor/cl/test/test_tf4.py) | ok | ok |
| [test_blas.py](https://github.com/hughperkins/tensorflow-cl/blob/v0.13.0/tensorflow/stream_executor/cl/test/test_blas.py) | runs ok, but segfault at end | ok, but segfault at end |
| [test_reductions.py](https://github.com/hughperkins/tensorflow-cl/blob/v0.13.0/tensorflow/stream_executor/cl/test/test_reductions.py) | ok | ok |
| [linear_regression.py](https://github.com/hughperkins/TensorFlow-Examples/blob/enforce-gpu/examples/2_BasicModels/linear_regression.py) | ok | ok |
| [logistic_regression.py](https://github.com/hughperkins/TensorFlow-Examples/blob/enforce-gpu/examples/2_BasicModels/logistic_regression.py) | cost is nan | ok |
| [nearest_neighbor.py](https://github.com/hughperkins/TensorFlow-Examples/blob/enforce-gpu/examples/2_BasicModels/nearest_neighbor.py) | accuracy 0.12, seems a little low | accuracy 0.12, seems a bit low |
| [multilayer_perceptron.py](https://github.com/hughperkins/TensorFlow-Examples/blob/enforce-gpu/examples/3_NeuralNetworks/multilayer_perceptron.py) | a bit slow, otherwise seems ok | a bit slow, otherwise seems ok |
| [recurrent_network.py](https://github.com/hughperkins/TensorFlow-Examples/blob/enforce-gpu/examples/3_NeuralNetworks/recurrent_network.py) | cost looks ok, accuracy seems broken | cost looks ok, accuracy seems broken |

Aymeric Damien's [linear_regression.py](https://github.com/hughperkins/TensorFlow-Examples/blob/enforce-gpu/examples/2_BasicModels/linear_regression.py) running on Intel HD5500 using Beignet v1.2.1:

<img src="doc/img/linearregressiononbeignet_hd5500.png?raw=true" width="600" />

## Installation 

- For now, Ubuntu 16.04 is supported.  In the future, I plan to support Mac OS X too
- You need:
  - the tensorflow non-gpu installation pre-requisites,
   - an OpenCL 1.2-enabled GPU, and  OpenCL 1.2-enabled drivers
   - python 3
- Simply download https://github.com/hughperkins/tensorflow-cl/releases/download/v0.13.0/tensorflow-0.11.0rc0-py3-none-any.whl , and
- Install using pip:
```
pip install --upgrade tensorflow-0.11.0rc0-py3-none-any.whl
```

If you want, you can [build from source](doc/build-from-source.md)

## Design/architecture

- tensorflow code stays 100% [NVIDIA® CUDA™](https://www.nvidia.com/object/cuda_home_new.html)
- [cuda-on-cl](https://github.com/hughperkins/cuda-on-cl) compiles the CUDA code into OpenCL
- Cedric Nugteren's [CLBlast](https://github.com/CNugteren/CLBlast) provides BLAS (matrix multiplications)

## Related projects

### DNN Libraries
- [OpenCL Torch](https://github.com/hughperkins/distro-cl)
- [DeepCL](https://github.com/hughperkins/DeepCL)

### OpenCL middleware
- [CLBlast](https://github.com/CNugteren/CLBlast) BLAS for OpenCL
- [cuda-on-cl](https://github.com/hughperkins/cuda-on-cl)  Compile CUDA apps for OpenCL
- [EasyCL](https://github.com/hughperkins/EasyCL)   Handles running kernels, passing in arguments etc, on OpenCL

## News

- Nov 10:
  - released wheel [v0.13.0](https://github.com/hughperkins/tensorflow-cl/releases/download/v0.13.0/tensorflow-0.11.0rc0-py3-none-any.whl)
     - beignet test results fairly solidly match K520 results now
     - fixed the regression on `not_equal` operator
     - removed the spam from memory copy  
- Nov 9:
  - fixed unary and binary operators on beignet
  - note that the tools/bazel.rc.templ has changed.  Please make sure to copy the new value into tools/bazel.rc, or re-run configure (probably need to do `bazel clean` anyway, so might as well do `./configure`)
