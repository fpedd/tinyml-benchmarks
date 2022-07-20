# TinyML Benchmarks
While the TinyML benchmarks from [MLPerf™](https://github.com/mlcommons/tiny) are great, I found them verbose and hard to use for my projects. Thus, I assembled this collection of ready-to-go and easy-to-use benchmarks. They are based on the quantized models from the MLPerf™ TinyML benchmarks. However, they are completely self-contained and have no external dependencies. The code for running them using [TensorFlow Lite Micro](https://github.com/tensorflow/tflite-micro) is less than 100 lines. All data files are automatically generated using a couple of Python scripts that can be found in my [fork](https://github.com/fabianpedd/tiny) of the TinyML benchmarks MLPerf™ repo.

Furthermore, these benchmarks are not only intended to give feedback on performance but also on functionality. Thus, they can be used to verify the correctness of the underlying kernel implementation.

Currently, I have generated 25 samples for every model. However, I can generate upwards of 1000 samples using Python scripts.

**Disclaimer: These benchmarks are obviously of little use as they are! They are intended to be used as a baseline for integration into a larger project!**

## Benchmarks
This repository contains the four core benchmarks from the TinyML MLPerf™ benchmark suite:
- [`/benchmarks/aww`](/benchmarks/aww) Audio wake word - Keyword spotting using a depthwise separable convolutional neural network (DS-CNN)
- [`/benchmarks/ic`](/benchmarks/ic) Image classification - Small image classification using the Cifar10 dataset and a ResNet model architecture
- [`/benchmarks/toy`](/benchmarks/toy) ToyCar anomaly detection - Detecting anomalies in machine operating sounds using a deep autoencoder
- [`/benchmarks/vww`](/benchmarks/vww) Visual wake words - Image classification based on the presence of people using a MobileNet architecture
