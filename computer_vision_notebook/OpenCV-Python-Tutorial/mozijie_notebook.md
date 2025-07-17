# OpenCV-Python-Tutorial 学习笔记

## 作者：墨子杰

## 0.笔记说明

> 该笔记为本人学习CodecWang大神的OpenCV-Python-Tutorial教程所记录的笔记
>
> 部分代码按照自己的习惯做了修改，也加了一些自己的东西
>
> CodecWang大神OpenCV-Python-Tutorial教程的Github网址如下：
>
> https://github.com/CodecWang/opencv-python-tutorial
>
> 学习过程中参考了段力辉老师所翻译的《OpenCV-Python 中文教程》
>
> 也参考了Github的该仓库：https://github.com/makelove/OpenCV-Python-Tutorial?tab=readme-ov-file
>
> 我不确定上述Github仓库的作者是不是段力辉老师，暂且默认是吧哈哈。
>
> 感谢以上所有老师的开源教程

!> 特别注意：本笔记所有资源文件，图片，视频等均可下载 



## 1.简介与安装

### 1.1 OpenCV简介

OpenCV是一个基于Apache2.0许可（开源）发行的跨平台计算机视觉和机器学习软件库，可以运行在Linux、Windows、Android和Mac OS操作系统上。

它轻量级而且高效由一系列 C 函数和少量 C++ 类构成，同时提供了Python、Ruby、MATLAB等语言的接口，实现了图像处理和计算机视觉方面的很多通用算法。

### 1.2 OpenCV-Python安装与导入

我的相应版本为：Python=3.12  OpenCV-Python=4.11

**安装**

在对应的终端执行以下命令即可（建议conda环境）

> 有关conda环境的创建相关内容可以看我侧边栏的墨子杰积累笔记—Python知识积累

```cmd
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple/ opencv-python
```

**导入**

```python
import cv2
```

### 1.3 小试牛刀：打开图片并计算读取时间

所需要的图片资源：

**lena.jpg**

<img alt="lena" src="computer_vision_notebook/OpenCV-Python-Tutorial/mozijie_notebook.assets/lena.jpg"/>

**代码：**

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：OpenCV-Python-Tutorial 
@File    ：cv2_read_img_time.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/7/11 下午4:01 
"""
import time

import cv2

"""
cv2.getTickCount 函数返回从参考点到这个函数被执行的时钟数。
所以当你在一个函数执行前后都调用它的话，你就会得到这个函数的执行时间(时钟数)。

cv2.getTickFrequency 返回时钟频率，或者说每秒钟的时钟数。
所以你可以按照下面的方式得到一个函数运行了多少秒:
"""
start_time = cv2.getTickCount()/cv2.getTickFrequency()

img = cv2.imread('lena.jpg')

print(rf'执行时间：{cv2.getTickCount()/cv2.getTickFrequency() - start_time}')
```

## 2.图片

所需要的图片资源：

**lena.jpg**

<img alt="lena" src="computer_vision_notebook/OpenCV-Python-Tutorial/mozijie_notebook.assets/lena.jpg"/>

### 2.1 读入/显示/保存图像

**代码及笔记：**

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：OpenCV-Python-Tutorial 
@File    ：cv2_read_show_save_img.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/7/11 下午4:25 
"""
import cv2

"""
cv.imread()
- 参数 1：图片的文件名
  - 如果图片放在当前文件夹下，直接写文件名就行，如'lena.jpg'
  - 否则需要给出绝对路径，如'D:\mycode\lena.jpg'
- 参数 2：读入方式，省略即采用默认值
  - `cv2.IMREAD_COLOR`：彩色图，默认值(1)
  - `cv2.IMREAD_GRAYSCALE`：灰度图(0)
  - `cv2.IMREAD_UNCHANGED`：包含透明通道的彩色图(-1)
"""
img = cv2.imread('lena.jpg', 0)

"""
使用函数 cv2.imshow() 显示图像。窗口会自动䖲整为图像大小。
- 参数1:窗口的名字
- 参数2：图片
你可以创建多个窗口，但是必须给他们不同的名字
"""
# 2.显示图片
cv2.imshow('lena', img)
"""
cv2.waitKey()是一个键盘绑定函数。需要指出的是它的时间尺度是毫秒级。
函数等待特定的几毫秒，看是否有键盘输入。
特定的几毫秒之内，如果按下任意键，这个函数会返回按键的 ASCII码值，程序将会继续运行。
如果没有键盘输入，返回值为-1，如果我们设置这个函数的参数为0，那它将会无限期的等待键盘输入。
它也可以被用来检测特定键是否被按下，例如按键 a是否被按下，这个后面我们会接着讨论。
"""
cv2.waitKey(0)  # 这里按下任何按键都行

# 先定义窗口，后显示图片
"""
你也可以先创建一个窗口，之后再加载图像。 
这种情况下，你可以决定窗口是否可以调整大小。使用到的函数是 cv2.namedWindow()。
初始设定函数标签是 Cv2.WINDOW_AUTOSIZE。
但是如果你把标签改成Cv2.WINDOW_NORMAL，你就可以调整窗口大小了。
当图像维度太大，或者要添加轨迹条时，调整窗口大小将会很有用"""
cv2.namedWindow('lena2', cv2.WINDOW_NORMAL)
cv2.imshow('lena2', img)
"""
在这里测试是否可以读取到按下按键的ASCII码值
"""
key = cv2.waitKey(0)     # 你可以试着按下1，看是不是输出的49
print(key)
"""
cv2.destroyAIlWindows()可以轻易删除任何我们建立的窗口。
如果你想删除特定的窗口可以使用 cv2.destroyWindow()，在括号内输入你想删除的窗口名。
"""
cv2.destroyAllWindows()

# 3.保存图片
"""
使用函数 cv2.imwrite() 来保存一个图像。
首先需要一个文件名，之后才是你要保存的图像。
"""
cv2.imwrite('lena_gray.jpg', img)
```



### 2.2 使用matplotlib读入显示彩色图

**代码及笔记：**

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：OpenCV-Python-Tutorial 
@File    ：matplotlib_read_show.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/7/11 下午4:50 
"""
import cv2
import matplotlib as mpl
mpl.use('TkAgg')  # 设置 Matplotlib 使用 TkAgg 后端
import matplotlib.pyplot as plt

img = cv2.imread('lena.jpg', cv2.IMREAD_COLOR)  # 读取为BGR格式的彩色图

"""
在 OpenCV 中有超过 150 中进行颜色空间转换的方法。
但是你以后就会发现我们经常用到的也就两种:BGR ↔ Gray 和 BGR ↔ HSV。
我们要用到的函数是:cv2.cvtColor(input image,flag)，其中 flag就是转换类型。
对于 BGR ↔ Gray 的转换,我们要使用的 fag 就是 cv2.COLOR_BGR2GRAY
同样对于 BGR ↔ HSV的转换,我们用的 fag 就是 cv2.COLOR_BGR2HSV.

这里我们使用是cv2.COLOR_BGR2RGB将BGR格式转换为RGB格式
"""
img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

plt.imshow(img_rgb)  # 显示转换后的RGB图像
plt.show()  # 展示窗口

```



## 3.视频

### 3.1 用摄像头/文件打开视频

**由于我用的是台式机，而且没装摄像头，我这里就用一个测试视频打开**



#### 3.1.1 cv2.VideoCapture.get、set详解

>你可以使用函数 cap.get(propId)来获得视频的一些参数信息。
>
>这里propId 可以是 0到 18 之间的任何整数。每一个数代表视频的一个属性，**具体可以见下表**
>
>其中的一些值可以使用 cap.set(propId,value)来修改，value 就是你想要设置成的新值。
>
>**这里注意，使用set修改参数时，如果你是打开的视频文件，很多参数无法通过set方法修改**
>
>**但是如果你是通过打开摄像头，那么set的一些视频相关参数就有可能可以修改了**

<table> 
 <thead> 
  <tr> 
   <th>param</th> 
   <th>define</th> 
  </tr> 
 </thead> 
 <tbody> 
  <tr> 
   <td>cv2.VideoCapture.get(0)</td> 
   <td>CV_CAP_PROP_POS_MSEC 视频文件的当前位置（播放）以毫秒为单位</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(1)</td> 
   <td>CV_CAP_PROP_POS_FRAMES 基于以0开始的被捕获或解码的帧索引</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(2)</td> 
   <td>CV_CAP_PROP_POS_AVI_RATIO 视频文件的相对位置（播放）：0=电影开始，1=影片的结尾。</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(3)</td> 
   <td>CV_CAP_PROP_FRAME_WIDTH 在视频流的帧的宽度</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(4)</td> 
   <td>CV_CAP_PROP_FRAME_HEIGHT 在视频流的帧的高度</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(5)</td> 
   <td>CV_CAP_PROP_FPS 帧速率</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(6)</td> 
   <td>CV_CAP_PROP_FOURCC 编解码的4字-字符代码</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(7)</td> 
   <td>CV_CAP_PROP_FRAME_COUNT 视频文件中的帧数</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(8)</td> 
   <td>CV_CAP_PROP_FORMAT 返回对象的格式</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(9)</td> 
   <td>CV_CAP_PROP_MODE 返回后端特定的值，该值指示当前捕获模式</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(10)</td> 
   <td>CV_CAP_PROP_BRIGHTNESS 图像的亮度(仅适用于照相机)</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(11)</td> 
   <td>CV_CAP_PROP_CONTRAST 图像的对比度(仅适用于照相机)</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(12)</td> 
   <td>CV_CAP_PROP_SATURATION 图像的饱和度(仅适用于照相机)</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(13)</td> 
   <td>CV_CAP_PROP_HUE 色调图像(仅适用于照相机)</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(14)</td> 
   <td>CV_CAP_PROP_GAIN 图像增益(仅适用于照相机)（Gain在摄影中表示白平衡提升）</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(15)</td> 
   <td>CV_CAP_PROP_EXPOSURE 曝光(仅适用于照相机)</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(16)</td> 
   <td>CV_CAP_PROP_CONVERT_RGB 指示是否应将图像转换为RGB布尔标志</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(17)</td> 
   <td>CV_CAP_PROP_WHITE_BALANCE × 暂时不支持</td> 
  </tr> 
  <tr> 
   <td>cv2.VideoCapture.get(18)</td> 
   <td>CV_CAP_PROP_RECTIFICATION 立体摄像机的矫正标注（目前只有DC1394 v.2.x后端支持这个功能）</td> 
  </tr> 
 </tbody> 
</table>

<table> 
 <thead> 
  <tr> 
   <th>capture.set</th> 
   <th>作用</th> 
  </tr> 
 </thead> 
 <tbody> 
  <tr> 
   <td>capture.set(cv2.CAP_PROP_FRAME_WIDTH, 1080);</td> 
   <td>宽度</td> 
  </tr> 
  <tr> 
   <td>capture.set(cv2.CAP_PROP_FRAME_HEIGHT, 960);</td> 
   <td>高度</td> 
  </tr> 
  <tr> 
   <td>capture.set(cv2.CAP_PROP_FPS, 30);</td> 
   <td>帧率 帧/秒</td> 
  </tr> 
  <tr> 
   <td>capture.set(cv2.CAP_PROP_BRIGHTNESS, 1);</td> 
   <td>亮度</td> 
  </tr> 
  <tr> 
   <td>capture.set(cv2.CAP_PROP_CONTRAST,40)</td> 
   <td>对比度 40</td> 
  </tr> 
  <tr> 
   <td>capture.set(cv2.CAP_PROP_SATURATION, 50);</td> 
   <td>饱和度 50</td> 
  </tr> 
  <tr> 
   <td>capture.set(cv2.CAP_PROP_HUE, 50);</td> 
   <td>色调 50</td> 
  </tr> 
  <tr> 
   <td>capture.set(cv2.CAP_PROP_EXPOSURE, 50);</td> 
   <td>曝光 50 </td> 
  </tr> 
 </tbody> 
</table>

#### 3.1.2 资源视频demo_video.mp4


!> 特别注意：该视频可下载，可倍速播放，代码里需要用到此视频。如果要跑代码可以把视频下载下来

<video controls width="640">
  <source src="computer_vision_notebook/OpenCV-Python-Tutorial/mozijie_notebook.assets/demo_video.mp4" type="video/mp4" />
</video>


#### 3.1.3 代码

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：OpenCV-Python-Tutorial 
@File    ：capture_videos_with_camera.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/7/16 上午9:35 
"""
import cv2
from loguru import logger

"""
为了获取视频，你应该创建一个 VideoCapture 对象。
他的参数可以是设备的索引号，或者是一个视频文件。
参数是0，表示打开笔记本的内置摄像头。
参数是视频文件路径则打开视频，如camera = cv2.VideoCapture("../test.avi")
设备索引号就是在指定要使用的摄像头。一般的笔记本电脑都有内置摄像头。所以参数就是0。
你可以通过设置成1 或者其他的来选择别的摄像头。
"""
camera = cv2.VideoCapture(rf"./demo_video.mp4")    # 创建相机对象

"""
有时 cap 可能不能成功的初始化摄像头设备。这种情况下上面的代码会报错。
你可以使用 cap.isOpened()，来检查是否成功初始化了。如果返回值是True，那就没有问题。
否则就要使用函数 cap.open()。
"""
logger.debug(rf"相机打开状态：{camera.isOpened()}")
if not camera.isOpened():  # 如果相机未打开，打开它
    camera.open(rf"./demo_video.mp4")

"""
你可以使用函数 cap.get(propId)来获得视频的一些参数信息。
这里propId 可以是 0到 18 之间的任何整数。每一个数代表视频的一个属性
其中的一些值可以使用 cap.set(propId,value)来修改，value 就是你想要设置成的新值。
例如，我可以使用 cap.get(3)和 cap.get(4)来查看每一帧的宽和高。默认情况下得到的值是 640X360。
但是我可以使用 ret=cap.set(3,320)和 ret=cap.set(4,180)来把宽和高改成 320X180。
"""
camera_width, camera_height = camera.get(3), camera.get(4)   # 获取视频宽度高度
camera_fps = camera.get(5)  # 获取视频帧率
logger.debug(rf'相机宽度{camera_width}，相机高度{camera_height}，视频帧率为{camera_fps}帧/s')   # 打印参数

while True:
    """
    cap.read()按帧读取视频，ret,frame是获cap.read()方法的两个返回值。
    其中ret是布尔值，如果读取帧是正确的则返回True。
    如果文件读取到结尾，它的返回值就为False。
    frame就是每一帧的图像，是个三维矩阵。
    """
    ret, frame = camera.read()  # 读取一帧图片
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)  # 转化为灰度图
    cv2.imshow('frame', gray)   # 显示图片
    if cv2.waitKey(30) & 0xFF == ord('1'):   # 延迟30毫秒，等待按下"1"键退出循环
        break

camera.release()    # 释放相机
cv2.destroyAllWindows()     # 删除所有窗口

```

### 3.2 保存视频

**由于我用的是台式机，没有摄像头，所以保存视频代码修改一下**

**改成从demo_video读取图片，改成灰度再保存**

用到的视频还是3.1里面的视频demo_video.mp4



**代码**

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：OpenCV-Python-Tutorial 
@File    ：save_video.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/7/16 下午2:14 
"""

"""
读取一个视频，转为灰度视频保存
"""
import cv2
from loguru import logger

capture = cv2.VideoCapture(rf"./demo_video.mp4")    # 打开视频文件

# 定义编码方式并创建VideoWriter对象
"""
cv2.VideoWriter_fourcc 是 OpenCV 中用于定义视频编解码器的函数.
它将四个字符的编码（FourCC）转换为一个用于视频编码器的整数。
FourCC 是一种四字符编码，用于指定视频文件中使用的压缩方式。
'XVID'：常用于 .avi 格式的视频文件。
'MP4V'：常用于 .mp4 格式的视频文件。
'MJPG'：适用于使用 Motion JPEG 编码的视频。
VideoWriter_fourcc返回一个整数，该整数用于创建 cv2.VideoWriter 对象时指定视频编码格式。
"""
fourcc = cv2.VideoWriter_fourcc(*'mp4v')    # 编码方式为MP4V
fps = capture.get(cv2.CAP_PROP_FPS)     # 获取原视频帧率
width = int(capture.get(cv2.CAP_PROP_FRAME_WIDTH))  # 获取原视频宽度
height = int(capture.get(cv2.CAP_PROP_FRAME_HEIGHT))    # 获取原视频高度
output_path = 'mozijie.mp4'     # 文件名称和路径
frame_size = (width, height)     # 视频像素大小
"""
cv2.VideoWriter() 是 OpenCV 提供的一个类，用于将图像帧保存为视频文件。
它允许设置视频编码格式、帧率、分辨率等参数，非常适合在视频处理任务中生成输出视频文件
第一个参数是要保存的文件的路径
fourcc 指定编码器
fps 要保存的视频的帧率
frameSize 要保存的文件的画面尺寸
isColor 指示是黑白画面还是彩色的画面
"""
outfile = cv2.VideoWriter(output_path, fourcc, fps, frame_size)  # 创建视频写入对象

while True:
    ret, frame = capture.read()     # 读取一帧图片
    if not ret:
        logger.debug(rf'视频处理完成')
        break

    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)  # 转化为灰度图

    # 单通道转三通道
    gray_3ch = cv2.cvtColor(gray, cv2.COLOR_GRAY2BGR)

    # 写入文件
    outfile.write(gray_3ch)

    cv2.imshow('gray_3ch', gray_3ch)
    if cv2.waitKey(1) == ord('q'):
        break

# 释放所有资源
capture.release()       # 关闭视频文件
outfile.release()        # 关闭视频写入器
cv2.destroyAllWindows()       # 关闭所有OpenCV窗口

logger.debug(rf'视频保存完成：{output_path}')
```



## 27.分水岭算法图像分割

### 27.1目标

1. 使用分水岭算法基于掩模的图像分割
2. 函数：**cv2.watershed()**

### 27.2原理

>任何一副灰度图像都可以被看成拓扑平面，灰度值高的区域可以被看成是山峰，灰度值低的区域可以被看成是山谷。
>
>我们向每一个山谷中灌不同颜色的水。随着水的位的升高，不同山谷的水就会相遇汇合，为了防止不同山谷的水汇合，我们需要在水汇合的地方构建起堤坝。
>
>不停的灌水，不停的构建堤坝知道所有的山峰都被水淹没。我们构建好的堤坝就是对图像的分。这就是分水岭算法的背后哲理。
>
>但是这种方法通常都会得到过度分割的结果，这是由噪声或者图像中其他不规律的因素造成的。
>
>为了减少这种影响，OpenCV 采用了基于掩模的分水岭算法，在这种算法中我们要设置那些山谷点会汇合，那些不会。
>
>这是一种交互式的图像分割。我们要做的就是给我们已知的对象打上不同的标签。
>
>如果某个区域肯定是前景或对象，就使用某个颜色(或灰度值)标签标记它。
>
>如果某个区域肯定不是对象而是背景就使用另外一个颜色标签标记。
>
>而剩下的不能确定是前景还是背景的区域就用 0标记。这就是我们的标签。
>
>然后实施分水岭算法每一次灌水，我们的标签就会被更新，当两个不同颜色的标签相遇时就构建堤坝，直到将所有山峰淹没，最后我们得到的边界对象(堤坝)
>
>的值为-1。

### 27.3分割硬币代码

需要的图片资源：

<img alt="water_coins" src="computer_vision_notebook/OpenCV-Python-Tutorial/mozijie_notebook.assets/water_coins.jpg"/>

```python
#!/usr/bin/env python
# -*- coding: UTF-8 -*-
"""
@Project ：OpenCV-Python-Tutorial 
@File    ：split_coins.py
@IDE     ：PyCharm 
@Author  ：Mozijie
@Date    ：2025/7/16 下午5:00 
"""
import numpy as np
import cv2
from matplotlib import pyplot as plt
from loguru import logger

def mozijie_watershed(img_path):

    # 1. 图像读取与预处理
    img = cv2.imread(img_path)  # 读取硬币图像
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)  # 转为灰度图像：彩色转单通道灰度

    """
    2. 图像二值化处理
    cv2.threshold: 图像阈值处理函数
    参数说明:
      gray: 输入灰度图像
      0: 阈值（这里设为0因为使用OTSU会自动计算最佳阈值）
      255: 最大值（当像素值超过阈值时赋予的值）
      cv2.THRESH_BINARY_INV + cv2.THRESH_OTSU:
          THRESH_BINARY_INV - 反向二值化（小于阈值设为255，大于设为0）
          THRESH_OTSU - 使用OTSU算法自动确定最佳阈值
    返回值:
      ret: 实际使用的阈值
      thresh: 二值化后的图像
    """
    ret, thresh = cv2.threshold(gray, 0, 255, cv2.THRESH_BINARY_INV+cv2.THRESH_OTSU)
    logger.debug(rf'二值化实际使用的阈值：{ret}')

    # 展示原图与二值化的图
    # cv2.imshow('原始图像', img)
    # cv2.imshow('二值化阈值分割后', thresh)

    # 3. 形态学操作去除噪声
    # np.ones: 创建全为1的数组
    # 参数说明: (3,3) - 创建3x3的矩阵, np.uint8 - 无符号8位整数类型
    kernel = np.ones((3, 3), np.uint8)  # 创建3x3的结构元素（用于形态学操作）

    """
    cv2.morphologyEx: 形态学操作函数
    参数说明:
      thresh: 输入二值图像
      cv2.MORPH_OPEN: 开运算（先腐蚀后膨胀）
      kernel: 结构元素
      iterations=2: 执行2次开运算
    作用: 去除小的噪声点，平滑物体边界
    """
    opening = cv2.morphologyEx(thresh, cv2.MORPH_OPEN, kernel, iterations=2)
    # cv2.imshow('opening', opening)

    # 4. 确定背景区域
    """
    cv2.dilate: 膨胀操作函数
    参数说明:
      opening: 输入图像
      kernel: 结构元素
      iterations=3: 执行3次膨胀
    作用: 扩大前景区域，确保背景区域被完全覆盖
    作用：扩大前景区域，填充硬币之间的小孔洞
    原理：用结构元素扫描图像，将锚点覆盖区域的最大值赋给锚点
    """
    sure_bg = cv2.dilate(opening, kernel, iterations=3)
    # cv2.imshow('sure_bg', sure_bg)

    # 5. 确定前景区域（硬币中心区域）
    """
    cv2.distanceTransform: 距离变换函数
    cv2.distanceTransform(src,distanceType,maskSize)
    第二个参数 1,2,3 分别表示 CV_DIST_L1, CV_DIST_L2 , CV_DIST_C
    这里使用的是2
    作用: 计算每个前景像素到最近背景像素的距离
    参数说明:
      opening: 输入二值图像
      1: 距离类型 (CV_DIST_L2: 曼哈顿距离)
      5: 掩模大小 (5x5)
    """
    # 距离变换的基本含义是计算一个图像中非零像素点到最近的零像素点的距离，
    # 最常见的距离变换算法就是通过连续的腐蚀操作来实现，
    # 腐蚀操作的停止条件是所有前景像素都被完全腐蚀。这样根据腐蚀的先后顺序，
    # 我们就得到各个前景像素点到前景中心骨架像素点的距离。根据各个像素点的距离值，
    # 设置为不同的灰度值。这样就完成了二值图像的距离变换
    dist_transform = cv2.distanceTransform(opening, 2, 5)
    # 由于distanceTransform返回的是浮点数，所以下面要进行一下归一化处理
    dist_transform = cv2.normalize(dist_transform, None, 0, 255, cv2.NORM_MINMAX)
    dist_transform = np.uint8(dist_transform)
    # cv2.imshow('dist_transform', dist_transform)

    # 对距离变换结果进行阈值处理，获取确定的前景区域
    # 原理: 距离变换值最大的区域是硬币的中心区域
    ret, sure_fg = cv2.threshold(
        dist_transform,  # 输入距离变换图像
        0.7 * dist_transform.max(),  # 阈值设为最大距离的70%
        255,  # 超过阈值的像素设为255
        0  # 阈值类型 (0表示二值化)
    )

    # 6. 确定未知区域（硬币边界区域）
    sure_fg = np.uint8(sure_fg)  # 将浮点型距离变换结果转为8位无符号整数
    # cv2.imshow('sure_fg', sure_fg)

    # cv2.subtract: 图像减法操作
    # 作用: sure_bg - sure_fg，得到硬币边界区域
    # 原理: 背景区域减去确定的前景区域 = 不确定的边界区域
    unknown = cv2.subtract(sure_bg, sure_fg)
    # cv2.imshow('unknown', unknown)

    # 7. 标记连通区域
    """
    cv2.connectedComponents: 标记连通区域
    参数:
      sure_fg: 二值图像，前景为255，背景为0
    返回值:
      ret: 连通区域数量（包括背景）
      markers: 标记图像，相同连通区域有相同整数标记
      
    作用: 识别图像中的独立物体（硬币中心区域）
    输出:
    ret: 找到的连通区域数量（包括背景）
    markers: 与输入图像相同尺寸的数组，背景标记为0，每个物体标记为不同整数（1,2,3,...）
    """
    ret, markers = cv2.connectedComponents(sure_fg)
    logger.debug(f"找到 {ret-1} 个硬币中心区域")  # 减去背景区域

    # 8. 标记未知区域（分水岭算法要求未知区域标记为0）
    """
    背景加1: 将背景区域从0变为1（分水岭算法要求背景为1）
    未知区域设为0: 分水岭算法将未知区域视为边界，标记为0
    分水岭标记规则:
    0: 未知区域（边界）
    1: 背景区域
    ≥2: 前景物体
    """
    markers = markers + 1  # 背景区域加1
    markers[unknown == 255] = 0  # 未知区域设为0

    # 9. 应用分水岭算法
    """
    cv2.watershed: 分水岭算法
    参数:
      img: 原始彩色图像（算法需要颜色信息）
      markers: 输入/输出的标记图像
    """
    markers = cv2.watershed(img, markers)

    # 10. 创建彩色分割结果
    # 创建彩色分割图像
    segmented = np.zeros_like(img)  # 创建与原始图像相同大小的黑色图像

    # 为每个硬币分配随机颜色
    coin_count = markers.max() - 1  # 硬币数量（减去背景）
    logger.debug(f"分水岭算法找到 {coin_count} 个硬币")

    # 生成HSV色调值（0-180范围），均匀分布在色轮上
    hues = np.linspace(0, 150, coin_count, endpoint=False, dtype=np.uint8)

    # 创建渐变色硬币
    for i, hue in zip(range(2, markers.max() + 1), hues):
        # 创建HSV颜色 (Hue, Saturation, Value)
        # 饱和度设为200（鲜艳），亮度设为220（明亮）
        hsv_color = np.array([[[hue, 200, 220]]], dtype=np.uint8)

        # 转换为BGR颜色
        bgr_color = cv2.cvtColor(hsv_color, cv2.COLOR_HSV2BGR)[0][0]

        # 给该硬币区域上色
        segmented[markers == i] = bgr_color

    # 边界标记为蓝色
    segmented[markers == -1] = [255, 100, 0]  # 蓝色边界 (BGR格式)

    # 可选：在原始图像上叠加分割结果
    overlay = cv2.addWeighted(img, 0.7, segmented, 0.3, 0)

    # 显示结果
    cv2.imshow('Original Image', img)
    cv2.imshow('Segmented Coins', segmented)
    cv2.imshow('Overlay Result', overlay)
    cv2.imwrite('result.jpg', overlay)
    cv2.waitKey(0)
    cv2.destroyAllWindows()

if __name__ == '__main__':
    # pic_path = 'two_dog.png'
    pic_path = 'water_coins.jpg'
    mozijie_watershed(pic_path)
```



**运行结果**

<img alt="result" src="computer_vision_notebook/OpenCV-Python-Tutorial/mozijie_notebook.assets/result.jpg"/>























