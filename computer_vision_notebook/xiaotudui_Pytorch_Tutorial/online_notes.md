# 小土堆Pytorch教程笔记

## ——作者：墨子杰

**Tips：**

1. **很多没有记笔记的内容是我之前学过了，本笔记只简要记录一些学到的新知识**

2. **小土堆Pytorch视频教程网址如下：**

   [PyTorch深度学习快速入门教程（绝对通俗易懂！）【小土堆】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1hE411t7RN/?spm_id_from=333.1387.0.0)

3. 



## 1. Python中的两大法宝函数dir()和help()



- **dir() 函数**：能让我们知道工具箱以及工具箱中的分隔区有什么东西**（打开，看见）**
- **help() 函数：**能让我们知道每个工具是如何使用的，工具的使用方法**（说明书）**

```python
# 使用方法如下
dir(torch)
```

<img alt="img" src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/c244e06ea939f8c5551adbab2ec4599f.png"/>

```python
dir(torch.cuda)
```

<img alt="img" src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/ec1607713d5bba9184da5307cf9a1362.png"/>



```python
dir(torch.cuda.is_available)
```

可以看到**输出的前后都带有双下划线__**，这是一种规范，说明这个变量不容许篡改，即它不再是一个分隔区，而是一个确确实实的函数**（相当于工具，可以用help()函数）**

<img alt="img" src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/c610bb5fe4eeeca79dce48359059107d.png"/>

```python
# 注意is_available 不带括号
help(torch.cuda.is_available)
```

<img alt="img" src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/54f3ae3ec225df1858f6256997b8cb9e.png"/>

返回一个布尔值（True或False），表明cuda是否可用

**总结**

- dir() 函数：打开package
- help() 函数：官方的解释文档，看函数怎么用

## 2. PyTorch加载数据

### 2.1 PyTorch 读取数据涉及两个类：Dataset & Dataloader 

Dataset：提供一种方式，获取其中需要的数据及其对应的真实的 label 值，并完成编号。主要实现以下两个功能：

 *  如何获取每一个数据及其label
 *  告诉我们总共有多少的数据

Dataloader：打包（batch\_size），为后面的神经网络提供不同的数据形式

### 2.2 数据集的几种组织形式 

数据集 hymenoptera\_data（识别蚂蚁和蜜蜂的数据集，二分类）

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/7c465cd5e04796acc6bbe5548ee19f83.png"/>

1.  train 里有两个文件夹：ants 和 bees，其中分别都是一些蚂蚁和蜜蜂的图片
2.  train\_images是一个文件夹，train\_labels是另一个文件夹，如OCR数据集
3.  label直接为图片的名称

### 2.3 初识Dataset类 

```python
from torch.utils.data import Dataset
# 查看Dataset类的介绍：
help(Dataset)
# 或者
Dataset??
```



Dataset 是一个抽象类，所有数据集都需要继承这个类，所有子类都需要重写 \_\_getitem\_\_ 的方法，这个方法主要是获取每个数据集及其对应 label，还可以重写长度类 \_\_len\_\_

```python
def __getitem__(self, index):
    raise NotImplementedError

def __add__(self, other):
    return ConcatDataset([self, other])
```

## 3. Dataset类代码实战 

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch
@File    ：3.read_data.py
@IDE     ：PyCharm
@Author  ：mozijie
@Date    ：2025/11/17 下午12:54
'''

from torch.utils.data import Dataset
import os
from PIL import Image

class MyDateset(Dataset):   # 自定义数据集类，继承了Dataset
    
    def __init__(self, root_dir, label_dir):
        self.root_dir = root_dir    # 图片文件夹路径
        self.label_dir = label_dir  # 标签文件夹路径
        self.path = os.path.join(root_dir, label_dir)   # 拼接得到完整路径
        self.img_path = os.listdir(self.path)  # 获取标签文件夹下的所有文件名
    
    def __getitem__(self, idx):
        """
        __getitem__ 方法用于根据索引获取数据集中的一个样本。
        它接受一个索引 idx 作为参数，返回对应索引的图像和标签。
        """ 
        img_name = self.img_path[idx]   # 根据索引获取对应的文件名
        img_path_name = os.path.join(self.path, img_name)   # 拼接得到完整的图片路径
        image = Image.open(img_path_name)   # 使用PIL库打开图像文件
        label = self.label_dir   # 标签为文件夹名称
        return image, label  # 返回图像和标签
    
    def __len__(self):
        """
        __len__ 方法返回数据集的大小，即样本的数量。
        这里返回标签文件夹下的文件数量。
        """
        return len(self.img_path)   # 返回数据集的大小
    

if __name__ == "__main__":
    root_dir = rf"hymenoptera_data\train"   # 数据集训练根目录
    ants_label_dir = "ants"  # 蚂蚁标签文件夹名称
    bees_label_dir = "bees" # 蜜蜂标签文件夹名称
    ants_dataset = MyDateset(root_dir=root_dir, label_dir=ants_label_dir)   # 创建蚂蚁数据集实例
    bees_dataset = MyDateset(root_dir=root_dir, label_dir=bees_label_dir)   # 创建蜜蜂数据集实例
    print("Ants dataset length:", len(ants_dataset))
    image, label = ants_dataset[0]
    image.show() # 显示图像
 
    print("First ant image size:", image.size)
    print("First ant image label:", label)

    print("Bees dataset length:", len(bees_dataset))
    image, label = bees_dataset[0]
    print("First bee image size:", image.size)
    print("First bee image label:", label)

    train_dataset = ants_dataset + bees_dataset
    print("Combined train dataset length:", len(train_dataset))
```

## 4. TensorBoard的使用

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch
@File    ：4.test_TensorBoard.py
@IDE     ：PyCharm
@Author  ：mozijie
@Date    ：2025/11/17 下午12:54
'''
from torch.utils.tensorboard import SummaryWriter
import numpy as np
from PIL import Image

if __name__ == "__main__":

    writer = SummaryWriter("logs")  # 创建SummaryWriter对象，指定日志文件夹为"logs"
    
    image_path = r"hymenoptera_data\train\ants\0013035.jpg"  # 图像文件路径
    image_path2 = r"hymenoptera_data\train\ants\5650366_e22b7e1065.jpg"  # 另一个图像文件路径
    img_pil = Image.open(image_path)  # 使用PIL库打开图像文件
    img_np = np.array(img_pil)  # 将PIL图像转换为NumPy数组
    img_np2 = np.array(Image.open(image_path2))  # 将另一个PIL图像转换为NumPy数组

    """
    add_image 方法用于将图像数据写入TensorBoard。
    参数说明：
    tag: 图像的标签，用于在TensorBoard中区分不同的图像。
    img_tensor: 要写入的图像数据，通常是一个NumPy数组或PyTorch张量。
    global_step: 全局步数，用于在TensorBoard中表示图像的顺序。
    dataformats: 图像数据的格式，默认为'CHW'（通道、高度、宽度）。
    这里使用'HWC'表示高度、宽度、通道。
    """
    writer.add_image("Ant Image", img_np, 0, dataformats='HWC')  # 将图像数据写入TensorBoard，标签为"Ant Image"，步数为0，数据格式为'HWC'（高度、宽度、通道）
    writer.add_image("Ant Image", img_np2, 1, dataformats='HWC')  # 将图像数据写入TensorBoard，标签为"Ant Image"，步数为1，数据格式为'HWC'（高度、宽度、通道）
    # y=x 的数据记录
    for i in range(100):
        """
        add_scalar 方法用于记录标量数据到TensorBoard。
        参数说明：
        tag: 标量数据的标签，用于在TensorBoard中区分不同的数据。
        scalar_value: 要记录的标量值,也就是y的值。
        global_step: 全局步数，用于在TensorBoard中表示数据的顺序。
        也就是x的值，通常表示训练的迭代次数或时间步数。
        walltime: 可选参数，表示记录数据的时间戳，默认为None。
        new_style: 可选参数，表示是否使用新的样式记录数据，默认为False。
        double_precision: 可选参数，表示标量值的精度，默认为False。
        """
        writer.add_scalar(tag="y=x", scalar_value=i, global_step=i)  # 记录标量数据，标签为"y=x"，值为i，步数为i
        writer.add_scalar(tag="y=2x", scalar_value=2*i, global_step=i)  # 记录标量数据，标签为"y=2x"，值为2*i，步数为i
    
    writer.close()  # 关闭SummaryWriter，确保所有数据都被写入日志文件


```

**运行上面这段代码后就会在项目的logs目录下生成对应的日志事件文件**

如何打开事件文件？

在 Terminal 里输入：

```python
tensorboard --logdir=logs  # logdir=事件文件所在文件夹名
```

结果如图：

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/09b9b70312a681a4c383a9de2329f446.png"/>

为了防止和别人冲突（一台服务器上有好几个人训练，默认打开的都是6006端口），也可以指定端口，命令如下：

```python
tensorboard --logdir=logs --port=6007  # logdir=事件文件所在文件夹名
```

## 5. 常见transforms的使用 

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch
@File    ：5. transforms.py
@IDE     ：PyCharm
@Author  ：mozijie
@Date    ：2025/11/17 下午12:54
'''

from torch.utils.data import Dataset, DataLoader
from PIL import Image
import os
from torchvision import transforms

class MyData(Dataset):

    def __init__(self, root_dir, image_dir, label_dir, transform=None):
        self.root_dir = root_dir
        self.image_dir = image_dir
        self.label_dir = label_dir
        self.label_path = os.path.join(self.root_dir, self.label_dir)
        self.image_path = os.path.join(self.root_dir, self.image_dir)
        self.image_list = os.listdir(self.image_path)
        self.label_list = os.listdir(self.label_path)
        self.transform = transform
        # 因为label 和 Image文件名相同，进行一样的排序，可以保证取出的数据和label是一一对应的
        self.image_list.sort()
        self.label_list.sort()

    def __getitem__(self, idx):
        img_name = self.image_list[idx]
        label_name = self.label_list[idx]
        img_item_path = os.path.join(self.root_dir, self.image_dir, img_name)
        label_item_path = os.path.join(self.root_dir, self.label_dir, label_name)
        img = Image.open(img_item_path)
        with open(label_item_path, 'r') as f:
            label = f.readline()

        # 对图像进行transform操作
        if self.transform:
            img = self.transform(img)


        return img, label

    def __len__(self):
        assert len(self.image_list) == len(self.label_list)
        return len(self.image_list)


# transforms.Compose 用于将多个图像转换操作组合在一起，形成一个复合的转换流程。
transform = transforms.Compose([transforms.Resize(400), transforms.ToTensor()])
root_dir = "dataset/train"
image_ants = "ants_image"
label_ants = "ants_label"
ants_dataset = MyData(root_dir, image_ants, label_ants, transform=transform)
image_bees = "bees_image"
label_bees = "bees_label"
bees_dataset = MyData(root_dir, image_bees, label_bees, transform=transform)




```

**Compose 的使用** 

把不同的 transforms 结合在一起，后面接一个数组，里面是不同的transforms

```python
# Example:图片首先要经过中心裁剪，再转换成Tensor数据类型
transforms.Compose([
    transforms.CenterCrop(10),  # CenterCrop 裁剪图像的中心区域，裁剪后的大小为10x10像素
    transforms.PILToTensor(),   # PILToTensor 将PIL图像转换为PyTorch张量，张量的形状为 (C, H, W)，其中C是通道数，H是高度，W是宽度
    transforms.ConvertImageDtype(torch.float),  # ConvertImageDtype 将图像张量的数据类型转换为指定的类型，这里转换为torch.float类型
])
```



## 6. torchvision中datasets的使用

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch
@File    ：6.datasets_transform.py
@IDE     ：PyCharm
@Author  ：mozijie
@Date    ：2025/11/17 下午12:54
'''
import torchvision
from torch.utils.tensorboard import SummaryWriter

# 定义数据转换操作，将图像转换为张量
data_transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor(),
])

# 加载CIFAR-10训练和测试数据集，应用定义的数据转换操作
"""
参数说明：
root: 数据集存储的根目录。
train: 布尔值，指定是加载训练集（True）还是测试集（False）。
transform: 应用于图像数据的转换操作。
download: 布尔值，指定是否从互联网下载数据集（如果本地不存在）。
"""
train_dataset = torchvision.datasets.CIFAR10(root='./data', 
                                             train=True, 
                                             transform=data_transform, 
                                             download=True)
test_dataset = torchvision.datasets.CIFAR10(root='./data', 
                                            train=False, 
                                            transform=data_transform, 
                                            download=True)

# 获取并打印测试数据集中的第一张图像及其形状
img, label = test_dataset[0]
print(img)  # 输出图像的张量表示
print(img.shape)  # 输出图像的形状

# 使用TensorBoard记录CIFAR-10测试数据集中的前10张图像
writer = SummaryWriter("logs_cifar10")
# 将前10张图像写入TensorBoard日志
for i in range(10):
    img, label = test_dataset[i]
    writer.add_image("CIFAR10_images", img, i)
writer.close()  # 关闭SummaryWriter，确保所有数据都被写入日志文件
```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/image-20251117122418704.png" alt="image-20251117122418704" style="zoom:80%;" />



## 7. DataLoader 的使用 

 *  dataset：告诉程序中数据集的位置，数据集中索引，数据集中有多少数据（想象成一叠扑克牌）
 *  dataloader：将数据加载到神经网络中，每次从dataset中取数据，通过dataloader中的参数可以设置如何取数据（想象成抓的一组牌）

[torch.utils.data — PyTorch 1.10 documentation][torch.utils.data _ PyTorch 1.10 documentation]

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch
@File    ：7.dataloader.py
@IDE     ：PyCharm
@Author  ：mozijie
@Date    ：2025/11/17 下午12:54
'''

import torchvision
from torch.utils.data import Dataset, DataLoader
from torch.utils.tensorboard import SummaryWriter

# 定义数据转换操作，将图像转换为张量
my_transform = torchvision.transforms.Compose([
    torchvision.transforms.ToTensor(),
])
# 加载CIFAR-10训练和测试数据集，应用定义的数据转换操作
train_dataset = torchvision.datasets.CIFAR10(root='./data',
                                             train=True, 
                                             transform=my_transform,
                                             download=True)

test_dataset = torchvision.datasets.CIFAR10(root='./data',
                                            train=False,
                                            transform=my_transform,
                                            download=True)
# 使用DataLoader加载测试数据集，设置批量大小为64，不打乱数据，不丢弃最后一个不完整的批次
"""
DataLoader 参数说明：
dataset: 要加载的数据集对象。
batch_size: 每个批次加载的样本数量。
shuffle: 是否在每个epoch开始时打乱数据。
num_workers: 用于数据加载的子进程数量。
drop_last: 如果数据集大小不能被批次大小整除，是否丢弃最后一个不完整的批次。
"""
test_loader = DataLoader(test_dataset, batch_size=64, shuffle=False,num_workers=0, drop_last=False)

# 获取并打印测试数据集中的第一张图像及其形状和标签
img, label = test_dataset[0]
print(img.shape)
print(label)

# 遍历测试数据加载器，打印每个批次的图像形状和标签
"""
我们可以看到每个批次包含64张图像，每张图像的形状为(3, 32, 32)，对应CIFAR-10数据集的RGB图像大小为32x32像素。
标签是一个包含64个元素的张量，表示每张图像对应的类别。
打印输出如下：
torch.Size([3, 32, 32])
3
torch.Size([64, 3, 32, 32])
tensor([3, 8, 8, 0, 6, 6, 1, 6, 3, 1, 0, 9, 5, 7, 9, 8, 5, 7, 8, 6, 7, 0, 4, 9,
        5, 2, 4, 0, 9, 6, 6, 5, 4, 5, 9, 2, 4, 1, 9, 5, 4, 6, 5, 6, 0, 9, 3, 9,
        7, 6, 9, 8, 0, 3, 8, 8, 7, 7, 4, 6, 7, 3, 6, 3])
"""
for data in test_loader:
    imgs, labels = data
    # print(imgs.shape)
    # print(labels)

# 使用TensorBoard记录CIFAR-10测试数据加载器中的图像
writer = SummaryWriter("logs_cifar10_dataloader")

# 将测试数据加载器中的图像写入TensorBoard日志
for step,data in enumerate(test_loader):
    imgs, labels = data # 获取一个批次的图像和标签
    writer.add_images("CIFAR10_dataloader_images", imgs, step)  # 将图像数据写入TensorBoard日志，标签为"CIFAR10_dataloader_images"，步数为step

writer.close()  # 关闭SummaryWriter，确保所有数据都被写入日志文件
```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/image-20251117122210771.png" alt="image-20251117122210771" style="zoom: 80%;" /><img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/image-20251117122221856.png" alt="image-20251117122221856" style="zoom: 80%;" />

## 8. nn.Module的使用

Pytorch官网左侧：Python API（相当于package，提供了一些不同的工具）

关于神经网络的工具主要在torch.nn里 

[torch.nn — PyTorch 1.10 documentation][torch.nn _ PyTorch 1.10 documentation]



**Containers** 

包含6个模块：

 *  Module
 *  Sequential
 *  ModuleList
 *  ModuleDict
 *  ParameterList
 *  ParameterDict

其中最常用的是 Module 模块（为所有神经网络提供基本骨架）

```java
CLASS torch.nn.Module  #搭建的 Model都必须继承该类
```

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：8.nn_module.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/17 下午12:54 
'''
from torch import nn
import torch

# 定义一个简单的神经网络模块，继承自nn.Module
class mozijieNet(nn.Module):
    def __init__(self):
        # 调用父类的构造函数，初始化模块
        super(mozijieNet, self).__init__()

    def forward(self, x):
        # 前向传播函数，定义输入数据如何通过网络进行计算
        output = x+1 # 简单地将输入张量加1作为输出
        return output

if __name__ == '__main__':
    mozijie = mozijieNet()  # 创建mozijieNet类的实例
    x = torch.tensor([1.0, 2.0, 3.0])   # 创建一个输入张量
    y = mozijie(x)  # 将输入张量传递给神经网络模块，得到输出张量
    print(y)  # 输出结果应为 tensor([2., 3., 4.])
```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/image-20251117133639180.png" alt="image-20251117133639180" style="zoom:80%;" />

## 9. 卷积层函数conv2d

[torch.nn — PyTorch 1.10 documentation][torch.nn _ PyTorch 1.10 documentation 1]

Convolution Layers

[Conv2d — PyTorch 1.10 documentation][Conv2d _ PyTorch 1.10 documentation]

torch.nn 和 torch.nn.functional 的区别：前者是后者的封装，更利于使用

点击 torch.nn.functional - Convolution functions - conv2d

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/d8ef5c71a35ba63eb3ff194d7419a840.png"/>

### 9.1 stride（步进） 

可以是单个数，或元组（sH,sW） — 控制横向步进和纵向步进

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/bb61dd9b746dfa4e1b0c242166e88bbd.png"/>

当 stride = 2 时，横向和纵向都是2，输出是一个2×2的矩阵

\--------------------------------------------------------------------------------------------------------------------------------

### 9.2 reshape函数

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/10ef473a94000c0224b25727df5f2393.png"/>

 *  input：尺寸要求是batch，几个通道，高，宽（4个参数）
 *  weight：尺寸要求是输出，in\_channels（groups一般为1），高，宽（4个参数）

使用 torch.reshape 函数，将输入改变为要求输入的维度

实现上图的代码：

```java
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：9.conv2d.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/17 下午1:37 
'''
import torch
import torch.nn.functional as F

input =torch.tensor([[1,2,0,3,1],
                     [0,1,2,3,1],
                     [1,2,1,0,0],
                     [5,2,3,1,1],
                     [2,1,0,1,1]])   #将二维矩阵转为tensor数据类型
# 卷积核kernel
kernel = torch.tensor([[1,2,1],
                       [0,1,0],
                       [2,1,0]])

# 尺寸只有高和宽，不符合要求
print(input.shape)  #5×5
print(kernel.shape)  #3×3

# 尺寸变换为四个数字
input = torch.reshape(input,(1,1,5,5))  #通道数为1，batch大小为1
kernel = torch.reshape(kernel,(1,1,3,3))
print(input.shape)
print(kernel.shape)

output = F.conv2d(input,kernel,stride=1)  # .conv2d(input:Tensor, weight:Tensor, stride)
print(output)   #输出卷积结果
```

结果为：

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/06cd9c460ee55694045b58bf13f7f001.png"/>

当将步进 stride 改为 2 时：

```java
output2 = F.conv2d(input,kernel,stride=2)
print(output2)
```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/e080efb32c25c972b49c60fa7cfcc8d0.png"/>

\---------------------------------------------------------------------------------------------------------------------------------

### 9.3 padding（填充） 

在输入图像左右两边进行填充，决定填充有多大。可以为一个数或一个元组（分别指定高和宽，即纵向和横向每次填充的大小）。默认情况下不进行填充

padding=1：将输入图像左右上下两边都拓展一个像素，空的地方默认为0

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/681fe3cd55633d859a854cef4f608026.png"/>

代码实现：

```java
output3 = F.conv2d(input,kernel,stride=1,padding=1)
print(output3)
```

输出结果如下：可以看出输出尺寸变大

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/39d45373ccdbdba369cec4bbf6bb9d82.png" style="zoom:80%;" />



## 10. 卷积层函数conv2d

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：10.nn_conv2d.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/17 下午2:03 
'''

# CIFAR10数据集
import torch
import torchvision
from torch import nn
from torch.nn import Conv2d
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

# 这里用测试数据集，因为训练数据集太大了
dataset = torchvision.datasets.CIFAR10("./data",
                                       train=False,
                                       transform=torchvision.transforms.ToTensor(),
                                       download=True)
dataloader = DataLoader(dataset, batch_size=64)


# 搭建神经网络MozijieNet，继承自nn.Module
class MozijieNet(nn.Module):
    def __init__(self):
        super(MozijieNet, self).__init__()
        # 因为是彩色图片，所以in_channels=3
        self.conv1 = Conv2d(in_channels=3,
                            out_channels=6,
                            kernel_size=3,
                            stride=1,
                            padding=0)  # 卷积层conv1

    def forward(self, x):  # 输出为x
        x = self.conv1(x)
        return x

mozijie_net = MozijieNet()  # 初始化网络
# 打印一下网络结构
print(mozijie_net)
# Tudui((conv1): Conv2d(3, 6, kernel_size=(3, 3), stride=(1, 1)))

writer = SummaryWriter("logs_mozijienet_conv2d")
for step, data in enumerate(dataloader):
    imgs, targets = data # 获取图片和标签
    output = mozijie_net(imgs) # 将图片输入网络，获得输出
    # 输入大小 torch.Size([64, 3, 32, 32])  batch_size=64，
    # in_channels=3（彩色图像），每张图片是32×32的
    print(imgs.shape)
    # 经过卷积后的输出大小 torch.Size([64, 6, 30, 30])
    # 卷积后变成6个channels，因为用两个卷积核来卷积
    # 原始图像减小，所以是30×30的
    print(output.shape)
    writer.add_images("input", imgs, step)

    # 6个channel无法显示。torch.Size([64, 6, 30, 30]) ——> [xxx,3,30,30]
    # 第一个值不知道为多少时写-1，会根据后面值的大小进行计算
    output = torch.reshape(output, (-1, 3, 30, 30))
    writer.add_images("output", output, step)
```



<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/image-20251117143130103.png" alt="image-20251117143130103" style="zoom:80%;" />

## 11. 最大池化操作

[torch.nn — PyTorch 1.10 documentation][torch.nn _ PyTorch 1.10 documentation 2]

[Pooling layers][torch.nn _ PyTorch 1.10 documentation 2]

 *  MaxPool：最大池化（下采样）
 *  MaxUnpool：上采样
 *  AvgPool：平均池化
 *  AdaptiveMaxPool2d：自适应最大池化

最常用：[MaxPool2d — PyTorch 1.10 documentation][MaxPool2d _ PyTorch 1.10 documentation]

### 11.1 MaxPool2d参数 

```java
CLASS torch.nn.MaxPool2d(kernel_size, stride=None, padding=0, dilation=1, return_indices=False, ceil_mode=False)
# kernel_size 池化核
```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/c3d7859d665b58ac619e3a561c09b5b6.png" style="zoom:80%;" />

> 注意，卷积中stride默认为1，而池化中stride默认为kernel\_size 

### 11.2 ceil\_mode参数

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/1d7ca459e3f458cb4d3503cee6de7049.png"/>

Ceil\_mode 默认情况下为 False，对于最大池化一般只需设置 kernel\_size 即可 

\--------------------------------------------------------------------------------------------------------------------------------

### 11.3 输入输出维度计算公式 

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/6e9adfae757477691859cc5730548f12.png" style="zoom:80%;" />

\--------------------------------------------------------------------------------------------------------------------------------

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：11.nn_maxpool.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/18 下午3:01 
'''
import torch
import torchvision
from torch import nn
from torch.nn import MaxPool2d
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

# 加载CIFAR-10测试数据集
dataset = torchvision.datasets.CIFAR10("./data",train=False,download=True,transform=torchvision.transforms.ToTensor())
dataloader = DataLoader(dataset,batch_size=64)

#最大池化无法对long数据类型进行实现,
# 利用dtype=torch.float32将input变成浮点数的tensor数据类型
input = torch.tensor([[1,2,0,3,1],
                      [0,1,2,3,1],
                      [1,2,1,0,0],
                      [5,2,3,1,1],
                      [2,1,0,1,1]],dtype=torch.float32)

#-1表示torch自动计算batch_size
input = torch.reshape(input,(-1,1,5,5))
print(input.shape)

# 搭建神经网络
class MozijieNet(nn.Module):
    def __init__(self):
        super(MozijieNet, self).__init__()
        """
        MaxPool2d参数说明：
        kernel_size: 池化核的大小，可以是单个整数或一个整数元组，表示高度和宽度。
        stride: 池化操作的步幅。如果未指定，则默认为kernel_size。
        padding: 在输入的每一条边补充的零的数量。可以是单个整数或一个整数元组。
        dilation: 控制池化窗口元素之间的间距。默认值为1，表示没有间距。
        return_indices: 如果设置为True，池化操作将返回池化窗口中最大元素的索引。默认值为False。
        ceil_mode: 如果设置为True，输出大小将通过向上取整计算。
        """
        self.maxpool1 = MaxPool2d(kernel_size=3,ceil_mode=True)
    def forward(self,input):
        output = self.maxpool1(input)
        return output

# 创建神经网络
mozijie = MozijieNet()
# 前向传播
output = mozijie(input)
print(output)
""""
我们可以看到，经过最大池化操作后，输出的尺寸变小了。原始输入的尺寸是(1, 1, 5, 5)，
经过3x3的最大池化后，输出的尺寸变为(1, 1, 2, 2)。
这是因为最大池化操作在每个3x3的区域内选取了最大的值，从而减少了空间尺寸。
他的实际意义就是降低特征图的尺寸，从而减少计算量，同时保留重要的特征信息。
"""

# 在CIFAR-10数据集上使用最大池化
mozijie2 = MozijieNet()
writer = SummaryWriter("./logs_maxpool_simple")
for step, data in enumerate(dataloader):
    imgs, targets = data
    print(rf"正在处理第{step}批图片")
    writer.add_images("input",imgs,step)
    # output尺寸池化后不会有多个channel，原来是3维的图片，
    # 经过最大池化后还是3维的，不需要像卷积一样还要reshape操作
    # （影响通道数的是卷积核个数）
    output = mozijie2(imgs)
    writer.add_images("output",output,step)

writer.close()
```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/image-20251118151105110.png" alt="image-20251118151105110" style="zoom:80%;" />

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/image-20251118155149709.png" alt="image-20251118155149709" style="zoom:80%;" />

### 11.4 为什么要进行最大池化？最大池化的作用是什么？ 

最大池化的目的是保留输入的特征，同时把数据量减小（数据维度变小），对于整个网络来说，进行计算的参数变少，会训练地更快

 *  如上面案例中输入是5x5的，但输出是3x3的，甚至可以是1x1的
 *  类比：1080p的视频为输入图像，经过池化可以得到720p，也能满足绝大多数需求，传达视频内容的同时，文件尺寸会大大缩小

池化一般跟在卷积后，卷积层是用来提取特征的，一般有相应特征的位置是比较大的数字，最大池化可以提取出这一部分有相应特征的信息

池化不影响通道数

池化后一般再进行非线性激活

## 12. 非线性激活

 *  [Padding Layers][]（对输入图像进行填充的各种方式）  
    几乎用不到，nn.ZeroPad2d（在输入tensor数据类型周围用0填充）  
    nn.ConstantPad2d（用常数填充）  
    在 Conv2d 中可以实现，故不常用
 *  [Non-linear Activations (weighted sum, nonlinearity)][Non-linear Activations _weighted sum_ nonlinearity]
 *  [Non-linear Activations (other)][Non-linear Activations _other]

非线性激活：给神经网络引入一些非线性的特征

非线性越多，才能训练出符合各种曲线或特征的模型（提高泛化能力）

\--------------------------------------------------------------------------------------------------------------------------------

### 12.1 RELU激活函数 

[ReLU — PyTorch 1.10 documentation][ReLU _ PyTorch 1.10 documentation]

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/5d376ad0c12bc2863d0813da02d32cf9.png" style="zoom: 50%;" />

输入：(N,\*) N 为 batch\_size，\*不限制

代码举例：RELU 

```java
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：12.nn_relu.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/18 下午4:04 
'''
import torch
from torch import nn
from torch.nn import ReLU

# 创建输入张量
input = torch.tensor([[1,-0.5],
                      [-1,3]])

input = torch.reshape(input,(-1,1,2,2))
print(input.shape)   #torch.Size([1, 1, 2, 2])
print(input)

# 搭建神经网络
class MozijieNet(nn.Module):
    def __init__(self):
        super(MozijieNet, self).__init__()
        """
        ReLu参数说明：
        inplace: 一个布尔值，表示是否进行原地操作。
        如果设置为True，则会直接修改输入张量，节省内存空间；
        如果为False，则会创建一个新的张量来存储结果。默认值为False。
        作用：ReLU（Rectified Linear Unit）是一种常用的激活函数，
        定义为f(x)=max(0,x)。
        它的主要作用是引入非线性特性，使神经网络能够学习和表示复杂的函数关系。
        ReLU函数在输入为正值时保持不变，而在输入为负值时输出为零。
        这种特性有助于缓解梯度消失问题，加快训练速度，并提高模型的表达能力。
        """
        self.relu1 = ReLU()  #inplace默认为False
    def forward(self,input):
        output = self.relu1(input)
        return output

# 创建网络
mozijie = MozijieNet()
output = mozijie(input)
print(output)
```

运行结果：

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/image-20251118161342967.png" alt="image-20251118161342967" style="zoom:80%;" />

跟输入对比可以看到：小于0的值被0截断，大于0的值仍然保留



### 12.2 Sigmoid激活函数

[Sigmoid — PyTorch 1.10 documentation][Sigmoid _ PyTorch 1.10 documentation]

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/8c10e39c553646a94b9f37f4e328fa40.png" style="zoom:80%;" />

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/e3f1fa283d549dbe5daf6c0b148e84a6.png" style="zoom:50%;" />

输入：(N,\*) N 为 batch\_size，\*不限制

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：12.nn_sigmoid.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/18 下午4:14 
'''
import torchvision.datasets
from torch import nn
from torch.nn import Sigmoid
from torch.utils.data import DataLoader
from torch.utils.tensorboard import SummaryWriter

# 加载数据集
dataset = torchvision.datasets.CIFAR10("./data",train=False,download=True,transform=torchvision.transforms.ToTensor())
dataloader = DataLoader(dataset,batch_size=64)

# 搭建神经网络
class MozijieNet(nn.Module):
    def __init__(self):
        super(MozijieNet, self).__init__()
        """
        Sigmoid参数说明：
        inplace: 布尔值，表示是否进行原地操作。如果设置为True，则会直接修改输入张量以节省内存；
        如果为False，则会创建一个新的张量来存储结果。默认值为False。
        公式：Sigmoid(x) = 1 / (1 + exp(-x))
        作用：将输入值映射到0和1之间，常用于二分类任务的输出层，以及神经网络中的激活函数。
        适用场景：适用于需要将输出限制在0到1范围内的场景，例如概率预测。
        影响：使用Sigmoid函数可以引入非线性，使神经网络能够学习复杂的模式。
        然而，Sigmoid函数在输入值较大或较小时会导致梯度消失问题，影响深层网络的训练效果。
        计算示例：
        输入：x = 0
        计算：Sigmoid(0) = 1 / (1 + exp(0)) = 1 / (1 + 1) = 0.5
        输出：0.5
        这个例子展示了当输入为0时，Sigmoid函数的输出为0.5，位于0和1的中间位置。
        """
        self.sigmoid1 = Sigmoid()  #inplace默认为False
    def forward(self,input):
        output = self.sigmoid1(input)
        return output

# 创建网络
mozijie = MozijieNet()

# 写入tensorboard
writer = SummaryWriter("./logs_sigmoid")
step = 0
for data in dataloader:
    imgs,targets = data
    writer.add_images("input",imgs,global_step=step)
    output = mozijie(imgs)
    writer.add_images("output",output,step)
    step = step + 1

writer.close()
```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/离线版本个人笔记.assets/image-20251118161656334.png" alt="image-20251118161656334" style="zoom:80%;" />

## 13. 















































