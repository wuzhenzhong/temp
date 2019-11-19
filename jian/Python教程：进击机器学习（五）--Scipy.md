# Python教程：进击机器学习（五）--Scipy

[![img](https://upload.jianshu.io/users/upload_avatars/5027777/2584be9b-c3e6-4522-90e0-570d548875a5.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/84e77b8600d1)

[whytin](https://www.jianshu.com/u/84e77b8600d1)关注

0.0412017.08.05 21:19:27字数 721阅读 1,141

# Scipy简介

Scipy是一个高级的科学计算库，它和Numpy联系很密切，Scipy一般都是操控Numpy数组来进行科学计算，所以可以说是基于Numpy之上了。Scipy有很多子模块可以应对不同的应用，例如插值运算，优化算法、图像处理、数学统计等。

以下列出Scipy的子模块：

| 模块名            | 功能               |
| ----------------- | ------------------ |
| scipy.cluster     | 向量量化           |
| scipy.constants   | 数学常量           |
| scipy.fftpack     | 快速傅里叶变换     |
| scipy.integrate   | 积分               |
| scipy.interpolate | 插值               |
| scipy.io          | 数据输入输出       |
| scipy.linalg      | 线性代数           |
| scipy.ndimage     | N维图像            |
| scipy.odr         | 正交距离回归       |
| scipy.optimize    | 优化算法           |
| scipy.signal      | 信号处理           |
| scipy.sparse      | 稀疏矩阵           |
| scipy.spatial     | 空间数据结构和算法 |
| scipy.special     | 特殊数学函数       |
| scipy.stats       | 统计函数           |

## 文件输入和输出：scipy.io

这个模块可以加载和保存matlab文件：

```python
>>> from scipy import io as spio
>>> a = np.ones((3, 3))
>>> spio.savemat('file.mat', {'a': a}) # 保存字典到file.mat
>>> data = spio.loadmat('file.mat', struct_as_record=True)
>>> data['a']
array([[ 1.,  1.,  1.],
       [ 1.,  1.,  1.],
       [ 1.,  1.,  1.]])
```

.关于这个模块的文档：[https://docs.scipy.org/doc/scipy/reference/io.html#module-scipy.io](https://link.jianshu.com/?t=https://docs.scipy.org/doc/scipy/reference/io.html#module-scipy.io)

## 线性代数操作：scipy.linalg

假如我们要计算一个方阵的行列式，我们需要调用det()函数：

```python
>>> from scipy import linalg
>>> arr = np.array([[1, 2],
...                 [3, 4]])
>>> linalg.det(arr)
-2.0
>>> arr = np.array([[3, 2],
...                 [6, 4]])
>>> linalg.det(arr) 
0.0
```

比如求一个矩阵的转置：

```python
>>> arr = np.array([[1, 2],
...                 [3, 4]])
>>> iarr = linalg.inv(arr)
>>> iarr
array([[-2. ,  1. ],
       [ 1.5, -0.5]])
```

更多关于[scipy.linalg
](https://link.jianshu.com/?t=http://docs.scipy.org/doc/scipy/reference/linalg.html#module-scipy.linalg).

## 快速傅里叶变换：scipy.fftpack

首先我们用numpy初始化正弦信号：

```python
>>> import numpy as np
>>> time_step = 0.02
>>> period = 5.
>>> time_vec = np.arange(0, 20, time_step)
>>> sig = np.sin(2 * np.pi / period * time_vec) + \
...       0.5 * np.random.randn(time_vec.size)
```

如果我们要计算该信号的采样频率，可以用scipy.fftpack.fftfreq()函数，计算它的快速傅里叶变换使用scipy.fftpack.fft():

```python
>>> from scipy import fftpack
>>> sample_freq = fftpack.fftfreq(sig.size, d=time_step)
>>> sig_fft = fftpack.fft(sig)
```

Numpy中也有用于计算快速傅里叶变换的模块：numpy.fft
但是scipy.fftpack是我们的首选，因为应用了更多底层的工具，工作效率要高一些。关于[scipy.fftpack
](https://link.jianshu.com/?t=http://docs.scipy.org/doc/scipy/reference/fftpack.html#module-scipy.fftpack)更多文档。

## 优化器：scipy.optimize

scipy.optimize通常用来最小化一个函数值，我们举个栗子：
构建一个函数并绘制函数图：

```python
>>> def f(x):
...     return x**2 + 10*np.sin(x)
>>> x = np.arange(-10, 10, 0.1)
>>> plt.plot(x, f(x)) 
>>> plt.show() 
```



![img](https://upload-images.jianshu.io/upload_images/5027777-cbbf3589ac97d116.png?imageMogr2/auto-orient/strip|imageView2/2/w/440/format/webp)

一条曲线

如果我们要找出这个函数的最小值，也就是曲线的最低点。就可以用到BFGS优化算法(Broyden–Fletcher–Goldfarb–Shanno algorithm)：

```python
>>> optimize.fmin_bfgs(f, 0)
Optimization terminated successfully.
         Current function value: -7.945823
         Iterations: 5
         Function evaluations: 24
         Gradient evaluations: 8
array([-1.30644003])
```

可以得到最低点的值为-1.30644003，optimize.fmin_bfgs(f, 0)第二个参数0表示从0点的位置最小化，找到最低点（该点刚好为全局最低点）。假如我从3点的位置开始梯度下降，那么得到的将会是局部最低点 3.83746663：

```python
>>> optimize.fmin_bfgs(f, 3, disp=0)
array([ 3.83746663])
```

假如你无法选出the global minimum的邻近点作为初始点的话可以使用[scipy.optimize.basinhopping()
](https://link.jianshu.com/?t=http://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.basinhopping.html#scipy.optimize.basinhopping)，具体就不展开描述。关于这个模块的其他功能，参考[scipy.optimize
](https://link.jianshu.com/?t=http://docs.scipy.org/doc/scipy/reference/optimize.html#module-scipy.optimize)

## 统计工具：scipy.stats

首先我们随机生成1000个服从正态分布的数：



![img](https://upload-images.jianshu.io/upload_images/5027777-7fd856be23470a56.png?imageMogr2/auto-orient/strip|imageView2/2/w/440/format/webp)

image.png

```python
>>> a = np.random.normal(size=1000)
#用stats模块计算该分布的均值和标准差。
>>> loc, std = stats.norm.fit(a)
>>> loc     
0.0314345570...
>>> std     
0.9778613090...
#中位数
>>> np.median(a)     
0.04041769593...
```

这个工具还是蛮好用的，更多参考：[scipy.stats
](https://link.jianshu.com/?t=http://docs.scipy.org/doc/scipy/reference/stats.html#module-scipy.stats)

还有像scipy的其他模块（计算积分、信号处理、图像处理的模块）就不一一介绍了。其实机器学习最基础的部分还是属于一些统计算法和优化算法。对这一部分还有兴趣继续了解的，戳这里：[https://docs.scipy.org/doc/scipy/reference/index.html](https://link.jianshu.com/?t=https://docs.scipy.org/doc/scipy/reference/index.html)
Python中关于科学计算的工具就介绍到这里。

**Ref**：[http://www.scipy-lectures.org/intro/scipy.html](https://link.jianshu.com/?t=http://www.scipy-lectures.org/intro/scipy.html)