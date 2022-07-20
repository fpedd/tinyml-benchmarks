# TinyML Benchmarks
While the TinyML benchmarks form [MLPerf™]() are a great, I found them verbose and hard to use for my projects. Thus, I assembled this collection of read-to-go and easy to use benchmarks. They are based on the quantized models from the MLPerf™. However, they are completely self contained and have no external dependencies. The code for running them on [TensorFlow Lite Micro](https://github.com/tensorflow/tflite-micro) is less than a 100 lines. All data files are automatically generated using a couple of Python scripts that I will release shortly.

Furthermore, these benchmarks are not only intended to give feedback on performance, but also on functionality. Thus, they can be used to very correctness of the underlying kernel implementation.

Currently, I have generated 25 samples for every model. However, I can generate upwards of 1000 samples using the Python scripts.

## Benchmarks
This repository contains the four core benchmarks from the TinyML MLPerf™ benchmark suite:
- `aww` Audio wake word - Keyword spotting using a depthwise separable convolutional neural network (DS-CNN)
- `ic` Image classification	- Small image classification using the Cifar10 dataset and a ResNet model architecture
- `toy` ToyCar anomaly detection - Detecting anomalies in machine operating sounds using a deep auto encoder
- `vww` Visual wake words - Image classification based on presence of people using a MobileNet architecture
