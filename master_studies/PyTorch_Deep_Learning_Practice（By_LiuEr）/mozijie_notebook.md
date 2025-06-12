# 《PyTorch深度学习实践》学习笔记

## ——BY mozijie

本课程为河北工业大学刘洪普老师所授

课程地址：https://www.bilibili.com/video/BV1Y7411d7Ys?spm_id_from=333.788.videopod.episodes&vd_source=7c23485904ea3584fb46e603e43ca789

## 本人学习进度：12.循环神经网络（基础篇）

## 1.Overview

暂无

## 2.线性模型

### 2.1 课堂例题（y=wx)

**——穷举可视化y=w·x关于权重w取值的情况**

> ①对给定数据训练一个线性模型，但是不知道权重w取什么值训练效果更好
> ——穷举法，在一个确定的范围内，按照一定间隔对权重w进行采样并计算不同情况下的损失值
> 绘制出误差值和权重值之间的函数曲线，就可以判断最优权重值的取值
>
> ②绘图法在深度学习中常用来确定算法中的超参数值，如epoch；
> 也会利用可视化工具来检查当前模型的训练程度，来判断是否出现过拟合或欠拟合的状况

```py
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：PyTorch_Deep_Learning_Practice 
@File    ：visual_MES.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/4/25 下午4:08 
"""

import numpy as np
import matplotlib as mpl

mpl.use('TkAgg')  # 设置 Matplotlib 使用 TkAgg 后端
import matplotlib.pyplot as plt

# 实现定义好数据集的x值和y值
x_data = [1.0, 2.0, 3.0]
y_data = [2.0, 4.0, 6.0]


# 定义预测函数，因为是要确定损失值随着w的变化情况
def forward(x, w):
    return x * w


# 定义损失函数的计算方式，仅针对一个样本而言
def loss(x, y, w):
    y_pred = forward(x, w)
    return (y - y_pred) * (y - y_pred)


# 程序的目的是为了比较mse和w之间的关系，所以要把其二者的结果对应放入列表中
w_list = []
mse_list = []

# 根据老师说的【穷举法】思想，在一个确定的范围找到mse和w之间的关系
for w in np.arange(0.0, 4.1, 0.1):
    print('w = ', w)
    l_sum = 0  # 存放所有样本的损失函数值
    for x_val, y_val in zip(x_data, y_data):
        y_pred_val = forward(x_val, w)
        loss_val = loss(x_val, y_val, w)
        l_sum += loss_val
        print('\t', x_val, y_val, y_pred_val, loss_val)
    print('MSE=', l_sum / len(x_data))
    w_list.append(w)
    mse_list.append(l_sum / len(x_data))

plt.plot(w_list, mse_list)
plt.ylabel('Loss')
plt.xlabel('w')
plt.show()

```

**运行结果：**

<img alt="img" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/img.png"/>



### 2.2 课后习题（y=w·x+b)

**——穷举并可视化y=w·x+b关于权重对(w,b)的情况**

> ①np.meshgrid()就是将给定的x轴和y轴分别的一维向量，转换生成所包含的二维平面区域的二维向量，生成两个二维向量，
> 都是[y_dim,x_dim]大小的，其中依次从两个二维向量取出坐标组成的点优先按照x轴进行变化。
>
> ②三维函数绘制模板：
> fig = plt.figure()
> ax = fig.add_subplot(111, projection='3d')
> ax.plot_surface(x, y, z)

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：PyTorch_Deep_Learning_Practice
@File    ：visual_MES_exercise.py
@IDE     ：PyCharm
@Author  ：Mozijie
@Date    ：2025/4/25 下午4:26
"""

import numpy as np
import matplotlib as mpl

mpl.use('TkAgg')  # 设置 Matplotlib 使用 TkAgg 后端
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
from matplotlib import cm

# 自定义数据集的x值和y值
x_data = [1.0, 2.0, 3.0]
y_data = [9.0, 11.0, 13.0]


# 定义预测函数
def forward(x, w, b):
    return x * w + b


# 定义损失函数（单个样本）
def loss(x, y, w, b):
    y_pred = forward(x, w, b)
    return (y - y_pred) ** 2


# 网格参数
w_list = np.arange(0.0, 4.1, 0.1)
b_list = np.arange(5.0, 9.0, 0.1)

# 生成网格
ww, bb = np.meshgrid(w_list, b_list)  # 注意：ww的形状是(len(b_list), len(w_list))

mse_list = []
mse_w_b_list = []

# 调整循环顺序：先遍历b，再遍历w
for b in b_list:  # 先遍历b
    for w in w_list:  # 再遍历w
        l_sum = 0
        for x_val, y_val in zip(x_data, y_data):
            l_sum += loss(x_val, y_val, w, b)
        mse_val = l_sum / len(x_data)
        mse_list.append(mse_val)
        mse_w_b_list.append([mse_val, w, b])

# 转换为二维数组，形状为 (len(b_list), len(w_list))
mse = np.array(mse_list).reshape(len(b_list), len(w_list))  # 正确的形状

# 寻找最小MSE及其对应的w和b
min_mse = np.min(mse)
min_indices = np.unravel_index(np.argmin(mse), mse.shape)
min_b = b_list[min_indices[0]]  # 因为b是第一个维度（行）
min_w = w_list[min_indices[1]]  # w是第二个维度（列）

print(f"Minimum MSE: {min_mse}")
print(f"Optimal w: {min_w}")
print(f"Optimal b: {min_b}")

# 绘制3D曲面图
fig = plt.figure(figsize=(12, 8))
ax = fig.add_subplot(111, projection='3d')

# 绘制曲面
surf = ax.plot_surface(ww, bb, mse, rstride=1, cstride=1, cmap=cm.viridis, alpha=0.8)

# 标记最低点
ax.scatter(min_w, min_b, min_mse, color='red', s=100, marker='o', label='Minimum Point')

# 添加文本标注
text = f'y = {min_w:.1f}x + {min_b:.1f}'  # 构造公式字符串
ax.text(min_w, min_b, min_mse + 5, text, fontsize=16, color='red', ha='center', va='bottom')

# 添加标签和标题
ax.set_xlabel('Weight (w)')
ax.set_ylabel('Bias (b)')
ax.set_zlabel('Mean Squared Error (MSE)')
ax.set_title('3D Surface Plot of MSE vs. w and b')

# 添加颜色条和图例
fig.colorbar(surf, shrink=0.5, aspect=5)
ax.legend()

# 调整视角（可选）
ax.view_init(elev=30, azim=75)  # 设置视角仰角30度，方位角45度

plt.tight_layout()
plt.show()

```

**运行结果：**

<img alt="exercise_img" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/exercise_img.png"/>

## 3.梯度下降算法

### 3.1 穷举法的弊端

> 按照上面一讲，我们使用穷举法罗列出所有可能的权重取值情况，然后通过可视化工具来寻找其中的最优解，这是有很多弊端的：
>
> - 其一，随着权重个数的增多，搜素的空间会呈指数型增长
> - 其二，最优解的分布与权重的关系可能很复杂，不能用简单的线性搜索去解决

### 3.2 分治法的弊端

> 按照我们传统算法设计思路，我么也会联想到用分治法：
>
> <img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/7652a32f6ad226aa81cbd4f32014a48e.png"/>
>
> - 其一，在现在深度学习网络中，权重参数实在过多，分治法也很难应付；
> - 其二，分治法很容易陷入局部最优的困境中

### 3.3 贪心法

> 所谓梯度下降法，就是在每一步权重更新时，沿着**当前负梯度方向（最小化问题）**按照规定的步长进行下一步搜寻。
>
> <img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/6532908dcc8eff4a7b35216d93a9fcce.png"/>
>
> - 本质就是贪心算法——也就是按照当前最优的策略进行搜寻；
>
> - 贪心算法并不一定能得到最优结果，梯度下降算法也是一样，不一定总能找到全局最优解，反而会找到很多局部最优点。
>
> - 虽然梯度下降算法存在着这样的问题，但是它依然被广泛应用于深度学习之中，因为在深度学习模型所使用的激活函数中“局部最优”的问题并不显著，往往找到的都是全局最优点。反而更应该关注“鞍点”梯度消失的问题。如下图，当g等于0时，也就是图像为水平时，w就不在改变。梯度下降算法也就不再迭代。
>
>   <img alt="image-20250507153014194" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250507153014194.png"/>
>
> - 



### 3.4 梯度下降与随机梯度下降

> **梯度下降公式的由来：**
>
> 将损失函数对w进行求导，计算导数，作为梯度下降公式里面的更新权重，这里面的α作为学习率，**在训练过程中，如果损失值出现增大的情况，说明训练失败，最多的原因就是学习率设置过大，降低学习率重新计算即可**
>
> <img alt="image-20250507153641703" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250507153641703.png"/>
>
> ①比较其二者的权重更新公式
>
> - 所谓随机梯度下降，就是每次只随机取一个数据点计算其损失值，并根据这一个样本的损失值进行梯度求解，权重更新；
>
> - 随机梯度下降和梯度下降都是batch-梯度下降的特况
>
> <img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/68bdbd4271de9fa723f9efdbc83d0dc9.png"/>
>
> ②随机梯度下降的特点
>
> - **可以辅助解决“鞍点”的问题**，因为在真实场景中采集到的数据都是有噪声的，**噪声带进来的随机偏差就有可能帮助训练走出“梯度消失”的困境；**
> - 但是因为每两个数据点之间的训练是相互关联的，所以随机梯度下降算法中对于不同数据点函数值的计算**不可以采取并行的技术**；
> - **batch-梯度下降**的提出就可以很好地**兼顾梯度下降的并行化技术和随机梯度下降的优化性能** 。batch是指，每次选一批样本进行训练，这一批既不会包含全部，也不是单个计算。

### 3.5 梯度下降代码实现

```py
'''
梯度下降代码示例（含绘图）
'''
import numpy as np
import matplotlib.pyplot as plt
from matplotlib import cm
from mpl_toolkits.mplot3d import Axes3D

# 实现定义好数据集的x值和y值
x_data = [1.0,2.0,3.0]
y_data = [2.0,4.0,6.0]

# 定义好初始权重
w = 1.0

# 定义预测函数，因为是要确定损失值随着w的变化情况
# 后续会根据计算的梯度值来动态更新权重w
def forward(x):
    return x*w

# 定义损失函数的计算方式，使用梯度下降算法，计算数据集内所有样本的损失MSE
def cost(x,y):
    cost = 0
    for x_i,y_i in zip(x,y):
        y_i_p = forward(x_i)
        cost += (y_i_p-y_i) ** 2
    return cost / len(x)

# 定义梯度的计算方式，根据MSE的公式得到梯度的一般表达式
def gradient(x,y):
    grad = 0
    for x_i,y_i in zip(x,y):
        grad += 2 * x_i * (x_i * w - y_i)
    return grad / len(x)

print('Predict (before training)',4,forward(4))
epochs = []
costs = []
# 按照梯度下降算法进行训练
for epoch in range(100):
    epochs.append(epoch)
    cost_val = cost(x_data,y_data)	# 计算MSE
    costs.append(cost_val)	# 如果对用于绘制表格的数据也使用round操作，则图形的锯齿形会很明显不平滑
    grad_val = gradient(x_data,y_data)		# 计算更新权重
    w -= 0.01 * grad_val	# 梯度下降公式，这里学习率设置为0.01
    print('Epoch:',epoch,'w=',round(w,2),'loss=',round(cost_val,2))
print('Predict (after training)',4,round(forward(4),2))

plt.plot(epochs,costs)
plt.ylabel('Cost')
plt.xlabel('Epoch')
plt.show()

```

**运行结果：**

<img alt="image-20250507161107286" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250507161107286.png"/>

### 3.6 随机梯度下降代码实现

```python
'''
随机梯度下降代码示例（含绘图）
'''
import numpy as np
import random
import matplotlib.pyplot as plt
from matplotlib import cm
from mpl_toolkits.mplot3d import Axes3D

# 实现定义好数据集的x值和y值
x_data = [1.0, 2.0, 3.0]
y_data = [2.0, 4.0, 6.0]

# 定义好初始权重
w = 1.0


# 定义预测函数，因为是要确定损失值随着w的变化情况
# 后续会根据计算的梯度值来动态更新权重w
def forward(x):
    return x * w


# 定义损失函数的计算方式，使用梯度下降算法，计算传入的某一样本的损失MSE
def loss(x, y):
    y_pred = forward(x)
    return (y - y_pred) ** 2


# d定义梯度的计算方式，根据MSE的公式得到梯度的一般表达式
def gradient(x, y):
    return 2 * x * (x * w - y)


print('Predict (before training)', 4, forward(4))
epochs = []
costs = []

# 按照随机梯度下降算法进行训练
"""
这里的随机梯度，做了一些修改，每次随机取x_data中的一个样本计算梯度，损失值。
"""
for epoch in range(100):
    index = random.randint(0, 2)  # 随机抽取数据集中的样本
    x = x_data[index]
    y = y_data[index]
    grad = gradient(x, y)
    w -= 0.01 * grad
    print('\tgrad：', x, y, round(grad, 2))
    l = loss(x, y)  # 其实本质来说，只要能推导出梯度计算的一般公式，以及对权重进行更新,loss的计算只是为了输出而已
    epochs.append(epoch)
    costs.append(l)
    print('Epoch:', epoch, 'w=', round(w, 2), 'loss=', round(l, 2))
print('Predict (after training)', 4, round(forward(4), 2))

plt.plot(epochs, costs)
plt.ylabel('Cost')
plt.xlabel('Epoch')
plt.show()

```

**运行结果：**

<img alt="image-20250509172538777" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250509172538777.png"/>

## 4.反向传播

### 4.1 什么是神经网络？

如图：输入层5个节点，第一层6个节点，第二层7个节点
w相当于每层之前的权重矩阵，维度取决于左右两层的节点个数

<img alt="image-20250512163842664" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250512163842664.png"/>

### 4.2 什么是计算图？

<img alt="image-20250512164758713" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250512164758713.png"/>

### 4.3 以上两层神经网络出现的问题

不论几层神经网络，只要是每层神经网络都做线性运算，那么最终计算公式都会被化简为一个简单的线性模型。因此增加神经网络的层数就显得没有意义了。为了解决这个问题。我们在每一层计算后增加一个非线性函数的向量计算。

<img alt="image-20250512165617373" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250512165617373.png"/>

### 4.4 反向传播示例

<img alt="image-20250512172642006" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250512172642006.png"/>

### 4.5 线性模型y=w*x的反向传播

<img alt="image-20250512175533637" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250512175533637.png"/>

### 4.6 线性模型y=w*x的反向传播作业

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250513101723963.png" alt="image-20250513101723963" style="zoom:25%;" />

### 4.7 线性模型y=w*x+b的反向传播作业

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/3197cdcad45e6539045c36ef5fbc008.png" alt="3197cdcad45e6539045c36ef5fbc008" style="zoom:25%;" />

### 4.8 使用Pytorch进行线性模型y=w*x的训练

（有关Pytorch的一些tips）

> - 在定义forward函数、loss函数的过程实质上就是在定义和绘制一个计算图
> - 在计算loss的过程中，因为w是一个需要记录梯度的tensor，所以计算过程中梯度就保存下来了
> - 每一次运行backward实质上就是计算图的释放过程，所以下一次再计算loss的时候，实质上是重新绘制了一个计算图；之所以采用这种机制，是因为在神经网络训练过程中每一次的计算图并不一定是与前一次保持一致的
> - **权重tensor中含有这个权重的数据data以及损失函数关于这个权重的梯度grad**，但是梯度grad本身也是一个tensor类——tensor类直接进行数学运算，数学符号会进行重载，是在进行张量之间的运算，实质上是在构建计算图；要想直接对数据进行运算，则需要取出其中的data（tensor.data）来进行；因为对权重进行数值更新的这一过程，将来不需要利用这个过程计算梯度，因此也不需要保留相应的计算图；同理对标量的取出是用tensor.item，防止产生计算图；在训练的过程中如果把数值操作写成张量操作，则很有可能导致计算图模型不断累加，从而撑爆内存。
> - 虽然每一次后向传播可以取得更新的梯度值，并且会将计算图瓦解，但框架会一直保留张量中的梯度值，所以每次对权重更新完，进行下一次迭代之前都要先将梯度值清零

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：PyTorch_Deep_Learning_Practice（By_LiuEr） 
@File    ：gradient_descent(PyTorch).py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/5/13 上午10:41 
"""

import torch
import matplotlib as mpl

mpl.use('TkAgg')  # 设置 Matplotlib 使用 TkAgg 后端
import matplotlib.pyplot as plt

# 实现定义好数据集的x值和y值
x_data = [1.0, 2.0, 3.0]
y_data = [2.0, 4.0, 6.0]

# 定义好初始权重,使用张量;为了使用计算图的反向传播，需要记录梯度值
w = torch.Tensor([1.0])  # 此时将权重定义为一个Tensor，初始值设为1.0
w.requires_grad = True  # 需要计算梯度


# 定义前向计算过程，实质上是构建计算图，使用张量运算
def forward(x):
    return x * w


# 定义损失函数的计算方式，构建计算图，为后续BP做准备，也是针对某一个数据点而言的
def loss(x, y):
    y_pred = forward(x)
    return (y - y_pred) ** 2


print('Predict (before training)', 4, forward(4).item())
epochs = []
costs = []
# 使用计算图模型，按照BP算法计算梯度并更新权值
for epoch in range(100):
    epochs.append(epoch)
    for x, y in zip(x_data, y_data):
        l = loss(x, y)
        l.backward()  # 自动反向传播，计算梯度
        print('\tgrad：', x, y, round(w.grad.item(), 2))
        """
        注意这里我们不直接选取w和w.grad来进行计算，前两者都属于张量，如果直接拿来计算，会产生计算图
        在此为了避免计算图，直接取常量值
        """
        w.data -= 0.01 * w.grad.item()  # w.data 里面存放w的值，w.grad.item()存放梯度的值。

        w.grad.data.zero_()  # 每一次对权重更新完后都需要将梯度清零
    costs.append(l.item())  # 每次选取最后一次loss的值保留
    print('progress:', epoch, l.item())

print('Predict (after training)', 4, round(forward(4).item(), 2))

plt.plot(epochs, costs)
plt.ylabel('Cost')
plt.xlabel('Epoch')
plt.show()
```
**训练图：**

<img alt="image-20250513114105675" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250513114105675.png"/>


> 反向传播就是网络加深版的梯度下降，其过程：
> ①先计算整个模型的损失（前向传播）
> ②反向传播，得到梯度值
> ③对权重进行更新
> pytorch框架相当于把梯度计算函数（对loss函数进行求导，以及多层嵌套函数的求导链式法则）给提前封装好进行调用了
>
> p.s. 因此在实际应用中使用梯BP算法，我们只需要根据实际问题定义好损失函数即可
> 
>

### 4.6 课后作业：PyTorch非线性梯度下降实现

——使用PyTorch训练二次多项式：y=w1x2+w2x+b

- 题目

  <img alt="image-20250513112139627" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250513112139627.png"/>

- 计算图模型

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/66eb2cd23df06b33eb12f1f3a3e11cd.png"/>



- python程序（10000轮训练）

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：PyTorch_Deep_Learning_Practice（By_LiuEr） 
@File    ：Nonlinear_gradient_descent(PyTorch).py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/5/13 上午11:18 
"""

import torch
import matplotlib as mpl

mpl.use('TkAgg')  # 设置 Matplotlib 使用 TkAgg 后端
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

# 实现定义好数据集的x值和y值
x_data = [1.0, 2.0, 3.0]
y_data = [2.0, 4.0, 6.0]

# 定义好初始权重,使用张量;为了使用计算图的反向传播，需要记录梯度值
w_1 = torch.Tensor([1.0])
w_2 = torch.Tensor([1.0])
b = torch.Tensor([1.0])
w_1.requires_grad = True
w_2.requires_grad = True
b.requires_grad = True


# 定义前向计算过程，实质上是构建计算图，使用张量运算
def forward(x):
    return x ** 2 * w_1 + x * w_2 + b


# 定义损失函数的计算方式，构建计算图，为后续BP做准备，也是针对某一个数据点而言的
def loss(x, y):
    y_pred = forward(x)
    return (y - y_pred) ** 2


print('Predict (before training)', 4, round(forward(4).item(), 2))
epochs = []
costs = []
# 使用计算图模型，按照BP算法计算梯度并更新权值
for epoch in range(10000):
    epochs.append(epoch)
    for x, y in zip(x_data, y_data):
        l = loss(x, y)
        l.backward()
        print('\tgrad：', x, y, round(w_1.grad.item(), 2), round(w_2.grad.item(), 2), round(b.grad.item(), 2))
        w_1.data -= 0.01 * w_1.grad.item()
        w_2.data -= 0.01 * w_2.grad.item()
        b.data -= 0.01 * b.grad.item()

        w_1.grad.data.zero_()  # 每一次对权重更新完后都需要将梯度清零
        w_2.grad.data.zero_()
        b.grad.data.zero_()
    costs.append(l.item())
    print('progress:', epoch, l.item())

print('Predict (after training)', 4, round(forward(4).item(), 2))
print(rf'训练出的解析式：y = {w_1.item()}x^2 + {w_2.item()}x + {b.item()}')

plt.plot(epochs, costs)
plt.ylabel('Cost')
plt.xlabel('Epoch')
plt.show()

```

**训练图：**

<img alt="image-20250513114215544" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250513114215544.png"/>

**训练出的解析式：y = 0.0008878321386873722x^2 + 1.9960287809371948x + 0.003689199686050415**

## 5.用PyTorch实现线性回归 ##

### 5.1 PyTorch线性回归流程与代码实现 

>  *  使用PyTorch构建训练模型的一般步骤： 
>     ①准备数据集 
>     ②设计模型：使用**相关类**构建模型，用以计算预测值 
>     ③使用PyTorch的应用接口来构建损失函数和优化器 
>     ④编写循环迭代的训练过程——前向计算，反向传播和梯度更新，训练过程如下——
>      *  **根据前向计算计算y**
>      *  **使用损失函数类计算损失函数值**
>      *  **注意梯度清零**
>      *  **进行反向传播**
>      *  **权重更新**

>  *  任务重点发生变化： 
>     ①以前手动写训练过程代码时，关键在于计算损失函数的梯度值的一般表达式  
>     ②现在使用pytorch包构建模型时，**关键在于构建计算图**

>  *  数据维度处理 
>     ①输入和输出都需要构建成矩阵形式； 
>     ②在神经网络模型的构建中，最重要的结构就是线性单元，其中要想确定权重w和偏置项b的维度，首先需要明确输入值x和输出值y_hat的输出维度； 
>     ③根据问题的不同形式，**x和y的形状各异，这也导致了最终得到的损失函数可能是标量，向量或矩阵；**但不管loss是什么形式，**最后进行反向传播前，都要计算出loss的一个标量值。** 
>     p.s. 比如对loss向量的每一个单元求和，如果不取标量出来，就会对计算图进行累加操作，从而使得内存被撑爆。

```Python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：PyTorch_Deep_Learning_Practice（By_LiuEr） 
@File    ：linear_regression_model(PyTorch).py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/5/27 下午3:20 
"""

import torch
import matplotlib as mpl

mpl.use('TkAgg')  # 设置 Matplotlib 使用 TkAgg 后端
import matplotlib.pyplot as plt

# 实现定义好数据集的x值和y值
x_data = torch.tensor([[1.0], [2.0], [3.0]])  # 此时的x_data和y_data 都是一个3*1的矩阵
y_data = torch.tensor([[2.0], [4.0], [6.0]])


# 构建线性回归训练模型
class LinearModel(torch.nn.Module):
    # 以下两个函数是必须要实现的，最少要实现的。
    def __init__(self):
        super(LinearModel, self).__init__()  # 继承父类，torch.nn.Module里面有很多模型，以后基本所有的模型都继承与这个类
        self.linear = torch.nn.Linear(1, 1)  # 定义一个线性计算单元，构造一个Linear类(继承自Module类)对象，它包含权重ω和偏置b

    # 这里的forward实际上就是调用了Linear类(继承自Module类)本身实现在__call__方法中的forward
    def forward(self, x):
        """
        注意这里的self.linear是一个对象，对象后面带括号并且里面有参数在Python中是一个奇怪的现象
        一般只有函数或者方法才会后面加括号带参数。这里为什么合法。
        因为self.linear所代表的Linear类(继承自Module类)，类内实现了__call__方法
        python中的__call__方法具体见5.3讲解
        在Linear类中，会定义一个__call__方法，里面会有对forward的调用
        """
        y_pred = self.linear(x)  # 可调用对象，进行了相应的forward计算
        return y_pred


if __name__ == '__main__':

    model = LinearModel()  # 对实现的模型类进行实例化，同样也是可调用的

    # 构建损失函数的计算和优化器
    """
    使用MSELoss构建损失函数，torch.nn.MSELoss是一个继承自`nn.Module`模块的类
    MSELoss的具体实现公式和源码解析 见5.2 PyTorch相关知识中的第3条
    """
    criterion = torch.nn.MSELoss(size_average=False)
    # 这里使用SGD优化器，第一个参数传入模型的权重参数，第二个参数是学习率
    optimizer = torch.optim.SGD(model.parameters(), lr=0.01)

    epochs = []
    costs = []
    # 训练过程
    for epoch in range(300):
        epochs.append(epoch)
        y_pred = model(x_data)  # 前馈算y_pred
        loss = criterion(y_pred, y_data)  # 计算损失值
        costs.append(loss.item())  # 使用item()传入标量
        print(epoch, loss.item())  # 因为这里loss是个对象，print在调用的时候会自动进行__str__()的解析，不会产生计算图

        optimizer.zero_grad()  # 一定要注意梯度归零
        loss.backward()  # 进行反向传播
        optimizer.step()  # 进行梯度的更新，对参数列表中包含的参数使用预先设置的学习率进行更新

    # 对训练结果进行输出
    print('w = ', model.linear.weight.item())
    print('b = ', model.linear.bias.item())  # 使用.item()因为w和b都是张量对象，这里转化为标量

    # 进行模型测试
    x_test = torch.Tensor([[4.0]])
    y_test = model(x_test)
    print('x_test = ', x_test.item())
    print('y_pred = ', y_test.data)

    # 训练过程可视化
    plt.plot(epochs, costs)
    plt.ylabel('Cost')
    plt.xlabel('Epoch')
    plt.show()

```

**运行结果（300轮）：**

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250527153436865.png" alt="image-20250527153436865"  /><img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250527152735371.png" alt="image-20250527152735371" style="zoom: 80%;" />

### 5.2 PyTorch相关知识 

①`nn.Linear`类中对`__call()__`方法进行了实现，且其内部有对函数`forward()`的调用，故在定义模型时需要对`forward()`函数进行实现。  

②`nn.Linear`类相当于是对线性计算单元的封装，里面包含两个张量类型的成员：权重和偏置项
<img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/cb324861f423c90299ada34ad4f58252.png"/>

③根据下图所示按照MSE规则计算损失函数的过程，它也是一个计算图，是继承自`nn.Module`模块的，我们所调用的`nn.MSELoss`就是继承自该模块的类。
<img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/7dd651626a9012c8a065981e15823eb3.png"/>

**源码详解：**

**size_average 参数** 代表着最后求出的损失值是否要求均值，设为True是要求均值，反而不求(事实上，加不加均值影响不大)

**reduce 参数** 代表最终是否要求和，默认求和，一般我们也默认为True，因为求和之后是标量，更好计算。

公式：
$$
\mathrm{MSE}=\frac{1}{\mathrm{N}}\sum_{\mathrm{i}=1}^\mathrm{n}(\mathrm{x_i}-\mathrm{y_i})^2
$$
④代码中模型与类之间的关系：
<img alt="3b33724582f3c7020e7328b3026abca2" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/3b33724582f3c7020e7328b3026abca2.png"/>

### 5.3 Python类中的\_\_call\_\_()方法

\__call__()是一种magic method，在类中实现这一方法可以使该类的实例（对象）像函数一样被调用。默认情况下该方法在类中是没有被实现的。

\_\_call\_\_()方法的作用其实是把一个类的实例化对象变成了可调用对象，也就是说把一个类的实例化对象变成了可调用对象，只要类里实现了\_\_call\_\_()方法就行。如当类里没有实现\__call__()时，此时的对象p 只是个类的实例，不是一个可调用的对象，当调用它时会报错：‘Person’ object is not callable.

**先看一个简单的案例**

```python
class People(object):
    def __init__(self, name):
        self.name = name

    def __call__(self, my_name):
        print("my name is " + my_name)
        print("hello " + self.name)


a = People('派大星！')
a.__call__('墨子杰')  # 调用方法一
a('墨子杰')  # 调用方法二
```

**结果：**

```python
my name is 墨子杰
hello 派大星！
my name is 墨子杰
hello 派大星！
```



### 5.4 课后作业：测试不同优化器的效果

> 比较不同优化器下的线性回归并可视化  
> p.s.因为LBFGS的setp()调用方法具有特殊性，以下并未对LBFGS进行实现  
> <img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/ada9f9e322964875a9beeea4049a42b8.png"/>

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：PyTorch_Deep_Learning_Practice（By_LiuEr） 
@File    ：test_different_optimizers.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/5/27 下午3:47 
"""

import torch
import matplotlib as mpl

mpl.use('TkAgg')  # 设置 Matplotlib 使用 TkAgg 后端
import matplotlib.pyplot as plt

# 实现定义好数据集的x值和y值
x_data = torch.tensor([[1.0], [2.0], [3.0]])
y_data = torch.tensor([[2.0], [4.0], [6.0]])


# 构建训练模型
class LinearModel(torch.nn.Module):
    def __init__(self):
        super(LinearModel, self).__init__()  # 继承父类
        self.linear = torch.nn.Linear(1, 1)  # 定义一个线性计算单元，继承Module块，也可以自主实现反向传播

    def forward(self, x):
        y_pred = self.linear(x)  # 可调用对象，进行了相应的forward计算
        return y_pred


model1 = LinearModel()  # 定义的模型结构初始化
model2 = LinearModel()
model3 = LinearModel()
model4 = LinearModel()
model5 = LinearModel()
model7 = LinearModel()
model8 = LinearModel()
models = [model1, model2, model3, model4, model5, model7, model8]

# 构建损失函数的计算和优化器
criterion = torch.nn.MSELoss(size_average=False)

op1 = torch.optim.SGD(model1.parameters(), lr=0.01)  # 以下分别尝试不同的优化器
op2 = torch.optim.Adagrad(model2.parameters(), lr=0.01)
op3 = torch.optim.Adam(model3.parameters(), lr=0.01)
op4 = torch.optim.Adamax(model4.parameters(), lr=0.01)
op5 = torch.optim.ASGD(model5.parameters(), lr=0.01)
op7 = torch.optim.RMSprop(model7.parameters(), lr=0.01)
op8 = torch.optim.Rprop(model8.parameters(), lr=0.01)
ops = [op1, op2, op3, op4, op5, op7, op8]

titles = ['SGD', 'Adagrad', 'Adam', 'Adamax', 'ASGD', 'RNSprop', 'Rprop']

fig, ax = plt.subplots(2, 4, figsize=(20, 10), dpi=100)
# 训练过程，对每个优化器都进行尝试
index = 0
for item in zip(ops, models):  # 分别尝试每个优化器
    epochs = []
    costs = []

    model = item[1]
    op = item[0]

    for epoch in range(100):
        epochs.append(epoch)
        y_pred = model(x_data)
        loss = criterion(y_pred, y_data)
        costs.append(loss.item())
        print(epoch, loss)

        op.zero_grad()
        loss.backward()
        op.step()

    # 对训练结果进行输出
    print('w = ', model.linear.weight.item())
    print('b = ', model.linear.bias.item())

    # 进行模型测试
    x_test = torch.Tensor([[4.0]])
    y_test = model(x_test)
    print('y_pred = ', y_test.data)

    # 训练过程可视化
    a = int(index / 4)
    b = int(index % 4)
    ax[a][b].set_ylabel('Cost')
    ax[a][b].set_xlabel('Epoch')
    ax[a][b].set_title(titles[index])
    ax[a][b].plot(epochs, costs)
    index += 1
plt.show()

```

## 6.逻辑斯蒂回归

### 6.1 背景与概念

 * 基于分类问题中**属性是类别性的**，所以不能采取基于序数的线性回归模型，而提出了新的分类模型——**逻辑斯蒂回归模型**，输出每个样本在**各个预测值上的概率值**。

 * 为了将最终的输出值控制在概率值P∈\[0,1\]的合理范围中，需要在线性单元输出后再加上一个**非线性映射**，这里我们使用**饱和函数sigmoid**

 * 逻辑斯蒂回归方程

   <img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250527203049511.png" alt="image-20250527203049511" style="zoom:50%;" />

   当x属于全体实数时，y的值域为[0,1]。

   <img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250527203449262.png" style="zoom:50%;" />

   对应的sigmoid曲线如下

   <img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250527203405991.png" alt="image-20250527203405991" style="zoom: 80%;" />

 * 由于sigmoid函数的出现，在解决分类问题的过程中就可以通过**线性模型计算出来的值，叠加一个逻辑斯蒂模型，**就能将y_hat变成我们想要的[0,1]之间。逻辑斯蒂回归模型如下

   <img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250527204203019.png" alt="image-20250527204203019" style="zoom:67%;" />

 * 因为模型发生了变化，**损失值的计算方式也要发生相应的变化**；因为线性回归模型只需要**比较两个实数之间的差异**，所以可以使用**均方误差值衡量**；但是逻辑回归得到的结果是在描述事件的概率分布，因此我们的**损失函数也变成比较两个分布之间的差异，这里我们使用交叉熵计算公式。**

   **二分类的逻辑斯蒂回归**的**损失函数**如下

   <img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250527205103895.png" alt="image-20250527205103895" style="zoom: 67%;" />

   我们希望loss越小越好

   当y=1时，loss = -log(y_hat) , 此时我们希望loss越小越好，那么y_hat就要越大越好，但是y_hat的值不会超过1，因此趋近于1

   当y=0时，loss = -log(1-y_hat)，此时我们希望loss越小越好，那么y_hat就要越小越好，但是y_hat的值不会小于0，因此趋近于0

   我们发现**这个公式可以让y_hat和y的真实值重合，这就是loss函数要达到的目的**

 * **小批量的二分类逻辑斯蒂方程**

   <img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250527205739170.png" alt="image-20250527205739170" style="zoom: 50%;" />

 *  

### 6.2 PyTorch常用分类数据集介绍

1. MNIST数据集

   **数据集介绍**

   手写数字数据集，包含60000张训练样本和10000张测试样本，类别为10，分别为0-9 的数字

   **数据集的下载与导入**

   第一次运行代码时，会自动下载数据集，保存在root路径下，下一次运行时就不会下载了，即使download=True

   ```python
   import torchvision
   
   train_set = torchvision.datasets.MNIST(root='../dataset/mnist', train=True, download=True)
   test_set = torchvision.datasets.MNIST(root='../dataset/mnist', train=False, download=True)
   ```

2. CIFAR-10数据集

   **数据集介绍**

   动物图片数据集，包含50000张训练样本和10000张测试样本，类别为10，分别为10种动物的图像

   **数据集的下载与导入**

   ```python
   import torchvision
   
   train_set = torchvision.datasets.CIFAR10(root='../dataset/cifar10', train=True, download=True)
   test_set = torchvision.datasets.CIFAR10(root='../dataset/cifar10', train=False, download=True)
   ```

### 6.2 逻辑斯蒂回归-二分类代码实现

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
'''
@Project ：py-torch_-deep_-learning_-practice 
@File    ：binary_logistic_regression_model(PyTorch).py.py
@IDE     ：PyCharm 
@Author  ：mozijie
@Date    ：2025/5/27 下午9:08 
'''
import numpy as np
import torch
import torch.nn.functional as F
import matplotlib as mpl

mpl.use('TkAgg')  # 设置 Matplotlib 使用 TkAgg 后端
import matplotlib.pyplot as plt

# 二分类问题的输出值往往是0或1
x_data = torch.tensor([[1.0], [2.0], [3.0]])
y_data = torch.tensor([[0.0], [0.0], [1.0]])


# 构建二分类逻辑斯蒂回归训练模型
class LogisticRegressionModel(torch.nn.Module):
    def __init__(self):
        super(LogisticRegressionModel, self).__init__()  # 这一块和线性回归的模型差距不大，因为sigmoid函数是无参数的，不需进行初始化
        self.linear = torch.nn.Linear(1, 1)

    def forward(self, x):
        y_pred = F.sigmoid(self.linear(x))  # 比线性回归模型就多了一步sigmoid非线性映射
        return y_pred


model = LogisticRegressionModel()

# 构建损失函数的计算和优化器
"""
这里的损失函数，我们不使用线性模型里面的MSE
而使用BCE，也叫二分类交叉熵损失函数，6.1里面有讲
"""
criterion = torch.nn.BCELoss(size_average=False)  # 逻辑回归模型适用二分类交叉熵损失函数
# 这里的优化器没变，我们还是使用SGD
op = torch.optim.SGD(model.parameters(), lr=0.01)

epochs = []
costs = []

# 训练过程（和线性回归模型的训练过程一样）
for epoch in range(1000):
    epochs.append(epoch)
    y_pred = model(x_data)
    loss = criterion(y_pred, y_data)
    costs.append(loss.item())
    print(epoch, loss.item())

    op.zero_grad()  # 梯度归零
    loss.backward()  # 反向传播
    op.step()  # 梯度更新

# 对训练结果进行输出（事实上这里面的w和b已经没有意义了）
print('w = ', model.linear.weight.item())
print('b = ', model.linear.bias.item())

# 进行模型测试
X = np.linspace(0, 10, 200)  # 定义测试数据，0-10之间，取200个点
x_test = torch.Tensor(X).view(200, 1)  # 转换为200行1列的张量
y_test = model(x_test)  # 预测输出
Y = y_test.data.numpy()  # 转换为numpy数组
plt.ylabel('Probability of Pass')  # 定义Y轴名称
plt.xlabel('Hours')  # 定义X轴名称
plt.plot(X, Y)
plt.grid()
plt.show()

# 训练过程可视化
plt.ylabel('Cost')
plt.xlabel('Epoch')
plt.plot(epochs, costs)
plt.show()

```

## 7.多维特征的输入 

### 7.1 输入特征的变化

所谓输入特征的维度变化，影响的只是线性单元中控制**输入维度**的那一个参数 ；  

在之前讲的例子中，每一个线性单元所做的工作即对**一个实数输入x**，乘上一个权重之后再加上一个偏置项，（之后再进行非线性映射），在这种情况下——**输入的数据维度是1**，线性单元的**权重维度也是1x1大小的**；  

在实际的问题中，一个数据项含有**多种特征属性**且都与该问题相关联，此时我们输入的**不再是一个实数，而是一个向量**，同样**权重单元也不再是1x1的，而也应该是一个相应维度的向量；**输入与权重的运算就转换成**向量之间的点乘运算**，数与数之间的四则运算也同理转换成**向量/矩阵的四则运算**。 
<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/6326d4fdb60dfa118a29f963a7bbbb72.png" alt="在这里插入图片描述" style="zoom:150%;" />

### 7.2 神经网络的本质：找到合适的空间变换

①从前面的讨论中，我们不难发现，如果将一个**M维的数据传入**线性计算单元，得到**N维的输出**，那么实际上是在做一个**矩阵线性变换**——从M维空间映射到N维空间。  

②神经网络的学习面向的大多是现实社会中的问题，大多是要找到**不同维度空间的非线性映射**，我们采用将一层线性映射分解成多层线性映射（这不会改变线性映射的性质）并**在每一层映射后加入非线性变换**，以拟合现实生活中可能出现的各种复杂的变化情况。 

③ 多层神经网络事实上，在**每一层都是一次降维的过程**，如下图所示。他不会一下从8为降到1维，而是**分层逐级降维**，从8维到6维到2维到1维，中间经过了**三层神经网络**，每层的神经网络一般使用一种**非线性的复杂函数**来进行计算。

<img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/93264c6907561d0694ceb7b10f8f8f3e.png"/>  
对于程序编写来说，多维特征的数据输入，只在**数据集构造**和**模型构造**的代码上会有些许的改动。

```python
#多维输入的模型构造
class Model(torch.nn.Module):
    def __init__(self):
        super(Model,self).__init__()
        self.linear = torch.nn.Linear(8,1)  #明确了输入数据是8维的，输出数据是1维的，从而定义了权重矩阵的维度
        self.sigmoid = torch.nn.Sigmoid()  #增加非线性映射

    def forward(self,x):
        y_pred = self.sigmoid(self.linear(x))
        return y_pred

model = Model()
```

### 7.3 项目：多层神经网络实现糖尿病病情预测

【需求描述】
<img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/138649f3cbdbdd4259fe1b6c0b3ad567.png"/>

【功能实现】  

①在准备数据集上，注意把导入的**数据集的X值和Y标签进行区分**；  

②在构建模型上，这里使用的`Sigmoid`是直接继承自`nn.Module`模块下的一个模块；作为一个**非线性映射层**，是可以在后面构建计算图的时候进行复用的；  

③虽然数据维度和网络结构发生了变化，但因为最后的输出仍然是一维的概率值，所以损失函数和优化器和之前的单层逻辑回归相比，并未发生改变。

④注释里面使用了ReLU激活函数，测试了一下效果并不好。

```java
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：PyTorch_Deep_Learning_Practice（By_LiuEr） 
@File    ：prediction_of_diabetes.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/5/28 上午10:41 
"""
"""
基于PyTorch多层神经网络实现糖尿病病情预测
"""
import numpy as np
import torch
import matplotlib as mpl

mpl.use('TkAgg')  # 设置 Matplotlib 使用 TkAgg 后端
import matplotlib.pyplot as plt

# 准备数据
xy = np.loadtxt(rf'diabetes.csv', delimiter=',', dtype=np.float32)
x_data = torch.from_numpy(xy[:, :-1])
y_data = torch.from_numpy(xy[:, [-1]])


# 自定义多层神经网络模型
class Model(torch.nn.Module):
    def __init__(self):
        super(Model, self).__init__()
        self.linear1 = torch.nn.Linear(8, 6)
        self.linear2 = torch.nn.Linear(6, 4)
        self.linear3 = torch.nn.Linear(4, 1)
        self.sigmoid = torch.nn.Sigmoid()  # 增加非线性映射
        # self.relu = torch.nn.ReLU()

    def forward(self, x):
        # 在多层的神经网络中书写前向计算的逻辑时，只用一个变量x串联整个输入输出
        x = self.sigmoid(self.linear1(x))
        x = self.sigmoid(self.linear2(x))
        x = self.sigmoid(self.linear3(x))
        return x

    # def forward(self, x):
    #     # 在多层的神经网络中书写前向计算的逻辑时，只用一个变量x串联整个输入输出
    #     x = self.relu(self.linear1(x))
    #     x = self.relu(self.linear2(x))
    #     x = self.relu(self.linear3(x))
    #     return x


model = Model()

# 构建损失函数的计算和优化器
criterion = torch.nn.BCELoss(size_average=True)  # 逻辑回归模型适用二分类交叉熵损失函数
op = torch.optim.SGD(model.parameters(), lr=0.01)  # 定义优化器

epochs = []
costs = []
# 训练过程
for epoch in range(1000):
    epochs.append(epoch)
    # 前向计算
    y_pred = model(x_data)
    loss = criterion(y_pred, y_data)  # 计算损失值
    costs.append(loss.item())
    print(epoch, loss.item())

    op.zero_grad()  # 梯度归零
    loss.backward()  # 反向传播

    # 权重更新
    op.step()

# 训练过程可视化
plt.ylabel('Cost')
plt.xlabel('Epoch')
plt.plot(epochs, costs)
plt.show()

```

**训练结果：**

<img alt="image-20250529145155921" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250529145155921.png"/>

## 8.数据集加载 

### 8.1 Mini-Batch形式

在神经网络训练过程中采用的工具类，诸如`Dataset` 和 `DataLoader`

 * `Dataset`主要用于构造数据集，该数据集应该能够支持索引结构；

 * `DataLoader`主要用于加载数据集，支持训练时的`Mini-Batch`形式。  

   讲述上面的工具类，主要原因是在训练过程的**Mini-Batch形式**，该形式是**介于Batch梯度下降和随机梯度下降之间的一种数据集加载方式**，可以较好地兼顾到**训练的随机性和时间的效率性**。

1.  在Mini-Batch模式下，训练过程要写成嵌套循环的形式

```python
for epoch in range(training_epochs):
    #对每一个mini-batch进行遍历
    for i in range(total_batch):	# 每次循环执行一次Mini-Batch
        # do something
```

 *  外层循环控制迭代的次数
 *  内层循环控制mini-bacth的索引

2. 概念辨析

①Epoch

所有的数据样本都参与了一次训练，就称为是一次Epoch

②Batch-size

每次进行一轮训练时所投入的训练样本的规模

③Iteration

通俗来说，就是上面嵌套循环中内层循环执行的次数，iteration=训练样本总数/batch-size



### 8.2 DataLoader工作机制

①首先DataLoader类需要传入一个支持索引操作的数据集兑现Dataset 

②对原始数据集中的各个数据项进行**随机打乱操作（Shuffle）** 

③使用**Loader工具**，按照给定的Batch-Size大小将原始的数据集**划分成以Batch-Size为基本大小的**，**Batch为基本单位的数据单元**

<img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/b13fac19372eb212af54996bb016ae24.png"/>



### 8.3 Dataset构造

①在torch.utils.data中有**Dataset抽象类**，我们需要根据自己的需要**继承这个抽象类**，来实现自己的数据集类；  
②构造的数据集类中需要实现一些魔术方法：

 *  `init`:对数据集类进行**初始化**
 *  `getitem`:对数据集进行**索引**操作
 *  `len`:对数据集中的**数据项个数**进行返回

③数据集的数据加载方式（`init`和`getitem`的实现）

 *  若数据集本身**规模不是很大**，可以直接将原**数据集全部加载到内存中**，则`getitem`方法只需要**从内存中顺序读出即可**；
 *  若数据集本身**规模较大**，则可以**将数据在磁盘中按照文件夹分装**，将输入出的各个**文件夹名以列表的形式载入内存中**；则`getitem`的方法就是**按照文件夹路径从磁盘中索引并读出文件**。

```python
import torch
from torch.utils.data import Dataset
from torch.utils.data import DataLoader

# 准备数据,自定义数据集类，用以继承Dataset抽象类
class DiabetesDataset(Dataset):
    def __init__(self):
        pass
    def __getitem__(self, item):
        #该方法的实现是为了实例化后的数据集对象支持下标操作
        #dataset[index]
        pass
    def __len__(self):
        #该方法的实现是为了方便我们输出数据集每一个batch的数据项条目数
        pass

dataset = DiabetesDataset()#实例化这个数据集类
train_loader = DataLoader(dataset=dataset,batch_size=32,shuffle=True,num_workers=2)
'''
dataset:数据集对象
batch_size:批量训练的大小
shuffle:是否要进行随机打乱
num_workers:使用多少个线程对数据进行载入操作；使用并行化可以提高数据读取的效率
'''
```

### 8.4 用Dataset和DataLoader进行糖尿病分类

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：PyTorch_Deep_Learning_Practice（By_LiuEr） 
@File    ：classific_of diabetes_with_dataset_dataLoader.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/5/29 下午2:27 
"""
'''
使用带Dataset, DataLoader的基于PyTorch多层神经网络实现糖尿病病情预测和分类
'''
import numpy as np
import torch
from torch.utils.data import Dataset, DataLoader
import matplotlib as mpl

mpl.use('TkAgg')  # 设置 Matplotlib 使用 TkAgg 后端
import matplotlib.pyplot as plt


# 准备数据,自定义数据集类，用以继承Dataset抽象类
class DiabetesDataset(Dataset):
    def __init__(self, filepath):
        xy = np.loadtxt(filepath, delimiter=',', dtype=np.float32)
        self.x_data = torch.from_numpy(xy[:, :-1])
        self.y_data = torch.from_numpy(xy[:, [-1]])
        self.len = xy.shape[0]

    def __getitem__(self, index):
        # 该方法的实现是为了实例化后的数据集对象支持下标操作
        return self.x_data[index], self.y_data[index]

    def __len__(self):
        # 该方法的实现是为了方便我们输出数据集每一个batch的数据项条目数
        return self.len


# 自定义多层模型
class Model(torch.nn.Module):
    def __init__(self):
        super(Model, self).__init__()
        self.linear1 = torch.nn.Linear(8, 6)
        self.linear2 = torch.nn.Linear(6, 4)
        self.linear3 = torch.nn.Linear(4, 1)
        self.sigmoid = torch.nn.Sigmoid()  # 增加非线性映射

    def forward(self, x):
        # 在多隐层的网络中书写前向计算的逻辑时，只用一个变量x串联整个输入输出
        # 这是一种习惯
        x = self.sigmoid(self.linear1(x))
        x = self.sigmoid(self.linear2(x))
        x = self.sigmoid(self.linear3(x))
        return x


if __name__ == '__main__':

    dataset = DiabetesDataset(rf'../Lecture_07_Multiple_Dimension_Input/diabetes.csv')  # 实例化这个数据集类
    train_loader = DataLoader(dataset=dataset, batch_size=32, shuffle=True, num_workers=2)
    '''
    dataset:数据集对象
    batch_size:批量训练的大小
    shuffle:是否要进行随机打乱
    num_workers:使用多少个线程对数据进行载入操作；使用并行化可以提高数据读取的效率
    '''
    model = Model()

    # 构建损失函数的计算和优化器
    criterion = torch.nn.BCELoss(size_average=True)  # 逻辑回归模型适用二分类交叉熵损失函数
    op = torch.optim.SGD(model.parameters(), lr=0.01)

    epochs = []
    costs = []
    # 训练过程，包括前向计算和反向传播
    for epoch in range(100):
        epochs.append(epoch)
        loss_sum = 0.0
        for i, data in enumerate(train_loader, 0):
            # 注意这里的data，里面一次随机存放了32行数据，因为我们的batch_size设置为32
            # 所以我们的第二层循环每次会循环  csv数据总行数/32 次
            inputs, labels = data
            # 从train_loader中取出数据data
            # 从数据data中提取属性x和标签y
            # 并且自动转换成tensor张量传给inputs和data两个变量

            y_pred = model(inputs)
            loss = criterion(y_pred, labels)  # 计算损失值
            loss_sum += loss.item()  # 损失值累加
            print(epoch, i, loss.item())

            op.zero_grad()  # 梯度归零
            loss.backward()  # 反向传播

            op.step()  # 权重更新
        costs.append(loss_sum / (i + 1))  # 当前轮数的平均损失值

    # 训练过程可视化
    plt.ylabel('Cost')
    plt.xlabel('Epoch')
    plt.plot(epochs, costs)
    plt.show()

```

**训练结果：**

<img alt="image-20250529145047897" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250529145047897.png"/>

### 8.5 MINST数据集的导入

在torchvision的datasets库中存放了一些常用的数据集

具体如下：

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250529150050822.png" alt="image-20250529150050822" style="zoom: 33%;" />

其中最常见的数据集为：MINST

PyTorch导入MINST数据集代码如下

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：PyTorch_Deep_Learning_Practice（By_LiuEr） 
@File    ：import_MINST_dataset.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/5/29 下午2:54 
"""
import torch
from torch.utils.data import DataLoader
from torchvision import transforms
from torchvision import datasets

train_dataset = datasets.MNIST(root='../dataset/mnist', train=True, transform=transforms.ToTensor(), download=True)
test_dataset = datasets.MNIST(root='../dataset/mnist', train=False, transform=transforms.ToTensor(), download=True)
train_loader = DataLoader(dataset=train_dataset, batch_size=32, shuffle=True)
test_loader = DataLoader(dataset=test_dataset, batch_size=32, shuffle=False)
for epoch in range(100):
    for batch_idx, (inputs, target) in enumerate(train_loader):
        pass

```

### 8.6 作业：kaggle竞赛titanic

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250529150458033.png" alt="image-20250529150458033" style="zoom: 50%;" />

**暂时不做，学完再做**

## 9.多分类问题 

### 9.1 多分类的输出处理

 *  有m种可能的输出标签，就将最后一层设计成有m个输出，每个输出都表示该数据是其中某一类标签的可能性；
 *  为了方便比较，输出的值应该满足某种分布的性质——每个输出值为正；输出值的总和为1；

> 也就是说，在分类问题中，必须满足输出的每一个值都代表着数据的分布； 
> p.s.在二分类问题中，经过sigmoid函数后输出一个值，其实就默认满足了分布的性质（p∈(0,1)）

### 9.2 Softmax层的计算

经过上面的分析，我们在多分类问题中得到的输出前加上一个softmax层，就可以满足上方关于输出值的限定。

如下图，为什么使用指数的运算，是因为**指数的值永远是大于0的**

这里的Zi是前面线性运算+SIGMOD后的结果，最后再加一个softmax层，softmax层公式如下

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250529170512725.png" alt="image-20250529170512725" style="zoom: 50%;" />

**softmax公式计算过程：**

<img alt="image-20250529170457460" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250529170457460.png"/>
①：将每一个输出值Zi都采用**指数幂的运算**，保证结果都是**正数**； 
②：分母是每个指数幂结果的求和，保证最后输出的K个结果的总和为1（本质就是**归一化操作**)**。**

**损失函数计算：**

 *  主要思想还是参照二分类中的交叉熵损失函数；但要注意的是**交叉熵的计算公式loss = -(ylogy_hat+(1-y)log(1-y_hat))**中虽然是有两项，但实际计算中**有一项的值恒为零**；
 *  参照交叉熵的思想，我们对多分类的标签进行**独热编码（one-hot**），即目标标签值形如\[0,0,…0,0,1,0,…,0\]这样的**稀疏向量**；因此**损失函数实际为loss = -ylogy_hat** 
    <img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250529171202458.png" alt="image-20250529171202458" style="zoom:50%;" />

```python
# numpy实现多分类交叉熵损失函数
y = np.array([1,0,0]) #标签值，说明该数据是第1类
z = np.array([0.2,0.1,-0.1]) #神经网络最后一层的输出值
y_pred = np.exp(z) / np.exp(z).sum() #Softmax层的输出值
loss = (-y * np.log(y_pred)).sum()	# 损失函数

# pytorch框架中对多分类损失函数的调用
# PyTorch里面继承了多分类交叉熵损失函数torch.nn.CrossEntropyLoss()
# 它封装了上述一系列复杂的非线性计算，方便我们编写代码
y = torch.LongTensor([0]) 	# y必须是一个长整形的张量，这里选择第0个分类
z = torch.Tensor([0.2,0.1,-0.1])
criterion = torch.nn.CrossEntropyLoss()		# 构造损失函数
loss = criterion(z,y)
 #只需要声明一个损失函数计算对象
 #传入标签值和线性单元输出值即可
```

> Exercise9-1：CrossEntropyLoss v.s. NLLLoss  
> 对两类损失函数的完整计算过程的理解可以参考这篇博客[《Pytorch详解NLLLoss和CrossEntropyLoss》][Pytorch_NLLLoss_CrossEntropyLoss]



### 9.3 项目：MNIST手写数字分类(GPU版本)

**1.引入transforms 实现 PlL lmage to Tensor**

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250529173716878.png" alt="image-20250529173716878" style="zoom:50%;" />

```python
transform = transforms.Compose([
    transforms.ToTensor(),	# 这一步实现PlL lmage to Tensor，也就是0-255到0-1的转变，最后转为1*28*28的Tensor张量
    transforms.Normalize((0.1307),(0.3081))	# 设置均值=0.1307和标准差=0.3081，这里的均值和标准差是MINST所有数据集数据一起算出来的结果
])
```

**2.完整代码**

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：PyTorch_Deep_Learning_Practice（By_LiuEr） 
@File    ：MNIST_Handwritten_Digit_Classification.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/5/29 下午5:52 
"""
import time

import torch
from torchvision import transforms  # transforms主要是PyTorch中对图像处理的模块
from torchvision import datasets
from torch.utils.data import DataLoader
import torch.nn.functional as F

# 检查GPU可用性
device = torch.device(rf"cuda:0" if torch.cuda.is_available() else "cpu")

# 准备数据,转换成张量类型的数据，并进行归一化操作
batch_size = 64
transform = transforms.Compose([
    transforms.ToTensor(),  # 这一步实现PlL lmage to Tensor，也就是0-255到0-1的转变，最后转为1*28*28的Tensor张量
    transforms.Normalize((0.1307), (0.3081))  # 设置均值=0.1307和标准差=0.3081，这里的均值和标准差是MINST所有数据集数据一起算出来的结果
])
# 训练集
train_dataset = datasets.MNIST(root="../dataset/mnist",
                               train=True, download=True, transform=transform)
train_loader = DataLoader(train_dataset, shuffle=True, batch_size=batch_size, pin_memory=True, num_workers=2)

# 测试集
test_dataset = datasets.MNIST(root="../dataset/mnist", train=False,
                              download=True, transform=transform)
test_loader = DataLoader(test_dataset, shuffle=True, batch_size=batch_size, pin_memory=True, num_workers=2)


# 自定义多层模型
class Net(torch.nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.linear1 = torch.nn.Linear(784, 512)  # 第一层神经网络将784维降到512维
        self.linear2 = torch.nn.Linear(512, 256)  # 第二层神经网络将512维降到256维
        self.linear3 = torch.nn.Linear(256, 128)  # 第三层神经网络将256维降到128维
        self.linear4 = torch.nn.Linear(128, 64)  # 第四层神经网络将128维降到64维
        self.linear5 = torch.nn.Linear(64, 10)  # 第四层神经网络将64维降到10维

    def forward(self, x):  # 使用ReuLU激活单元
        x = x.view(-1, 784)  # 改变张量的形状，由28*28 变为 784
        x = F.relu(self.linear1(x))
        x = F.relu(self.linear2(x))
        x = F.relu(self.linear3(x))
        x = F.relu(self.linear4(x))
        return self.linear5(x)  # 最后一层线性层的结果直接输出，不再激活


model = Net().to(device)  # 将模型移动到GPU

# 构建损失函数的计算和优化器
criterion = torch.nn.CrossEntropyLoss()  # 多分类交叉熵损失函数
op = torch.optim.SGD(model.parameters(), lr=0.01, momentum=0.5)  # 采用SGD


# 训练过程，包括前向计算和反向传播，封装成一个函数
def train(epoch):
    running_loss = 0.0
    for batch_idx, data in enumerate(train_loader, 0):
        inputs, target = data  # 输入和目标
        inputs, target = inputs.to(device), target.to(device)
        op.zero_grad()  # 梯度归零

        # 前向计算
        outputs = model(inputs)
        loss = criterion(outputs, target)  # 计算损失值
        # 反向传播与权值更新
        loss.backward()
        op.step()

        running_loss += loss.item()
        if batch_idx % 300 == 299:  # 每训练300代就输出一次
            print(rf'第{epoch + 1}轮,第{batch_idx + 1}批次,损失值为：{running_loss / 300:.3f}')
            running_loss = 0.0  # 损失值清零


# 测试过程，封装成函数
def test():
    correct = 0  # 正确的图像个数
    total = 0  # 总测试图像个数
    with torch.no_grad():  # 因为test的过程无需反向传播，也就不需要计算梯度
        for data in test_loader:
            images, labels = data
            images, labels = images.to(device), labels.to(device)
            outputs = model(images)
            _, predicted = torch.max(outputs.data, dim=1)  # 沿着每一行找那一行最大的下表
            # 所以得到的数据标签也是一个矩阵
            total += labels.size(0)  # 同样labels也是一个Nx1的张量
            correct += (predicted == labels).sum().item()  # 计算正确的个数
        print(rf'Accuracy on test set: {100 * correct / total}%')


# 主函数逻辑
if __name__ == '__main__':
    train_mode = 'GPU' if torch.cuda.is_available() else 'CPU'
    # 如果你的电脑有GPU和cuda环境，就会使用GPU训练，否则使用CPU训练
    print(rf'当前训练使用{train_mode}模式')
    time1 = time.time()
    for epoch in range(10):  # 一共训练10epochs
        train(epoch)
        if epoch % 5 == 4:
            test()  # 每训练5轮进行一次测试

    use_time = time.time() - time1
    print(rf"训练用时：{use_time}")

```

### 9.3 课后作业:ottp电商产品多分类模型

<img alt="image-20250529181603759" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250529181603759.png"/>

> 实验过程中比较费力的几个点：
>
> 1.  多分类交叉熵损失函数计算的本质用到了独热编码的思想，但是在实际调用过程中无需手动将标签值转换成独热编码；
> 2.  train.csv文件的标签值是字符串类型，需要转换成数字类型
> 3.  训练过程中的数据类型及其转换要格外注意

```python
import numpy as np
import torch
from torch.utils.data import Dataset,DataLoader
import torch.nn.functional as F
import pandas as pd

#定义数据集
class OttoProduct(Dataset):
    def __init__(self):
        x = np.loadtxt('../dataset/otto/train.csv',delimiter=',',skiprows = 1,usecols = list(range(1,94)))
        self.len = x.shape[0]
        self.x_data = torch.from_numpy(x)
        y = pd.get_dummies((np.loadtxt('../dataset/otto/train.csv', delimiter=',', dtype = str, skiprows=1,
                       usecols=94)))#转换成独热编码
        self.y_data = torch.from_numpy(np.argmax(y.values,axis = 1))

    def __getitem__(self, index):
        return self.x_data[index],self.y_data[index]

    def __len__(self):
        return self.len
dataset = OttoProduct() #实例化数据集
# print(dataset.x_data[:10])
print(dataset.y_data[:10])
train_loader = DataLoader(dataset = dataset,batch_size = 32,shuffle = True)

#定义分类网络结构
class Net(torch.nn.Module):
    def __init__(self):
        super(Net,self).__init__()
        self.l1 = torch.nn.Linear(93,64)
        self.l2 = torch.nn.Linear(64,32)
        self.l3 = torch.nn.Linear(32,9)

    def forward(self,x):
        x = x.view(-1,93)
        x = F.relu(self.l1(x))
        x = F.relu(self.l2(x))
        return self.l3(x)
model = Net()#实例化网络

#构建损失函数和优化器
criterion = torch.nn.CrossEntropyLoss()
op = torch.optim.SGD(model.parameters(),lr = 0.01,momentum = 0.5)

#训练过程
def train(epoch):
    running_loss = 0.0
    for batch_idx,data in enumerate(train_loader,0):
        inputs,target = data
        op.zero_grad()

        #前向计算
        inputs = torch.tensor(inputs,dtype = torch.float32)
        outputs = model(inputs)
        # print(outputs)
        loss = criterion(outputs,target)

        #反向传播与权值优化
        loss.backward()
        op.step()

        running_loss += loss.item()
        if batch_idx % 500 == 499:#每训练500iterations就打印出结果
            print('[%d,%5d] loss: %3f'%(epoch+1,batch_idx+1,running_loss / 500))
            running_loss = 0.0

#主函数逻辑
if __name__ == '__main__':
    for epoch in range(10):#一共训练10个epochs
        train(epoch)
```

## 10.卷积神经网络（基础）

### 10.1 CNN的整体计算框架 

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530100958875.png" style="zoom: 67%;" />

①相较**全连接网络**来说，CNN采用**卷积核的层次架**构是为了**保留输入数据的空间特征信息**； 
②CNN从本质上来说，就是通过**网络的叠加对原始数据做特征提取**（Feature Extraction），将**原始数据空间映射到目标特征空间**，再对**映射后得到的特征图**，进**行向量拉伸**，连上一个FC和分类层。

### 10.2 convolution（卷积） 

**1.图像基础知识**

一张图像分为通道C，宽W，高H,三个层段

黑白图像为单通道，rgb彩色图像为3通道，有些图像还有4通道等

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530110002159.png" alt="image-20250530110002159" style="zoom:67%;" />

**2.卷积的运算过程**

首先我们给入一个1\*5\*5的单通道图像，宽高都是5个像素

现在我们的卷积核定义为一个5*5的矩形，在图像上会从左上角框选出一个和卷积核长宽相同的区域

这个区域里的数会和卷积核里面的数做数乘，数乘后的结果，放到输出的矩阵中。

具体流程如下图，给出了计算2次卷积的示例。

**第一次卷积计算**

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530110720762.png" alt="image-20250530110720762" style="zoom:50%;" />

**第二次卷积计算**

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530111118277.png" alt="image-20250530111118277" style="zoom:50%;" />

**多通道的卷积**

对于多通道的卷积，其实就是分别在每一层进行单通道的卷积（数乘）运算，再将各通道的运算结果按位求和。

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530111755037.png" alt="image-20250530111755037" style="zoom:67%;" />



<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530111815561.png" alt="image-20250530111815561" style="zoom: 67%;" />

**多个卷积核，多通道的卷积**

这里的卷积核是**四维**的了，（分别为个数N，通道数C，宽W，高H）

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530112658072.png" alt="image-20250530112658072" style="zoom:67%;" />

考虑最一般的卷积操作，有以下要点和结论： 
①**卷积核的通道数量**应该和输入数据的**通道数量保持一致**； 
②经过**卷积运算后数据**的**通道数量**应该和**卷积核的个数（是个数不是通道数）保持一致**；
③在卷积层和卷积运算中，**输入图像的长宽和卷积核的大小并不存在对应关系**，根据需求进行设定即可

```python
import torch

# 相关参数设定
in_channels,out_channels = 5,10	# 输入通道数和输出通道数
width,height = 100,100	# 图像宽高
kernel_size = 3	# 卷积核宽高
batch_size = 1 #在pytorch的实现中，所有数据都要采用mini-batch的形式

# 随机化输入数据
input_data = torch.randn(batch_size,in_channels,width,height) # 注意数据的维度写法(B,C,W,H)

# 构造卷积层
# 注意卷积层模型需传递的参数，输入通道代表卷积核的通道，输出通道代表卷积的个数
conv_layer = torch.nn.Conv2d(in_channels,out_channels,kernel_size)	

#卷积计算输出结果
output_data = conv_layer(input_data)

#以下打印输入出和卷积层参数的维度，体会其中的维度变化
print(input_data.shape)
print(output_data.shape)
print(conv_layer.weight.shape)
'''
运行结果：
torch.Size([1, 5, 100, 100])	分别为输入图像的个数（或者叫batch），通道数，宽度，高度
torch.Size([1, 10, 98, 98])		分别为输出图像的个数（或者叫batch），通道数，宽度，高度
torch.Size([10, 5, 3, 3])		卷积核的个数，通道数，核宽，核高
'''
```

### 10.3 padding（补零） 

有时我们在进行卷积计算的过程中，不希望卷积前后的图像宽高改变，

例如一个5\*5的图像 在经过3\*3的卷积核之后，会变成3\*3的图像，我们希望他保持5\*5不变，那么就需要在原来的图片四周补0，补成7\*7 的图像

这样在经过3\*3的卷积核之后，还是会成为5\*5的图像。如下图所示

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530142410309.png" alt="image-20250530142410309" style="zoom:67%;" />

```python
import torch
input_data = [3,4,6,5,7,
              2,4,6,8,2,
              1,6,7,8,4,
              9,7,4,6,2,
              3,7,5,4,1]
input_data = torch.Tensor(input_data).view(1,1,5,5)	# 将输入数据变形成（B,C,W,H）的形状

conv_layer = torch.nn.Conv2d(1,1,kernel_size=3,padding=1,bias = False)	# 卷积层，这里padding=1说明外部扩充一层0

kernel = torch.Tensor([1,2,3,4,5,6,7,8,9]).view(1,1,3,3)	# 卷积核应该满足（in_channel,out_channel,k_w,k_h)
conv_layer.weight.data = kernel.data  # 手动赋予卷积层权值

output_data = conv_layer(input_data)
print(output_data)
'''
运行结果：
tensor([[[[ 91., 168., 224., 215., 127.],
          [114., 211., 295., 262., 149.],
          [192., 259., 282., 214., 122.],
          [194., 251., 253., 169.,  86.],
          [ 96., 112., 110.,  68.,  31.]]]],
       grad_fn=<MkldnnConvolutionBackward>)
'''
```

### 10.4 stride（步长）

*  通过在实例化卷积层对象的时候设置位置参数`stride = xxx`来设定该卷积层**运算的步长**；
*  步长改变的是**卷积核每次右移（或下移）中心移动的像素点数**；
*  不同的stride会使得卷积运算的数**据结果的形状发生相应变化**。

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530143016642.png" alt="image-20250530143016642" style="zoom:50%;" />

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530143033791.png" alt="image-20250530143033791" style="zoom:50%;" />

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530143045506.png" alt="image-20250530143045506" style="zoom:50%;" />

```python
conv_layer = torch.nn.Conv2d(1,1,kernel_size=3,padding=1,stride=2,bias = False)
```

### 10.5 Max\_Pooling（最大池化）

<img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/d50f1fe81c68a027c7c50648f1179b34.png"/>

①最大池化层就是在给定的kernel\_size的区域中**选择当前最大的值**作为输出中一个元素值； 
②最大池化层**没有参数**，只需要**指定kernel\_size的大小即可**； 
③池化计算过程**与通道数无关**，因此计算前后数据的**通道数也不会发生变化**。

```python
import torch
input_data = [3,4,6,5,
              2,4,6,8,
              1,6,7,8,
              9,7,4,6]
input_data = torch.Tensor(input_data).view(1,1,4,4)

maxpooling_layer = torch.nn.MaxPool2d(kernel_size=2)	# 定义最大池化层
output_data = maxpooling_layer(input_data)
print(output_data)
'''
运行结果：
tensor([[[[4., 8.],
          [9., 8.]]]])
'''
```

### 10.6 卷积层模型实例 

①**卷积-池化-激活**或者**卷积-激活-池化**的顺序都可以，只要激活在两次卷积运算之间进行即可； 
②要注意在**进行全连接之前**，首先将**张量拉伸成一维的**；

```python
import torch

class Net(torch.nn.Module):
    def __init__(self):
        super(Net,self).__init__()
        self.conv1 = torch.nn.Conv2d(1,10,kernel_size=5)	# 定义第一层卷积层
        self.conv2 = torch.nn.Conv2d(10,20,kernel_size=5)	# # 定义第二层卷积层
        self.pooling = torch.nn.MaxPool2d(2)	# 定义池化层
        self.fc = torch.nn.Linear(320,10) # 定义全连接线性层

    def _forward(self,x):
        batch_size = x.size(0)
        x = F.relu(self.pooling(self.conv1(x)))	# 卷积后使用relu激活
        x = F.relu(self.pooling(self.conv2(x)))
        #将形如（n,1,28,28）的数据拉伸成（n,784）的形式，batch_size保持不变
        x = x.view(batch_size,-1) #进行FC之前将向量拉长
        x = self.fc(x) # 采用多分类，最后一层不进行激活
        return x
    
model = Net()
```

### 10.7 模型迁移为GPU模式

1.  设定可行的设备参量

```java
device = torch.device("cuda:0" if torch.cuda is available() else "cpu")
```

2. 将模型及其参数进行迁移

```java
model.to(device)
```

3. 将数据集的输入输出进行迁移

```java
inputs,targets = inputs.to(device),targets.to(device)
```

### 10.8 项目：基于CNN和PyTorch的MNIST手写数字分类

需要实现的模型示意图

<img alt="image-20250530144542028" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530144542028.png"/>

<img alt="image-20250530144812667" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530144812667.png"/>

**代码实现：**

```	python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：PyTorch_Deep_Learning_Practice（By_LiuEr） 
@File    ：MNIST_Handwritten_Digit_Classification.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/5/29 下午5:52 
"""
import time

import torch
from torchvision import transforms  # transforms主要是PyTorch中对图像处理的模块
from torchvision import datasets
from torch.utils.data import DataLoader
import torch.nn.functional as F

# 检查GPU可用性
device = torch.device(rf"cuda:0" if torch.cuda.is_available() else "cpu")

# 准备数据,转换成张量类型的数据，并进行归一化操作
batch_size = 64
transform = transforms.Compose([
    transforms.ToTensor(),  # 这一步实现PlL lmage to Tensor，也就是0-255到0-1的转变，最后转为1*28*28的Tensor张量
    transforms.Normalize((0.1307), (0.3081))  # 设置均值=0.1307和标准差=0.3081，这里的均值和标准差是MINST所有数据集数据一起算出来的结果
])
# 训练集
train_dataset = datasets.MNIST(root="../dataset/mnist",
                               train=True, download=True, transform=transform)
train_loader = DataLoader(train_dataset, shuffle=True, batch_size=batch_size, pin_memory=True, num_workers=2)

# 测试集
test_dataset = datasets.MNIST(root="../dataset/mnist", train=False,
                              download=True, transform=transform)
test_loader = DataLoader(test_dataset, shuffle=True, batch_size=batch_size, pin_memory=True, num_workers=2)


# 自定义多层模型
class Net(torch.nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.linear1 = torch.nn.Linear(784, 512)  # 第一层神经网络将784维降到512维
        self.linear2 = torch.nn.Linear(512, 256)  # 第二层神经网络将512维降到256维
        self.linear3 = torch.nn.Linear(256, 128)  # 第三层神经网络将256维降到128维
        self.linear4 = torch.nn.Linear(128, 64)  # 第四层神经网络将128维降到64维
        self.linear5 = torch.nn.Linear(64, 10)  # 第四层神经网络将64维降到10维

    def forward(self, x):  # 使用ReuLU激活单元
        x = x.view(-1, 784)  # 改变张量的形状，由28*28 变为 784
        x = F.relu(self.linear1(x))
        x = F.relu(self.linear2(x))
        x = F.relu(self.linear3(x))
        x = F.relu(self.linear4(x))
        return self.linear5(x)  # 最后一层线性层的结果直接输出，不再激活


model = Net().to(device)  # 将模型移动到GPU

# 构建损失函数的计算和优化器
criterion = torch.nn.CrossEntropyLoss()  # 多分类交叉熵损失函数
op = torch.optim.SGD(model.parameters(), lr=0.01, momentum=0.5)  # 采用SGD


# 训练过程，包括前向计算和反向传播，封装成一个函数
def train(epoch):
    running_loss = 0.0
    for batch_idx, data in enumerate(train_loader, 0):
        inputs, target = data  # 输入和目标
        inputs, target = inputs.to(device), target.to(device)
        op.zero_grad()  # 梯度归零

        # 前向计算
        outputs = model(inputs)
        loss = criterion(outputs, target)  # 计算损失值
        # 反向传播与权值更新
        loss.backward()
        op.step()

        running_loss += loss.item()
        if batch_idx % 300 == 299:  # 每训练300代就输出一次
            print(rf'第{epoch + 1}轮,第{batch_idx + 1}批次,损失值为：{running_loss / 300:.3f}')
            running_loss = 0.0  # 损失值清零


# 测试过程，封装成函数
def test():
    correct = 0  # 正确的图像个数
    total = 0  # 总测试图像个数
    with torch.no_grad():  # 因为test的过程无需反向传播，也就不需要计算梯度
        for data in test_loader:
            images, labels = data
            images, labels = images.to(device), labels.to(device)
            outputs = model(images)
            _, predicted = torch.max(outputs.data, dim=1)  # 沿着每一行找那一行最大的下表
            # 所以得到的数据标签也是一个矩阵
            total += labels.size(0)  # 同样labels也是一个Nx1的张量
            correct += (predicted == labels).sum().item()  # 计算正确的个数
        print(rf'Accuracy on test set: {100 * correct / total}%')


# 主函数逻辑
if __name__ == '__main__':
    train_mode = 'GPU' if torch.cuda.is_available() else 'CPU'
    # 如果你的电脑有GPU和cuda环境，就会使用GPU训练，否则使用CPU训练
    print(rf'当前训练使用{train_mode}模式')
    time1 = time.time()
    for epoch in range(10):  # 一共训练10epochs
        train(epoch)
        if epoch % 5 == 4:
            test()  # 每训练5轮进行一次测试

    use_time = time.time() - time1
    print(rf"训练用时：{use_time}")

```

**训练结果：**

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530151716024.png" alt="image-20250530151716024" style="zoom: 80%;" />

### 10.9 作业：使用更复杂CNN实现MNIST手写数字分类

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250530152011425.png" alt="image-20250530152011425" style="zoom: 67%;" />

```python
import torch
from torchvision import transforms
from torchvision import  datasets
from torch.utils.data import DataLoader
import torch.nn.functional as F
import matplotlib.pyplot as plt

#超参数定义
BATCH_SIZE = 512
EPOCHS = 20
DEVICE = torch.device("cuda" if torch.cuda.is_available() else "cpu")

#准备数据,转换成张量类型的数据，并进行归一化操作
batch_size = 64
transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.Normalize((0.1307),(0.3081))
])
train_dataset = datasets.MNIST(root = "../dataset/mnist",
                               train = True,download=False,transform = transform)
train_loader = DataLoader(train_dataset,shuffle = True,batch_size = batch_size)

test_dataset = datasets.MNIST(root = "../dataset/mnist",train = False,
                              download=False,transform = transform)
test_loader = DataLoader(test_dataset,shuffle = True,batch_size = batch_size)

#自定义网络模型
class Model(torch.nn.Module):
    def __init__(self):
        super(Model,self).__init__()
        self.conv1 = torch.nn.Conv2d(1,10,kernel_size = 5)
        self.conv2 = torch.nn.Conv2d(10,20,kernel_size=3,padding = 1)
        self.conv3 = torch.nn.Conv2d(20,20,kernel_size=3,padding = 1)
        self.pooling = torch.nn.MaxPool2d(2)
        self.l1 = torch.nn.Linear(180,16)
        self.l2 = torch.nn.Linear(16,10)

    def forward(self, x):
        batch_size = x.size(0)
        x = self.pooling(F.relu(self.conv1(x)))
        x = self.pooling(F.relu(self.conv2(x)))
        x = self.pooling(F.relu(self.conv3(x)))
        x = x.view(batch_size, -1)  # 进行FC之前将向量拉长
        x = self.l1(x)
        return self.l2(x)
model = Model().to(DEVICE)

#构建损失函数的计算和优化器
criterion = torch.nn.CrossEntropyLoss()#多分类交叉熵损失函数
op = torch.optim.SGD(model.parameters(),lr = 0.01)#采用SGD


#训练过程，包括前向计算和反向传播，封装成一个函数
def train(epoch):
    running_loss = 0.0
    for batch_idx,data in enumerate(train_loader,0):
        inputs,target = data
        op.zero_grad()

        #前向计算
        outputs = model(inputs)
        loss = criterion(outputs,target)
        #反向传播与权值更新
        loss.backward()
        op.step()

        running_loss += loss.item()
        if batch_idx % 300 == 299:#每训练300代就输出一次
            print('[%d,%5d] loss: %3f' % (epoch+1,batch_idx+1,running_loss / 300))
            running_loss = 0.0

#测试过程，封装成函数
def vali():
    correct = 0
    total = 0
    with torch.no_grad():#因为test的过程无需反向传播，也就不需要计算梯度
        for data in test_loader:
            images,labels = data
            outputs = model(images)
            _,predicted = torch.max(outputs.data,dim = 1)#因为是按批给的数据
            #所以得到的数据标签也是一个矩阵
            total += labels.size(0) #同样labels也是一个Nx1的张量
            correct += (predicted == labels).sum().item()
        print('Accuracy on test set: %d %%'%(100 * correct / total))

#主函数逻辑
if __name__ == '__main__':
    for epoch in range(10): #一共训练10epochs
        train(epoch)
        vali()

'''
运行结果：
[1,  300] loss: 2.224153
[1,  600] loss: 0.954984
[1,  900] loss: 0.318726
Accuracy on test set: 93 %
[2,  300] loss: 0.198788
[2,  600] loss: 0.157404
[2,  900] loss: 0.145021
Accuracy on test set: 96 %
[3,  300] loss: 0.122874
[3,  600] loss: 0.120096
[3,  900] loss: 0.105074
Accuracy on test set: 96 %
[4,  300] loss: 0.094667
[4,  600] loss: 0.095276
[4,  900] loss: 0.090835
Accuracy on test set: 97 %
[5,  300] loss: 0.084520
[5,  600] loss: 0.084876
[5,  900] loss: 0.078178
Accuracy on test set: 97 %
[6,  300] loss: 0.074427
[6,  600] loss: 0.072950
[6,  900] loss: 0.074670
Accuracy on test set: 98 %
[7,  300] loss: 0.067066
[7,  600] loss: 0.068477
[7,  900] loss: 0.066709
Accuracy on test set: 98 %
[8,  300] loss: 0.060533
[8,  600] loss: 0.060498
[8,  900] loss: 0.064969
Accuracy on test set: 98 %
[9,  300] loss: 0.056990
[9,  600] loss: 0.057694
[9,  900] loss: 0.057002
Accuracy on test set: 98 %
[10,  300] loss: 0.056185
[10,  600] loss: 0.056508
[10,  900] loss: 0.050906
Accuracy on test set: 98 %
'''
```

## 11.卷积神经网络（高级）

### 11.1 GoogleNet 

**1、网络结构**

可以看到网络结构趋向于**复杂**，那么我们在定义实现这个网络时，就要尽可能**减少代码**的冗余： 

​	①面向过程编程中，使用函数进行功能封装 

​	②**面向对象**编程中，使用**类进行功能封装**

<img alt="image-20250601181838102" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250601181838102.png"/>

**2、Inception** 

该网络构成的基本思路：

因为事先无法知道超参数怎样选择才能使得网络具有最优的结果

因此对**各种可能的超参数结构进行一个罗列**，通过**训练结果自然可以看出哪种超参数更优**。
<img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/94950c01759dc8d56a31cb6fa7ef96f0.png"/>
p.s.其中因为**各路分支采取的卷积核宽高不一致**，但是在最终拼接的时候要求图像块的尺寸WXH是一致的，所以需要规定好stride（步长)和padding（补零)。

**3.1x1卷积**

①它可以**跨越不同通道的相同位置的元素值**，也可以说成是实现了**信息融合**； 
<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250601183421171.png" alt="****" style="zoom:67%;" />

②1x1卷积最直接的作用就是**改变数据的通道数目**；

③从应用角度来说，1x1卷积的**结构可以大大减少计算量**。 

这里可以看到经过一次1x1卷积后，计算量减少了一个量级

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250601183828655.png" style="zoom: 67%;" />

**4.实现** 

**网络构建与前向传播：**

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250601184928969.png" alt="image-20250601184928969" style="zoom:67%;" />

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250601184950096.png" alt="image-20250601184950096" style="zoom:67%;" />

**按照通道合并代码**

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250601185001273.png" alt="image-20250601185001273" style="zoom:80%;" />

<img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/170409ef145b14cbf4fcd3da7e0d1287.png"/>
<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250601185250726.png" alt="image-20250601185250726" style="zoom: 80%;" />

 *  Inception块各个计算分支的实现

```python
import torch
import torch.nn.functional as F
# 对Inception网络块中的各个计算分支进行实现
# 以下每个代码块均按照以下逻辑展开：
# 先写类定义中的初始化
# 再写出数据的前向传播计算过程

# Average-Pooling + 1x1 Conv
self.branch_pool = nn.Conv2d(in_channels,24,kernel_size = 1)

branch_pool = F.avg_pool2d(x,kernel_size = 3,stride = 1,padding = 1)#维度一致，使用padding
branch_pool = self.branch_pool(branch_pool)

# 1x1 Conv
self.branch1x1 = nn.Conv2d(in_channels,16,kernel_size = 1)

branch1x1 = self.branch1x1(x)

# 1x1 Conv + 5x5 Conv
self.branch5x5_1 = nn.Conv2d(in_channels,16,kernel_size = 1)
self.branch5x5_2 = nn.Conv2d(16,24,kernel_size = 5,padding = 2)#维度一致，使用padding

branch5x5 = self.branch5x5_1(x)
branch5x5 = self.branch5x5_2(branch5x5)

#1x1 Conv + 3x3 Conv + 3x3 Conv
self.branch3x3_1 = nn.Conv2d(in_channels,16,kernel_size = 1)
self.branch3x3_2 = nn.Conv2d(16,24,kernel_size = 3,padding = 1)
self.branch3x3_3 = nn.Conv2d(24,24,kernel_size = 3,padding = 1)

branch3x3 = self.branch3x3_1(x)
branch3x3 = self.branch3x3_2(branch3x3)
branch3x3 = self.branch3x3_3(branch3x3)
```

 *  Inception块和含有Inception块的网络结构的代码实现

```python
# 首先对Inception网络块进行抽象封装
# 其他的网络结构则可以直接调用封装好的Incpetion块来构成完整网络
class InceptionA(nn.Module):
    def __init__(self,in_channels):
        super(InceptionA,self).__init__()
        self.branch1x1 = nn.Conv2d(in_channels, 16, kernel_size=1)

        self.branch5x5_1 = nn.Conv2d(in_channels, 16, kernel_size=1)
        self.branch5x5_2 = nn.Conv2d(16, 24, kernel_size=5, padding=2)

        self.branch3x3_1 = nn.Conv2d(in_channels, 16, kernel_size=1)
        self.branch3x3_2 = nn.Conv2d(16, 24, kernel_size=3, padding=1)
        self.branch3x3_3 = nn.Conv2d(24, 24, kernel_size=3, padding=1)

        self.branch_pool = nn.Conv2d(in_channels, 24, kernel_size=1)

    def forward(self,x):
        branch1x1 = self.branch1x1(x)

        branch5x5 = self.branch5x5_1(x)
        branch5x5 = self.branch5x5_2(branch5x5)

        branch3x3 = self.branch3x3_1(x)
        branch3x3 = self.branch3x3_2(branch3x3)
        branch3x3 = self.branch3x3_3(branch3x3)

        branch_pool = F.avg_pool2d(x,kernel_size = 3,stride = 1,padding = 1)
        branch_pool = self.branch_pool(branch_pool)

        outputs = [branch1x1,branch5x5,branch3x3,branch_pool]
        return torch.cat(outputs,dim = 1)#沿着通道方向进行堆叠
```

### 11.2 ResNet 

1.  **提出背景**

总的来说，ResNet框架的诞生源于深度学习中**网络越来越深**和**训练越来越难之间的一个trade-off**： 

①一方面，我们希望网络**尽可能学习到更加复杂和细粒度的特征**；  

②另一方面，深层网络在训练之中会**碰到梯度消失的问题**。

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250601190413715.png" alt="image-20250601190413715" style="zoom: 67%;" />

2. **解决梯度消失的基本思想**

 *  跳连接 
    <img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250601190836669.png" style="zoom:67%;" />

梯度消失产生的原因：在链式求导法则下，**大量小于1的数字连乘最终会趋向于0**，使得接近输入层的**网络权值无法得到很好的训练**。  

解决的方法：

在进行**激活函数之前**，这层的**输出值先和输入值进行一个叠加**，这样在进行梯度求导时，**接近于0的梯度就会变成接近于1**，连乘时就不再会产生趋近于0的问题。

3. **实现** 
   （1）残差网络块的实现
   <img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/52349ce0930003086bfa2f4b5dff61cf.png"/>

```python
# 残差网络块的实现
class ResidualBlock(nn.Module):
    def __init__(self,channels):
        super(ResidualBlock,self).__init__()
        self.channels = channels
        self.conv1 = nn.Conv2d(channels,channels,kernel_size = 3,padding = 1)
        self.conv2 = nn.Conv2d(channels,channels,kernel_size = 3,padding = 1)
        # 因为残差块的输入出最后要叠加起来一起进行激活，所以通道、长和宽这里都处理成一致的
        
    def forward(self,x):
        y = F.relu(self.conv1(x))
        y = self.conv2(y)
        return F.relu(x+y)
```

（2）残差网络块在整个深度神经网络中的实现
<img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/c583f8e57e972414d25a77d93b790fa6.png"/>

```java
#利用残差块搭建网络结构
class Net(nn.Module):
    def __init__(self):
        super(Net,self).__init__()
        self.conv1 = nn.Conv2d(1, 16, kernel_size=5, padding=2)
        self.conv2 = nn.Conv2d(16, 32, kernel_size=5, padding=2)
        self.mp = nn.MaxPool2d(2)

        self.rblock1 = ResidualBlock(16)
        self.rblock2 = ResidualBlock(32)

        self.fc = nn.Linear(512,10)

    def forward(self,x):
        in_size = x.size(0)
        x = self.mp(F.relu(self.conv1(x)))
        x = self.rblock1(x)
        x = self.mp(F.relu(self.conv2(x)))
        x = self.rblock2(x)
        x = x.view(in_size,-1)
        x = self.fc(x)
        return x
```



### 11.3 课后作业 

1. 阅读论文He K, Zhang X, Ren S, et al. Identity Mappings in Deep Residual Networks[C]，实现几种不同的ResNet的构造方式

   <img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250601191843679.png" alt="image-20250601191843679" style="zoom: 67%;" />

2. 阅读论文Huang G, Liu Z, Laurens V D M, et al. Densely Connected Convolutional Networks[J]. 2016:2261-2269. 学习Densely-connected卷积网络

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250601191905493.png" alt="image-20250601191905493" style="zoom:67%;" />

### 11.4 接下来的路

1. 阅读花书《深度学习》
2. 阅读pytorch文档（通读）
3. 复现经典工作（先读代码，再写代码）
4. 扩充视野（找到一个方向，阅读这个领域的大量论文，融汇贯通，自己找到创新点，编写论文，发论文）