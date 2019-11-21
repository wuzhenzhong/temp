# 快速上手Pytorch



[看_有灰碟](https://www.jianshu.com/u/ad22d2ea223c)关注

0.0622018.11.19 12:24:03字数 1,490阅读 5,414

------

这篇文章需要大家对深度学习里的神经网络训练有一定的基础，我以前训练网络一直都是用的TensorFlow，后面需要把模型和数据迁移到Pytorch平台上去，发现很多里面有很多知识点需要注意，写这篇文章一方面是给自己做个笔记，总结下自己的经验，另一方面是为了方便想要快速上手Pytorch的同学。这篇文章主要内容有：

- Tensorflow的PlayGround
- Pytorch介绍和安装
- Torch和Torchvision里的常用包
- Variable、Tensor、Numpy之间的关系
- CPU与GPU
- 示例--GAN生成MINIST数据

#### Tensorflow的PlayGround

PlayGround是一个在线演示、实验的神经网络平台，是一个入门神经网络非常直观的网站。这个图形化平台非常强大，将神经网络的训练过程直接可视化。假若有的同学刚刚想入门深度学习这一领域，可以去看看：
PlayGround地址：[http://playground.tensorflow.org](http://playground.tensorflow.org/)
这里也有一篇PlayGround介绍写的非常详细的文章：
参考地址：https://finthon.com/tensorflow-playground-nn/

#### Pytorch介绍和安装



![img](https://upload-images.jianshu.io/upload_images/4688102-252ef9cfa8056492.png?imageMogr2/auto-orient/strip|imageView2/2/w/397/format/webp)



2017年1月，由Facebook人工智能研究院（FAIR）基于Torch推出了PyTorch。Pytorch和Torch底层实现都用的是C语言，但是Torch的调用需要掌握Lua语言，相比而言使用Python的人更多，根本不是一个数量级，所以Pytorch基于Torch做了些底层修改、优化并且支持Python语言调用。
它是一个基于Python的可续计算包，目标用户有两类：

1. 使用GPU来运算numpy
2. 一个深度学习平台，提供最大的灵活型和速度

> 如何安装Pytorch呢？

- 基础环境
  一台PC设备、一张高性能NVIDIA显卡(可选)、Ubuntu系统
- 安装步骤

1. Anaconda(可选)和Python
2. 显卡驱动和CUDA
3. 运行Pytorch的安装命令

- 相关资料
  详细的安装教程https://blog.csdn.net/zzlyw/article/details/78674543
  Pytorch中文网[https://www.pytorchtutorial.com](https://www.pytorchtutorial.com/)

#### Torch和Torchvision里的常用包

> Torch

- `torch`：张量相关的运算，例如`创建`、`索引`、`切片`、`连接`、`转置`、`加减乘除`等
- `torch.nn`：包含搭建网络层的模块（Modules）和一系列的loss函数，例如`全连接`、`卷积`、`池化`、`BN批处理`、`dropout`、`CrossEntropyLoss`、`MSELoss`等
- `torch.nn.functional`：常用的激活函数`relu`、`leaky_relu`、`sigmoid`等
- `torch.autograd`：提供Tensor所有操作的自动求导方法
- `torch.optim`：各种参数优化方法，例如`SGD`、`AdaGrad`、`RMSProp`、`Adam`等
- `torch.nn.init`：可以用它更改`nn.Module`的默认参数初始化方式
- `torch.utils.data`：用于加载数据

> Torchvision

- `torchvision.datasets`：常用数据集，`MNIST`、`COCO`、`CIFAR10`、`Imagenet`等
- `torchvision.models`：常用模型，`AlextNet`、`VGG`、`ResNet`、`DenseNet`等
- `torchvision.transforms`：图片相关处理，`裁剪`、`尺寸缩放`、`归一化`等
- `torchvision.utils`：将给定的Tensor保存成image文件

#### Variable、Tensor、Numpy之间的关系

- Numpy
  NumPy是Python语言的一个扩充程序库。支持高级大量的维度数组与矩阵运算,此外也针对数组运算提供大量的数学函数库。
  例子：

```python
>>> import numpy as np
>>> x=np.array([[1,2,3],[9,8,7],[6,5,4]])
```

- Tensor
  PyTorch 提供一种类似 NumPy 的抽象方法来表征张量（或多维数组），它可以利用 GPU 来加速训练。

  

  ![img](https://upload-images.jianshu.io/upload_images/4688102-32674b76f06fc70e.png?imageMogr2/auto-orient/strip|imageView2/2/w/394/format/webp)

- Variable

  

  ![img](https://upload-images.jianshu.io/upload_images/4688102-178e3e89ea17564b.png?imageMogr2/auto-orient/strip|imageView2/2/w/289/format/webp)

1. PyTorch 张量的简单封装
2. 帮助建立计算图
3. Autograd（自动微分库）的必要部分
4. 将关于这些变量的梯度保存在 .grad 中

- Tensor、Variable、Numpy之间相互转化

1. 将Numpy矩阵转换为Tensor张量
   `sub_ts = torch.from_numpy(sub_img)`
2. 将Tensor张量转化为Numpy矩阵
   `sub_np1 = sub_ts.numpy()`
3. 将Tensor转换为Variable
   `sub_va = Variable(sub_ts)`
4. 将Variable转换为Tensor
   `sub_np2 = sub_va.data`

#### CPU与GPU

Pytorch支持CPU运行，但是速度非常慢，一张好的NVIDIA显卡能够大大减少网络训练时间，以我自己经验来看，15年MacBook Pro 与戴尔工作站附加一张显存11GB的1080ti显卡相比，后者速度是前者速度的224倍，尤其训练复杂网络一定要在GPU上跑。Pytorch中把数据和模型从CPU迁移到GPU非常简单：





![img](https://upload-images.jianshu.io/upload_images/4688102-1e38ffa780cfdf09.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

直接对变量、张量、模型使用`.cuda()`即可把他们迁移到GPU上，反过来迁移到CPU上，使用`.cpu()`。
当有多行显卡时，想充分利用它们，则可使用`model = nn.DataParallel(model)`命令：



![img](https://upload-images.jianshu.io/upload_images/4688102-53bf96fa8496fb67.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)



> 常见问题

- 这里的不同位置包含GPU与CPU，还包含不同GPU之间
- 不同位置的`Variable`之间不能直接相互运算
- 不同位置的`Tensor`直接不能直接相互运算
- 不同位置的`Variable`和`模型`不能直接训练
- 使用指定显卡：`.cuda(<显卡号数>)`

#### 示例--GAN生成MINIST数据

最后看个实例，如何使用GAN网络生成MINIST 数据，主要内容有：





![img](https://upload-images.jianshu.io/upload_images/4688102-fe25f49caaef1b44.png?imageMogr2/auto-orient/strip|imageView2/2/w/1024/format/webp)

###### MNIST数据集

MNIST数据集是一个手写体数据集，图片大小都是28x28，包含0-9共10个数字，各种风格：





![img](https://upload-images.jianshu.io/upload_images/4688102-b5e909160185ce5c.png?imageMogr2/auto-orient/strip|imageView2/2/w/266/format/webp)



下载好的数据集:





![img](https://upload-images.jianshu.io/upload_images/4688102-4b8f985dcef1980a.png?imageMogr2/auto-orient/strip|imageView2/2/w/439/format/webp)

测试集`t10k`开头，训练集`train`开头，`images`是图片，`labels`是标签

###### GAN网络模型





![img](https://upload-images.jianshu.io/upload_images/4688102-377bb02053690755.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)


输入100长度的噪声向量，经过一个全连接，两个卷积层，一个下采样之后生成成28x28大小的图片，这一部分是生成器生成的假图片和MNIST里的真图片经过两个卷积层下采样之后，再次经历两个全连接层后输出一个1长度的单位向量，代表输入图片为真，代表输入图片为假

##### GAN训练和Loss





![img](https://upload-images.jianshu.io/upload_images/4688102-48226f36c8a599a8.png?imageMogr2/auto-orient/strip|imageView2/2/w/555/format/webp)





![img](https://upload-images.jianshu.io/upload_images/4688102-447120d17a784a1a.png?imageMogr2/auto-orient/strip|imageView2/2/w/452/format/webp)


训练判别器D时，要使得V整体变大，训练生成器G时，要使得V整体变小。这是一个博弈的过程，就像制造假钱的犯罪团伙和验钞机的关系，犯罪团伙需要努力提高技术，让验钞机无法识别出来其制造的假币，而验钞机要能够正确的分辨出真正的纸币还有假币。理论上当判别器D只有一半的概率能识别出假图片时，就已经收敛了，实际上达不到一半的概率，没关系，使得假图片概率尽量高就行了，最终看上去效果不错。这是一张由生成器生成的假图片，你能区分出来吗？



![img](https://upload-images.jianshu.io/upload_images/4688102-27108b3aed8be60a.png?imageMogr2/auto-orient/strip|imageView2/2/w/274/format/webp)



##### 可视化

可视化方式有两种，一种是利用`torchvision`里面的包 `torchvision.utils`，另外一种是利用`visdom`插件，下面是二者的对比：



![img](https://upload-images.jianshu.io/upload_images/4688102-40800788575bff29.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)


上面那张生成的假图片就是利用里的save_image函数来存储在本地的。而以下这张图是利用，在浏览器中查看到的效果：



![img](https://upload-images.jianshu.io/upload_images/4688102-8596994ba49be914.png?imageMogr2/auto-orient/strip|imageView2/2/w/469/format/webp)


`visdom`



> 具体的代码实现去工程里查看，这里给出分享地址：
> https://github.com/gcfrun/GAN_MNIST_Pytorch
> `mnist_data.py`：数据输入模块
> `mnist_net.py`：网络模型模块
> `mnist_loss.py`：Loss计算模块
> `mnist_train.py`：迭代训练模块
> `mnist_visual.py`：可视化模块