# python 数据可视化工具包 matplotlib

> matplotlib 是一个 python 的 2D 绘图库。大量的学术期刊，书籍出版物使用它来绘制专业的数据可视化图表。matplotlib 支持跨平台，可运行在 python 脚本，python 解释器, IPython，Jupyter notebook, web 应用服务器，以及四个 GUI（Graphical User Interface） 工具包中。使用matplotlib ，只需要几行代码，便可绘制折线图，直方图，功率谱（power spectra）, 条形图，误差图，散点图等。

## 1. 安装

```
pip install matplotlib
复制代码
```

## 2. 导入

ps: 在 jupyter notebook 环境需要添加 `%matplotlib inline` ，使得绘图生成在 notebook 页面。其他环境需要去掉 `%matplotlib inline`。

```
import matplotlib.pyplot as plt
%matplotlib inline
复制代码
```

## 3. 基本概念

matplotlib 中定义了四个基本概念：figure, axes, axis, artist。准确理解这四个基本概念，能更好的理解和熟练使用 matplotlib 绘制你想要的图表。

为了更直观的理解这四个概念，没有比直接使用 matplotlib 将这些抽象的概念绘制出来更好的方式了。

```
import numpy as np

figure, axes = plt.subplots(1, 2, figsize=(8, 4))
figure.suptitle("figure")

x = np.linspace(0, 2, 100)

axes1, axes2 = axes

axes1.plot(x, x, label='artist legend')
axes2.plot(x, x**2, label='artist legend')

axes1.set_title("axes 1")
axes2.set_title("axes 2")

axes1.set_xlabel("axis x of axes 1")
axes2.set_xlabel("axis x of axes 2")

axes1.set_ylabel("axis y of axes 1")
axes2.set_ylabel("axis y of axes 2")

axes1.legend()
axes2.legend()

axes1.grid()
axes2.grid()

plt.show()
复制代码
```



![img](https://user-gold-cdn.xitu.io/2019/9/8/16d10339a0115364?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



通过观察绘制出来的图表，我们可以很直观的看到，各个“抽象概念”的在图表中对应的具体表示，以及它们之间的层次关系。

有了这些可视化呈现，再来逐一理解它们，就很容易了。

### 3.1 figure

从图中可以看出，figure 表示整个绘图区域，它更多扮演的就是一个容器的角色，所有其他绘图元素都必须要包含在 figure 中，通过 figsize 参数限定 figure 大小，同时也限定了绘图区域的大小。此外，通过使用 `figure.suptitle(title)` 方法，也可以为整个绘图区域，设置一个标题，但这不是必需的（事实上，如果不给 figure 设置标题，我们这里就无法直观感知到它作为容器的存在）。

由此可见，要使用 matplotlib 绘制图表或任何可视化呈现，就必须先**显式**或**隐式**（直接使用 plt.plot 等函数绘制时）创建一个 figure 对象。

下面，简单介绍两种最常用的创建 figure 的方法。根据个人习惯，可任选一种方式即可。个人更多使用第二种方式。

#### 3.1.1 plt.figure(num=None, figsize=None, dpi=None, facecolor=None, edgecolor=None, frameon=True, clear=False)

`plt.figure()` 是任何创建 figure 对象的底层工厂函数。它提供创建 figure 了所有细节。

| 参数          | 描述                                                         |
| :------------ | :----------------------------------------------------------- |
| **num**       | 整数或字符串。figure 编号，即 figure 对象的 number 属性，可以理解为 figure 对象的 ID；如果编号为 num 的 figure 在上下文中已经存在，则直接返回存在 figure 引用，而不创建新的 figure; 默认为 None , 会创建一个新的 figure 对象，并将 number 加1 。 |
| **figsize**   | 元组。figure 的宽高，单位 英尺。默认值为 `matplotlib.rcParams["figure.figsize"]` 。 |
| **dpi**       | 整数。分辨率。默认值为 `matplotlib.rcParams["figure.dpi"]` 。 |
| **facecolor** | 字符串或颜色对象。背景颜色。默认为白色。                     |
| **edgecolor** | 字符串或颜色对象。边框颜色。默认为白色。                     |
| **frameon**   | Bool。                                                       |
| **clear**     | Bool。为 True 时，清空当前已经存在的 figure 的绘制。默认不清空。 |

代码示例，理解 num 的作用：

```
figure1 = plt.figure()
print("this is a new figure instance, whose number is {}.\n".format(figure1.number))
figure2 = plt.figure()
print("this is a new figure instance, whose number is {}.\n".format(figure2.number))

figure1_ref = plt.figure(num=1) # 等效于 plt.figure(1)
print("this is a existed figure instance, whose number is {}.\n".format(figure1_ref.number))
if figure1_ref == figure1:
    print("figure1_ref is equal to figure1.")

plt.show()
复制代码
```

可以看到，上面脚本运行后，并没有显示可见的图表。正如上文所说，figure 仅仅是个容器，真正我们所想到的包含在 figure 容器中，带有坐标系的一个个图表，是下文将要介绍的 `axes` 。使用 `plt.figure()` 方法创建的 figure 要显示图表，必须调用 `plt.plot()` 等绘制函数。代码如下：

```
# figure 1
plt.figure(figsize=(9, 3))
plt.suptitle("figure 1")

names = ['a', 'b', 'c']
values = [1, 10, 100]

plt.subplot(131)
plt.bar(names, values)

plt.subplot(132)
plt.scatter(names, values)

plt.subplot(133)
plt.plot(names, values)

plt.show()

# figure 2
plt.figure(figsize=(9, 3))
plt.suptitle("figure 2")

names = ['d', 'e', 'f']
values = [100, 10, 1]

plt.subplot(131)
plt.bar(names, values)

plt.subplot(132)
plt.scatter(names, values)

plt.subplot(133)
plt.plot(names, values)

plt.show()
复制代码
```



![img](https://user-gold-cdn.xitu.io/2019/9/8/16d10339a024de4b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2019/9/8/16d10339a9ab01c5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



#### 3.1.2 plt.subplots(nrows=1, ncols=1, sharex=False, sharey=False, figsize=None)

plt.subplots() 是 plt.figure() 的上层封装，提供了一次创建 figure 和 axes 便捷方式。事实上，除了 nrows，ncols，sharex，sharey 等新增的参数，任何 plt.figure() 支持的参数，都可以通过 plt.subplots() 传递, 例如 figsize 参数。

plt.subplots() 它返回一个元组。元组的第一个元素为 figure 对象；第二个元素是用于绘制图表的 axes 对象，需要注意的是，通过参数 nrows，ncols 创建不只一个 axes 时，元组的第二个元素是一个 axes 对象的 numpy 数组。

可见，不同于 plt.figure() ，plt.subplots() 在创建 figure 对象时，同时也创建了一个或多个 axes 对象。因此，创建后就可以看到图表。

如下代码，创建了包含 2 行 3 列，共 6 个 axes 的 figure 对象。

```
figure, axes = plt.subplots(2, 3, figsize=(9, 6))
figure.suptitle("2 rows 3 cols")
plt.show()

print("type of return tuple[1] is {}".format(type(axes)))
复制代码
```



![img](https://user-gold-cdn.xitu.io/2019/9/8/16d10339af070fc9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



如果省略 `nrows` 和 `ncols`, 将只创建一个 axes。如下：

```
figure, axes = plt.subplots()
figure.suptitle("only one axes")
plt.show()

print("type of return tuple[1] is {}".format(type(axes)))
复制代码
```



![img](https://user-gold-cdn.xitu.io/2019/9/8/16d10339ab8b01cc?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 3.2 axes

> "This is what you think of as 'a plot'".

经过上面的论述，相信理解上面这句话，简直 "So easy!"。没错，axes 正是我们绘制各种绚丽图表的真正主角！

上文已经提到，使用 plt.subplots() 方法创建图表，会返回所包含的 axes 对象。这些 axes 对象就是 matplotlib 提供绘制图表的面向对象接口。使用 axes 对象的各种绘制方法，可以在图表中对应 axes 坐标空间，绘制任何图表，文字和图片。

#### 3.2.1 plot([x], y, [fmt])

以 x 为横轴，y 为纵轴，fmt 为格式字符串（定义颜色，标记，线型等），绘制线图或标记。并且可以定义多个 x, y, fmt 组合，一次绘制多个不同图形。

以下提供一些绘制示例。

**在同一个 axes 中绘制不同图形** ：

```
figure, axes = plt.subplots()
t = np.arange(0., 5., 0.2)

axes.plot(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')

plt.show()
复制代码
```



![img](https://user-gold-cdn.xitu.io/2019/9/8/16d10339abad70b4?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



**在一个 figure 中的两个 axes 绘制不同图形，并且共享 x 轴(axis)：**

```
def f(t):
    return np.exp(-t) * np.cos(2*np.pi*t)

t1 = np.arange(0.0, 5.0, 0.1)
t2 = np.arange(0.0, 5.0, 0.02)

figure, axes = plt.subplots(2, 1, sharex=True)
ax1, ax2 = axes

ax1.plot(t1, f(t1), 'bo', t2, f(t2), 'k')
ax2.plot(t2, np.cos(2*np.pi*t2), 'r--')

plt.show()
复制代码
```



![img](https://user-gold-cdn.xitu.io/2019/9/8/16d10339d26a2b65?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



#### 3.2.2 散点图: scatter(x, y, s=None, c=None, alpha=None)

x 横轴；y 纵轴；s 点的大小，默认是 `rcParams['lines.markersize']` 的平方；c 颜色；alpha 透明度。

```
np.random.seed(19680801)

N = 50
x = np.random.rand(N)
y = np.random.rand(N)
colors = np.random.rand(N)
area = (30 * np.random.rand(N))**2 

plt.scatter(x, y, s=area, c=colors, alpha=0.5)

plt.show()
复制代码
```



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="432" height="288"></svg>)



#### 3.2.3 条形图: bar(x, height, width=0.8)

x 横坐标位置; height 条形高度; width 条形宽度。

```
plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号

labels = ['G1', 'G2', 'G3', 'G4', 'G5']
men_means = [20, 34, 30, 35, 27]
women_means = [25, 32, 34, 20, 25]

x = np.arange(len(labels))  # 标签位置
width = 0.35  # 条形宽度

fig, ax = plt.subplots()
# 绘制条形图
rects1 = ax.bar(x - width/2, men_means, width, label='男')
rects2 = ax.bar(x + width/2, women_means, width, label='女')

ax.set_ylabel('得分')
ax.set_title('分小组性别得分')
ax.set_xticks(x)
ax.set_xticklabels(labels)
ax.legend()


def autolabel(rects):
    """在每个条形上添加文本，显示条形的高度"""
    for rect in rects:
        height = rect.get_height()
        ax.annotate('{}'.format(height),
                    xy=(rect.get_x() + rect.get_width() / 2, height),
                    xytext=(0, 3),  # 增加三个点的垂直偏移
                    textcoords="offset points",
                    ha='center', va='bottom')


autolabel(rects1)
autolabel(rects2)

fig.tight_layout()

plt.show()
复制代码
```



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="432" height="288"></svg>)



#### 3.2.4 水平条形图: barh(y, width, height=0.8)

y 纵坐标位置; width 条形宽度; height 条形高度。

```
category_names = ['Strongly disagree', 'Disagree',
                  'Neither agree nor disagree', 'Agree', 'Strongly agree']
results = {
    'Question 1': [10, 15, 17, 32, 26],
    'Question 2': [26, 22, 29, 10, 13],
    'Question 3': [35, 37, 7, 2, 19],
    'Question 4': [32, 11, 9, 15, 33],
    'Question 5': [21, 29, 5, 5, 40],
    'Question 6': [8, 19, 5, 30, 38]
}


def survey(results, category_names):
    """
    Parameters
    ----------
    results : dict
        A mapping from question labels to a list of answers per category.
        It is assumed all lists contain the same number of entries and that
        it matches the length of *category_names*.
    category_names : list of str
        The category labels.
    """
    labels = list(results.keys())
    data = np.array(list(results.values()))
    data_cum = data.cumsum(axis=1)
    category_colors = plt.get_cmap('RdYlGn')(
        np.linspace(0.15, 0.85, data.shape[1]))

    fig, ax = plt.subplots(figsize=(9.2, 5))
    ax.invert_yaxis()
    ax.xaxis.set_visible(False)
    ax.set_xlim(0, np.sum(data, axis=1).max())

    for i, (colname, color) in enumerate(zip(category_names, category_colors)):
        widths = data[:, i]
        starts = data_cum[:, i] - widths
        ax.barh(labels, widths, left=starts, height=0.5,
                label=colname, color=color)
        xcenters = starts + widths / 2

        r, g, b, _ = color
        text_color = 'white' if r * g * b < 0.5 else 'darkgrey'
        for y, (x, c) in enumerate(zip(xcenters, widths)):
            ax.text(x, y, str(int(c)), ha='center', va='center',
                    color=text_color)
    ax.legend(ncol=len(category_names), bbox_to_anchor=(0, 1),
              loc='lower left', fontsize='small')

    return fig, ax


survey(results, category_names)
plt.show()
复制代码
```



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="662" height="360"></svg>)



#### 3.2.5 饼图: pie(data, explode=None, labels=None, colors=None, autopct=None..)

```
fig, ax = plt.subplots(figsize=(8, 4), subplot_kw=dict(aspect="equal"))

recipe = ["375 g flour",
          "75 g sugar",
          "250 g butter",
          "300 g berries"]

data = [float(x.split()[0]) for x in recipe]
ingredients = [x.split()[-1] for x in recipe]


def func(pct, allvals):
    absolute = int(pct/100.*np.sum(allvals))
    return "{:.1f}%\n({:d} g)".format(pct, absolute)


wedges, texts, autotexts = ax.pie(data, autopct=lambda pct: func(pct, data),
                                  textprops=dict(color="w"))

ax.legend(wedges, ingredients,
          title="Ingredients",
          loc="center left",
          bbox_to_anchor=(1, 0, 0.5, 1))

plt.setp(autotexts, size=8, weight="bold")

ax.set_title("Matplotlib bakery: A pie")

plt.show()
复制代码
```



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="576" height="288"></svg>)



#### 3.2.6 直方图 hist(data, bins=None, range=None, density=None, weights=None...)

```
np.random.seed(19680801)

# 样本数据
mu = 100  # 均值
sigma = 15  # 标准差
x = mu + sigma * np.random.randn(437)

num_bins = 50

fig, ax = plt.subplots()

# 绘制直方图
n, bins, patches = ax.hist(x, num_bins, density=1)

# 最佳拟合线
y = ((1 / (np.sqrt(2 * np.pi) * sigma)) *
     np.exp(-0.5 * (1 / sigma * (bins - mu))**2))

ax.plot(bins, y, "--")
ax.set_xlabel("智商")
ax.set_ylabel("概率密度")
ax.set_title("IQ直方图: 平均值=100, 标准差=15")

# 调整间距以防止 ylabel 被遮挡
fig.tight_layout()
plt.show()
复制代码
```



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="432" height="288"></svg>)



#### 3.2.7 2维直方图: hist2d(x, y, bins=10, range=None, density=False, weights=None, ...)

```
import matplotlib.colors as mcolors
from numpy.random import multivariate_normal

data = np.vstack([
    multivariate_normal([10, 10], [[3, 2], [2, 3]], size=100000),
    multivariate_normal([30, 20], [[2, 3], [1, 3]], size=1000)
])

gammas = [0.8, 0.5, 0.3]

fig, axes = plt.subplots(nrows=2, ncols=2, figsize=(8, 6))

axes[0, 0].set_title('Linear normalization')
axes[0, 0].hist2d(data[:, 0], data[:, 1], bins=100)

for ax, gamma in zip(axes.flat[1:], gammas):
    ax.set_title(r'Power law $(\gamma=%1.1f)$' % gamma)
    ax.hist2d(data[:, 0], data[:, 1],
              bins=100, norm=mcolors.PowerNorm(gamma))

fig.tight_layout()

plt.show()
复制代码
```



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="576" height="432"></svg>)



#### 3.2.8 显示图片: imshow(X)

```
with open('dog.jpeg', 'rb') as image_file:
    image = plt.imread(image_file)

fig, ax = plt.subplots()
ax.imshow(image)
ax.set_title("Hi, 我们又见面了！")

ax.axis('off')
plt.show()
复制代码
```



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="432" height="288"></svg>)



### 3.3 axis

虽然 axis 和 axes 中文翻译都是轴的意思，但是，axes 的意思更像是由 axis 构成的坐标空间，正如上文介绍的，我们可以在这个坐标空间绘制各种图像和显示图片；如果说 axes 是抽象意义的“轴”，那么 axis 就是真正意义的坐标轴；2维绘图中，只有两个 axis 即 横轴 x 和纵轴 y。由于 axis 是构成 axes 一部分，因此我们可以使用 axes 对象的方法，来设置 x 轴和 y 轴的属性。

```
figure, axes = plt.subplots(1, 2, figsize=(6, 3))

ax1, ax2 = axes

# 设置坐标显示区间
xmin, xmax, ymin, ymax = 0, 10, 50, 100
ax1.axis([xmin, xmax, ymin, ymax])

# 隐藏 axis
ax2.set_axis_off()
ax2.set_title("axis off.")

plt.show()
复制代码
```



![img](https://user-gold-cdn.xitu.io/2019/9/8/16d1033a15863e56?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 3.4 artist

artist 中文翻译是艺术家，在 matplotlib 中，它具有更一般的意义，即所有可见的元素都是 artist。如: title, legend, grid, axis, tick, label, text等等。

```
np.random.seed(19680801)
data = np.random.randn(2, 100)

fig, axs = plt.subplots(2, 2, figsize=(5, 5))
axs[0, 0].hist(data[0])
axs[1, 0].scatter(data[0], data[1])
axs[0, 1].plot(data[0], data[1])
axs[1, 1].hist2d(data[0], data[1])

plt.show()
```