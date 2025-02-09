# Tensors

> 译者：[Daydaylight](https://github.com/Daydaylight)
>
> 项目地址：<https://pytorch.apachecn.org/2.0/tutorials/beginner/basics/tensorqs_tutorial>
>
> 原始地址：<https://pytorch.org/tutorials/beginner/basics/tensorqs_tutorial.html>

Tensors 是一种特殊的数据结构，与数组和矩阵非常相似。在 PyTorch 中，我们使用 tensors 对模型的输入和输出以及模型的参数进行编码。

Tensors 类似于 [NumPy’s](https://numpy.org/) 的 ndarrays，不同的是 tensors 可以在 GPU 或其他硬件加速器上运行。事实上，tensors 和 NumPy 数组通常可以共享相同的底层内存，这样就不需要复制数据了(参见使用 NumPy 的 Bridge)。
Tensors 也针对自动微分进行了优化(我们将在后面的 [Autograd](autogradqs_tutorial.html) 部分中看到更多细节介绍)。如果您熟悉 ndarray，那么您就会熟悉 Tensor API。如果不熟悉，那么请跟我来！
```py
import torch
import numpy as np
```
## 初始化Tensor
Tensors 可以用不同的方式初始化。看看下面的例子:

**直接从原生数据创建**

Tensors 可以直接从数据中创建，数据类型可以自动推断。
```py
data = [[1, 2],[3, 4]]
x_data = torch.tensor(data)
```
**从 NumPy 数组创建**

Tensors 可以从 NumPy 数组创建(反之亦然 —— 参见 `bridge-to-np-label`)。
```py
np_array = np.array(data)
x_np = torch.from_numpy(np_array)
```

**从 tensor 变量创建**

新 tensor 将继承参数 tensor 的属性(形状、数据类型) ，除非显式重写。
```py
x_ones = torch.ones_like(x_data) # 继承 x_data 的属性
print(f"Ones Tensor: \n {x_ones} \n")

x_rand = torch.rand_like(x_data, dtype=torch.float) # 覆盖从 x_data 继承的数据类型
print(f"Random Tensor: \n {x_rand} \n")
```
输出：
```py
Ones Tensor:
 tensor([[1, 1],
        [1, 1]])

Random Tensor:
 tensor([[0.8823, 0.9150],
        [0.3829, 0.9593]])
```

**从随机数据或常量创建**

``shape`` 是 tensor 维数的元组。在下面的实例中，它决定了输出 tensor 的维数。

```py
shape = (2,3,)
rand_tensor = torch.rand(shape)
ones_tensor = torch.ones(shape)
zeros_tensor = torch.zeros(shape)

print(f"Random Tensor: \n {rand_tensor} \n")
print(f"Ones Tensor: \n {ones_tensor} \n")
print(f"Zeros Tensor: \n {zeros_tensor}")
```
输出：
```py
Random Tensor:
 tensor([[0.3904, 0.6009, 0.2566],
        [0.7936, 0.9408, 0.1332]])

Ones Tensor:
 tensor([[1., 1., 1.],
        [1., 1., 1.]])

Zeros Tensor:
 tensor([[0., 0., 0.],
        [0., 0., 0.]])
```
## Tensor 的属性
Tensor 属性描述了它们的形状、数据类型和存储它们的设备。
```py
tensor = torch.rand(3,4)

print(f"Shape of tensor: {tensor.shape}")
print(f"Datatype of tensor: {tensor.dtype}")
print(f"Device tensor is stored on: {tensor.device}")
```
输出：
```py
Shape of tensor: torch.Size([3, 4])
Datatype of tensor: torch.float32
Device tensor is stored on: cpu
```
## Tensors 的操作

tensor 有超过100种操作，包括算术、线性代数、矩阵操作(转置、索引、切片) 、抽样等，在[这里](https://pytorch.org/docs/stable/torch.html)有详细描述。

这些操作都可以在 GPU 上运行(通常比在 CPU 上运行的速度更快)。如果你使用 Colab，通过 Runtime > Change runtime type > GPU 来分配一个GPU。

默认情况下，tensors 是在 CPU 上创建的。我们需要使用 ``.to`` 方法显式地将 tensors 移动到 GPU 上(在检查GPU的可用性之后)。请记住，在不同的设备上复制大型的 tensors，在时间和内存上都是很昂贵的!


```py
# We move our tensor to the GPU if available
if torch.cuda.is_available():
    tensor = tensor.to("cuda")
```


尝试列表中的一些操作。如果您熟悉 NumPy API，您会发现使用 Tensor API 简直易如反掌。


**类似 numpy 索引和分片的标准操作：**
```py
tensor = torch.ones(4, 4)
print(f"First row: {tensor[0]}")
print(f"First column: {tensor[:, 0]}")
print(f"Last column: {tensor[..., -1]}")
tensor[:,1] = 0
print(tensor)
```

输出：
```py
First row: tensor([1., 1., 1., 1.])
First column: tensor([1., 1., 1., 1.])
Last column: tensor([1., 1., 1., 1.])
tensor([[1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.]])
```
**连接 tensors**

您可以使用 ``torch.cat`` 将一系列tensor沿着给定的维数连接起来。另请参见 [torch.stack](https://pytorch.org/docs/stable/generated/torch.stack.html)，它是另一个tensor连接运算符，与 ``torch.cat`` 略有不同。



```py
t1 = torch.cat([tensor, tensor, tensor], dim=1)
print(t1)
```
输出：
```py
tensor([[1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.],
        [1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.],
        [1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.],
        [1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.]])
```

**算术运算**



```py
# This computes the matrix multiplication between two tensors. y1, y2, y3 will have the same value
# ``tensor.T`` returns the transpose of a tensor
y1 = tensor @ tensor.T
y2 = tensor.matmul(tensor.T)

y3 = torch.rand_like(y1)
torch.matmul(tensor, tensor.T, out=y3)


# This computes the element-wise product. z1, z2, z3 will have the same value
z1 = tensor * tensor
z2 = tensor.mul(tensor)

z3 = torch.rand_like(tensor)
torch.mul(tensor, tensor, out=z3)
```
输出：
```py

tensor([[1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.]])
```

**单个元素的 tensors**

如果你有一个一维的 tensors 变量，通过将 tensors 的所有值聚合成一个值，就可以使用 ``item()`` 将它转换成 Python 数值:

```py
agg = tensor.sum()
agg_item = agg.item()
print(agg_item, type(agg_item))
```
输出：
```py
12.0 <class 'float'>
```

**就地操作**

将修改结果存储到操作数中的操作被称为就地操作，通常它们以后缀 ``_`` 来表示。例如：``x.copy_(y)``, ``x.t_()``, 将改变 ``x``。

```py
print(f"{tensor} \n")
tensor.add_(5)
print(tensor)
```
输出：
```py
tensor([[1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.],
        [1., 0., 1., 1.]])

tensor([[6., 5., 6., 6.],
        [6., 5., 6., 6.],
        [6., 5., 6., 6.],
        [6., 5., 6., 6.]])
```
<div class="alert alert-info"><h4>注意</h4><p>就地操作可以节省一些内存，但是在计算导数时可能会出现问题，因为会立即丢失历史记录。因此，不鼓励使用它们。</p></div>




## 与 NumPy 的桥梁
CPU 上的 Tensors 和 NumPy 数组可以共享它们的底层内存存储，更改其中一个将更改另一个。
### Tensor 到 NumPy 数组

```py
t = torch.ones(5)
print(f"t: {t}")
n = t.numpy()
print(f"n: {n}")
```
输出：
```py
t: tensor([1., 1., 1., 1., 1.])
n: [1. 1. 1. 1. 1.]
```
tensor 的变化反映到 NumPy 数组中。

```py
t.add_(1)
print(f"t: {t}")
print(f"n: {n}")
```
输出：
```py
t: tensor([2., 2., 2., 2., 2.])
n: [2. 2. 2. 2. 2.]
```

### NumPy 数组 到 Tensor

```py
n = np.ones(5)
t = torch.from_numpy(n)
print(f"t: {t}")
print(f"n: {n}")
```
输出：
```py
t: tensor([1., 1., 1., 1., 1.])
n: [1. 1. 1. 1. 1.]
```
NumPy 数组中的更改反映到 tensor 中。
```py
np.add(n, 1, out=n)
print(f"t: {t}")
print(f"n: {n}")
```
输出：
```py
t: tensor([2., 2., 2., 2., 2.], dtype=torch.float64)
n: [2. 2. 2. 2. 2.]
```
