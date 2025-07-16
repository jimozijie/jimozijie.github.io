# OpenCV-Python-Tutorial 学习笔记

## 作者：墨子杰

## 0.笔记说明

> 该笔记为本人学习CodecWang大神的OpenCV-Python-Tutorial教程所记录的笔记，也加了一些自己的东西
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







































