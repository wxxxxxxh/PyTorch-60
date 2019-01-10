# 什么是PyTorch
### 什么是pytorch？
	- 对numpy的一种替代，并可以使用GPUs
	- 一个可以提供最大灵活度与速度的深度学习平台

### 基础开始
- **张量（Tensor）**
	张量和numpy中的数组（ndarray）非常相似，但是比较6的是张量可以利用GPU进行加速计算（厉害吧～）
	想要使用pytorch进行基本的张量定义或者常见运算，最起码要先引入库，也就是一定要包含：
```
from __future__ import print_function
import torch
```

	可以利用以下语句建立一个大小为5x3的/未初始化的张量：
```
x = torch.empty(5, 3)
```

	可以利用以下语句建立一个随机初始化的张量：
```
x = torch.rand(5 ,3)
```

	可以利用以下语句建立一个元素全部为0/类型为long的张量：
```
x = torch.zeros(5, 3, dtype=torch.long)
```

	直接利用一组数据（比如[5.5, 6]）来建立一个张量：
```
x = torch.tensor([5.5, 6])
```

	或者也可以利用一个已存在的tensor_1来建立一个新的tensor_2，如果不特别的指定新tensor_2的数据类型的话，那新tensor_2的数据类型就会和已存在的那个tensor_1一样。
```
#大小和old_x一致，类型在这里重新指定为float类型
x = torch.randn_like(old_x, dtype = torch.float) 
```

	想获得一个向量的大小（也即shape），可以使用：
```
x.size()
```

- **操作**
	在pytorch中，有许多运算操作，包括同种操作有多种形式。
	1. 求和
	第一种：直接相加（+）
```
x + y
```
	第二种，使用以下方法：
```
torch.add(x, y)
```
	第三种，提供一个参数来代表最后相加的结果
```
torch.add(x, y, out=result)
#result的大小就是和值
```
	第四种，在给数A加上数B的值：
```
y.add_(x)
#这样y的值会变成x+y，要注意add_()中的_
```
	::任何对变量本身进行的操作，比如add，比如转置T，都在方法中加入一个_，即add_(),T_()::

	2. 可以像numpy一样进行索引/切片等操作，比如
```
print(x[:, 1])
```

	3. 如果想改变张量的形状（shape），可以使用：
```
#给定一个4x4的x
y = x.View(16)    #将形状改成了16
z = x.View(-1, 8). #-1表示将根据其他参数来推测这个值，比如一共16个数，					   给定8后自动计算出-1处为2
```

	4. 只有一个元素的tensor，可以通过以下方法得到单独的数值：
```
#假设给定了x = tensor([0.5538])
print(x.item()).   #将会输出0.5538
```

### Numpy与Tensor的联系
	Numpy的数组与Tensor中的张量是可以相互转化的，且非常方便。
	要注意的是，将numpy中的array和torch中的tensor他们在转化后是共享变量的地址的，这意味着一荣俱荣，一损俱损。

	1. tensor转array
```
#假设a为tensor
b = a.numpy()
```

	2. array转tensor
```
#假设a为numpy中的array
b = torch.from_numpy(a)
```
:::所有的在CPU中的，且不是字符类型的tensor都可以和array互相转换:::

### CUDA张量
	张量可以被送到GPU中进行使用：
```
利用“torch.device”来将张量送入或者拉出GPU
if torch.cuda.is_available():
    device = torch.device("cuda")     # 获得cuda实例
    y = torch.ones_like(x, device=device)  # 直接在GPU创建张量
    x = x.to(device)   #将x送入cuda实例中，也可以用.to("cuda")
    z = x + y
    print(z.to("cpu", torch.double))  #送回CPU中，同时指定数据类型
```