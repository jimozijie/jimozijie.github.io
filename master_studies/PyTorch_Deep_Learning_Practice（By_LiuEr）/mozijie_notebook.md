# 《PyTorch深度学习实践》学习笔记

## ——BY mozijie

本课程为河北工业大学刘洪普老师所授

课程地址：https://www.bilibili.com/video/BV1Y7411d7Ys?spm_id_from=333.788.videopod.episodes&vd_source=7c23485904ea3584fb46e603e43ca789

## 本人学习进度：12.循环神经网络（基础篇）

## 1.Overview

暂无

## 2.线性模型

### 2.1课堂例题（y=wx)

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



### 2.2课后习题（y=w·x+b)

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

### 3.1穷举法的弊端

> 按照上面一讲，我们使用穷举法罗列出所有可能的权重取值情况，然后通过可视化工具来寻找其中的最优解，这是有很多弊端的：
>
> - 其一，随着权重个数的增多，搜素的空间会呈指数型增长
> - 其二，最优解的分布与权重的关系可能很复杂，不能用简单的线性搜索去解决

### 3.2分治法的弊端

> 按照我们传统算法设计思路，我么也会联想到用分治法：
>
> <img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/7652a32f6ad226aa81cbd4f32014a48e.png"/>
>
> - 其一，在现在深度学习网络中，权重参数实在过多，分治法也很难应付；
> - 其二，分治法很容易陷入局部最优的困境中

### 3.3贪心法

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



### 3.4梯度下降与随机梯度下降

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

### 3.5梯度下降代码实现

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

### 3.6随机梯度下降代码实现

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

### 4.1什么是神经网络？

如图：输入层5个节点，第一层6个节点，第二层7个节点
w相当于每层之前的权重矩阵，维度取决于左右两层的节点个数

<img alt="image-20250512163842664" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250512163842664.png"/>

### 4.2什么是计算图？

<img alt="image-20250512164758713" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250512164758713.png"/>

### 4.3以上两层神经网络出现的问题

不论几层神经网络，只要是每层神经网络都做线性运算，那么最终计算公式都会被化简为一个简单的线性模型。因此增加神经网络的层数就显得没有意义了。为了解决这个问题。我们在每一层计算后增加一个非线性函数的向量计算。

<img alt="image-20250512165617373" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250512165617373.png"/>

### 4.4反向传播示例

<img alt="image-20250512172642006" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250512172642006.png"/>

### 4.5线性模型y=w*x的反向传播

<img alt="image-20250512175533637" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250512175533637.png"/>

### 4.6线性模型y=w*x的反向传播作业

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/image-20250513101723963.png" alt="image-20250513101723963" style="zoom:25%;" />

### 4.7线性模型y=w*x+b的反向传播作业

<img src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/3197cdcad45e6539045c36ef5fbc008.png" alt="3197cdcad45e6539045c36ef5fbc008" style="zoom:25%;" />

### 4.8使用Pytorch进行线性模型y=w*x的训练

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

### 4.6课后作业：PyTorch非线性梯度下降实现

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

### 5.1流程与代码实现 

>  *  使用PyTorch构建训练模型的一般步骤： 
>     ①准备数据集 
>     ②设计模型：使用**相关类**构建模型，用以计算预测值 
>     ③使用PyTorch的应用接口来构建损失函数和优化器 
>     ④编写循环迭代的训练过程——前向计算，反向传播和梯度更新，训练过程如下——
>      *  根据前向计算计算y
>      *  使用损失函数类计算损失函数值
>      *  进行反向传播，注意梯度清零
>      *  权重更新

>  *  任务重点发生变化： 
>     ①以前手动写训练过程代码时，关键在于计算损失函数的梯度值的一般表达式  
>     ②现在使用pytorch包构建模型时，**关键在于构建计算图**

>  *  数据维度处理 
>     ①输入和输出都需要构建成矩阵形式； 
>     ②在神经网络模型的构建中，最重要的结构就是线性单元，其中要想确定权重w和偏置项b的维度，首先需要明确输入值x和输出值y_hat的输出维度； 
>     ③根据问题的不同形式，**x和y的形状各异，这也导致了最终得到的损失函数可能是标量，向量或矩阵；**但不管loss是什么形式，**最后进行反向传播前，都要计算出loss的一个标量值。** 
>     p.s. 比如对loss向量的每一个单元求和，如果不取标量出来，就会对计算图进行累加操作，从而使得内存被撑爆。

```Python
import numpy as np
import torch
import random
import matplotlib.pyplot as plt
from matplotlib import cm
from mpl_toolkits.mplot3d import Axes3D

#实现定义好数据集的x值和y值
x_data = torch.tensor([[1.0],[2.0],[3.0]])	# 此时的x_data和y_data 都是一个3*1的矩阵
y_data = torch.tensor([[2.0],[4.0],[6.0]])


#构建训练模型
class LinearModel(torch.nn.Module):
    # 以下两个函数是必须要实现的，最少要实现的。
    def __init__(self):
        super(LinearModel,self).__init__()#继承父类，torch.nn.Module里面有很多模型，以后基本所有的模型都继承与这个类
        self.linear = torch.nn.Linear(1,1)#定义一个线性计算单元，构造一个Linear类(继承自Module类)对象，它包含权重ω和偏置b

    def forward(self,x):
        y_pred = self.linear(x)#可调用对象，进行了相应的forward计算
        return y_pred

model = LinearModel()#对实现的模型类进行实例化，同样也是可调用的

#构建损失函数的计算和优化器
criterion = torch.nn.MSELoss(size_average = False)#计算均值与否本质上只是线性系数的差别
optimizer = torch.optim.SGD(model.parameters(),lr = 0.01)#调用所继承父类的函数找到当前需要训练的参数列表

epochs = []
costs = []
#训练过程
for epoch in range(100):
    epochs.append(epoch)
    y_pred = model(x_data)
    loss = criterion(y_pred,y_data)
    costs.append(loss.item())
    print(epoch,loss)#因为这里loss是个对象，print在调用的时候会自动进行__str__()的解析

    optimizer.zero_grad()#一定要注意梯度归零
    loss.backward()
    optimizer.step()#进行梯度的更新，对参数列表中包含的参数使用预先设置的学习率进行更新

#对训练结果进行输出
print('w = ',model.linear.weight.item())
print('b = ',model.linear.bias.item())#使用.item()因为w和b都是张量对象

#进行模型测试
x_test = torch.Tensor([[4.0]])
y_test = model(x_test)
print('y_pred = ',y_test.data)

#训练过程可视化
plt.plot(epochs,costs)
plt.ylabel('Cost')
plt.xlabel('Epoch')
plt.show()
```

### 5.2PyTorch相关知识 

> ①`nn.Linear`类中对`__call()__`方法进行了实现，且其内部有对函数`forward()`的调用，故在定义模型时需要对`forward()`函数进行实现。  
>
> ②`nn.Linear`类相当于是对线性计算单元的封装，里面包含两个张量类型的成员：权重和偏置项  
> <img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/cb324861f423c90299ada34ad4f58252.png"/>
>
> ③根据下图所示按照MSE规则计算损失函数的过程，它也是一个计算图，是需要继承自`nn.Module`模块的，我们所调用的`nn.MSELoss`就是继承自该模块的类。  
> <img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/7dd651626a9012c8a065981e15823eb3.png"/>
>
> ④代码中模型与类之间的关系：  
> <img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/3b33724582f3c7020e7328b3026abca2.png"/>

### 5.3课后作业 

> 比较不同优化器下的线性回归并可视化  
> p.s.因为LBFGS的setp()调用方法具有特殊性，以下并未对LBFGS进行实现  
> <img alt="在这里插入图片描述" src="master_studies/PyTorch_Deep_Learning_Practice（By_LiuEr）/mozijie_notebook.assets/ada9f9e322964875a9beeea4049a42b8.png"/>

```python
import numpy as np
import torch
import random
import matplotlib.pyplot as plt
from matplotlib import cm
from mpl_toolkits.mplot3d import Axes3D

#实现定义好数据集的x值和y值
x_data = torch.tensor([[1.0],[2.0],[3.0]])
y_data = torch.tensor([[2.0],[4.0],[6.0]])


#构建训练模型
class LinearModel(torch.nn.Module):
    def __init__(self):
        super(LinearModel,self).__init__()#继承父类
        self.linear = torch.nn.Linear(1,1)#定义一个线性计算单元，继承Module块，也可以自主实现反向传播

    def forward(self,x):
        y_pred = self.linear(x)#可调用对象，进行了相应的forward计算
        return y_pred

model1 = LinearModel()  # 定义的模型结构初始化
model2 = LinearModel()
model3 = LinearModel()
model4 = LinearModel()
model5 = LinearModel()
#model6 = LinearModel()
model7 = LinearModel()
model8 = LinearModel()
models = [model1,model2,model3,model4,model5,model7,model8]
#构建损失函数的计算和优化器
criterion = torch.nn.MSELoss(size_average = False)

op1 = torch.optim.SGD(model1.parameters(),lr = 0.01)#以下分别尝试不同的优化器
op2 = torch.optim.Adagrad(model2.parameters(),lr = 0.01)
op3 = torch.optim.Adam(model3.parameters(),lr = 0.01)
op4 = torch.optim.Adamax(model4.parameters(),lr = 0.01)
op5 = torch.optim.ASGD(model5.parameters(),lr = 0.01)
#op6 = torch.optim.LBFGS(model6.parameters(),lr = 0.01)
op7 = torch.optim.RMSprop(model7.parameters(),lr = 0.01)
op8 = torch.optim.Rprop(model8.parameters(),lr = 0.01)
ops = [op1,op2,op3,op4,op5,op7,op8]

titles = ['SGD','Adagrad','Adam','Adamax','ASGD','RNSprop','Rprop']

fig,ax = plt.subplots(2,4,figsize = (20,10),dpi = 100)
#ax[0][0].imshow()
#训练过程，对每个优化器都进行尝试
index = 0
for item in zip(ops,models):#分别尝试每个优化器
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
    #plt.subplots(2,1,index)
    a = int(index /4)
    b = int(index % 4)
    ax[a][b].set_ylabel('Cost')
    ax[a][b].set_xlabel('Epoch')
    ax[a][b].set_title(titles[index])
    ax[a][b].plot(epochs, costs)
    index += 1
plt.show()
```
