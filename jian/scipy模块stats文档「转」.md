# scipy模块stats文档「转」

[![img](https://upload.jianshu.io/users/upload_avatars/9652337/c1a8a9e1-e898-4d22-9f60-789f999d2a32.jpeg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/7fd357f8e382)

[程序猪小羊](https://www.jianshu.com/u/7fd357f8e382)关注

0.3872018.01.31 07:13:23字数 3,156阅读 558

## [scipy模块stats文档](https://link.jianshu.com/?t=http%3A%2F%2Fwww.cnblogs.com%2Fttrrpp%2Fp%2F6822214.html)

[https://github.com/yiyuezhuo/scipy.stats-doc-ch](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fyiyuezhuo%2Fscipy.stats-doc-ch)

[https://docs.scipy.org/doc/scipy/reference/tutorial/stats.html](https://link.jianshu.com/?t=https%3A%2F%2Fdocs.scipy.org%2Fdoc%2Fscipy%2Freference%2Ftutorial%2Fstats.html)

[http://blog.csdn.net/pipisorry/article/details/49515215](https://link.jianshu.com/?t=http%3A%2F%2Fblog.csdn.net%2Fpipisorry%2Farticle%2Fdetails%2F49515215)

## 介绍

在这个教程我们讨论一部分scipy.stats模块的特性。这里我们的意图是提供给使用者一个关于这个包的实用性知识。我们推荐reference manual来介绍更多的细节。

注意：这个文档还在发展中。

## 随机变量

有一些通用的概率分布类被封装在continuous random variables以及 discrete random variables中。有80多个连续性随机变量(RVs)以及10余个离散随机变量已经用 这些类建立。同样，新的程序和分布可以被用户新建（如果你构造了一个，请提供它给我们帮助发展 这个包）。

所有统计函数被放在子包scipy.stats中，且有这些函数的一个几乎完整的列表可以使用 info(stats)获得。这个列表里的随机变量也可以从stats子包的docstring中获得介绍。

在接下来的讨论中，我们着重于连续性随机变量(RVs)。几乎所有离散变量也符合下面的讨论， 尽管我们将“离散分布的特殊之处”指出它们的一些区别。

下面的示例代码我们假设`scipy.stats`包已被下述方式导入。

```source-python
>>> from scipy import stats
```

有些例子假设对象被这样的方式导入（不用输完整路径）了。

```source-python
>>> from scipy.stats import norm
```

### 获得帮助

所有分布可以使用help函数得到解释。为获得这些信息只需要使用像这样的简单调用：

```source-python
>>> print norm.__doc__
```

作为例子，我们用这种方式获取分布的上下界

```source-python
>>> print 'bounds of distribution lower: %s, upper: %s' % (norm.a,norm.b)
bounds of distribution lower: -inf, upper: inf
```

我们可以通过调用`dir(norm)`来获得关于这个（正态）分布的所有方法和属性。应该看到， 一些方法是私有方法尽管其并没有以名称表示出来（比如它们前面没有以下划线开头）， 比如`veccdf`就只用于内部计算（试图使用那些方法将引发警告，因为它们可能会在后续开发中被移除）

为了获得*真正*的主要方法，我们列举冻结分布的方法（我们将在下文解释何谓*冻结*）

```source-python
>>> rv = norm()
>>> dir(rv)  # reformatted
    ['__class__', '__delattr__', '__dict__', '__doc__', '__getattribute__',
    '__hash__', '__init__', '__module__', '__new__', '__reduce__', '__reduce_ex__',
    '__repr__', '__setattr__', '__str__', '__weakref__', 'args', 'cdf', 'dist',
    'entropy', 'isf', 'kwds', 'moment', 'pdf', 'pmf', 'ppf', 'rvs', 'sf', 'stats']
```

最后，我们能通过内省获得所有的可用分布的信息。

```source-python
>>> import warnings
>>> warnings.simplefilter('ignore', DeprecationWarning)
>>> dist_continu = [d for d in dir(stats) if
...                 isinstance(getattr(stats,d), stats.rv_continuous)]
>>> dist_discrete = [d for d in dir(stats) if
...                  isinstance(getattr(stats,d), stats.rv_discrete)]
>>> print 'number of continuous distributions:', len(dist_continu)
number of continuous distributions: 84
>>> print 'number of discrete distributions:  ', len(dist_discrete)
number of discrete distributions:   12
```

### 通用方法

连续随机变量的主要公共方法如下：

- rvs:随机变量（就是从这个分布中抽一些样本）
- pdf：概率密度函数。
- cdf：累计分布函数
- sf：残存函数（1-CDF）
- ppf：分位点函数（CDF的逆）
- isf：逆残存函数（sf的逆）
- stats:返回均值，方差，（费舍尔）偏态，（费舍尔）峰度。
- moment:分布的非中心矩。

让我们使用一个标准正态(normal)随机变量(RV)作为例子。

```source-python
>>> norm.cdf(0)
0.5
```

为了计算在一个点上的cdf，我们可以传递一个列表或一个numpy数组。

```source-python
>>> norm.cdf([-1., 0, 1])
array([ 0.15865525,  0.5       ,  0.84134475])
>>> import numpy as np
>>> norm.cdf(np.array([-1., 0, 1]))
array([ 0.15865525,  0.5       ,  0.84134475])
```

相应的，像pdf,cdf之类的简单方法可以用`np.vectorize`进行矢量化.

一些其他的实用通用方法:

```source-python
>>> norm.mean(), norm.std(), norm.var()
(0.0, 1.0, 1.0)
>>> norm.stats(moments = "mv")
(array(0.0), array(1.0))
```

为了找到一个分布的中中位数，我们可以使用分位数函数ppf，它是cdf的逆。

```source-python
>>> norm.ppf(0.5)
0.0
```

为了产生一个随机变量列,使用`size`关键字参数。

```source-python
>>> norm.rvs(size=5)
array([-0.35687759,  1.34347647, -0.11710531, -1.00725181, -0.51275702])
```

不要认为norm.rvs(5)产生了五个变量。

```source-python
>>> norm.rvs(5)
7.131624370075814
```

欲知其意，请看下一部分的内容。

### 偏移(Shifting)与缩放(Scaling)

所有连续分布可以操纵loc以及scale参数调整分布的location和scale属性。作为例子， 标准正态分布的location是均值而scale是标准差。

```source-python
>>> norm.stats(loc = 3, scale = 4, moments = "mv")
(array(3.0), array(16.0))
```

通常经标准化的分布的随机变量X可以通过变换(X-loc)/scale获得。它们的默认值是loc=0以及scale=1.

聪明的使用loc与scale可以帮助以灵活的方式调整标准分布达到所想要的效果。 为了进一步说明缩放的效果，下面给出期望为1/λ指数分布的cdf。

F(x)=1−exp(−λx)

通过像上面那样使用scale，可以看到如何得到目标期望值。

```source-python
>>> from scipy.stats import expon
>>> expon.mean(scale=3.)
3.0
```

均匀分布也是令人感兴趣的：

```source-python
>>> from scipy.stats import uniform
>>> uniform.cdf([0, 1, 2, 3, 4, 5], loc = 1, scale = 4)
array([ 0\.  ,  0\.  ,  0.25,  0.5 ,  0.75,  1\.  ])
```

最后，联系起我们在前面段落中留下的norm.rvs(5)的问题。事实上，像这样调用一个分布， 其第一个参数，像之前的5，是把loc参数调到了5，让我们看：

```source-python
>>> np.mean(norm.rvs(5, size=500))
4.983550784784704
```

在这里，为解释最后一段的输出：norm.rvs(5)产生了一个正态分布变量，其期望，即loc=5.

我倾向于明确的使用loc,scale作为关键字而非像上面那样依赖参数的顺序。 因为这看上去有点令人困惑。我们在我们解释“冻结RV”的主题之前澄清这一点。

### 形态(shape)参数

虽然一般连续随机变量都可以通过赋予loc和scale参数进行偏移和缩放，但一些分布还需要 额外的形态参数确定其形态。作为例子，看到这个伽马分布，这是它的密度函数

γ(x,a)=λ(λx)a−1Γ(a)e−λx,

它要求一个形态参数a。注意到λ的设置可以通过设置scale关键字为1/λ进行。

让我们检查伽马分布的形态参数的名字的数量。（我们从上面知道其应该为1）

```source-python
>>> from scipy.stats import gamma
>>> gamma.numargs
1
>>> gamma.shapes
'a'
```

现在我们设置形态变量的值为1以变成指数分布。所以我们可以容易的比较是否得到了我们所期望的结果。

```source-python
>>>  gamma(1, scale=2.).stats(moments="mv")
(array(2.0), array(4.0))
```

注意我们也可以以关键字的方式指定形态参数：

```source-python
>>> gamma(a=1, scale=2.).stats(moments="mv")
(array(2.0), array(4.0))
```

### 冻结分布

不断地传递loc与scale关键字最终会让人厌烦。而冻结RV的概念被用来解决这个问题。

```source-python
>>> rv = gamma(1, scale=2.)
```

通过使用rv，在任何情况下我们不再需要包含scale与形态参数。显然，分布可以被多种方式使用， 我们可以通过传递所有分布参数给对方法的每次调用（像我们之前做的那样）或者可以对一个分 布对象先冻结参数。让我们看看是怎么回事：

```source-python
>>> rv.mean(), rv.std()
(2.0, 2.0)
```

这正是我们应该得到的。

### 广播

像pdf这样的简单方法满足numpy的广播规则。作为例子，我们可以计算t分布的右尾分布的临界值 对于不同的概率值以及自由度。

```source-python
>>> stats.t.isf([0.1, 0.05, 0.01], [[10], [11]])
array([[ 1.37218364,  1.81246112,  2.76376946],
       [ 1.36343032,  1.79588482,  2.71807918]])
```

这里，第一行是以10自由度的临界值，而第二行是以11为自由度的临界值。所以， 广播规则与下面调用了两次isf产生的结果相同。

```source-python
>>> stats.t.isf([0.1, 0.05, 0.01], 10)
array([ 1.37218364,  1.81246112,  2.76376946])
>>> stats.t.isf([0.1, 0.05, 0.01], 11)
array([ 1.36343032,  1.79588482,  2.71807918])
```

但是如果概率数组，如[0.1,0.05,0.01]与自由度数组,如[10,11,12]具有相同的数组形态， 则进行对应匹配，我们可以分别得到10%，5%，1%尾的临界值对于10，11,12的自由度。

```source-python
>>> stats.t.isf([0.1, 0.05, 0.01], [10, 11, 12])
array([ 1.37218364,  1.79588482,  2.68099799])
```

### 离散分布的特殊之处

离散分布的方法的大多数与连续分布很类似。当然像pdf被更换为密度函数pmf，没有估计方法， 像fit就不能用了。而scale不是一个合法的关键字参数。Location参数， 关键字loc则仍然可以使用用于位移。

cdf的计算要求一些额外的关注。在连续分布的情况下，累积分布函数在大多数标准情况下是严格递增的， 所以有唯一的逆。而cdf在离散分布却一般是阶跃函数，所以cdf的逆，分位点函数，要求一个不同的定义：

ppf(q) = min{x : cdf(x) >= q, x integer}

为了更多信息可以看这里。

我们可以看这个超几何分布的例子

```source-python
>>> from scipy.stats import hypergeom
>>> [M, n, N] = [20, 7, 12]
```

如果我们在一些整数点使用cdf，则它们的cdf值再作用ppf会回到开始的值。

```source-python
>>> x = np.arange(4)*2
>>> x
array([0, 2, 4, 6])
>>> prb = hypergeom.cdf(x, M, n, N)
>>> prb
array([ 0.0001031991744066,  0.0521155830753351,  0.6083591331269301,
        0.9897832817337386])
>>> hypergeom.ppf(prb, M, n, N)
array([ 0.,  2.,  4.,  6.])
```

如果我们使用的值不是cdf的函数值，则我们得到一个更高的值。

```source-python
>>> hypergeom.ppf(prb + 1e-8, M, n, N)
array([ 1.,  3.,  5.,  7.])
>>> hypergeom.ppf(prb - 1e-8, M, n, N)
array([ 0.,  2.,  4.,  6.])
```

### 分布拟合

非冻结分布的参数估计的主要方法：

- `fit`：分布参数的极大似然估计，包括location与scale
- `fit_loc_scale`: 给定形态参数确定下的location和scale参数的估计
- `nnlf`:负对数似然函数
- `expect`:计算函数pdf或pmf的期望值。

### 性能问题与注意事项

分布方法的性能与运行速度根据分布的不同表现差异极大。方法的结果可以由两种方式获得， 精确计算或使用独立于各具体分布的通用算法。

精确计算一般更快。为了进行精确计算，要么直接使用解析公式，要么使用`scipy.special`中的 函数，对于`rvs`还可以使用`numpy.random`里的函数。

另一方面，如果不能进行精确计算，将使用通用方法进行计算。于是为了定义一个分布， 只有pdf异或cdf是必须的；通用方法使用数值积分和求根法进行求解。作为例子， rgh = stats.gausshyper.rvs(0.5, 2, 2, 2, size=100)以这种方式创建了100个随机变量 （抽了100个值），这在我的电脑上花了19秒（译者：我花了3.5秒）， 对比取一百万个标准正态分布的值只需要1秒。

### 遗留问题

scipy.stats里的分布最近进行了升级并且被仔细的检查过了，不过仍有一些问题存在。

- 分布在很多参数区间上的值被测试过了，然而在一些奇葩的临界条件，仍然可能有错误的值存在。
- fit的极大似然估计以默认值作为初始参数将不会工作的很好，用户必须指派合适的初始参数。 并且，对于一些分布使用极大似然估计本身就不是一个好的选择。

## 构造具体的分布

下一个例子展示了如何建立你自己的分布。更多的例子见分布用法以及统计检验

### 创建一个连续分布，继承`rv_continuous`类

创建连续分布是非常简单的.

```source-python
>>> from scipy import stats
>>> class deterministic_gen(stats.rv_continuous):
...     def _cdf(self, x):
...         return np.where(x < 0, 0., 1.)
...     def _stats(self):
...         return 0., 0., 0., 0.

>>> deterministic = deterministic_gen(name="deterministic")
>>> deterministic.cdf(np.arange(-3, 3, 0.5))
array([ 0.,  0.,  0.,  0.,  0.,  0.,  1.,  1.,  1.,  1.,  1.,  1.])
```

令人高兴的是，pdf也能被自动计算出来：

```source-python
>>>
>>> deterministic.pdf(np.arange(-3, 3, 0.5))
array([  0.00000000e+00,   0.00000000e+00,   0.00000000e+00,
         0.00000000e+00,   0.00000000e+00,   0.00000000e+00,
         5.83333333e+04,   4.16333634e-12,   4.16333634e-12,
         4.16333634e-12,   4.16333634e-12,   4.16333634e-12])
```

注意这种用法的性能问题，参见“性能问题与注意事项”一节。这种缺乏信息的通用计算可能非常慢。 而且再看看下面这个准确性的例子：

```source-python
>>> from scipy.integrate import quad
>>> quad(deterministic.pdf, -1e-1, 1e-1)
(4.163336342344337e-13, 0.0)
```

但这并不是对pdf积分的正确的结果，实际上结果应为1.让我们将积分变得更小一些。

```source-python
>>> quad(deterministic.pdf, -1e-3, 1e-3)  # warning removed
(1.000076872229173, 0.0010625571718182458)
```

这样看上去好多了，然而，问题本身来源于pdf不是来自包给定的类的定义。

### 继承`rv_discrete`类

在之后我们使用stats.rv_discrete产生一个离散分布，其有一个整数区间截断概率。

#### 通用信息

通用信息可以从 rv_discrete的 docstring中得到。

```source-python
>>> from scipy.stats import rv_discrete
>>> help(rv_discrete)
```

从中我们得知：

> “你可以构建任意一个像P(X=xk)=pk一样形式的离散rv，通过传递(xk,pk)元组序列给 rv_discrete初始化方法(通过values=keyword方式)，但其不能有0概率值。”

接下来，还有一些进一步的要求：

- keyword必须给出。
- Xk必须是整数
- 小数的有效位数应当被给出。

事实上，如果最后两个要求没有被满足，一个异常将被抛出或者导致一个错误的数值。

#### 一个例子

让我们开始办，首先

```source-python
>>> npoints = 20   # number of integer support points of the distribution minus 1
>>> npointsh = npoints / 2
>>> npointsf = float(npoints)
>>> nbound = 4   # bounds for the truncated normal
>>> normbound = (1+1/npointsf) * nbound   # actual bounds of truncated normal
>>> grid = np.arange(-npointsh, npointsh+2, 1)   # integer grid
>>> gridlimitsnorm = (grid-0.5) / npointsh * nbound   # bin limits for the truncnorm
>>> gridlimits = grid - 0.5   # used later in the analysis
>>> grid = grid[:-1]
>>> probs = np.diff(stats.truncnorm.cdf(gridlimitsnorm, -normbound, normbound))
>>> gridint = grid
```

然后我们可以继承rv_discrete类

```source-python
>>> normdiscrete = stats.rv_discrete(values=(gridint,
...              np.round(probs, decimals=7)), name='normdiscrete')
```

现在我们已经定义了这个分布，我们可以调用其所有常规的离散分布方法。

```source-python
>>> print 'mean = %6.4f, variance = %6.4f, skew = %6.4f, kurtosis = %6.4f'% \
...       normdiscrete.stats(moments =  'mvsk')
mean = -0.0000, variance = 6.3302, skew = 0.0000, kurtosis = -0.0076

>>> nd_std = np.sqrt(normdiscrete.stats(moments='v'))
```

#### 测试上面的结果

让我们产生一个随机样本并且比较连续概率的情况。

```source-python
>>> n_sample = 500
>>> np.random.seed(87655678)   # fix the seed for replicability
>>> rvs = normdiscrete.rvs(size=n_sample)
>>> rvsnd = rvs
>>> f, l = np.histogram(rvs, bins=gridlimits)
>>> sfreq = np.vstack([gridint, f, probs*n_sample]).T
>>> print sfreq
[[ -1.00000000e+01   0.00000000e+00   2.95019349e-02]
 [ -9.00000000e+00   0.00000000e+00   1.32294142e-01]
 [ -8.00000000e+00   0.00000000e+00   5.06497902e-01]
 [ -7.00000000e+00   2.00000000e+00   1.65568919e+00]
 [ -6.00000000e+00   1.00000000e+00   4.62125309e+00]
 [ -5.00000000e+00   9.00000000e+00   1.10137298e+01]
 [ -4.00000000e+00   2.60000000e+01   2.24137683e+01]
 [ -3.00000000e+00   3.70000000e+01   3.89503370e+01]
 [ -2.00000000e+00   5.10000000e+01   5.78004747e+01]
 [ -1.00000000e+00   7.10000000e+01   7.32455414e+01]
 [  0.00000000e+00   7.40000000e+01   7.92618251e+01]
 [  1.00000000e+00   8.90000000e+01   7.32455414e+01]
 [  2.00000000e+00   5.50000000e+01   5.78004747e+01]
 [  3.00000000e+00   5.00000000e+01   3.89503370e+01]
 [  4.00000000e+00   1.70000000e+01   2.24137683e+01]
 [  5.00000000e+00   1.10000000e+01   1.10137298e+01]
 [  6.00000000e+00   4.00000000e+00   4.62125309e+00]
 [  7.00000000e+00   3.00000000e+00   1.65568919e+00]
 [  8.00000000e+00   0.00000000e+00   5.06497902e-01]
 [  9.00000000e+00   0.00000000e+00   1.32294142e-01]
 [  1.00000000e+01   0.00000000e+00   2.95019349e-02]]
```





6人点赞



[LA & STAT](https://www.jianshu.com/nb/21795457)