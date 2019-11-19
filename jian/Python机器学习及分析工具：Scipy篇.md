# Python机器学习及分析工具：Scipy篇

[殉道者之花火](https://www.jianshu.com/u/a9ba8193314e)关注

42018.08.28 23:23:59字数 1,615阅读 52,201



![img](https://upload-images.jianshu.io/upload_images/147042-474309bc64cadcb9.png?imageMogr2/auto-orient/strip|imageView2/2/w/356/format/webp)

------

  Scipy是一个用于数学、科学、工程领域的常用软件包，可以处理插值、积分、优化、图像处理、常微分方程数值解的求解、信号处理等问题。它用于有效计算Numpy矩阵，使Numpy和Scipy协同工作，高效解决问题。
  Scipy是由针对特定任务的子模块组成：

| 模块名            | 应用领域           |
| :---------------- | :----------------- |
| scipy.cluster     | 向量计算/Kmeans    |
| scipy.constants   | 物理和数学常量     |
| scipy.fftpack     | 傅立叶变换         |
| scipy.integrate   | 积分程序           |
| scipy.interpolate | 插值               |
| scipy.io          | 数据输入输出       |
| scipy.linalg      | 线性代数程序       |
| scipy.ndimage     | n维图像包          |
| scipy.odr         | 正交距离回归       |
| scipy.optimize    | 优化               |
| scipy.signal      | 信号处理           |
| scipy.sparse      | 稀疏矩阵           |
| scipy.spatial     | 空间数据结构和算法 |
| scipy.special     | 一些特殊的数学函数 |
| scipy.stats       | 统计               |

------

## scipy.io

- 载入和保存matlab文件

```python
from scipy import io as spio
from numpy as np
x = np.ones((3,3))
spio.savemat('f.mat',{'a':a})
data = spio.loadmat('f.mat',struct_as_record=True)
data['a']
```

- 读取图片

```jsx
from scipy import misc
misc.imread('picture')
```

------

# scipy.special

  special库中的特殊函数都是超越函数，所谓超越函数是指变量之间的关系不能用有限次加、减、乘、除、乘方、开方 运算表示的函数。如初等函数中的三角函数、反三角函数与对数函数、指数函数都是初等超越函数，一般来说非初等函数都是超越函数。

> 初等函数：指由基本初等函数经过有限次四则运算与复合运算所得到的函数

  下面我们对scipy.special中的部分常用函数进行说明:

- ![\gamma](https://math.jianshu.com/math?formula=%5Cgamma)函数
    ![\gamma](https://math.jianshu.com/math?formula=%5Cgamma)函数，也称伽玛函数，是由欧拉积分定义的函数，是阶乘函数在实数域与复数域上的扩展，当![Re(z)>0](https://math.jianshu.com/math?formula=Re(z)%3E0)时,![\gamma](https://math.jianshu.com/math?formula=%5Cgamma)函数可以定义为：![\gamma(z)=\int_{0}^{+\infty}\frac{t^{z-1}}{e^t}\text{d}t](https://math.jianshu.com/math?formula=%5Cgamma(z)%3D%5Cint_%7B0%7D%5E%7B%2B%5Cinfty%7D%5Cfrac%7Bt%5E%7Bz-1%7D%7D%7Be%5Et%7D%5Ctext%7Bd%7Dt)
  由[解析延拓原理](https://www.wikiwand.com/zh-hans/解析延拓)可以将这个定义拓展到整个复数域上，非正整数除外。
    在scipy.special中使用scipy.special.gamma()实现![\gamma](https://math.jianshu.com/math?formula=%5Cgamma)函数的计算，如果对于精度有更高要求，可以使用采用对数坐标的scipy.special.gammaln()函数进行计算。
- 贝塞尔函数
    在介绍贝塞尔函数之前，先对贝塞尔方程进行说明，一般来说，我们将形如
  ![x^2\frac{d^2y}{dx^2}+x\frac{dy}{dx}+(x^2-\alpha^2)y=0](https://math.jianshu.com/math?formula=x%5E2%5Cfrac%7Bd%5E2y%7D%7Bdx%5E2%7D%2Bx%5Cfrac%7Bdy%7D%7Bdx%7D%2B(x%5E2-%5Calpha%5E2)y%3D0)
  的二阶线性常微分方程称为贝塞尔方程,而贝塞尔方程的标准解函数![y(x)](https://math.jianshu.com/math?formula=y(x))就是贝塞尔函数。其中，参数![\alpha](https://math.jianshu.com/math?formula=%5Calpha)被称为对应贝塞尔函数的阶。贝塞尔方程是一个二阶线性齐次常微分方程，必然存在两个线性无关的解，然而通常情况下它的解无法用初等函数表示，但是当![\alpha=n](https://math.jianshu.com/math?formula=%5Calpha%3Dn)时，注意到![x=0](https://math.jianshu.com/math?formula=x%3D0)是贝尔塞方程的[正则奇点](https://baike.baidu.com/item/正则奇点/3252785)，则由常微分方程的广义幂级数解法可以得出贝塞尔方程的两个广义幂级数解：

------

![y_{1}=J_{n}(x)=\sum_{k=0}^{\infty}\frac{(-1)^k}{\gamma(n+k+ 1)\gamma(k+1)}(\frac{x}{2})^{2k+n}](https://math.jianshu.com/math?formula=y_%7B1%7D%3DJ_%7Bn%7D(x)%3D%5Csum_%7Bk%3D0%7D%5E%7B%5Cinfty%7D%5Cfrac%7B(-1)%5Ek%7D%7B%5Cgamma(n%2Bk%2B%201)%5Cgamma(k%2B1)%7D(%5Cfrac%7Bx%7D%7B2%7D)%5E%7B2k%2Bn%7D)
![y_{2}=J_{-n}(x)=\sum_{k=0}^{\infty}\frac{(-1)^k}{\gamma(-n+k+ 1)\gamma(k+1)}(\frac{x}{2})^{2k-n}](https://math.jianshu.com/math?formula=y_%7B2%7D%3DJ_%7B-n%7D(x)%3D%5Csum_%7Bk%3D0%7D%5E%7B%5Cinfty%7D%5Cfrac%7B(-1)%5Ek%7D%7B%5Cgamma(-n%2Bk%2B%201)%5Cgamma(k%2B1)%7D(%5Cfrac%7Bx%7D%7B2%7D)%5E%7B2k-n%7D)

------

其中![y_{1}](https://math.jianshu.com/math?formula=y_%7B1%7D)被称为第一类贝塞尔函数，![y_{2}](https://math.jianshu.com/math?formula=y_%7B2%7D)被称为第二类贝塞尔函数（诺依曼函数）。（求解方法参考[常微分方程教程](https://book.douban.com/subject/1191930/) 7.4 广义幂级数解法）。
  贝塞尔方程是在[柱坐标](https://baike.baidu.com/item/柱坐标)或[球坐标](https://baike.baidu.com/item/球坐标)下使用[分离变量法](https://baike.baidu.com/item/分离变量法)求解拉普拉斯方程和[亥姆霍兹方程](https://baike.baidu.com/item/亥姆霍兹方程)时得到的,因此贝塞尔函数在波动问题以及各种涉及有势场的问题中占有非常重要的地位，其应用有：

> 在圆柱形波导中的电磁波传播问题
> 圆柱体中的热传导问题
> 圆形或环形薄膜的震动膜态分析问题
> 信号处理中的调频合成（[FM](https://baike.baidu.com/item/FM)synthesis）
> 波动声学

  在scipy.special中使用scipy.special.jn()计算![n](https://math.jianshu.com/math?formula=n)阶贝塞尔函数

- 椭圆函数
    椭圆函数是定义在有限复平面上[亚纯](https://zh.wikipedia.org/wiki/亚纯函数)的双周期函数。所谓的双周期是指具有两个基本周期的单复变函数，即存在![\omega_{1},\omega_{2}](https://math.jianshu.com/math?formula=%5Comega_%7B1%7D%2C%5Comega_%7B2%7D)两个非0复数，对任意整数![m,n](https://math.jianshu.com/math?formula=m%2Cn)有：
  ![f(z+n\omega_{1}+m\omega_{2})=f(z)](https://math.jianshu.com/math?formula=f(z%2Bn%5Comega_%7B1%7D%2Bm%5Comega_%7B2%7D)%3Df(z))于是![\{n\omega_{1}+m\omega_{2}: n,m \in Z\}](https://math.jianshu.com/math?formula=%5C%7Bn%5Comega_%7B1%7D%2Bm%5Comega_%7B2%7D%3A%20n%2Cm%20%5Cin%20Z%5C%7D)构成![f(z)](https://math.jianshu.com/math?formula=f(z))的全部周期。
    在scipy.special中使用scipy.special.ellipj()函数计算椭圆函数。

- Erf(高斯曲线的面积)
    高斯曲线是指高斯分布也就是我们常说的正态分布
  ![X～N(\mu,\sigma^2)](https://math.jianshu.com/math?formula=X%EF%BD%9EN(%5Cmu%2C%5Csigma%5E2))
  其概率密度函数为：
  ![f(x)=\frac{1} {\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}](https://math.jianshu.com/math?formula=f(x)%3D%5Cfrac%7B1%7D%20%7B%5Csigma%5Csqrt%7B2%5Cpi%7D%7De%5E%7B-%5Cfrac%7B(x-%5Cmu)%5E2%7D%7B2%5Csigma%5E2%7D%7D)
  其概率密度函数曲线就是高斯曲线也叫钟形曲线，如下：

  

  ![img](https://upload-images.jianshu.io/upload_images/147042-0f811e15be291737.png?imageMogr2/auto-orient/strip|imageView2/2/w/650/format/webp)

  不同参数下的高斯曲线

  当

  

  时，高斯分布被称为标准正态分布。

  scipy.special使用scipy.special.erf()计算高斯曲线的面积。

  

## scipy.linalg

- scipy.linalg.det():计算方阵的行列式
- scipy.linalg.inv():计算方阵的逆
- scipy.linalg.svd():[奇异值](https://baike.baidu.com/item/奇异值)分解

## scipy.fftpack

  [快速傅立叶变换](https://www.wikiwand.com/zh-hans/快速傅里叶变换)（FFT）,是快速计算序列的离散傅立叶变换（DFT）或其逆变换的方法。FFT会通过把DFT矩阵分解为稀疏因子之积来快速计算此类变换。



![img](https://upload-images.jianshu.io/upload_images/147042-0071478b913118d7.gif?imageMogr2/auto-orient/strip|imageView2/2/w/300/format/webp)

傅立叶变换将函数的时域与频域相关联



scipy.fftpack使用：

- scipy.fftpack.fftfreq():生成样本序列
- scipy.fftpack.fft():计算快速傅立叶变换

## scipy.optimize

  scipy.optimize模块提供了函数最值、曲线拟合和求根的算法。

- 函数最值
    以寻找函数![f(x)=x^2+10sin(x)](https://math.jianshu.com/math?formula=f(x)%3Dx%5E2%2B10sin(x))的最小值为例进行说明：
    首先绘制目标函数的图形：

```python
from scipy import optimize
import numpy as np
import matplotlib.pyplot as plt

#定义目标函数
def f(x):
    return x**2+10*np.sin(x)

#绘制目标函数的图形
plt.figure(figsize=(10,5))
x = np.arange(-10,10,0.1)
plt.xlabel('x')
plt.ylabel('y')
plt.title('optimize')
plt.plot(x,f(x),'r-',label='$f(x)=x^2+10sin(x)$')
#图像中的最低点函数值
a = f(-1.3)
plt.annotate('min',xy=(-1.3,a),xytext=(3,40),arrowprops=dict(facecolor='black',shrink=0.05))
plt.legend()
plt.show()
```

图形输出如下：





![img](https://upload-images.jianshu.io/upload_images/147042-f6bb7cd11fc571bf.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

先确定最小值所在区间

显然这是一个非凸优化问题，对于这类函数得最小值问题一般是从给定的初始值开始进行一个梯度下降，在optimize中一般使用bfgs算法。

```css
optimize.fmin_bfgs(f,0)
```



![img](https://upload-images.jianshu.io/upload_images/147042-c550931f6cad7b75.png?imageMogr2/auto-orient/strip|imageView2/2/w/612/format/webp)

运行结果

结果显示在经过五次迭代之后找到了一个局部最低点-7.945823，显然这并不是函数的全局最小值，只是该函数的一个局部最小值，这也是拟牛顿算法（BFGS）的局限性，如果一个函数有多个局部最小值，拟牛顿算法可能找到这些局部最小值而不是全局最小值，这取决与初始点的选取。在我们不知道全局最低点，并且使用一些临近点作为初始点，那将需要花费大量的时间来获得全局最优。此时可以采用暴力搜寻算法，它会评估范围网格内的每一个点。对于本例，如下：

```bash
grid = (-10, 10, 0.1)
xmin_global = optimize.brute(f, (grid,))
print(xmin_global)
```

搜寻结果如下：



![img](https://upload-images.jianshu.io/upload_images/147042-5dd59d3761196abd.png?imageMogr2/auto-orient/strip|imageView2/2/w/196/format/webp)

全局最小值


  但是当函数的定义域大到一定程度时， [scipy.optimize.anneal()](http://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.anneal.html#scipy.optimize.anneal)提供了一个解决思路，使用模拟退火算法。

> 可以使用[scipy.optimize.fminbound](http://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.fminbound.html#scipy.optimize.fminbound)(function,a,b)得到指定范围([a,b])内的局部最低点。

- 函数零点

> [scipy.optimize.fsolve(f,x)](http://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.fsolve.html#scipy.optimize.fsolve):函数可以求解f=0的零点，x是根据函数图形特征预先估计的一个零点。

- 曲线拟合

> [scipy.optimize.curve_fit()](http://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.curve_fit.html#scipy.optimize.curve_fit):非线性最小二乘拟合

```python
from scipy import optimize

xdata = np.linspace(-10, 10, num=20)
ydata = f(xdata) + np.random.randn(xdata.size)
def f2(x, a, b):
    return a*x**2 + b*np.sin(x)

guess = [2, 2]
params, params_covariance = optimize.curve_fit(f2, xdata, ydata, guess)
print(params)
```

> scipy.optimize.leatsq():最小二乘法拟合

```python
'''
使用最小二乘法拟合直线
'''
import numpy as np
from scipy.optimize import leastsq
import matplotlib.pyplot as plt

#训练数据
Xi = np.array([8.19,2.72,6.39,8.71,4.7,2.66,3.78])
Yi = np.array([7.01,2.78,6.47,6.71,4.1,4.23,4.05])

#定义拟合函数形式
def func(p,x):
    k,b = p
    return k*x+b

#定义误差函数
def error(p,x,y,s):
    print(s)
    return func(p,x)-y

#随机给出参数的初始值
p = [10,2]

#使用leastsq()函数进行参数估计
s = '参数估计次数'
Para = leastsq(error,p,args=(Xi,Yi,s))
k,b = Para[0]
print('k=',k,'\n','b=',b)

#图形可视化
plt.figure(figsize = (8,6))
#绘制训练数据的散点图
plt.scatter(Xi,Yi,color='r',label='Sample Point',linewidths = 3)
plt.xlabel('x')
plt.ylabel('y')
x = np.linspace(0,10,1000)
y = k*x+b
plt.plot(x,y,color= 'orange',label = 'Fitting Line',linewidth = 2)
plt.legend()
plt.show()
```

拟合效果如下：



![img](https://upload-images.jianshu.io/upload_images/147042-41d897cd5af7f94e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

最小二乘法拟合直线

------

```python
'''
使用最小二乘法拟合正弦函数
'''
import numpy as np
from scipy.optimize import leastsq
import  matplotlib.pyplot as plt 

#定义拟合函数图形
def func(x,p):
    A,k,theta = p
    return A*np.sin(2*np.pi*k*x+theta)

#定义误差函数
def error(p,x,y):
    return y-func(x,p)

#生成训练数据
#随机给出参数的初始值
p0 = [10,0.34,np.pi/6]
A,k,theta = p0
x = np.linspace(0,2*np.pi,1000)
#随机指定参数

y0 = func(x,[A,k,theta])
#randn(m)从标准正态分布中返回m个值，在本例作为噪声
y1 = y0 + 2*np.random.randn(len(x))

#进行参数估计
Para = leastsq(error,p0,args=(x,y1))
A,k,theta = Para[0]
print('A=',A,'k=',k,'theta=',theta)


'''
图形可视化
'''
plt.figure(figsize=(20,8))
ax1 = plt.subplot(2,1,1)
ax2 = plt.subplot(2,1,2)

#在ax1区域绘图
plt.sca(ax1)
#绘制散点图
plt.scatter(x,y1,color='red',label='Sample Point',linewidth = 3)
plt.xlabel('x')
plt.xlabel('y')
y = func(x,p0)
plt.plot(x,y0,color='black',label='sine',linewidth=2)

#在ax2区域绘图
plt.sca(ax2)
e = y-y1
plt.plot(x,e,color='orange',label='error',linewidth=1)

#显示图例和图形
plt.legend()
plt.show()
```





![img](https://upload-images.jianshu.io/upload_images/147042-6e452d17ce933566.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

最小二乘法拟合曲线


关于Scipy更多内容后续会慢慢进行更新，如有需要请参考：[Scipy:高级科学计算](https://wizardforcel.gitbooks.io/scipy-lecture-notes/content/4.html)[Scipy官方文档](https://github.com/scipy/scipy/blob/master/scipy/optimize/optimize.py)