# OpenCv-Python 开源计算机视觉库 (一)

## 1. 初识 OpenCv

1999年，英特尔的 Gary Bradsky 发起了 OpenCv 项目，并于 2000 年发布第一个版本。2005年，OpenCv 被首次应用在 Stanley，这也是赢得同年 DARPA 大挑战赛的车型。如今，OpenCv 除了支持计算机视觉，还增加了众多机器学习相关算法，未来还将持续扩展。

OpenCV 支持众多主流编程语言，如：C++，Python，Java 等，可在不同的平台上使用，包括 Windows，Linux，OS X，Android 和 iOS。此外，基于 CUDA 的高速 GPU 运算接口，和 OpenCL 也在开发当中。

## 2. OpenCV-Python

本文介绍的是 OpenCv 的 Python 接口，即 `OpenCV-Python`，但它并非 OpenCv 的 Python 实现，而仅仅是原生 OpenCv C++ 实现的 Python 包装，也就是说，我们可以像普通 Python 模块一样导入使用，但后台运行的依然是 C++ 程序，这样既发挥了 Python 的简单易用性，也充分利用了 C++ 的执行高效性，可谓两者兼得。

值得注意的是，OpenCv-Python 使用 numpy 进行数值运算，所有的 OpenCv（C++）的数组结构都在内部转换成 numpy 数组。当然，这也使得它更容易与其他使用 numpy 的库集成，如：Scipy 和 Matplotlib 。

## 3. 安装

```
pip install opencv-python
复制代码
```

## 4. 功能概览

- GUI支持: 显示和保存图片和视频，控制鼠标事件和跟踪栏
- 核心运算：图片像素编辑，对图像执行算术运算，性能优化
- 图像处理：颜色空间变化，几何变换，图像阈值，平滑处理，渐变，边缘检测，融合，轮廓线，直方图，傅立叶变化，余弦变换，模版匹配，霍夫线变换，霍夫圆变换，图像分割，前景提取，
- 特征检测与描述：哈里斯角点检测，托马斯角点检测，SIFT，SURF，ORB，特征匹配，图像查找
- 视频分析：背景分割，目标追踪，
- 相机校准与三维重建：相机校准，姿态预测，极线几何，图像提取景深（3维重建）
- 机器学习：KNN(K 临近值)，SVM(支持向量机), K-Means Clustering(K均值聚类)
- 计算机影像学：图像去噪，图像复原，HDR
- 目标检测：人脸识别

## 5. 基本操作

导入模块：

```
import cv2 as cv
复制代码
```

### 5.1 图片打开, 显示, 保存

使用 `cv.imread()` 打开图片，返回的是一个 numpy 数组。

```
img = cv.imread('dog.jpeg')

print(type(img), img.shape)
复制代码
<class 'numpy.ndarray'> (320, 320, 3)
复制代码
```

上面得到是彩色图片的 numpy 数组，也可以直接得到灰度图片的 numpy 数组。

```
img_gray = cv.imread('dog.jpeg', 0)

print(type(img_gray), img_gray.shape)
复制代码
<class 'numpy.ndarray'> (320, 320)
复制代码
```

使用 `cv.imshow()` 显示图片，会打开一个窗口 GUI 界面，自动缩放图片到适合显示的大小，并跟踪鼠标移动，在图片下方跟踪栏，显示当前位置和像素值。`imshow()` 第一个参数是窗口界面标题，如下图 “image” 。

```
cv.imshow('image', img)
cv.waitKey(0)
cv.destroyAllWindows()
复制代码
```

如果对读取的图片数据(numpy 数组)进行了修改，想保存修改后的图片保存到磁盘，就需要用到 `cv.imwrite()`，函数接收两个参数，第1个参数为保存的文件名，第2个参数为图像数据，即 numpy 数组。

```
cv.imwrite('dog_gray.png', img_gray)
复制代码
True
复制代码
```

我们已经知道如何使用 opencv-python 打开，显示，保存图片，那么综合应用起来，可以做一个完整的小程序。

打开并读取图片灰度数据，显示图片窗口，等待用户键盘输入，按 `ESC` 键退出，按字母 `s` 键保存灰度图并退出。

```
img = cv.imread('dog.jpeg', 0) # 打开灰度图
cv.imshow('dog', img) # 在窗口显示图片
k = cv.waitKey(0) # 持续等待键盘事件
if k == 27:         # 按 ESC 键退出
    cv.destroyAllWindows()
elif k == ord('s'): # 按字母 s 键保存并退出
    cv.imwrite('dog_gray.png',img)
    cv.destroyAllWindows()
复制代码
```

### 5.2 视频捕获，播放，保存

#### 5.2.1 捕获实时视频流

从笔记本电脑内置摄像头，捕获实时视频流（一张张图片），并显示经过灰度处理后的视频帧，效果就是经过灰度处理后的视频。

```
cap = cv.VideoCapture(0)
if not cap.isOpened():
    print("无法打开视频输入设备!")
    exit()
while True:
    # 一帧一帧读取视频
    ret, frame = cap.read()
    # 如果成功读取到视频帧，返回 True
    if not ret:
        print("无法接收视频输入，请检查是否开启设备访问权限。正在退出程序...")
        break
    # 在此执行对帧的处理操作
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    # 显示处理后的帧
    cv.imshow("Capture Live Video Stream", gray)
    # 按字母 q 键退出程序
    if cv.waitKey(1) == ord('q'):
        break
# 释放设备访问，关闭所有窗口
cap.release()
cv.destroyAllWindows()
复制代码
```

#### 5.2.2 播放视频文件

```
cap = cv.VideoCapture('dance.mp4')
while cap.isOpened():
    # 一帧一帧读取视频
    ret, frame = cap.read()
    # 如果成功读取到视频帧，返回 True
    if not ret:
        print("无法接收视频输入，请检查是否开启设备访问权限。正在退出程序...")
        break
    # gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    cv.imshow('Play Video File', frame)
    if cv.waitKey(1) == ord('q'):
        break
cap.release()
cv.destroyAllWindows()
复制代码
```

#### 5.2.3 保存视频文件

从视频输入设备，如笔记本电脑内置摄像头，捕获实时视频流输入，进行一帧帧处理后，保存到文件 output.avi 。

```
cap = cv.VideoCapture(0)
# 定义编解码器并创建 VideoWriter 对象
fourcc = cv.VideoWriter_fourcc(*'XVID')
out = cv.VideoWriter('output.avi', fourcc, 20.0, (640,  480))
while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        print("无法接收视频输入，请检查是否开启设备访问权限。正在退出程序...")
        break
    # 对每一帧进行垂直翻转
    frame = cv.flip(frame, 0)
    # 写入翻转后的帧
    out.write(frame)
    cv.imshow('Capture Live Video Stream', frame)
    if cv.waitKey(1) == ord('q'):
        break
# 线程结束，释放所有资源
cap.release()
out.release()
cv.destroyAllWindows()
复制代码
```

------

坚持写专栏不易，如果觉得本文对你有帮助，记得点个赞。感谢支持！

- 个人网站: [kenblog.top](https://juejin.im/post/kenblog.top)
- github 站点: [kenblikylee.github.io](https://kenblikylee.github.io/)