# Scipy

[![img](https://upload.jianshu.io/users/upload_avatars/2324609/00e172ce3e1f?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/57a856fb8a1c)

[Aieru](https://www.jianshu.com/u/57a856fb8a1c)关注

12016.12.06 21:38:35字数 4,404阅读 24,984

# Scipy

scipy包含致力于科学计算中常见问题的各个工具箱。它的不同子模块相应于不同的应用。像插值，积分，优化，图像处理，统计，特殊函数等等。

scipy可以与其它标准科学计算程序库进行比较，比如GSL(GNU C或C++科学计算库)，或者Matlab工具箱。scipy是Python中科学计算程序的核心包; 它用于有效地计算numpy矩阵，来让numpy和scipy协同工作。

在实现一个程序之前，值得检查下所需的数据处理方式是否已经在scipy中存在了。作为非专业程序员，科学家总是喜欢重新发明造轮子，导致了充满漏洞的，未经优化的，很难分享和维护的代码。相反，Scipy程序经过优化和测试，因此应该尽可能使用。

目录

警告：这个教程离真正的数值计算介绍很远。因为枚举scipy中不同的子模块和函数非常无聊，我们集中精力代之以几个例子来给出如何使用scipy进行计算的大致思想。
scipy 由一些特定功能的子模块组成：

| 模块              | 功能               |
| ----------------- | ------------------ |
| scipy.cluster     | 矢量量化 / K-均值  |
| scipy.constants   | 物理和数学常数     |
| scipy.fftpack     | 傅里叶变换         |
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
| scipy.special     | 任何特殊数学函数   |
| scipy.stats       | 统计               |

它们全依赖numpy,但是每个之间基本独立。导入Numpy和这些scipy模块的标准方式是：

```python
import numpy as np
from scipy import stats   # 其它子模块相同
```

主scipy命名空间大多包含真正的numpy函数(尝试 scipy.cos 就是 np.cos)。这些仅仅是由于历史原因，通常没有理由在你的代码中使用import scipy

### 一、文件输入/输出：scipy.io

##### 导入和保存matlab文件:

```python
In [1]: from scipy import io as spio
In [3]: import numpy as np
In [4]: a = np.ones((3, 3))
In [5]: spio.savemat('file.mat', {'a': a})  # savemat as a dictionary
In [6]: data = spio.loadmat('file.mat', struct_as_record=True)

In [7]: data['a']
Out[7]: 
array([[ 1.,  1.,  1.],
       [ 1.,  1.,  1.],
       [ 1.,  1.,  1.]])
```

##### 读取图片：

```csharp
In [16]: from scipy import misc
In [17]: misc.imread('scikit.png')
Out[17]: 
array([[[255, 255, 255, 255],
        [255, 255, 255, 255],
        [255, 255, 255, 255],
        ..., 
        ..., 
        [255, 255, 255, 255],
        [255, 255, 255, 255],
        [255, 255, 255, 255]]], dtype=uint8)

In [18]: import matplotlib.pyplot as plt
In [19]: plt.imread('scikit.png')
Out[19]: 
array([[[ 1.,  1.,  1.,  1.],
        [ 1.,  1.,  1.,  1.],
        [ 1.,  1.,  1.,  1.],
        ..., 

       [[ 1.,  1.,  1.,  1.],
        [ 1.,  1.,  1.,  1.],
        [ 1.,  1.,  1.,  1.],
        ..., 
        [ 1.,  1.,  1.,  1.],
        [ 1.,  1.,  1.,  1.],
        [ 1.,  1.,  1.,  1.]]], dtype=float32)
```

##### 载入txt文件：

```css
numpy.loadtxt()
numpy.savetxt()
```

##### 智能导入文本/csv文件：

```css
numpy.genfromtxt()
numpy.recfromcsv()
```

##### 高速，有效率但numpy特有的二进制格式：

```css
numpy.save()
numpy.load()
```

### 二、特殊函数：scipy.special

特殊函数是先验函数。scipy.special的文档字符串写得非常好，所以我们不在这里列出所有函数。常用的有：

- 贝塞尔函数，如scipy.special.jn() (整数n阶贝塞尔函数)
- 椭圆函数: scipy.special.ellipj() (雅可比椭圆函数，……)
- 伽马函数：scipy.special.gamma()，还要注意scipy.special.gammaln,这个函数给出对数坐标的伽马函数，因此有更高的数值精度。

### 三、线性代数运算：scipy.linalg

scipy.linalg模块提供标准线性代数运算，依赖于底层有效率的实现(BLAS，LAPACK)。

scipy.linalg.det()函数计算方阵的行列式：

```csharp
In [22]: from scipy import linalg

In [23]: arr = np.array([[1, 2],
   ....:                [3, 4]])

In [24]: linalg.det(arr)
Out[24]: -2.0

In [25]: linalg.det(np.ones((3,4)))
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-25-375ad1d49940> in <module>()
----> 1 linalg.det(np.ones((3,4)))

/usr/lib/python2.7/site-packages/scipy/linalg/basic.pyc in det(a, overwrite_a)
    398     a1 = np.asarray_chkfinite(a)
    399     if len(a1.shape) != 2 or a1.shape[0] != a1.shape[1]:
--> 400         raise ValueError('expected square matrix')
    401     overwrite_a = overwrite_a or _datacopied(a1, a)
    402     fdet, = get_flinalg_funcs(('det',), (a1,))

ValueError: expected square matrix
py.linalg.inv()
```

函数计算方阵的逆：

```php
In [26]: arr = np.array([[1, 2],
                [3, 4]])

In [27]: iarr = linalg.inv(arr)

In [28]: iarr
Out[28]: 
array([[-2. ,  1. ],
       [ 1.5, -0.5]])

In [29]: np.allclose(np.dot(arr, iarr), np.eye(2))
Out[29]: True
```

最后计算奇异阵的逆(它的行列式为0)将会引发(raise)LinAlgError：

```rust
In [32]: arr = np.array([[3, 2],
                [6, 4]])

In [33]: linalg.inv(arr)
---------------------------------------------------------------------------
LinAlgError                               Traceback (most recent call last)
<ipython-input-33-52c04c854a80> in <module>()
----> 1 linalg.inv(arr)

/usr/lib/python2.7/site-packages/scipy/linalg/basic.pyc in inv(a, overwrite_a)
    346             inv_a, info = getri(lu, piv, overwrite_lu=1)
    347     if info > 0:
--> 348         raise LinAlgError("singular matrix")
    349     if info < 0:
    350         raise ValueError('illegal value in %d-th argument of internal '

LinAlgError: singular matrix
```

还有更多高级运算，如奇异值分解(SVD):

```undefined
In [34]: arr = np.arange(9).reshape((3, 3)) + np.diag([1, 0, 1])
In [35]: uarr, spec, vharr = linalg.svd(arr)
```

它的结果数组谱是：

```css
In [36]: spec
Out[36]: array([ 14.88982544,   0.45294236,   0.29654967])
```

原始矩阵可以由svd的输出用np.dot点乘重新组合得到：

```php
In [37]: sarr = np.diag(spec)
In [38]: svd_mat = uarr.dot(sarr).dot(vharr)
In [39]: np.allclose(svd_mat, arr)
Out[39]: True
```

SVD在信号处理和统计中运用很广。许多其它标准分解(QR,LU,Cholesky,Schur)，还有线性系统的解也可以从scipy.linalg中获得。

### 四、快速傅里叶变换：scipy.fftpack

scipy.fftpack模块用来计算快速傅里叶变换。作为示例，一个(噪声)输入信号可能像这样：

```cpp
In [40]: time_step = 0.02
In [41]: period = 5
In [42]: time_vec = np.arange(0, 20, time_step)
In [43]: sig = np.sin(2 * np.pi / period * time_vec) + \
   ....: 0.5 * np.random.randn(time_vec.size)
```

观测者并不指导信号频率，仅仅等间隔取样信号sig。信号应该来自一个真实的函数所以傅里叶变换将是对称的。scipy.fftpack.fftfreq()函数将生成取样频率，scipy.fftpack.fft()将计算快速傅里叶变换：

因为功率结果是对称的，仅仅需要使用谱的正值部分来找出频率：

```swift
In [48]: pidxs = np.where(sample_freq > 0)
In [49]: freqs = sample_freq[pidxs]
In [50]: power = np.abs(sig_fft)[pidxs]
```

信号频率可以这样被找到：

```php
In [51]: freq = freqs[power.argmax()]
In [52]: np.allclose(freq, 1./period)
Out[52]: True
```

现在高频噪声将被从傅里叶变换信号中移除：

```cpp
In [53]: sig_fft[np.abs(sample_freq) > freq] = 0
```

得到滤波信号，可以用scipy.fftpack.ifft函数计算：

```undefined
In [54]: main_sig = fftpack.ifft(sig_fft)
```

结果可以这样可视化：

```go
In [55]: plt.figure()
Out[55]: <matplotlib.figure.Figure at 0x4a9fb50>

In [56]: plt.plot(time_vec, sig)
Out[56]: [<matplotlib.lines.Line2D at 0x4ad3790>]

In [57]: plt.plot(time_vec, main_sig, linewidth=3)
/usr/lib/python2.7/site-packages/numpy/core/numeric.py:320: ComplexWarning: Casting complex values to real discards the imaginary part
  return array(a, dtype, copy=False, order=order)
Out[57]: [<matplotlib.lines.Line2D at 0x4ad3dd0>]

In [58]: plt.xlabel('Time [s]')
Out[58]: <matplotlib.text.Text at 0x4aad050>

In [59]: plt.ylabel('Amplitude')
Out[59]: <matplotlib.text.Text at 0x4aadbd0>

In [60]: plt.show()
```

### 五、numpy.fft

Numpy也有一个FFT实现(numpy.fft)。然而，通常scipy的应该优先使用，因为它使用了更有效率的底层实现。

工作示例：找到原始周期

工作示例：高斯图像模糊

卷积：

```cpp
f_1(t) = \int dt’K(t-t’)f_0(t’)
\tilde{f_1}(\omega)=\tilde{K}(\omega)\tilde{f_0}(\omega)
```

------

练习：登月图片消噪

检查提供的图像moonlanding.png，该图像被周期噪声严重污染了。在这个练习中，我们旨在使用快速傅里叶变换清除噪声。
用plt.imread加载图像。
使用scipy.fftpack中的2-D傅里叶函数找到并绘制图像的谱线(傅里叶变换)。可视化这个谱线对你有问题吗？如果有，为什么？
这个谱包含高频和低频成分。噪声是在谱线的高频部分中，所以设置一些成分为0(使用数组切片)。
应用逆傅里叶变换来看最后的图像。
我的消除噪声实例……

------

### 六、优化和拟合：scipy.optimize

优化是找到最小值或等式的数值解的问题。

scipy.optimization子模块提供了函数最小值(标量或多维)、曲线拟合和寻找等式的根的有用算法。

```jsx
from scipy import optimize
```

#### 找到标量函数的最小值

让我们定义以下函数

```python
In [2]: def f(x):
   ...:     return x**2 + 10 * np.sin(x)

In [3]: x = np.arange(-10, 10, 0.1)

In [4]: plt.plot(x, f(x))
Out[4]: [<matplotlib.lines.Line2D at 0x3e2a4d0>]

In [5]: plt.show()
```

该函数在大约-1.3有个全局最小值,在3.8有个局部最小值。

找到这个函数最小值一般而有效的方法是从初始点使用梯度下降法。BFGS算法^1是做这个的好方法：

```css
In [6]: optimize.fmin_bfgs(f, 0)
Optimization terminated successfully.
         Current function value: -7.945823
         Iterations: 5
         Function evaluations: 24
         Gradient evaluations: 8
Out[6]: array([-1.30644003])
```

这个方法一个可能的问题在于，如果函数有局部最小值，算法会因初始点不同找到这些局部最小而不是全局最小:

```cpp
In [7]: optimize.fmin_bfgs(f, 3, disp=0)
Out[7]: array([ 3.83746663])
```

如果我们不知道全局最小值的邻近值来选定初始点，我们需要借助于耗费资源些的全局优化。为了找到全局最小点，最简单的算法是蛮力算法^2，该算法求出给定格点的每个函数值。

```cpp
In [10]: grid = (-10, 10, 0.1)
In [11]: xmin_global = optimize.brute(f, (grid,))
In [12]: xmin_global
Out[12]: array([-1.30641113])
```

对于大点的格点，scipy.optimize.brute()变得非常慢。scipy.optimize.anneal()提供了使用模拟退火的替代函数。对已知的不同类别全局优化问题存在更有效率的算法，但这已经超出scipy的范围。一些有用全局优化软件包是OpenOpt、IPOPT、PyGMO和PyEvolve。

为了找到局部最小，我们把变量限制在(0, 10)之间，使用scipy.optimize.fminbound():

```undefined
In [13]: xmin_local = optimize.fminbound(f, 0, 10)
In [14]: xmin_local
Out[14]: 3.8374671194983834
```

注意：在高级章节部分数学优化：找到函数最小值中有关于寻找函数最小值更详细的讨论。

#### 找到标量函数的根

为了寻找根，例如令f(x)=0的点，对以上的用来示例的函数f我们可以使用scipy.optimize.fsolve():

```php
In [17]: root = optimize.fsolve(f, 1)  # 我们的初始猜测是1
In [18]: root
Out[18]: array([ 0.])
```

注意仅仅一个根被找到。检查f的图像在-2.5附近有第二个根。我们可以通过调整我们的初始猜测找到这一确切值：

```cpp
In [19]: root = optimize.fsolve(f, -2.5)
In [20]: root
Out[20]: array([-2.47948183])
```

#### 曲线拟合

假设我们有从被噪声污染的f中抽样到的数据：

```dart
In [21]: xdata = np.linspace(-10, 10, num=20)
In [22]: ydata = f(xdata) + np.random.randn(xdata.size)
```

如果我们知道函数形式(当前情况是x^2 + sin(x))，但是不知道幅度。我们可以通过最小二乘拟合拟合来找到幅度。首先我们定义一个用来拟合的函数：

```python
In [23]: def f2(x, a, b):
   ....:     return a*x**2 + b*np.sin(x)
```

然后我们可以使用scipy.optimize.curve_fit()来找到a和b：

```csharp
In [24]: guess = [2, 2]
In [25]: params, params_covariance = optimize.curve_fit(f2, xdata, ydata, guess)
In [26]: params
Out[26]: array([  1.00439471,  10.04911441])
```

现在我们找到了f的最小值和根并且对它使用了曲线拟合。我们将一切放在一个单独的图像中:

注意：Scipy>=0.11中提供所有最小化和根寻找算法的统一接口scipy.optimize.minimize(),scipy.optimize.minimize_scalar()和scipy.optimize.root()。它们允许通过method关键字方便地比较不同算法。

你可以在scipy.optimize中找到用来解决多维问题的相同功能的算法。

------

练习：曲线拟合温度数据

在阿拉斯加每个月的温度上下限，从一月开始，以摄氏单位给出。
max: 17, 19, 21, 28, 33, 38, 37, 37, 31, 23, 19, 18
min: -62, -59, -56, -46, -32, -18, -9, -13, -25, -46, -52, -58
绘制这些温度限
定义函数来描述最小和最大温度。
提示：这个函数以一年为周期。
提示：包括时间偏移。
对数据使用这个函数scipy.optimize.curve_fit()
绘制结果。是否拟合合理？
如果不合理，为什么？
拟合精度的最大最小温度的时间偏移是否一样？

------

练习：2维最小化

六峰值驼背函数：

```undefined
f(x,y) = (4 - 2.1x^2 + \frac{x^4}{3})x^2 + xy + (4y^2 - 4)y^2
```

有全局和多个局部最小。找到这个函数的全局最小。

提示：

变量应该限制在-2 < x < 2 , -1 < y < 1.
使用numpy.meshgrid()和plt.imshow来可视地搜寻区域。
使用scipy.optimize.fmin_bfgs()或其它多维极小化器。
这里有多少极小值？这些点上的函数值是多少？如果初始猜测是(x, y) = (0, 0)会发生什么？

参见总结练习非线性最小二乘拟合：在点抽取地形激光雷达数据上的应用，来看另一个，更高级的例子。

------

### 七、统计和随机数： scipy.stats

scipy.stats包括统计工具和随机过程的概率过程。各个随机过程的随机数生成器可以从numpy.random中找到。

#### 直方图和概率密度函数

给定一个随机过程的观察值，它们的直方图是随机过程的pdf(概率密度函数)的估计器：

```python
In [1]: import numpy as np
In [2]: a = np.random.normal(size=1000)
In [3]: bins = np.arange(-4, 5)
In [4]: bins
Out[4]: array([-4, -3, -2, -1,  0,  1,  2,  3,  4])

In [5]: histogram = np.histogram(a, bins=bins, normed=True)[0]
In [6]: bins = 0.5*(bins[1:] + bins[:-1])
In [7]: bins
Out[7]: array([-3.5, -2.5, -1.5, -0.5,  0.5,  1.5,  2.5,  3.5])

In [8]: from scipy import stats
In [9]: b = stats.norm.pdf(bins)  # norm是正态分布
In [10]: import matplotlib.pyplot as plt
In [11]: plt.plot(bins, histogram)
Out[11]: [<matplotlib.lines.Line2D at 0x3378b10>]

In [12]: plt.plot(bins, b)
Out[12]: [<matplotlib.lines.Line2D at 0x3378fd0>]

In [13]: plt.show()
```

如果我们知道随机过程属于给定的随机过程族，比如正态过程。我们可以对观测值进行最大似然拟合来估计基本分布参数。这里我们对观测值拟合一个正态过程：

```cpp
In [14]: loc, std = stats.norm.fit(a)
In [15]: loc
Out[15]: 0.0052651057415999758

In [16]: std
Out[16]: 0.97945439802779732
```

------

练习：概率分布

从参数为1的伽马分布生成1000个随机数,然后绘制这些样点的直方图。你能够在其上绘制pdf吗(应该匹配)？

另外：这些分布有些有用的方法。通过阅读它们的文档字符串或使用IPython的tab补全来探索它们。你能够通过对你的随机变量使用拟合找到形状参数1吗？

------

#### 百分位

中位数是来观测值之下一半之上一半的值。

```css
In [3]: np.median(a)
Out[3]: -0.047679175711778043
```

它也被叫作50百分位点，因为有50%的观测值在它之下：

```css
In [6]: stats.scoreatpercentile(a, 50)
Out[6]: -0.047679175711778043
```

同样我们可以计算百分之九十百分点：

```css
In [7]: stats.scoreatpercentile(a, 90)
Out[7]: 1.2541592439997036
```

百分位是CDF的一个估计器(累积分布函数)。

#### 统计检测

统计检测是决策指示。例如，我们有两个样本集，我们假设它们由高斯过程生成。我们可以使用T检验来决定是否两个样本值显著不同：

```cpp
In [8]: a = np.random.normal(0, 1, size=100)
In [9]: b = np.random.normal(1, 1, size=10)
In [10]: stats.ttest_ind(a, b)
Out[10]: (array(-2.4119199601156796), 0.01755485116571583)
```

输出结果由以下部分组成：

- T统计量：它是这么一种标志，与不同两个随机过程之间成比例并且幅度和差异的显著程度有关^3。
- p值：两个过程相同的概率。如果接近1,这两个过程是几乎完全相同的。越靠近零，两个过程越可能有不同的均值。

### 八、插值：scipy.interpolate

scipy.interpolate对从实验数据拟合函数来求值没有测量值存在的点非常有用。这个模块基于来自netlib项目的FITPACK Fortran 子程序。

通过想象接近正弦函数的实验数据：

```cpp
In [1]: measured_time = np.linspace(0, 1, 10)
In [2]: noise = (np.random.random(10)*2 - 1) * 1e-1
In [3]: measures = np.sin(2 * np.pi * measured_time) + noise
```

scipy.interpolate.interp1d类会构建线性插值函数：

```jsx
In [4]: from scipy.interpolate import interp1d
In [5]: linear_interp = interp1d(measured_time, measures)
```

然后scipy.interpolate.linear_interp实例需要被用来求得感兴趣时间点的值：

```undefined
In [6]: computed_time = np.linspace(0, 1, 50)
In [7]: linear_results = linear_interp(computed_time)
```

三次插值也能通过提供可选关键字参数kind来选择：[^4]

```bash
In [8]: cubic_interp = interp1d(measured_time, measures, kind='cubic')
In [9]: cubic_results = cubic_interp(computed_time)
```

结果现在被集合在以下Matplotlib图像中

scipy.interpolate.interp2d与scipy.interpolate.interp1d相似，但是面向二维数组。注意，对interp族，计算时间必须在测量时间范围内。参见Maximum wind speed prediction at the Sprogø station的总结练习获得更高级的插值示例。

### 九、数值积分：scipy.integrate Fusy,

最通用的积分程序是scipy.integrate.quad():

```python
In [10]: from scipy.integrate import quad
In [11]: res, err = quad(np.sin, 0, np.pi/2)
In [12]: np.allclose(res, 1)
Out[12]: True

In [13]: np.allclose(err, 1 - res)
Out[13]: True
```

其它可用的积分方案有fixed_quad, quadrature, romberg。

scipy.integrate也是用来积分常微分方程(ODE)的功能程序。特别是，scipy.integrate.odeint()是个使用LSODA(Livermore Solver for Ordinary Differential equations with Automatic method switching for stiff and non-stiff problems)通用积分器。参见ODEPACK Fortran library获得更多细节。

odeint解决这种形式的一阶ODE系统：

```undefined
dy/dt = rhs(y1, y2, .., t0,...)
```

作为简介，让我们解决ODEdy/dt = -2y,区间t = 0..4,初始条件y(t=0) = 1。首先函数计算导数的位置需要被定义：

```python
In [17]: def calc_derivative(ypos, time, counter_arr):
   ....:     counter_arr += 1
   ....:     return -2 * ypos                                               
   ....: 
```

一个额外的参数counter_arr被添加，用来说明函数可能在单个时间步中被多次调用，直到解收敛。计数数组被定义成：

In [18]: counter = np.zeros((1,), dtype=np.uint16)
弹道将被计算：

```jsx
In [19]: from scipy.integrate import odeint                                 
In [20]: time_vec = np.linspace(0, 4, 40)                                   
In [21]: yvec, info = odeint(calc_derivative, 1, time_vec,
   ....: args=(counter,), full_output=Tru)
```

因此导函数可以被调用40次(即时间步长数)，

```cpp
In [22]: counter
Out[22]: array([129], dtype=uint16)
```

十个最初的时间点(time step)每个的累积迭代次数，可以这样获得：

```go
In [23]: info['nfe'][:10]
Out[23]: array([31, 35, 43, 49, 53, 57, 59, 63, 65, 69], dtype=int32)
```

注意到在第一个时间步的解需要更多的迭代。解yvec的轨道现在可以被画出：

另一个使用scipy.integrate.odeint()的例子是一个阻尼弹簧-质点振荡器(二阶振荡)。附加在弹簧上质点的位置服从二阶常微分方程y'' + eps wo y' + wo^2 y= 0。其中wo^2 = k/m,k是弹簧常数，m是质量，eps=c/(2 m wo)，c是阻尼系数。(译者：为什么不用latex……)对于这个例子，我们选择如下参数：

```bash
In [24]: mass = 0.5  # kg
In [25]: kspring = 4  # N/m
In [26]: cviscous = 0.4  # N s/m
```

所以系统将是阻尼振荡，因为:

```cpp
In [27]: eps = cviscous / (2 * mass * np.sqrt(kspring/mass))
In [28]: eps < 1
Out[28]: True
```

对于scipy,integrate.odeint()求解器，二阶方程需要被转化成一个包含向量Y =y,y'的两个一阶方程的系统。定义nu = 2 eps * wo = c / m和om = wo^2 = k/m很方便：

```undefined
In [29]: nu_coef = cviscous /mass
In [30]: om_coef = kspring / mass
```

因此函数将计算速度和加速度通过：

```python
In [31]: def calc_deri(yvec, time, nuc, omc):
   ....:     return (yvec[1], -nuc * yvec[1] - omc * yvec[0])
   ....: 

In [32]: time_vec = np.linspace(0, 10, 100)

In [33]: yarr = odeint(calc_deri, (1, 0), time_vec, args=(nu_coef, om_coef))
```

最终的位置和速度在如下Matplotlib图像中显示

Scipy中不存在偏微分方程(PDE)求解器,一些解决PDE问题的Python软件包可以得到，像fipy和SfePy

(译者注:Python科学计算中洛伦兹吸引子微分方程的求解

### 十、信号处理：scipy.signal

```jsx
In [34]: from scipy import signal
```

scipy.signal.detrend()：移除信号的线性趋势：

```dart
In [35]: t = np.linspace(0, 5, 100)
In [36]: x = t + np.random.normal(size=100)
In [39]: import pylab as pl
In [40]: pl.plot(t, x, linewidth=3)
Out[40]: [<matplotlib.lines.Line2D at 0x3903c90>]

In [41]: pl.plot(t, signal.detrend(x), linewidth=3)
Out[41]: [<matplotlib.lines.Line2D at 0x3b38810>]
detrend
```

scipy.signal.resample():使用FFT重采样n个点。

```xml
In [42]: t = np.linspace(0, 5, 100)
In [43]: x = np.sin(t)
In [44]: pl.plot(t, x, linewidth=3)
Out[44]: [<matplotlib.lines.Line2D at 0x3f08a90>]

In [45]: pl.plot(t[::2], signal.resample(x, 50), 'ko')
Out[45]: [<matplotlib.lines.Line2D at 0x3f12950>]
resample
```

- Signal中有许多窗函数：scipy.signal.hamming(), scipy.signal.bartlett(), scipy.signal.blackman()…
- Signal中有滤波器(中值滤波scipy.signal.medfilt(), 维纳滤波scipy.signal.wiener())，但是我们将在图像部分讨论。

### 十一、图像处理：scipy.ndimage

scipy中致力于图像处理的子模块是scipy,ndimage。

```jsx
In [49]: from scipy import ndimage
```

图像处理程序可以根据它们执行的操作类别来分类。

#### 图像的几何变换:

改变方向，分辨率等

```python
In [50]: from scipy import misc
In [51]: lena = misc.lena()
In [52]: shifted_lena = ndimage.shift(lena, (50, 50))
In [53]: shifted_lena2 = ndimage.shift(lena, (50, 50), mode='nearest')
In [54]: rotated_lena = ndimage.rotate(lena, 30)
In [55]: cropped_lena = lena[50:-50, 50:-50]
In [56]: zoomed_lena = ndimage.zoom(lena, 2)
In [57]: zoomed_lena.shape
Out[57]: (1024, 1024)

In [63]: pl.subplot(321)
Out[63]: <matplotlib.axes.AxesSubplot at 0x4c00d90>

In [64]: pl.imshow(lena, cmap=cm.gray)
Out[64]: <matplotlib.image.AxesImage at 0x493aad0>

In [65]: pl.subplot(322)
Out[65]: <matplotlib.axes.AxesSubplot at 0x4941a10>

In [66]: #等
```

#### 图像滤镜

```jsx
In [76]: from scipy import misc
In [77]: lena = misc.lena()
In [78]: import numpy as np
In [79]: noisy_lena = np.copy(lena).astype(np.float)
In [80]: noisy_lena += lena.std()*0.5*np.random.standard_normal(lena.shape)
In [81]: blurred_lena = ndimage.gaussian_filter(noisy_lena, sigma=3)
In [82]: median_lena = ndimage.median_filter(blurred_lena, size=5)
In [83]: from scipy import signal
In [84]: wiener_lena = signal.wiener(blurred_lena, (5,5))
```

许多其它scipy.ndimage.filters和scipy.signal中的滤镜可以被应用到图像中。

------

练习: 比较不同滤镜图像的直方图

------

#### 数学形态学

数学形态学是源于几何论的数学形态学。它具有结合结构的特点并变换几何结构。二值图(黑白图)，特别能被用该理论转换：要转换的集合是邻近的非零值像素。这个理论也被拓展到灰度图中。

基本的数学形态操作使用一个结构元素(structuring element)来改变其它几何结构。

让我们首先生成一个结构元素：

```php
In [129]: el = ndimage.generate_binary_structure(2, 1)
In [130]: el
Out[130]: 
array([[False,  True, False],
       [ True,  True,  True],
       [False,  True, False]], dtype=bool)

In [131]: el.astype(np.int)
Out[131]: 
array([[0, 1, 0],
       [1, 1, 1],
       [0, 1, 0]])
```

- 腐蚀

```csharp
In [132]: a = np.zeros((7,7), dtype=int)
In [133]: a[1:6, 2:5] = 1
In [134]: a
Out[134]: 
array([[0, 0, 0, 0, 0, 0, 0],
       [0, 0, 1, 1, 1, 0, 0],
       [0, 0, 1, 1, 1, 0, 0],
       [0, 0, 1, 1, 1, 0, 0],
       [0, 0, 1, 1, 1, 0, 0],
       [0, 0, 1, 1, 1, 0, 0],
       [0, 0, 0, 0, 0, 0, 0]])

In [135]: ndimage.binary_erosion(a).astype(a.dtype)
Out[135]: 
array([[0, 0, 0, 0, 0, 0, 0],
       [0, 0, 0, 0, 0, 0, 0],
       [0, 0, 0, 1, 0, 0, 0],
       [0, 0, 0, 1, 0, 0, 0],
       [0, 0, 0, 1, 0, 0, 0],
       [0, 0, 0, 0, 0, 0, 0],
       [0, 0, 0, 0, 0, 0, 0]])


# 腐蚀移除对象使结构更小
In [136]: ndimage.binary_erosion(a, structure=np.ones((5,5))).astype(a.dtype)
Out[136]: 
array([[0, 0, 0, 0, 0, 0, 0],
       [0, 0, 0, 0, 0, 0, 0],
       [0, 0, 0, 0, 0, 0, 0],
       [0, 0, 0, 0, 0, 0, 0],
       [0, 0, 0, 0, 0, 0, 0],
       [0, 0, 0, 0, 0, 0, 0],
       [0, 0, 0, 0, 0, 0, 0]])
```

- 膨胀

```csharp
In [137]: a = np.zeros((5,5))
In [138]: a[2, 2] = 1
In [139]: a
Out[139]: 
array([[ 0.,  0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.,  0.],
       [ 0.,  0.,  1.,  0.,  0.],
       [ 0.,  0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.,  0.]])

In [140]: ndimage.binary_dilation(a).astype(a.dtype)
Out[140]: 
array([[ 0.,  0.,  0.,  0.,  0.],
       [ 0.,  0.,  1.,  0.,  0.],
       [ 0.,  1.,  1.,  1.,  0.],
       [ 0.,  0.,  1.,  0.,  0.],
       [ 0.,  0.,  0.,  0.,  0.]])
```

- 开操作(opening)

```csharp
In [141]: a = np.zeros((5,5), dtype=np.int)

In [142]: a[1:4, 1:4] = 1; a[4, 4] = 1

In [143]: a
Out[143]: 
array([[0, 0, 0, 0, 0],
       [0, 1, 1, 1, 0],
       [0, 1, 1, 1, 0],
       [0, 1, 1, 1, 0],
       [0, 0, 0, 0, 1]])

# 开操作可以移除小的对象
In [145]: ndimage.binary_opening(a, structure=np.ones((3,3))).astype(np.int)Out[145]: 
array([[0, 0, 0, 0, 0],
       [0, 1, 1, 1, 0],
       [0, 1, 1, 1, 0],
       [0, 1, 1, 1, 0],
       [0, 0, 0, 0, 0]])

# 开操作也能平滑边角
In [147]: ndimage.binary_opening(a).astype(np.int)
Out[147]: 
array([[0, 0, 0, 0, 0],
       [0, 0, 1, 0, 0],
       [0, 1, 1, 1, 0],
       [0, 0, 1, 0, 0],
       [0, 0, 0, 0, 0]])
```

- 闭操作(closing): ndimage.binary_closing

------

练习: 查看开操作腐蚀，然后膨胀的量。

一个开操作移除小的结构，而一个闭操作填补小的空洞。这种操作因此可被用来“清理”图像。

```ruby
In [149]: a = np.zeros((50, 50))
In [150]: a[10:-10, 10:-10] = 1
In [151]: a += 0.25*np.random.standard_normal(a.shape)
In [152]: mask = a>=0.5
In [153]: opened_mask = ndimage.binary_opening(mask)
In [154]: closed_mask = ndimage.binary_closing(opened_mask)
```

------

练习: 验证重构区域比初始区域更小。(如果闭操作在开操作之前则相反)

对灰度值图像，腐蚀(或者是膨胀)相当于用被集中在所关心像素点的结构元素所覆盖像素的最小(或最大)值替代当前像素点。

```csharp
In [173]: a = np.zeros((7,7), dtype=np.int)
In [174]: a[1:6, 1:6] = 3
In [175]: a[4,4] = 2; a[2,3] = 1
In [176]: a
Out[176]: 
array([[0, 0, 0, 0, 0, 0, 0],
       [0, 3, 3, 3, 3, 3, 0],
       [0, 3, 3, 1, 3, 3, 0],
       [0, 3, 3, 3, 3, 3, 0],
       [0, 3, 3, 3, 2, 3, 0],
       [0, 3, 3, 3, 3, 3, 0],
       [0, 0, 0, 0, 0, 0, 0]])

In [177]: ndimage.grey_erosion(a, size=(3,3))
Out[177]: 
array([[0, 0, 0, 0, 0, 0, 0],
       [0, 0, 0, 0, 0, 0, 0],
       [0, 0, 1, 1, 1, 0, 0],
       [0, 0, 1, 1, 1, 0, 0],
       [0, 0, 3, 2, 2, 0, 0],
       [0, 0, 0, 0, 0, 0, 0],
       [0, 0, 0, 0, 0, 0, 0]])
```

#### 图像测量

让我们首先生成一个漂亮的合成图像：

```cpp
In [178]: x, y = np.indices((100, 100))
In [179]: sig = np.sin(2*np.pi*x/50.)*np.sin(2*np.pi*y/50.)*(1+x*y/50.**2)**2

In [180]: mask = sig > 1 

#现在我们查找图像中对象的各种信息：
In [181]: labels, nb = ndimage.label(mask)
In [182]: nb
Out[182]: 8

In [183]: areas = ndimage.sum(mask, labels, xrange(1, labels.max()+1))
In [184]: areas
Out[184]: array([ 190.,   45.,  424.,  278.,  459.,  190.,  549.,  424.])

In [185]: maxima = ndimage.maximum(sig, labels, xrange(1, labels.max()+1))
In [186]: maxima
Out[186]: 
array([  1.80238238,   1.13527605,   5.51954079,   2.49611818,
         6.71673619,   1.80238238,  16.76547217,   5.51954079])

In [187]: ndimage.find_objects(labels==4)
Out[187]: [(slice(30L, 48L, None), slice(30L, 48L, None))]

In [188]: sl = ndimage.find_objects(labels==4)
In [189]: import pylab as pl
In [190]: pl.imshow(sig[sl[0]])  
Out[190]: <matplotlib.image.AxesImage at 0xb2fdcd0>
```

参见总结练习Image processing application: counting bubbles and unmolten grains获取更多高级示例。

------

总结练习

总结练习主要使用Numpy，Scipy和Matplotlib。它们提供一些现实生活中用Python计算的示例。既然基本的Numpy和scipy使用已经被介绍了，欢迎有兴趣的用户尝试这些练习。

练习：

斯普罗站最大风速预测

非线性最小二乘拟合：地形雷达数据的点提取

图像处理应用：计数气泡和未融颗粒

建议的解：

图像处理练习解的示例:玻璃中的未融颗粒