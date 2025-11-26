# 小土堆Pytorch教程笔记

## ——作者：墨子杰

**Tips：**

1. **很多没有记笔记的内容是我之前学过了，本笔记只简要记录一些学到的新知识**

2. **小土堆Pytorch视频教程网址如下：**

   [PyTorch深度学习快速入门教程（绝对通俗易懂！）【小土堆】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1hE411t7RN/?spm_id_from=333.1387.0.0)

3. 笔记中的代码可在我的Gitee仓库中找到：https://gitee.com/ji-mo-zijie/xiaotudui_pytorch_tutorial_notes


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

## 9. 卷积操作

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



## 13. 线性层Linear Layers

#### 13.1 Linear Layers

[Linear — PyTorch 1.10 documentation](https://pytorch.org/docs/stable/generated/torch.nn.Linear.html#torch.nn.Linear)

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/aafedc3ab8ef18e590d1f7dbb2ec26ee.png" style="zoom: 50%;" /> 

K和b在训练过程中神经网络会自行调整，以达到比较合理的预测

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/d981e547dcdc53a87653ccd7b6a11402.png" style="zoom: 80%;" />

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/468def0355031268851b3eb92e6a1646.png" style="zoom: 80%;" />

**vgg16 model**

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/42f40356f44a0767958de07aa46323d3.png" style="zoom:50%;" />

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/ed4d36ef6a5936a1d4547fac7fe2fe93.png"/>

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：13.nn_linear.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/21 下午12:52 
'''

import torch
import torchvision.datasets
from torch import nn
from torch.nn import Linear
from torch.utils.data import DataLoader

# 加载数据集
dataset = torchvision.datasets.CIFAR10("./data",train=False,transform=torchvision.transforms.ToTensor(),download=True)
dataloader = DataLoader(dataset,batch_size=64,drop_last=True)

class MozijieNet(nn.Module):
    def __init__(self):
        super(MozijieNet, self).__init__()
        """
        nn.Linear参数说明：
        in_features: 输入特征的数量，即输入张量的最后一个维度的大小。
        out_features: 输出特征的数量，即输出张量的最后一个维度的大小。
        bias: 布尔值，表示是否添加偏置项。如果设置为True，则会添加一个偏置向量；
        如果为False，则不添加。默认值为True。
        作用：线性变换层，用于将输入特征映射到输出特征空间，常用于神经网络的全连接层。
        适用场景：适用于需要进行线性变换的场景，如分类任务的输出层、回归任务等。
        影响：线性层可以学习输入特征与输出特征之间的线性关系，是神经网络中的基本构建块之一。
        计算示例：
        输入：input = torch.randn(64, 196608)  # 假设批量大小为64，输入特征为196608
        计算：
        linear = nn.Linear(196608, 10)  # 定义线性层
        output = linear(input)  # 前向传播
        输出：output的形状为(64, 10)，表示每个输入样本映射到10个输出特征。
        这个例子展示了如何使用nn.Linear层将输入特征映射到输出特征空间。
        通过学习权重和偏置，线性层能够捕捉输入与输出之间的线性关系。
        """
        self.linear1 = Linear(196608,10)
    def forward(self,input):
        output = self.linear1(input)
        return output

mozijie = MozijieNet()

for data in dataloader:
    imgs,targets = data
    print(imgs.shape)  #torch.Size([64, 3, 32, 32])
    """
    torch.flatten(input, start_dim=0, end_dim=-1) → Tensor
    将输入张量展平为一维张量，或者从指定的起始维度到结束维度展平为二维张量。
    参数说明：
    input: 要展平的输入张量。
    start_dim: 指定从哪个维度开始展平，默认为0。
    end_dim: 指定到哪个维度结束展平，默认为-1，表示最后一个维度。
    返回值：
    返回展平后的张量。
    """
    output = torch.flatten(imgs)   #摊平
    print(output.shape)   #torch.Size([196608])
    output = mozijie(output)
    print(output.shape)   #torch.Size([10])
```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251121131036466.png" alt="image-20251121131036466" style="zoom:80%;" />

#### 13.2 pytorch内置的一些网络模型 

 *  图片相关：torchvision [torchvision.models — Torchvision 0.11.0 documentation][torchvision.models]  
    分类、语义分割、目标检测、实例分割、人体关键节点识别（姿态估计）等等
 *  文本相关：torchtext 无
 *  语音相关：torchaudio [torchaudio.models — Torchaudio 0.10.0 documentation][torchaudio.models _ Torchaudio 0.10.0 documentation]

## 14. 简单模型搭建和 Sequential 的使用 

### 14.1 Sequential 

Containers中有Module、Sequential等

Sequential 是一个搭建网络模型的好工具，他可以一次性将很多模块写在一起

[Sequential — PyTorch 1.10 documentation][Sequential _ PyTorch 1.10 documentation]

Example：

```java
# Using Sequential to create a small model. When `model` is run,
# input will first be passed to `Conv2d(1,20,5)`. The output of
# `Conv2d(1,20,5)` will be used as the input to the first
# `ReLU`; the output of the first `ReLU` will become the input
# for `Conv2d(20,64,5)`. Finally, the output of
# `Conv2d(20,64,5)` will be used as input to the second `ReLU`
model = nn.Sequential(
          nn.Conv2d(1,20,5),
          nn.ReLU(),
          nn.Conv2d(20,64,5),
          nn.ReLU()
        )
```

好处：代码简洁易懂

\---------------------------------------------------------------------------------------------------------------------------------

### 14.2 搭建CIFAR10_Model

CIFAR 10：根据图片内容，识别其究竟属于哪一类（10代表有10个类别）

[CIFAR-10 and CIFAR-100 datasets](https://www.cs.toronto.edu/~kriz/cifar.html)

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/358e7bc80876463b7ff84bec85df047d.png"/>

第一次卷积：首先加了几圈 padding（图像大小不变，还是32x32），然后卷积了32次

 *  [Conv2d — PyTorch 1.10 documentation](https://pytorch.org/docs/stable/generated/torch.nn.Conv2d.html#torch.nn.Conv2d )
 *  输入尺寸是32x32，经过卷积后尺寸不变，如何设置参数？ —— padding=2，stride=1
 * 计算公式：<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/6ace0be8065ba2441f39ec56cbb60e8b.png"/>

**重要说明：**

**几个卷积核就是几通道的，一个卷积核作用于RGB三个通道后会把得到的三个矩阵的对应值相加，也就是说会合并，所以一个卷积核会产生一个通道**

**任何卷积核在设置padding的时候为保持输出尺寸不变都是卷积核大小的一半**

**通道变化时通过调整卷积核的个数来实现的，在 nn.conv2d 的参数中有 out_channel 这个参数，就是对应输出通道**

**kernel 的内容是不一样的，可以理解为不同的特征抓取，因此一个核会产生一个channel**

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：14.nn_seq.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/21 下午1:16 
'''
import torch
from torch import nn
from torch.nn import Conv2d, MaxPool2d, Flatten, Linear, Sequential

class MozijieNet(nn.Module):
    def __init__(self):
        super(MozijieNet, self).__init__()
        self.model1 = Sequential(
            Conv2d(3,32,5,padding=2),
            MaxPool2d(2),
            Conv2d(32,32,5,padding=2),
            MaxPool2d(2),
            Conv2d(32,64,5,padding=2),
            MaxPool2d(2),
            Flatten(),
            Linear(1024,64),
            Linear(64,10)
        )

    def forward(self,x):   #x为input
        x = self.model1(x)
        return x

mozijie = MozijieNet()
print(mozijie)

input = torch.ones((64,3,32,32))  #全是1，batch_size=64,3通道，32x32
output = mozijie(input)
print(output.shape)
```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251121134245919.png" alt="image-20251121134245919" style="zoom: 67%;" />

### 14.3 引入 tensorboard 可视化模型结构 

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：14.nn_seq_tensorboard.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/21 下午1:16 
'''
import torch
from torch import nn
from torch.nn import Conv2d, MaxPool2d, Flatten, Linear, Sequential
from torch.utils.tensorboard import SummaryWriter

class MozijieNet(nn.Module):
    def __init__(self):
        super(MozijieNet, self).__init__()
        self.model1 = Sequential(
            Conv2d(3,32,5,padding=2),
            MaxPool2d(2),
            Conv2d(32,32,5,padding=2),
            MaxPool2d(2),
            Conv2d(32,64,5,padding=2),
            MaxPool2d(2),
            Flatten(),
            Linear(1024,64),
            Linear(64,10)
        )

    def forward(self,x):   #x为input
        x = self.model1(x)
        return x

mozijie = MozijieNet()
print(mozijie)

input = torch.ones((64,3,32,32))  #全是1，batch_size=64,3通道，32x32
output = mozijie(input)
print(output.shape)

# 写入tensorboard计算图
writer = SummaryWriter("./logs_seq")
writer.add_graph(mozijie,input)   # add_graph 计算图
writer.close()
```

打开网址，双击图片中的矩形，可以放大每个部分：

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/ab07d2b5d4525cc9adf45640a3cb02f0.png" style="zoom: 80%;" />

## 15. 损失函数与反向传播

### 15.1 CrossEntropyLoss（交叉熵损失） 

适用于训练分类问题，有C个类别

例：三分类问题，person，dog，cat

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/5e817733bc16eb6c5ef8100e3fd43646.png"/>

 **公式中的 log 其实是ln**

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：15.nn_loss.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/21 下午2:02 
'''
import torch
from torch.nn import L1Loss, MSELoss, CrossEntropyLoss

# L1LOSS 计算公式： 1/n * Σ|yi - xi|
# 计算时要求数据类型为浮点数，不能是整型的long
inputs = torch.tensor([1,2,3],dtype=torch.float32)

targets = torch.tensor([1,2,5],dtype=torch.float32)

inputs = torch.reshape(inputs,(1,1,1,3))   # 1 batch_size, 1 channel, 1行3列
targets = torch.reshape(targets,(1,1,1,3))

loss = L1Loss()
result = loss(inputs,targets)
print(result)


# MSELOSS 计算公式： 1/n * Σ(yi - xi)^2
inputs = torch.tensor([1,2,3],dtype=torch.float32)
targets = torch.tensor([1,2,5],dtype=torch.float32)

inputs = torch.reshape(inputs,(1,1,1,3))   # 1 batch_size, 1 channel, 1行3列
targets = torch.reshape(targets,(1,1,1,3))

loss_mse = MSELoss()
result_mse = loss_mse(inputs,targets)
print(result_mse)

# CrossEntropyLoss 计算交叉熵损失
# 适用于多分类问题，输入是未经过softmax的logits，标签是类别的索引
x = torch.tensor([0.1,0.2,0.3])
y = torch.tensor([1])
x = torch.reshape(x,(1,3))
loss_cross = CrossEntropyLoss()
result_cross = loss_cross(x,y)
print(result_cross)
```



### 15.2 神经网络引入Loss Function

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：15.nn_loss_net.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/21 下午2:09 
'''

import torchvision.datasets
from torch import nn
from torch.nn import Conv2d, MaxPool2d, Flatten, Linear, Sequential
from torch.utils.data import DataLoader

dataset = torchvision.datasets.CIFAR10("./data",train=False,transform=torchvision.transforms.ToTensor(),download=True)
dataloader = DataLoader(dataset,batch_size=1)

class MozijieNet(nn.Module):
    def __init__(self):
        super(MozijieNet, self).__init__()
        self.model1 = Sequential(
            Conv2d(3,32,5,padding=2),
            MaxPool2d(2),
            Conv2d(32,32,5,padding=2),
            MaxPool2d(2),
            Conv2d(32,64,5,padding=2),
            MaxPool2d(2),
            Flatten(),
            Linear(1024,64),
            Linear(64,10)
        )

    def forward(self,x):   # x为input
        x = self.model1(x)
        return x

loss = nn.CrossEntropyLoss()

mozijie = MozijieNet()

for data in dataloader:
    imgs,targets = data
    outputs = mozijie(imgs)
    result_loss = loss(outputs,targets)
    print(result_loss)  # 神经网络输出与真实输出的误差
    result_loss.backward() # 反向传播计算梯度
    
```



<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251121141143935.png" alt="image-20251121141143935" style="zoom:80%;" />





## 16. 优化器

当使用损失函数时，可以调用损失函数的 backward，得到反向传播，反向传播可以求出每个需要调节的参数对应的梯度，有了梯度就可以利用优化器，优化器根据梯度对参数进行调整，以达到整体误差降低的目的

[torch.optim — PyTorch 1.10 documentation](https://pytorch.org/docs/stable/optim.html)

**搭建CIFAR10-Model，结合损失函数和优化器，也就是完整的训练过程**

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：16.nn_optim.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/21 下午7:02 
'''

import torch
import torchvision.datasets
from torch import nn
from torch.nn import Conv2d, MaxPool2d, Flatten, Linear, Sequential
from torch.utils.data import DataLoader

# 加载数据集并转为tensor数据类型
dataset = torchvision.datasets.CIFAR10("./data",train=False,transform=torchvision.transforms.ToTensor(),download=True)
dataloader = DataLoader(dataset,batch_size=1)


class MozijieNet(nn.Module):
    def __init__(self):
        super(MozijieNet, self).__init__()
        self.model1 = Sequential(
            Conv2d(3,32,5,padding=2),
            MaxPool2d(2),
            Conv2d(32,32,5,padding=2),
            MaxPool2d(2),
            Conv2d(32,64,5,padding=2),
            MaxPool2d(2),
            Flatten(),
            Linear(1024,64),
            Linear(64,10)
        )

    def forward(self,x):   # x为input，forward前向传播
        x = self.model1(x)
        return x

# 计算loss
loss = nn.CrossEntropyLoss()

# 搭建网络
mozijie = MozijieNet()

# 设置优化器
"""
SGD随机梯度下降法参数说明：
params: 要优化的参数，可以是一个可迭代对象（如模型的参数）或单个参数张量。
lr: 学习率，控制每次参数更新的步长大小。
momentum: 动量因子，默认为0。用于加速SGD在相关方向上的收敛，并减少震荡。
weight_decay: 权重衰减，默认为0。用于L2正则化，防止过拟合。
dampening: 抑制动量的衰减，默认为0。
nesterov: 布尔值，表示是否使用Nesterov动量，默认为False。
作用：SGD是一种常用的优化算法，用于通过计算梯度来更新神经网络的参数，以最小化损失函数。
适用场景：适用于各种神经网络训练任务，尤其是在大规模数据集上表现良好。
影响：SGD通过逐步调整参数，有助于模型更好地拟合训练数据，提高泛化能力
"""
optim = torch.optim.SGD(params=mozijie.parameters(), lr=0.01)  # SGD随机梯度下降法

# 训练20轮
for epoch in range(20):
    running_loss = 0.0  # 在每一轮开始前将loss设置为0
    for data in dataloader:
        imgs,targets = data  # imgs为输入，放入神经网络中
        outputs = mozijie(imgs)  # outputs为输入通过神经网络得到的输出，targets为实际输出
        result_loss = loss(outputs,targets) # 计算神经网络输出与真实输出的误差，即loss
        optim.zero_grad()  # 把网络模型中每一个可以调节的参数对应梯度设置为0
        result_loss.backward()  # backward反向传播求出每一个节点的梯度，是对result_loss，而不是对loss
        optim.step()  # 对每个参数进行调优
        running_loss = running_loss + result_loss  # 每一轮所有loss的和
    print(rf"第{epoch}轮损失值为：{running_loss}")
```

**训练结果：**

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251121191714471.png" alt="image-20251121191714471" style="zoom:80%;" />

## 17. 现有网络模型的使用及修改

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/7ef9b12c9811e18382f3a0707c7061e7.png" style="zoom:67%;" />

本节主要讲解 torchvision

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/97c607e002abb077178a76b1f1aae673.png"/>

本节主要讲解 Classification 里的 VGG 模型，数据集仍为 CIFAR10 数据集（主要用于分类）

[torchvision.models — Torchvision 0.11.0 documentation](https://pytorch.org/vision/stable/models.html#id2)

\---------------------------------------------------------------------------------------------------------------------------------

### 17.1 数据集 ImageNet 

使用torchvision下载ImageNet数据集前必须要先有 package scipy

```bash
pip install scipy
```



**ImageNet参数及下载** 

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/88ff0dd66a7b617c0ae714538f076d11.png" style="zoom:80%;" />

下载 ImageNet 数据集：

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：17.model_pretrained.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/21 下午7:43 
'''
import torchvision.datasets

# 加载ImageNet训练数据集，并将图像转换为张量
"""
ImageNet数据集包含大量标注好的图像，常用于训练和评估图像分类模型。
参数说明：
root: 数据集存储的根目录。
split: 指定加载训练集（'train'）还是验证集（'val'）。
download: 布尔值，指定是否从互联网下载数据集（如果本地不存在）。
transform: 应用于图像数据的转换操作，这里使用ToTensor将图像转换为张量。
"""

torchvision.datasets.ImageFolder()
train_data = torchvision.datasets.ImageNet(
    root="./data_image_net",
    split='train',
    download=True,
    transform=torchvision.transforms.ToTensor()
)
```

运行后会报错：

```python
RuntimeError: The archive ILSVRC2012_devkit_t12.tar.gz is not present in the root directory or is corrupted. You need to download it externally and place it in ./data_image_net.

进程已结束，退出代码为 1
```

**这种直接下载的方式有问题，无法下载，如果想下载数据用下面的方法，我在这里就不下载了**

迅雷下载地址：

[Imagenet完整数据集下载](https://blog.csdn.net/u014515463/article/details/80748125?spm=1001.2014.3001.5502)

第二种下载方法：

[下载、处理、加载ImageNet数据集（全网最详细）_imagenet数据集下载-CSDN博客](https://blog.csdn.net/weixin_47160526/article/details/132037269)

### 17.2 VGG16 模型 

VGG 11/13/16/19 常用16和19

**参数 pretrained=True/False** 

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/376f1914fd61c92a328f8fa415b8e895.png" style="zoom:80%;" />

 *  pretrained 为 False 的情况下，只是加载网络模型，参数都为默认参数，不需要下载
 *  为 True 时需要从网络中下载，卷积层、池化层对应的参数等等**（在ImageNet数据集中训练好的模型权重）**

```python
import torchvision.models

vgg16_false = torchvision.models.vgg16(pretrained=False)  # 另一个参数progress显示进度条，默认为True
vgg16_true = torchvision.models.vgg16(pretrained=True)
print('ok')
```

**总结：** 

 *  设置为 False 的情况，相当于网络模型中的参数都是初始化的、默认的
 *  设置为 True 时，网络模型中的参数在imageNet数据集上是训练好的，能达到比较好的效果

\---------------------------------------------------------------------------------------------------------------------------------

**vgg16 网络架构** 

```python
from torchvision.models import vgg16, VGG16_Weights

vgg16_true = vgg16(weights=VGG16_Weights.DEFAULT)    # 加载带预训练权重的VGG16模型
vgg16_false = vgg16(weights=None)      #加载非预训练的VGG16模型

print(vgg16_true)
```

输出：

```python
VGG(
  (features): Sequential(
# 输入图片先经过卷积，输入是3通道的、输出是64通道的，卷积核大小是3×3的
    (0): Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
# 非线性
    (1): ReLU(inplace=True)
# 卷积、非线性、池化...
    (2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (3): ReLU(inplace=True)
    (4): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
    (5): Conv2d(64, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (6): ReLU(inplace=True)
    (7): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (8): ReLU(inplace=True)
    (9): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
    (10): Conv2d(128, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (11): ReLU(inplace=True)
    (12): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (13): ReLU(inplace=True)
    (14): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (15): ReLU(inplace=True)
    (16): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
    (17): Conv2d(256, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (18): ReLU(inplace=True)
    (19): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (20): ReLU(inplace=True)
    (21): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (22): ReLU(inplace=True)
    (23): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
    (24): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (25): ReLU(inplace=True)
    (26): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (27): ReLU(inplace=True)
    (28): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
    (29): ReLU(inplace=True)
    (30): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
  )
  (avgpool): AdaptiveAvgPool2d(output_size=(7, 7))
  (classifier): Sequential(
    (0): Linear(in_features=25088, out_features=4096, bias=True)
    (1): ReLU(inplace=True)
    (2): Dropout(p=0.5, inplace=False)
    (3): Linear(in_features=4096, out_features=4096, bias=True)
    (4): ReLU(inplace=True)
    (5): Dropout(p=0.5, inplace=False)
# 最后线性层输出为1000（vgg16也是一个分类模型，能分出1000个类别）
    (6): Linear(in_features=4096, out_features=1000, bias=True)
  )
)
```

[ImageNet][ImageNet 1]

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/a4905bd17ccf7523d2863ae06fa86c65.png"/>

ImageNet是一个分类数量为1000类的数据集

\---------------------------------------------------------------------------------------------------------------------------------

### 17.3 修改VGG网络结构 

```java
train_data = torchvision.datasets.CIFAR10(root="./dataset",train=True,transform=torchvision.transforms.ToTensor(),download=True)
```

CIFAR10 把数据分成了10类，而 vgg16 模型把数据分成了 1000 类，如何应用这个网络模型呢？

 *  把最后线性层的 out\_features 从1000改为10
 *  在最后的线性层下面再加一层，in\_features为1000，out\_features为10

利用现有网络去改动它的结构，避免写 vgg16

很多框架会把 vgg16 当做前置的网络结构，提取一些特殊的特征，再在后面加一些网络结构，实现功能

**添加** 

以 vgg16\_true 为例讲解，实现上面的第二种思路：

```java
# 给 vgg16 添加一个线性层，输入1000个类别，输出10个类别
vgg16_true.add_module('add_linear',nn.Linear(in_features=1000,out_features=10))
print(vgg16_true)
```

结果如图：

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/47d45e88cd3e8e61c0e2cd10c08447b1.png" style="zoom: 67%;" />

如果想将 module 添加至 classifier 里：

```java
# 给 vgg16 添加一个线性层，输入1000个类别，输出10个类别
vgg16_true.classifier.add_module('add_linear',nn.Linear(in_features=1000,out_features=10))
print(vgg16_true)
```

结果如图：

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/f6239502f0f39242e741f42e14c5a213.png" style="zoom: 67%;" />

\---------------------------------------------------------------------------------------------------------------------------------

**修改** 

以上为添加，那么如何修改呢？

以 vgg16\_false 为例：

```python
vgg16_false = torchvision.models.vgg16(pretrained=False)  
print(vgg16_false)
```

结果如下：

```java
(classifier): Sequential(
    (0): Linear(in_features=25088, out_features=4096, bias=True)
    (1): ReLU(inplace=True)
    (2): Dropout(p=0.5, inplace=False)
    (3): Linear(in_features=4096, out_features=4096, bias=True)
    (4): ReLU(inplace=True)
    (5): Dropout(p=0.5, inplace=False)
    (6): Linear(in_features=4096, out_features=1000, bias=True)
  )
)
```

想将最后一层 Linear 的 out\_features 改为10：

```java
vgg16_false.classifier[6] = nn.Linear(4096,10)
print(vgg16_false)
```

结果如下：

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/19225351d63f92a9aa2783e8d980c46f.png" style="zoom:67%;" />

本节：

 *  如何加载现有的一些 pytorch 提供的网络模型
 *  如何对网络模型中的结构进行修改，包括添加自己想要的一些网络模型结构

**完整代码**

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：17.model_pretrained.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/21 下午7:43 
'''
import torchvision.datasets
import torch
from torchvision.models import vgg16, VGG16_Weights

# 加载ImageNet训练数据集，并将图像转换为张量
"""
ImageNet数据集包含大量标注好的图像，常用于训练和评估图像分类模型。
参数说明：
root: 数据集存储的根目录。
split: 指定加载训练集（'train'）还是验证集（'val'）。
download: 布尔值，指定是否从互联网下载数据集（如果本地不存在）。
transform: 应用于图像数据的转换操作，这里使用ToTensor将图像转换为张量。
"""
# train_data = torchvision.datasets.ImageNet(
#     root="./data_image_net",
#     split='train',
#     download=True,
#     transform=torchvision.transforms.ToTensor()
# )
"""
VGG16的预训练是在ImageNet上面训练的
"""
vgg16_true = vgg16(weights=VGG16_Weights.DEFAULT)    # 加载带预训练权重的VGG16模型
vgg16_false = vgg16(weights=None)      #加载非预训练的VGG16模型

# 打印未修改的模型结构
print(vgg16_true)

train_data = torchvision.datasets.CIFAR10(
    "./data",
    train=True,
    transform=torchvision.transforms.ToTensor(),
    download=True
)

# 1.新增
# 在模型后面添加一个新的线性层，用于适应CIFAR-10的10个类别
"""
add_module参数说明：
name: 新增模块的名称，作为该模块在模型中的标识符。
module: 要添加的模块，可以是任何继承自torch.nn.Module的子类实例。
作用：将一个新的子模块添加到现有的神经网络模型中，使其能够扩展功能或改变结构。
适用场景：适用于需要动态修改模型结构的场景，如在预训练模型后添加自定义层以适应特定任务。
"""
vgg16_true.classifier.add_module("add_linear", torch.nn.Linear(in_features=1000, out_features=10))
# 重新打印修改后的模型结构
print(vgg16_true)

# 2.修改
# 或者直接修改最后一层分类器
vgg16_false.classifier[6] = torch.nn.Linear(in_features=4096, out_features=10)
print(vgg16_false)
```



## 18. 模型的保存与读取 

### 18.1 两种方式保存模型 

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：18.model_save.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/21 下午10:33 
'''
import torch
import torchvision
from torch import nn

# 网络中模型的参数是没有经过训练的、初始化的参数
vgg16 = torchvision.models.vgg16(weights = None)
# 保存方式1,模型结构+模型参数
torch.save(vgg16, "vgg16_method1.pth")

# 保存方式2，模型参数（官方推荐）
# 把vgg16的状态保存为字典形式（Python中的一种数据格式）
torch.save(vgg16.state_dict(), "vgg16_method2.pth")

# 陷阱
class MozijieNet(nn.Module):
    def __init__(self):
        super(MozijieNet, self).__init__()
        self.conv1 = nn.Conv2d(3, 64, kernel_size=3)

    def forward(self, x):
        x = self.conv1(x)
        return x

mozijie = MozijieNet()
torch.save(mozijie, "mozijie_method1.pth"
```

运行后项目下会多出以下3个文件：

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251121224838209.png" alt="image-20251121224838209" style="zoom:80%;" />

### 18.2 两种方式加载模型 

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：18.model_load.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/21 下午10:34 
'''
import torch

# 方式1-》保存方式1，加载模型
import torchvision
from torch import nn
"""
这里的weights_only参数是为了防止加载模型时只加载权重参数而忽略模型结构。
当weights_only=False时，表示加载整个模型，包括模型结构和权重参数。
当weights_only=True时，表示只加载模型的权重参数，而不包括模型结构。
"""
model = torch.load("vgg16_method1.pth",weights_only=False)
# print(model)

# 方式2，加载模型
vgg16 = torchvision.models.vgg16(weights = None)
vgg16.load_state_dict(torch.load("vgg16_method2.pth",weights_only=False))
# model = torch.load("vgg16_method2.pth")
# print(vgg16)

"""
陷阱1:用方式1保存的话，加载时要让程序能够访问到其定义模型的类，否则会报错
"""

class MozijieNet(nn.Module):
    def __init__(self):
        super(MozijieNet, self).__init__()
        self.conv1 = nn.Conv2d(3, 64, kernel_size=3)

    def forward(self, x):
        x = self.conv1(x)
        return x

model = torch.load('mozijie_method1.pth',weights_only=False)
print(model)
```



<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251121225100043.png" alt="image-20251121225100043" style="zoom:80%;" />



## 19. 完整模型训练

### **19.1 model代码**

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：19.model.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/21 下午10:59 
'''
import torch
from torch import nn

# 搭建神经网络
class MozijieNet(nn.Module):
    def __init__(self):
        super(MozijieNet, self).__init__()
        self.model = nn.Sequential(
            nn.Conv2d(3, 32, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 32, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Conv2d(32, 64, 5, 1, 2),
            nn.MaxPool2d(2),
            nn.Flatten(),
            nn.Linear(64*4*4, 64),
            nn.Linear(64, 10)
        )

    def forward(self, x):
        x = self.model(x)
        return x


if __name__ == '__main__':
    mzj = MozijieNet()
    input = torch.ones((64, 3, 32, 32))
    output = mzj(input)
    print(output.shape)
```

### 19.2 train代码

```python
#!/usr/bin/env python  
# -*- coding: UTF-8 -*-  
'''
@Project ：xiaotudui-pytorch  
@File    ：19.train.py        
@IDE     ：PyCharm             
@Author  ：mozijie             
@Date    ：2025/11/21 下午10:59 
'''
import torch  # 导入 PyTorch 主包，提供张量与自动求导等功能
import torchvision  # 导入 torchvision，包含数据集与图像处理工具
from torch.utils.tensorboard import SummaryWriter  # 从 tensorboard 工具集中导入日志记录器 SummaryWriter
from torch import nn  # 导入 nn（neural network）模块用于构建网络结构与层
from torch.utils.data import DataLoader  # 导入 DataLoader 用于批量加载数据集


class MozijieNet(nn.Module):  # 定义一个自定义的神经网络模型类，继承 nn.Module
    def __init__(self):  # 构造函数，在实例化时初始化网络结构
        super(MozijieNet, self).__init__()  # 调用父类构造函数，注册参数与子模块
        self.model = nn.Sequential(  # 使用 nn.Sequential 搭建前馈网络，按顺序执行各层
            nn.Conv2d(3, 32, 5, 1, 2),      # 卷积层1：输入通道3(RGB)，输出通道32，卷积核5，步长1，padding=2 保持尺寸；输出 (B,32,32,32)
            nn.MaxPool2d(2),                # 最大池化1：核大小2，步长2，空间尺寸减半；输出 (B,32,16,16)
            nn.Conv2d(32, 32, 5, 1, 2),     # 卷积层2：输入通道32，输出通道32，保持空间尺寸；输出 (B,32,16,16)
            nn.MaxPool2d(2),                # 最大池化2：再次减半；输出 (B,32,8,8)
            nn.Conv2d(32, 64, 5, 1, 2),     # 卷积层3：通道数升至64，尺寸保持；输出 (B,64,8,8)
            nn.MaxPool2d(2),                # 最大池化3：尺寸减半；输出 (B,64,4,4)
            nn.Flatten(),                   # 展平层：将 (B,64,4,4) 展开为 (B, 64*4*4=1024)
            nn.Linear(64*4*4, 64),          # 全连接层1：输入1024特征，输出64，进行特征抽象
            nn.Linear(64, 10)               # 全连接层2：输入64，输出10，对应10个类别（如 CIFAR10）
        )

    def forward(self, x):  # 定义前向传播逻辑，输入张量 x 经网络得到输出
        x = self.model(x)  # 调用顺序容器执行各层计算
        return x


# 准备数据集（训练集）
train_data = torchvision.datasets.CIFAR10(  # 调用 CIFAR10 数据集类
    root="./data",                        # 数据集存储根目录（若不存在将自动创建）
    train=True,                            # 指定为训练集
    transform=torchvision.transforms.ToTensor(),  # 将 PIL/Image 转换为张量并归一化到 [0,1]
    download=True                          # 若本地无数据则自动下载
)
# 准备数据集（测试集）
test_data = torchvision.datasets.CIFAR10(   # 调用 CIFAR10 数据集类
    root="./data",                        # 同一根目录
    train=False,                           # 指定为测试集
    transform=torchvision.transforms.ToTensor(),  # 同样进行张量转换
    download=True                          # 自动下载测试集数据
)

# length 长度
train_data_size = len(train_data)  # 训练集样本总数（CIFAR10 为 50000）
test_data_size = len(test_data)    # 测试集样本总数（CIFAR10 为 10000）

print("训练数据集的长度为：{}".format(train_data_size))  # 打印训练集大小
print("测试数据集的长度为：{}".format(test_data_size))    # 打印测试集大小


# 利用 DataLoader 来加载数据集 
train_dataloader = DataLoader(train_data, batch_size=64)  # 构造训练集 DataLoader，每批 64 张图片
test_dataloader = DataLoader(test_data, batch_size=64)    # 构造测试集 DataLoader，每批 64 张图片

# 创建网络模型 
mzj = MozijieNet()  # 得到一个网络对象，用于后续训练与推理

# 定义用于分类任务的损失函数（交叉熵）
loss_fn = nn.CrossEntropyLoss() 

# 优化器  
learning_rate = 1e-2  # 设置学习率为 0.01
optimizer = torch.optim.SGD(mzj.parameters(), lr=learning_rate)  # 使用随机梯度下降优化模型参数

# 设置训练网络的一些参数  
# 记录训练的次数 
total_train_step = 0  
# 记录测试的次数 
total_test_step = 0 
# 训练的轮数
epoch = 10  # 将整个训练集迭代 10 次

# 创建 TensorBoard 日志记录器，用于可视化损失与准确率
writer = SummaryWriter("./logs_train")  # 输出日志目录为 ./logs_train

for i in range(epoch):  # 外层循环，遍历每一个训练轮次 epoch
    print("-------第 {} 轮训练开始-------".format(i+1))  

    # 切换模型到训练模式（启用 dropout/batchnorm 等）
    mzj.train()  # 调用 train() 方法
    for data in train_dataloader:  # 遍历训练数据的每个批次
        imgs, targets = data            # 解包当前批次：imgs 为图像张量 (B,3,32,32)，targets 为标签 (B)
        outputs = mzj(imgs)             # 前向传播得到 logits 输出 (B,10)
        loss = loss_fn(outputs, targets)  # 计算当前批次的交叉熵损失

        # 反向传播与参数更新
        optimizer.zero_grad()  # 清空上一批次残留的梯度
        loss.backward()        # 反向传播计算当前梯度
        optimizer.step()       # 使用优化器根据梯度更新参数

        total_train_step = total_train_step + 1  # 训练步计数器加 1
        if total_train_step % 100 == 0:  # 每 100 个批次打印一次当前损失并记录
            print("训练次数：{}, Loss: {}".format(total_train_step, loss.item()))  # 打印当前训练次数与对应损失
            writer.add_scalar("train_loss", loss.item(), total_train_step)       # 写入 TensorBoard 标量（训练损失）

    # 进入测试阶段，不更新参数
    mzj.eval()  # 切换为评估模式（禁用 dropout/batchnorm 的训练行为）
    total_test_loss = 0  # 初始化本轮测试集总损失累加器
    total_accuracy = 0   # 初始化本轮测试集总正确样本数
    with torch.no_grad():  # 关闭梯度计算，节省显存与加速推理
        for data in test_dataloader:  # 遍历测试集每一个批次
            imgs, targets = data               # 获取测试批次数据与标签
            outputs = mzj(imgs)                # 前向传播得到输出 logits
            loss = loss_fn(outputs, targets)   # 计算该批次的损失
            total_test_loss = total_test_loss + loss.item()  # 累加损失（使用标量值）
            accuracy = (outputs.argmax(1) == targets).sum().item()  # 计算该批次预测正确的样本数并转为 Python 标量
            total_accuracy = total_accuracy + accuracy       # 累加所有批次的正确样本数

    print("整体测试集上的Loss: {}".format(total_test_loss))  # 打印整个测试集的总损失（未取平均）
    print("整体测试集上的正确率: {}".format(total_accuracy/test_data_size))  # 打印准确率（正确样本数/总样本数）
    writer.add_scalar("test_loss", total_test_loss, total_test_step)            # 记录测试损失到 TensorBoard
    writer.add_scalar("test_accuracy", total_accuracy/test_data_size, total_test_step)  # 记录测试准确率
    total_test_step = total_test_step + 1  # 测试轮计数器加 1

    torch.save(mzj, "mozijie_{}.pth".format(i))  # 保存当前轮次的整个模型对象到文件（包含结构+参数）
    print("模型已保存")  # 打印保存提示

writer.close()  # 关闭 SummaryWriter，确保缓冲数据写入磁盘

```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251123231709460.png" alt="image-20251123231709460" style="zoom:80%;" />

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251123233455090.png" alt="image-20251123233455090" style="zoom:80%;" />

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251123233530499.png" alt="image-20251123233530499" style="zoom:80%;" />

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251123233541845.png" alt="image-20251123233541845" style="zoom:80%;" />

## 20.使用GPU训练模型

**这里只介绍最常用的那种方式**

**要在以下四个地方使用：**

```python
import torch

# 设置设备（GPU 优先，否则使用 CPU）
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print("Using device:", device)

"""
要在以下三个地方后面加上.to(device)
"""
```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/87c3182ff1bc4887d622759a15e2d2f0.png" style="zoom:67%;" />

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：20.train_gpu.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/23 下午11:28 
'''




import torch  # 导入 PyTorch 主包，提供张量与自动求导等功能
import torchvision  # 导入 torchvision，包含数据集与图像处理工具
from torch.utils.tensorboard import SummaryWriter  # 从 tensorboard 工具集中导入日志记录器 SummaryWriter
from torch import nn  # 导入 nn（neural network）模块用于构建网络结构与层
from torch.utils.data import DataLoader  # 导入 DataLoader 用于批量加载数据集

# 设置设备（GPU 优先，否则使用 CPU）
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print("Using device:", device)

class MozijieNet(nn.Module):  # 定义一个自定义的神经网络模型类，继承 nn.Module
    def __init__(self):  # 构造函数，在实例化时初始化网络结构
        super(MozijieNet, self).__init__()  # 调用父类构造函数，注册参数与子模块
        self.model = nn.Sequential(  # 使用 nn.Sequential 搭建前馈网络，按顺序执行各层
            nn.Conv2d(3, 32, 5, 1, 2),      # 卷积层1：输入通道3(RGB)，输出通道32，卷积核5，步长1，padding=2 保持尺寸；输出 (B,32,32,32)
            nn.MaxPool2d(2),                # 最大池化1：核大小2，步长2，空间尺寸减半；输出 (B,32,16,16)
            nn.Conv2d(32, 32, 5, 1, 2),     # 卷积层2：输入通道32，输出通道32，保持空间尺寸；输出 (B,32,16,16)
            nn.MaxPool2d(2),                # 最大池化2：再次减半；输出 (B,32,8,8)
            nn.Conv2d(32, 64, 5, 1, 2),     # 卷积层3：通道数升至64，尺寸保持；输出 (B,64,8,8)
            nn.MaxPool2d(2),                # 最大池化3：尺寸减半；输出 (B,64,4,4)
            nn.Flatten(),                   # 展平层：将 (B,64,4,4) 展开为 (B, 64*4*4=1024)
            nn.Linear(64*4*4, 64),          # 全连接层1：输入1024特征，输出64，进行特征抽象
            nn.Linear(64, 10)               # 全连接层2：输入64，输出10，对应10个类别（如 CIFAR10）
        )

    def forward(self, x):  # 定义前向传播逻辑，输入张量 x 经网络得到输出
        x = self.model(x)  # 调用顺序容器执行各层计算
        return x


# 准备数据集（训练集）
train_data = torchvision.datasets.CIFAR10(  # 调用 CIFAR10 数据集类
    root="./data",                        # 数据集存储根目录（若不存在将自动创建）
    train=True,                            # 指定为训练集
    transform=torchvision.transforms.ToTensor(),  # 将 PIL/Image 转换为张量并归一化到 [0,1]
    download=True                          # 若本地无数据则自动下载
)
# 准备数据集（测试集）
test_data = torchvision.datasets.CIFAR10(   # 调用 CIFAR10 数据集类
    root="./data",                        # 同一根目录
    train=False,                           # 指定为测试集
    transform=torchvision.transforms.ToTensor(),  # 同样进行张量转换
    download=True                          # 自动下载测试集数据
)

# length 长度
train_data_size = len(train_data)  # 训练集样本总数（CIFAR10 为 50000）
test_data_size = len(test_data)    # 测试集样本总数（CIFAR10 为 10000）

print("训练数据集的长度为：{}".format(train_data_size))  # 打印训练集大小
print("测试数据集的长度为：{}".format(test_data_size))    # 打印测试集大小


# 利用 DataLoader 来加载数据集
train_dataloader = DataLoader(train_data, batch_size=64)  # 构造训练集 DataLoader，每批 64 张图片
test_dataloader = DataLoader(test_data, batch_size=64)    # 构造测试集 DataLoader，每批 64 张图片

# 创建网络模型
mzj = MozijieNet()  # 得到一个网络对象，用于后续训练与推理
mzj = mzj.to(device)  # 将模型移动到指定设备（GPU 或 CPU）

# 定义用于分类任务的损失函数（交叉熵）
loss_fn = nn.CrossEntropyLoss()
loss_fn = loss_fn.to(device)  # 将损失函数移动到指定设备

# 优化器
learning_rate = 1e-2  # 设置学习率为 0.01
optimizer = torch.optim.SGD(mzj.parameters(), lr=learning_rate)  # 使用随机梯度下降优化模型参数

# 设置训练网络的一些参数
# 记录训练的次数
total_train_step = 0
# 记录测试的次数
total_test_step = 0
# 训练的轮数
epoch = 10  # 将整个训练集迭代 10 次

# 创建 TensorBoard 日志记录器，用于可视化损失与准确率
writer = SummaryWriter("./logs_train_gpu")  # 输出日志目录为 ./logs_train

for i in range(epoch):  # 外层循环，遍历每一个训练轮次 epoch
    print("-------第 {} 轮训练开始-------".format(i+1))

    # 切换模型到训练模式（启用 dropout/batchnorm 等）
    mzj.train()  # 调用 train() 方法
    for data in train_dataloader:  # 遍历训练数据的每个批次
        imgs, targets = data            # 解包当前批次：imgs 为图像张量 (B,3,32,32)，targets 为标签 (B)
        imgs = imgs.to(device)        # 将输入图像张量移动到指定设备
        targets = targets.to(device)  # 将目标标签张量移动到指定设备
        outputs = mzj(imgs)             # 前向传播得到 logits 输出 (B,10)
        loss = loss_fn(outputs, targets)  # 计算当前批次的交叉熵损失

        # 反向传播与参数更新
        optimizer.zero_grad()  # 清空上一批次残留的梯度
        loss.backward()        # 反向传播计算当前梯度
        optimizer.step()       # 使用优化器根据梯度更新参数

        total_train_step = total_train_step + 1  # 训练步计数器加 1
        if total_train_step % 100 == 0:  # 每 100 个批次打印一次当前损失并记录
            print("训练次数：{}, Loss: {}".format(total_train_step, loss.item()))  # 打印当前训练次数与对应损失
            writer.add_scalar("train_loss", loss.item(), total_train_step)       # 写入 TensorBoard 标量（训练损失）

    # 进入测试阶段，不更新参数
    mzj.eval()  # 切换为评估模式（禁用 dropout/batchnorm 的训练行为）
    total_test_loss = 0  # 初始化本轮测试集总损失累加器
    total_accuracy = 0   # 初始化本轮测试集总正确样本数
    with torch.no_grad():  # 关闭梯度计算，节省显存与加速推理
        for data in test_dataloader:  # 遍历测试集每一个批次
            imgs, targets = data               # 获取测试批次数据与标签
            imgs = imgs.to(device)        # 将输入图像张量移动到指定设备
            targets = targets.to(device)  # 将目标标签张量移动到指定设备
            outputs = mzj(imgs)                # 前向传播得到输出 logits
            loss = loss_fn(outputs, targets)   # 计算该批次的损失
            total_test_loss = total_test_loss + loss.item()  # 累加损失（使用标量值）
            accuracy = (outputs.argmax(1) == targets).sum().item()  # 计算该批次预测正确的样本数并转为 Python 标量
            total_accuracy = total_accuracy + accuracy       # 累加所有批次的正确样本数

    print("整体测试集上的Loss: {}".format(total_test_loss))  # 打印整个测试集的总损失（未取平均）
    print("整体测试集上的正确率: {}".format(total_accuracy/test_data_size))  # 打印准确率（正确样本数/总样本数）
    writer.add_scalar("test_loss", total_test_loss, total_test_step)            # 记录测试损失到 TensorBoard
    writer.add_scalar("test_accuracy", total_accuracy/test_data_size, total_test_step)  # 记录测试准确率
    total_test_step = total_test_step + 1  # 测试轮计数器加 1

    torch.save(mzj, "mozijie_{}.pth".format(i))  # 保存当前轮次的整个模型对象到文件（包含结构+参数）
    print("模型已保存")  # 打印保存提示

writer.close()  # 关闭 SummaryWriter，确保缓冲数据写入磁盘

```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251123233648758.png" alt="image-20251123233648758" style="zoom:80%;" />



## 21. 使用训练好的模型测试

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：xiaotudui-pytorch 
@File    ：21.test.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/11/23 下午11:50 
'''
import torch  # 导入 PyTorch 主包，提供张量与自动求导等功能
import torchvision  # 导入 torchvision，包含数据集与图像处理工具
from torch import nn  # 导入 nn（neural network）模块用于构建网络结构与层
from PIL import Image

# 设置设备（GPU 优先，否则使用 CPU）
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print("Using device:", device)

class MozijieNet(nn.Module):  # 定义一个自定义的神经网络模型类，继承 nn.Module
    def __init__(self):  # 构造函数，在实例化时初始化网络结构
        super(MozijieNet, self).__init__()  # 调用父类构造函数，注册参数与子模块
        self.model = nn.Sequential(  # 使用 nn.Sequential 搭建前馈网络，按顺序执行各层
            nn.Conv2d(3, 32, 5, 1, 2),      # 卷积层1：输入通道3(RGB)，输出通道32，卷积核5，步长1，padding=2 保持尺寸；输出 (B,32,32,32)
            nn.MaxPool2d(2),                # 最大池化1：核大小2，步长2，空间尺寸减半；输出 (B,32,16,16)
            nn.Conv2d(32, 32, 5, 1, 2),     # 卷积层2：输入通道32，输出通道32，保持空间尺寸；输出 (B,32,16,16)
            nn.MaxPool2d(2),                # 最大池化2：再次减半；输出 (B,32,8,8)
            nn.Conv2d(32, 64, 5, 1, 2),     # 卷积层3：通道数升至64，尺寸保持；输出 (B,64,8,8)
            nn.MaxPool2d(2),                # 最大池化3：尺寸减半；输出 (B,64,4,4)
            nn.Flatten(),                   # 展平层：将 (B,64,4,4) 展开为 (B, 64*4*4=1024)
            nn.Linear(64*4*4, 64),          # 全连接层1：输入1024特征，输出64，进行特征抽象
            nn.Linear(64, 10)               # 全连接层2：输入64，输出10，对应10个类别（如 CIFAR10）
        )

    def forward(self, x):  # 定义前向传播逻辑，输入张量 x 经网络得到输出
        x = self.model(x)  # 调用顺序容器执行各层计算
        return x


# 创建网络模型
mzj = MozijieNet()  # 得到一个网络对象，用于后续训练与推理
mzj = mzj.to(device)  # 将模型移动到指定设备（GPU 或 CPU）


test_img_path = rf"test_img/bird.png"  # 测试图像路径

img = Image.open(test_img_path)  # 使用 PIL 库打开图像文件
img = img.convert('RGB')    # 转换为 RGB 模式，确保有3个通道
transform = torchvision.transforms.Compose(
    [torchvision.transforms.Resize((32,32)),  # 32x32大小的PIL Image
    torchvision.transforms.ToTensor()])  # 转为Tensor数据类型
img = transform(img)
print(img.shape)

img = torch.reshape(img,(1,3,32,32))  # 调整张量形状以匹配模型输入要求（批量大小为1，3通道，32x32尺寸）
img = img.to(device)  # 将图像张量移动到指定设备（GPU 或 CPU）
print(img.shape)

model = torch.load(r"mozijie_9.pth",weights_only=False,map_location=device)  # 加载保存的模型参数文件
model = model.to(device)  # 将模型移动到指定设备（GPU 或 CPU）
model.eval()  # 模型转化为测试类型
with torch.no_grad():  # 节约内存和性能
    output = model(img) # 前向传播，得到模型输出

print(output.argmax(1)) # 输出预测类别索引


```

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251124000708865.png" alt="image-20251124000708865" style="zoom:80%;" />

<img src="computer_vision_notebook/xiaotudui_Pytorch_Tutorial/online_notes.assets/image-20251124000751722.png" alt="image-20251124000751722" style="zoom:80%;" />



**这里我们可以看到第二类是bird，鸟，说明我们预测正确**



## 22. 看看开源项目

略













































