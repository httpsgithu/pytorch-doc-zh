# 学习基本知识

> 译者：[Daydaylight](https://github.com/Daydaylight)
>
> 项目地址：<https://pytorch.apachecn.org/2.0/tutorials/beginner/basics/intro>
>
> 原始地址：<https://pytorch.org/tutorials/beginner/basics/intro.html>


大多数机器学习工作流程包括处理数据、创建模型、优化模型参数和保存已训练的模型。本教程向您介绍了在 PyTorch 中实现的一个完整的 ML 工作流，并提供了了解这些概念的更多链接。

我们将使用 FashionMNIST 数据集训练一个神经网络来预测输入图像是否属于下列类别之一: T 恤/上衣，裤子，套头衫，连衣裙，外套，凉鞋，衬衫，运动鞋，包，或脚踝靴。


本教程假设您基本熟悉 Python 和深度学习的概念。

## 运行教程代码


您可以通过以下几种方式运行本教程:


- **云端运行**: 这是最简单的入门方法! 每个部分的顶部都有一个 "在 Microsoft Learn 中运行" 和 "在 Google Colab 中运行" 的链接，分别在 Microsoft Learn 或 Google Colab 中打开一个 notebook，在一个完全托管的环境中运行代码。

- **本地运行**: 这个选项要求你首先在本地电脑上安装 PyTorch 和 TorchVision([安装说明](https://pytorch.org/get-started/locally/))。下载 notebook 或将代码复制到你喜欢的IDE中运行。


## 如何使用本指南
如果你熟悉其他深度学习框架，可以先看看[0. Quickstart](quickstart_tutorial.html) ，以快速熟悉 PyTorch 的 API。

如果你是深度学习框架的新手，请直接进入我们的分步指南的第一部分: [1. Tensors](tensor_tutorial.html) 。

