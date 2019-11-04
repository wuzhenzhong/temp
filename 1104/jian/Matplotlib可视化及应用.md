# Matplotlib可视化及应用



[aoqingy](https://www.jianshu.com/u/b4ddfc1d5704)关注

0.4282018.07.25 19:29:46字数 8,713阅读 1,379

# [一、概述](https://www.jianshu.com/p/db2e28201835)

深度学习的一个重要手段是训练数据和训练过程的可视化，因此，我们关于深度学习的系列介绍文章就从Matplotlib开始。

Matplotlib是一个在Python下实现的类MatLab的第三方库，是Python下最出色的绘图库，功能很完善，同时也继承了Python简单明了的风格，它可以很方便地设计和输出二维以及三维的数据，提供了常规的笛卡尔坐标、极坐标、求坐标、三维坐标等，可以画普通图、散点图、三维图、等高线图等。

Matplotlib有各种应用，本文的例子主要集中在深度学习方面。如果您希望在股票走势、报表分析或其他方面使用Matplotlib，其中使用的绘图元素会有稍有不同。

在学习Matplotlib过程中，笔者也感觉到，Matplotlib有很多值得改进的地方。例如，如果Matplotlib能画出相对一条线段特定角度的线段，或者能够将一条线段延长特定的长度，等等，那么它在平面几何的求解方面一定会有更大的作用。Matplotlib是一个完全开源的项目，实现这些功能是有可能的，当然这有赖于有志之士贡献自己的精力和时间。

# [二、一个简单的例子](https://www.jianshu.com/p/db2e28201835)

我们先从一个简单的例子开始。需要说明的是，为了让更多的用户能够实验本文档中的示例代码，我们使用Windows下的交互式Python工具Juypter Notebook作为代码运行环境。Juypter Notebook的安装过程可参见其他文档，假设您已经成功安装了该软件，运行Anocoda3软件包中的Juypter Notebook可以看到你如下的界面。



![img](https://upload-images.jianshu.io/upload_images/13250940-fcf5ad5cb6891283.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

首先，在文本框中输入以下命令，并点击Run按钮。

%matpplotlib inline

%matplotlib命令可以在当前的Jupyter

Notebook环境中启用绘图。这个命令提供一个可选参数，指定使用哪个matplotlib后端。一般情况下，我们都使用inline后台，这将在Jupyter Notebook输出中直接呈现绘图。需要了解，这条命令并不是程序代码的一部分。当在Ubuntu的Python环境下运行时，就必须将这一行去掉。

接下来，是实际的程序代码：

import numpy as np

import matplotlib.pyplot as plt

[x = np.arange(-np.pi, np.pi, np.pi/100)](https://www.jianshu.com/p/db2e28201835)

y = np.sin(x)

plt.plot(x, y)

plt.show()

以上程序的输出入下图所示。



![img](https://upload-images.jianshu.io/upload_images/13250940-1bc83c5533b40153.png?imageMogr2/auto-orient/strip|imageView2/2/w/390/format/webp)

语句import numpy as np导入Python的科学计算包。Matplotlib 充分利用了Python下的Numeric(Numarray)模块，提供了一种利用Python进行数据可视化的解决方案，进一步加强了Python用来进行科学计算的能力。

语句import matplotlib.pyplot as plt导入matplotlib包中的pyplot模块。

语句x = np.arange(-np.pi, np.pi, np.pi/100)在从-π到π，以π/100为步进，生成一个数组。换句话说，在[-π,π)区间内，按平均间隔取100个值，得到一个数组。

语句y

= np.sin(x)对x数组计算正弦函数值，得到是对应长度的数组。

语句plt.plot(x,

y)调用pyplot模块的plot函数，传入x和y数组。这个函数会用直线将x和y数组中对应元素的点用直线连接起来，最终得到一条正弦曲线。

最后，调用plt.show()函数将这条正弦曲线呈现出来。

我们看到，在上面的例子中，我们通过4条有效语句，就绘制了一条正弦曲线。实际上，matplotlib和python一样简洁明了，只要我们熟悉了其思想，就可以绘制出符合我们需要的各种图形。

在编写代码过程中，如果需要帮助，您可以访问：

[if !supportLists]²       [endif]画廊：Matplotlib的Gallery页面（https://matplotlib.org/gallery.html）中有上百幅缩略图，打开之后都有源程序。如果您需要绘制某种类型的图，可以在画廊中搜索对应的例子，大多数情况下，通过复制/粘贴基本上就能搞定。

[if !supportLists]²       [endif]API：如果要了解模块中某个函数的使用方法，可以访问Matplotlib的API文档（https://matplotlib.org/api/index.html）。

[if !supportLists]²       [endif]帮助：如果要了解模块中某个函数的使用方法，也可以在Jupyter Notebook环境下使用 help 命令，如下图所示。



![img](https://upload-images.jianshu.io/upload_images/13250940-a01f11b343d52373.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

# [三、Matplotlib](https://www.jianshu.com/p/db2e28201835)组成

[要理解Matplotlib](https://www.jianshu.com/p/db2e28201835)绘图，可以用我们日常生活中的绘画来进行类比。一般来说，我们绘画的过程是这样的：首先架上画板，然后在画板上铺一张画纸。有时候我们也会在不同位置铺上几张画纸，每张画纸是各自独立的参考系。我们在画纸上画各种类型的图画，比如水墨画、油画、工笔画、写意画、素描、漫画等。除此以外，一副完整的画，还需要加上题跋。书籍、碑帖、字画等前面的文字叫做题，写在后面的文字，叫做跋，总称题跋。

Matplotlib中专业术语大致对应如下：



![img](https://upload-images.jianshu.io/upload_images/13250940-5077b2011f514fda.png?imageMogr2/auto-orient/strip|imageView2/2/w/548/format/webp)

[if !supportLists]²       [endif]图形（Figure）：类似于上面提到的画板。可以认为是Matplotlib绘图的基础。

[if !supportLists]²       [endif]轴图（Axes）或子图（Subplot）：类似于上面的画纸。画板上有一张或多张画纸，实际绘图是在轴图或子图上完成的。

[if !supportLists]²       [endif]X轴（X Axis）和Y轴（Y Axis）：绘图所依据的坐标系统，相对于上面的参考系。只不过对于专业的画家，参考系是在自己的头脑中，并不一定要标在图纸上面。而在Matplotlib是，X轴和Y轴一般都需要呈现出来，包括其标签（Label）、轴线（Spine）、刻度（Tick）以及刻度标签等。

[if !supportLists]²       [endif]标题（Title）和图例（Legend）：类似于上面的题跋，具体来说：标题对应题，图例对应跋。和绘画一样，Matplotlib中的标题和图例也可以放在轴图或子图上的不同位置。

[if !supportLists]²       [endif]数据（Data）：无疑，Matplotlib中的数据相对于绘画中主要部分：内容。Matplotlib支持的绘图类型包括：普通图（Plot）、散点图（Scatter）和三维图（3D）等。对于绘图过程的每一笔，还可以指定使用的颜色（Color）、样式（Style）以及记号（Marker）等。

[if !supportLists]²       [endif]文本（Text）/注释（Annotation）：这在日常绘画中并不常见，但我们也可以把它归入绘画中的主要内容部分。Matplotlib使用文本和注释对重要数据进行说明。

上面的术语也可以参考下面的图。



![img](https://upload-images.jianshu.io/upload_images/13250940-6cbf527ee0673184.png?imageMogr2/auto-orient/strip|imageView2/2/w/508/format/webp)

上图显示的是只有一个轴图的情况。Matplotlib也支持在一个图形中绘制几个轴图。这个时候，就需要通过调用特定的函数将轴图添加进来。每个轴图是一个拥有自己独立坐标系统的绘图区域，它可以在图像的任意位置。



![img](https://upload-images.jianshu.io/upload_images/13250940-c9020994c0095833.png?imageMogr2/auto-orient/strip|imageView2/2/w/408/format/webp)

当然，轴图也可以一定规律来放置，这时候，我们称其为子图（Subplot）。轴图和子图是相互关联的：轴图是灵活的子图，而子图是按格栅方式组织，通过行与列来定位的特殊轴图。下图就是按2行2列排列方式显示深度学习中经常使用到的一些激励函数。



![img](https://upload-images.jianshu.io/upload_images/13250940-71bbc8c052cf36d1.png?imageMogr2/auto-orient/strip|imageView2/2/w/487/format/webp)

# [四、坐标系统](https://www.jianshu.com/p/db2e28201835)

[为了说明坐标系统，我们对第一个例子进行补充，这次我们将加上标题、刻度、刻度标签、图例等，并调整坐标系的位置。我们希望输出的效果图如下所示。](https://www.jianshu.com/p/db2e28201835)



![img](https://upload-images.jianshu.io/upload_images/13250940-d7683a40ad5569eb.png?imageMogr2/auto-orient/strip|imageView2/2/w/359/format/webp)

因为要显示中文标题，所以首先在代码中添加如下语句。

plt.rcParams['font.sans-serif']=['SimHei']

接下来三条语句生成数据，具体的解释可以参考前面的例子。

x  = np.arange(-np.pi, np.pi, np.pi/100)

y1  = np.sin(x)

y2  = np.cos(x)

坐标系统是相对于轴域而言的。如前所述，Matplotlib使用Figure和Axes进行绘图，前者类似于画板，后者类似于画纸。如果我们没有调用语句添加Figure和Axes，系统将使用默认的画板和画纸。调用gcf()和gca()函数将分别取得系统的默认画板和画纸，然后就可以调用画板和画纸的函数进行操作。

ax.set_title('正弦函数和余弦函数')为Matplotlib绘图添加标题。一般来说，标题会显示在图像的上方中心位置。您也可以传入loc=”left”或loc=”right”参数，将标题设置在上方左边位置或右边位置。当然，也可以在其他位置。

Matplotlib轴域的坐标系默认有left、bottom、right、top四根轴线，要将坐标系显示在图形中央，需要隐藏其中的两条，并移动另外两条。例如：将left轴和bottom轴的位置设置在中央，将right轴和top轴的颜色设为none达到隐藏效果。此外，对于left轴和bottom轴，调用set_smart_bounds方法。

ax.spines['left'].set_position('center')

ax.spines['right'].set_color('none')

ax.spines['bottom'].set_position('center')

ax.spines['top'].set_color('none')

ax.spines['left'].set_smart_bounds(True)

ax.spines['bottom'].set_smart_bounds(True)

接下来三条语句设置X轴的范围和刻度。范围设置在[-π,π)区间，并在-π、-π/2、0、π/2和π等位置表示对应的刻度标签。

Matplotlib轴域的坐标系默认有left、bottom、right、top四根轴线，要将坐标系显示在图形中央，需要隐藏其中的两条，并移动另外两条。例如：将left轴和bottom轴的位置设置在中央，将right轴和top轴的颜色设为none达到隐藏效果。此外，对于left轴和bottom轴，调用set_smart_bounds方法。

ax.set_xlim(-np.pi,  np.pi)

ax.set_xticks([-np.pi,  -np.pi/2, 0, np.pi/2, np.pi])

ax.set_xticklabels(['-$\pi$',  '-$\pi$/2', '0', '$\pi$/2', '$\pi$'])

对于Y轴，我们想呈现主刻度线和辅刻度线的效果。

def  major_tick(y, pos):

​    if not y in [-1.0, -0.5, 0, 0.5, 1.0]:

​        return ""

​    return y



ax.yaxis.set_major_locator(MultipleLocator(0.500))

ax.yaxis.set_major_formatter(FuncFormatter(major_tick))

ax.yaxis.set_minor_locator(AutoMinorLocator(2))

在坐标系统都设置好之后，调用plot函数分别画正弦和余弦两条曲线。系统会默认为两条曲线使用两种不同的颜色进行呈现。这次，我们传入label参数为两条曲线加上标签。

ax.plot(x,  y1, label="sin()")

ax.plot(x,  y2, label="cos()")

之后，调用legend添加图例并显示，它会将上述两条曲线的颜色和对应的标签进行说明。输出图纸，图例显示在左上角位置，其他情况下可能会显示在不同的位置。您也可以通过参数指定图例显示的位置。

# [五、数据属性](https://www.jianshu.com/p/db2e28201835)

这里所说的属性，是指Matplotlib基本绘图元素的属性，包括颜色、线型、记号等。用一个例子可以很清楚地了解。



![img](https://upload-images.jianshu.io/upload_images/13250940-1331b4fbbcf7fd9c.png?imageMogr2/auto-orient/strip|imageView2/2/w/467/format/webp)

分析一下这个例子的实现。

首先准备数据，

t =  np.arange(0.0, 1.0, 0.1)

s =  np.sin(2*np.pi*t)

然后定义线性和记号，

linestyles  = ['_', '-', '--', ':']

markers  = []

for m  in Line2D.markers:

​    try:

​        if len(m) == 1 and m != ' ':

​            markers.append(m)

​    except TypeError:

​        pass



styles  = markers + [

​    r'$\lambda$',

​    r'$\bowtie$',

​    r'$\circlearrowleft$',

​    r'$\clubsuit$',

​    r'$\checkmark$']

然后定义线性和记号，

linestyles  = ['_', '-', '--', ':']

markers  = []

for m  in Line2D.markers:

​    try:

​        if len(m) == 1 and m != ' ':

​            markers.append(m)

​    except TypeError:

​        pass



styles  = markers + [

​    r'$\lambda$',

​    r'$\bowtie$',

​    r'$\circlearrowleft$',

​    r'$\clubsuit$',

​    r'$\checkmark$']

然后定义颜色。

colors  = ('b', 'g', 'r', 'c', 'm', 'y', 'k')

程序的核心部分是一个双层循环。外层对行进行循环，内层对一行的列进行循环。对于某一行的某一列，我们为其添加对应的子图，并计算出对应的颜色、线型和记号，然后进行绘制，在绘制时，调用对应的函数把左边系刻度标签去掉了。

axisNum  = 0

for row  in range(6):

​    for col in range(5):

​        axisNum += 1

​        ax = plt.subplot(6, 5, axisNum)

​        color = colors[axisNum % len(colors)]

​        if axisNum < len(linestyles):

​            plt.plot(t, s,  linestyles[axisNum], color=color, markersize=10)

​        else:

​            style = styles[(axisNum -  len(linestyles)) % len(styles)]

​            plt.plot(t, s, linestyle='None',  marker=style, color=color, markersize=10)

​        ax.set_yticklabels([])

​        ax.set_xticklabels([])

## [5.1 颜色](https://www.jianshu.com/p/db2e28201835)

在上面的例子中，我们用到'b', 'g', 'r', 'c', 'm', 'y', 'k'等几种颜色。实际上，我们可以通过名字引用更多的颜色。这些颜色定义在colors模块的BASE_COLORS和mcolors.CSS4_COLORS中。下面是按色彩和饱和度进行排序，以4列方式呈现的颜色名称和效果图，我们把它放到这里方便查询。



![img](https://upload-images.jianshu.io/upload_images/13250940-07421b289ca317ef.png?imageMogr2/auto-orient/strip|imageView2/2/w/616/format/webp)

上例也是通过一个Matplotlib程序输出的，这个程序可以在画廊中搜索到。

## [5.2 线型](https://www.jianshu.com/p/db2e28201835)

前面的例子中，给出了四种线性：'_', '-', '--', ':'，效果如下图。



![img](https://upload-images.jianshu.io/upload_images/13250940-63d22c6731ee6c49.png?imageMogr2/auto-orient/strip|imageView2/2/w/424/format/webp)

如果您希望用到更多的线型，可以将linestyle参数指定为(, ())形式，如下表所示。

'solid'(0, ())

'loosely dotted' (0, (1, 10))

'dotted' (0, (1, 5))

'densely dotted' (0, (1, 1))

'loosely dashed' (0, (5, 10))

'dashed' (0, (5, 5))

'densely dashed' (0, (5, 1))

'loosely dashdotted' (0, (3, 10, 1, 10))

'dashdotted'(0, (3, 5, 1, 5))

'densely dashdotted' (0, (3, 1, 1, 1))

'loosely dashdotdotted'(0, (3, 10, 1, 10, 1, 10))

'dashdotdotted' (0, (3, 5, 1, 5, 1, 5))

'densely dashdotdotted' (0, (3, 1, 1, 1, 1, 1)))

这些形式对应的效果图如下。



![img](https://upload-images.jianshu.io/upload_images/13250940-7528e76379be5d5e.png?imageMogr2/auto-orient/strip|imageView2/2/w/733/format/webp)

## [5.3 记号](https://www.jianshu.com/p/db2e28201835)

前面例子中的记号从Line2D.markers数组中进行遍历。具体使用时，我们可以将marker参数指定为以下类型之一：'.',',','o','v','^','<','>','1','2','3','4','8','s','p','P','*',

'h','H','+','x','X','D','d','|','_'，对应效果如下图。



![img](https://upload-images.jianshu.io/upload_images/13250940-7de30dc93f0c99ab.png?imageMogr2/auto-orient/strip|imageView2/2/w/618/format/webp)

# [六、注释和文本](https://www.jianshu.com/p/db2e28201835)

接下来给出一个注释的例子。



![img](https://upload-images.jianshu.io/upload_images/13250940-4e9139d5ba2f826f.png?imageMogr2/auto-orient/strip|imageView2/2/w/486/format/webp)



![img](https://upload-images.jianshu.io/upload_images/13250940-099e3e29a181b5ab.png?imageMogr2/auto-orient/strip|imageView2/2/w/374/format/webp)

这个例子要在一个程序里画两个图。其实现方式也很直接。

首先创建第一个figure，并添加一个子图。

fig  = plt.figure(1, figsize=(8, 5))

ax  = fig.add_subplot(111, autoscale_on=False, xlim=(-1, 5), ylim=(-4, 3))

创建第二个Figure和子图的逻辑相似，只不过是，这里需要调用figure的clf方法将前面画板上的内容清空。

fig  = plt.figure(2)

fig.clf()

ax  = fig.add_subplot(111, autoscale_on=False, xlim=(-1, 5), ylim=(-5, 3))

在第二个子图上，我们画了一个椭圆，代码如下。

el  = Ellipse((2, -1), 0.5, 0.5)

ax.add_patch(el)

当然，我们关注的还是注释部分，下面分别介绍。

第一个图的第一个注释内容是staight，所注释的点座位为(0, 1)。

ax.annotate('straight',  xy=(0, 1), xycoords='data', xytext=(-50, 30), textcoords='offset points', arrowprops=dict(arrowstyle="->"))

第一个图的第二个注释内容包括两行内容，第一行为arc3,，第二行为rad 0.2。所注释的点座位为(0.5, -1)

ax.annotate('arc3,\nrad  0.2', xy=(0.5, -1), xycoords='data', xytext=(-80, -60), textcoords='offset  points', arrowprops=dict(arrowstyle="->", connectionstyle="arc3,rad=.2"))

第一个图的第三个注释内容也包括两行内容，第一行为arc,，第二行为angle 50。所注释的点座位为(1, 1)。

ax.annotate('arc,\nangle  50', xy=(1., 1), xycoords='data', xytext=(-90, 50), textcoords='offset  points', arrowprops=dict(arrowstyle="->",  connectionstyle="arc,angleA=0,armA=50,rad=10"))

第一个图的第四个注释。

ax.annotate('arc,\narms',  xy=(1.5, -1), xycoords='data', xytext=(-80, -60), textcoords='offset points',  arrowprops=dict(arrowstyle="->", connectionstyle="arc,angleA=0,armA=40,angleB=-90,armB=30,rad=7"))

第一个图的第五个注释。

ax.annotate('angle,\nangle  90', xy=(2., 1), xycoords='data', xytext=(-70, 30), textcoords='offset  points', arrowprops=dict(arrowstyle="->", connectionstyle="angle,angleA=0,angleB=90,rad=10"))

第一个图的第六个注释。

ax.annotate('angle3,\nangle  -90', xy=(2.5, -1), xycoords='data', xytext=(-80, -60), textcoords='offset  points', arrowprops=dict(arrowstyle="->", connectionstyle="angle3,angleA=0,angleB=-90"))

第一个图的第七个注释。

ax.annotate('angle,\nround',  xy=(3., 1), xycoords='data', xytext=(-60, 30), textcoords='offset points',  bbox=dict(boxstyle="round", fc="0.8"), arrowprops=dict(arrowstyle="->",  connectionstyle="angle,angleA=0,angleB=90,rad=10"))

第一个图的第八个注释。

ax.annotate('angle,\nround4',  xy=(3.5, -1), xycoords='data', xytext=(-70, -80), textcoords='offset points',  size=20, bbox=dict(boxstyle="round4,pad=.5", fc="0.8"), arrowprops=dict(arrowstyle="->",  connectionstyle="angle,angleA=0,angleB=-90,rad=10"))

第一个图的第九个注释。

ax.annotate('angle,\nshrink',  xy=(4., 1), xycoords='data', xytext=(-60, 30), textcoords='offset points', bbox=dict(boxstyle="round",  fc="0.8"), arrowprops=dict(arrowstyle="->", shrinkA=0,  shrinkB=10,                           

  connectionstyle="angle,angleA=0,angleB=90,rad=10"))

第一个图的第十个注释。

\#  You can pass an empty string to get only annotation arrows rendered

ann  = ax.annotate('', xy=(4., 1.), xycoords='data', xytext=(4.5, -1),  textcoords='data', arrowprops=dict(arrowstyle="<->", connectionstyle="bar",  ec="k", shrinkA=5, shrinkB=5))

第二个图的第一个注释。

ax.annotate('$->$',  xy=(2., -1), xycoords='data', xytext=(-150, -140), textcoords='offset points',  bbox=dict(boxstyle="round", fc="0.8"), arrowprops=dict(arrowstyle="->",  patchB=el,                           

  connectionstyle="angle,angleA=90,angleB=0,rad=10"))

第二个图的第二个注释。

ax.annotate('arrow\nfancy',  xy=(2., -1), xycoords='data', xytext=(-100, 60), textcoords='offset points',  size=20,

\#  bbox=dict(boxstyle="round", fc="0.8"),

arrowprops=dict(arrowstyle="fancy",  fc="0.6", ec="none", patchB=el,                           

  connectionstyle="angle3,angleA=0,angleB=-90"))

第二个图的第三个注释。

ax.annotate('arrow\nsimple',  xy=(2., -1), xycoords='data', xytext=(100, 60), textcoords='offset points',  size=20,

\#  bbox=dict(boxstyle="round", fc="0.8"),

arrowprops=dict(arrowstyle="simple",  fc="0.6", ec="none", patchB=el, connectionstyle="arc3,rad=0.3"))

第二个图的第四个注释。

ax.annotate('$->$',  xy=(2., -1), xycoords='data', xytext=(-150, -140), textcoords='offset  points', bbox=dict(boxstyle="round", fc="0.8"), arrowprops=dict(arrowstyle="->",  patchB=el,                           

  connectionstyle="angle,angleA=90,angleB=0,rad=10"))

第二个图的第五个注释。

ann  = ax.annotate('bubble,\ncontours', xy=(2., -1), xycoords='data', xytext=(0,  -70), textcoords='offset points', size=20,  bbox=dict(boxstyle="round", fc=(1.0, 0.7, 0.7), ec=(1., .5, .5)),

arrowprops=dict(arrowstyle="wedge,tail_width=1.",  fc=(1.0, 0.7, 0.7), ec=(1., .5, .5), patchA=None, patchB=el, relpos=(0.2,  0.8), connectionstyle="arc3,rad=-0.1"))

第二个图的第六个注释。

ann  = ax.annotate('bubble', xy=(2., -1), xycoords='data', xytext=(55, 0),  textcoords='offset points', size=20, va="center", bbox=dict(boxstyle="round",  fc=(1.0, 0.7, 0.7), ec="none"), arrowprops=dict(arrowstyle="wedge,tail_width=1.",  fc=(1.0, 0.7, 0.7), ec="none", patchA=None, patchB=el, relpos=(0.2,  0.5)))

在Matplotlib绘图的过程中，必须会用到一些特殊文本，例如数学符号。



![img](https://upload-images.jianshu.io/upload_images/13250940-a0005e7987a567a8.png?imageMogr2/auto-orient/strip|imageView2/2/w/491/format/webp)

第一行对应的输入为r"$W^{3\beta}_{\delta_1 \rho_1 \sigma_2} = U^{3\beta}_{\delta_1

\rho_1} + \frac{1}{8 \pi 2} \int^{\alpha_2}_{\alpha_2} d \alpha^\prime_2

\left[\frac{ U^{2\beta}_{\delta_1 \rho_1} - \alpha^\prime_2U^{1\beta}_{\rho_1

\sigma_2} }{U^{0\beta}_{\rho_1 \sigma_2}}\right]$"；

第二行对应的字符串为：r"$\alpha_i > \beta_i,\ \alpha_{i+1}^j = {\rm sin}(2\pi f_j

t_i) e^{-5 t_i/\tau},\ \ldots$",

第三行对应的字符串为：r"$\frac{3}{4},\ \binom{3}{4},\ \stackrel{3}{4},\ \left(\frac{5

\- \frac{1}{x}}{4}\right),\ \ldots$"；

第四行对应的字符串为：r"$\sqrt{2},\ \sqrt[3]{x},\ \ldots$"；

第五行对应的字符串为：r"$\mathrm{Roman}\ , \ \mathit{Italic}\ , \ \mathtt{Typewriter}

\ \mathrm{or}\ \mathcal{CALLIGRAPHY}$"；

第六行对应的字符串为：r"$\acute a,\ \bar a,\ \breve a,\ \dot a,\ \ddot a, \ \grave a,

\ \hat a,\ \tilde a,\ \vec a,\ \widehat{xyz},\ \widetilde{xyz},\ \ldots$"；

第七行对应的字符串为：r"$\alpha,\ \beta,\ \chi,\ \delta,\ \lambda,\ \mu,\ \Delta,\

\Gamma,\ \Omega,\ \Phi,\ \Pi,\ \Upsilon,\ \nabla,\ \aleph,\ \beth,\ \daleth,\

\gimel,\ \ldots$"；

第八行对应的字符串为：r"$\coprod,\ \int,\ \oint,\ \prod,\ \sum,\ \log,\ \sin,\

\approx,\ \oplus,\ \star,\ \varpropto,\ \infty,\ \partial,\ \Re,\ \leftrightsquigarrow,

\ \ldots$"。

有时候，我们需要将文本旋转一定角度呈现。只需要在调用pyplot的text方法时指定rotate参数。例如，下面的例子中，先画了一条角度为45度的直线。

\#  Plot diagonal line (45 degrees)

h  = plt.plot(np.arange(0, 10), np.arange(0, 10))

我们在第一个位置写上一条45度的文本。

\#  Locations to plot text

l1  = np.array((1, 1))



\#  Rotate angle

angle  = 45



\#  Plot text

th1  = plt.text(l1[0], l1[1], 'text not rotated correctly', fontsize=16,  rotation=angle, rotation_mode='anchor')

上面的方法只限于坐标系X轴和Y轴比例相同的情况。如果不相同，则需要进行变换。下面的代码中，我们在第二个位置写下角度变换后的文本。

\#  set limits so that it no longer looks on screen to be 45 degrees

plt.xlim([-10,  20])



\#  Locations to plot text

l2  = np.array((5, 5))



\#  Rotate angle

trans_angle  = plt.gca().[transData.transform_angles](https://www.jianshu.com/p/db2e28201835)(np.array((45,)),

​                                                   l2.reshape((1, 2)))[0]



\#  Plot text

th2  = plt.text(l2[0], l2[1], 'text rotated correctly', fontsize=16, rotation=trans_angle,  rotation_mode='anchor')

从下图可以看到，没有变换角度的文本和所画的直线不对应，而变换过角度的文本和所画的直线则完全对应。



![img](https://upload-images.jianshu.io/upload_images/13250940-6d7b25938ae96c0e.png?imageMogr2/auto-orient/strip|imageView2/2/w/366/format/webp)

# [七、轴图和子图](https://www.jianshu.com/p/db2e28201835)

轴图和子图的区别在前面已经介绍过。简而言之，子图是特殊的轴图，轴图是灵活的子图。它们分别调用add_axes和add_subplot函数向图形中添加一个轴图，两个函数都返回matplotlib.axes.Axes对象。但是，两个函数的调用机制有很大不同。

函数add_axes的调用方法是add_axes(rect)，其中rect是一个列表[x0, y0,

width, height]，标记新轴图在图形坐标系中左下角的位置(x0,y0)，以及新轴图的宽度width和高度height。因此，轴图定位在画板的绝对坐标位置。例如，以下语句在画板上添加一个和画板一样大小的轴图。

Fig =  plt.figure()

ax  = fig.add_axes([0,0,1,1])

函数add_subplot的调用方法并不直接提供放置轴图的位置选项。而是指定按照子图栅格摆放轴图的方式。最常用也是最简单的指定位置的方法是使用3个整数的记号。

fig  = plt.figure()

ax  = fig.add_subplot(231)

上述语句，一个新的轴图会在2行3列的栅格中的第一个位置创建。如果只要创建一个轴图，可以调用add_subplot(111)（在1行1列的栅格中的第一个位置创建轴图）。

这个方法的优点是让matplotlib来计算轴图的确切位置。默认情况下，add_subplot(111)将轴图定位在[0.125,0.11,0.775,0.77]，或者类似的位置，已经为轴图周围的标题和刻度（标签）保留的足够的空间。

大多数情况下，在画板上创建轴图，最常使用的方法是add_subplot。只有在您需要为轴图指定确切位置时，才会用到add_axes。

## [7.1 轴域](https://www.jianshu.com/p/db2e28201835)

我们看一个轴域的例子。



![img](https://upload-images.jianshu.io/upload_images/13250940-bd56c0a9950982fa.png?imageMogr2/auto-orient/strip|imageView2/2/w/366/format/webp)

如果您需要在任意位置创建轴图，调用figure的add_axes函数添加轴域。该函数有一个列表参数[left, bottom,

width, height]，分别为轴域在图形中的左下角位置以及长度和宽度。这些参数以图形的坐标体系为参数，范围在0到1之间。(0, 0)表示Figure的左下角位置，(1, 1)表示Figure的全部长度和宽度。

因此以下语句段，在(0.1,

0.1)位置处添加一个长和宽均为0.8的轴域。

ax1  = fig.add_axes([0.1,0.1,.8,.8])

ax1.set_xticks([])

ax1.set_yticks([])

ax1.text(0.6,  0.6, 'axes([0.1,0.1,.8,.8])', ha='center', va='center', size=16,alpha=.5)

以下语句段，在(0.2, 0.2)位置处添加一个长和宽均为0.3的轴域。

ax2  = fig.add_axes([0.2,0.2,.3,.3])

ax2.set_xticks([])

ax2.set_yticks([])

ax2.text(0.5,  0.5, 'axes([0.2,0.2,.3,.3])', ha='center', va='center', size=12, alpha=.5)

## [7.2 子域](https://www.jianshu.com/p/db2e28201835)

我们再看一个子域的例子。



![img](https://upload-images.jianshu.io/upload_images/13250940-2385ea4b034488c3.png?imageMogr2/auto-orient/strip|imageView2/2/w/467/format/webp)

这个例子通过GridSpec机制添加子图，我们先定义一个3×3的栅格。

G  = gridspec.GridSpec(3, 3)

第一个子图位于第一行，占据所有三个列，因此行号指定为0，列号用冒号表示所有列。

ax1  = fig.add_subplot(G[0, :])

ax1.set_xticks([])

ax1.set_yticks([])

ax1.text(0.5,  0.5, ',ha='center', va='center', size=20, alpha=.5)

第一个子图位于第二行，占据前面两个列，因此行号指定为1，列号用冒号-1表示倒数第1列之前的所有列，即第一列和第二列。

ax2  = fig.add_subplot(G[1,:-1])

ax2.set_xticks([])

ax2.set_yticks([])

ax2.text(0.5,  0.5, ',ha='center', va='center', size=20, alpha=.5)

第三个子图占据第二行和第三行，位于最后一个列，因此行号用1冒号表示第1列之后的所有列，即第二列和第三列，列号指定为-1表示倒数第1列，即第三列。

ax3  = fig.add_subplot(G[1:, -1])

ax3.set_xticks([])

ax3.set_yticks([])

ax3.text(0.5,  0.5, ',ha='center', va='center', size=20, alpha=.5)

第四个子图位于第三行第一列，因此行号用-1表示倒数第1行，即第三行，列号指定为0。

ax4  = fig.add_subplot(G[-1,0])

ax4.set_xticks([])

ax4.set_yticks([])

ax4.text(0.5,  0.5, ',ha='center', va='center', size=20, alpha=.5)

第五个子图位于第三行第二列，因此行号用-1表示倒数第1行，即第三行，列号用-2表示倒数第2列，即第二列。

ax5  = fig.add_subplot(G[-1,-2])

ax5.set_xticks([])

ax5.set_yticks([])

ax5.text(0.5,  0.5, ',ha='center', va='center', size=20, alpha=.5)

# [八、图的形状](https://www.jianshu.com/p/db2e28201835)

## [8.1 普通图](https://www.jianshu.com/p/db2e28201835)

前面介绍过许国普通图的例子，这里不再赘述。

## [8.2 散点图](https://www.jianshu.com/p/db2e28201835)

[以下是散点图的例子。](https://www.jianshu.com/p/db2e28201835)



![img](https://upload-images.jianshu.io/upload_images/13250940-fef85b3611e0e4d7.png?imageMogr2/auto-orient/strip|imageView2/2/w/407/format/webp)

在深度学习中，线性回归算法是基础的一环。我们需要训练出一条直线能够拟合如上图中的数据。

首先生成模拟数据。

x = np.arange(0, 2.0, 0.1)   #生成一个0到50的序列

y = x * 0.1 + 0.3 + np.random.normal(0.0,  0.03, 20)

接下来，以散点的方式呈现上面的数据。

ax.scatter(x,y,  label='Sample Data')

ax.set_title('Linear

  Regression',fontsize=20) # 添加标题，并设置字体大小

ax.set_xlabel('X

  Axis', fontsize=15) # 添加x轴，并设置字体大小

ax.set_ylabel('Y

  Axis', fontsize=15) # 添加y轴，并设置字体大小

ax.set_ylim(0.2,  0.6)

## [8.3 三维图](https://www.jianshu.com/p/db2e28201835)

Matplotlib通过axes3d模块支持三维图。

import  matplotlib.pyplot as plt

from  matplotlib import cm

from  mpl_toolkits.mplot3d import axes3d



fig  = plt.figure()

ax  = fig.gca(projection='3d')



X,  Y, Z = axes3d.get_test_data(0.05)

ax.plot_surface(X,  Y, Z, rstride=8, cstride=8, alpha=0.3)

cset  = ax.contourf(X, Y, Z, zdir='z', offset=-100, cmap=cm.coolwarm)

cset  = ax.contourf(X, Y, Z, zdir='x', offset=-40, cmap=cm.coolwarm)

cset  = ax.contourf(X, Y, Z, zdir='y', offset=40, cmap=cm.coolwarm)



ax.set_xlabel('X')

ax.set_xlim(-40,  40)

ax.set_ylabel('Y')

ax.set_ylim(-40,  40)

ax.set_zlabel('Z')

ax.set_zlim(-100,  100)



plt.show()



![img](https://upload-images.jianshu.io/upload_images/13250940-762dbd369c34a931.png?imageMogr2/auto-orient/strip|imageView2/2/w/356/format/webp)

## [8.4 等高图](https://www.jianshu.com/p/db2e28201835)

Matplotlib支持绘制等高图。

import  numpy as np

import  matplotlib.pyplot as plt



\#

  定义等高线高度函数

def  f(x, y):

​    return (1 - x / 2 + x ** 5 + y ** 3) *  np.exp(- x ** 2 - y ** 2)



\#

  数据数目

n  = 256

\#

  定义x, y

x  = np.linspace(-3, 3, n)

y  = np.linspace(-3, 3, n)



\#

  生成网格数据

X,  Y = np.meshgrid(x, y)



\#

  填充等高线的颜色, 8是等高线分为几部分

plt.contourf(X,  Y, f(X, Y), 8, alpha = 0.75, cmap = plt.cm.hot)

\#

  绘制等高线

C  = plt.contour(X, Y, f(X, Y), 8, colors = 'black')

\#

  绘制等高线数据

plt.clabel(C,  inline = True, fontsize = 10)



\#

  去除坐标轴

plt.xticks(())

plt.yticks(())

plt.show()



![img](https://upload-images.jianshu.io/upload_images/13250940-d857b793834a32e5.png?imageMogr2/auto-orient/strip|imageView2/2/w/356/format/webp)

# [九、动画](https://www.jianshu.com/p/db2e28201835)

## [9.1 二维动画](https://www.jianshu.com/p/db2e28201835)

Matplotlab支持生成二维动画。例如，我们可以将在曲线上某点P的求导做成一个动画，以更形象地阐述导数的概念，提高教学效果。



![img](https://upload-images.jianshu.io/upload_images/13250940-4aa571c2278f62c4.png?imageMogr2/auto-orient/strip|imageView2/2/w/655/format/webp)

## [9.2 三维动画](https://www.jianshu.com/p/db2e28201835)

我们通过动画的方式来呈现一个三维图形的效果。

首先导入需要的包。

\#  First import everthing you need

import  numpy as np

from  matplotlib import pyplot as plt

from  matplotlib import animation

from  mpl_toolkits.mplot3d import Axes3D

接下来准备数据。

x  = np.linspace(-10, 10, 101)

y  = x

x,  y = np.meshgrid(x, y)

z = x  ** 2 + y ** 2

创建画板和画纸。

\#  Create a figure and a 3D Axes

fig  = plt.figure()

ax =  Axes3D(fig)

画出动画的初始图。

\#  Create an init function and the animate functions.

\#  Both are explained in the tutorial. Since we are changing

\#  the the elevation and azimuth and no objects are really

\#  changed on the plot we don't have to return anything from

\#  the init and animate function. (return value is explained

\#  in the tutorial.

def  init():

​    ax.plot_surface(x, y, z, rstride=1,  cstride=1)

​    return fig,

定义动画的动态函数。

def  animate(i):

​    print(i)

​    ax.view_init(elev=30., azim=i)

​    return fig,

按固定套路生成动画对象。

\#  Animate

anim =  animation.FuncAnimation(fig, animate, init_func=init, frames=360,  interval=20, blit=True)

将动画保存在文件中。需要说明的是，必须在环境中安装ffmpeg模块，才能进行保存。

\#  Save

anim.save('3danim.mp4')

最后，我们可以打开保存的文件查看动画效果。



![img](https://upload-images.jianshu.io/upload_images/13250940-a28cbb1d697cfbb6.gif?imageMogr2/auto-orient/strip|imageView2/2/w/432/format/webp)

# [十、Matplotlib](https://www.jianshu.com/p/db2e28201835)可视化应用

Matplotlib是深度学习一个行之有效的手段。首先，它可以实现训练数据的可视化，无论是监督学习的分类问题和回归问题，还是无监督学习的聚类问题，都可以帮助我们设计有效的模型进行训练。例如，对于鸢尾花分类问题，我们将数据如下图可视化，就很清除地知道可以使用感知机或线性回归算法进行处理了。



![img](https://upload-images.jianshu.io/upload_images/13250940-472b0d2f1e2a1bf6.png?imageMogr2/auto-orient/strip|imageView2/2/w/640/format/webp)

在深度学习中，Matplotlib还可以实现训练过程的可视化，从而判断出训练算法或训练参数是否需要调整，以及在什么时候可以停止训练。这里需要一张训练过程收敛和不收敛的图进行比较。

[此外，深度学习在平面几何和立体几何中能够帮助呈现动画效果，从而起到辅助教学和求解的作用。例如，下面是一道经典的平面几何试题。](https://www.jianshu.com/p/db2e28201835)

已知：如图，O是半圆的圆心，C、E是圆上的两点，CD⊥AB，EF⊥AB，EG⊥CO，求证：CD=GF。



![img](https://upload-images.jianshu.io/upload_images/13250940-a4970ea4567d96b2.png?imageMogr2/auto-orient/strip|imageView2/2/w/237/format/webp)

就这道题而言，我们可以将C固定，让E在圆上变动，更直观地感知GF的变化。用Matplotlib可视化出来的动画效果如下：



![img](https://upload-images.jianshu.io/upload_images/13250940-5ae269666824e172.gif?imageMogr2/auto-orient/strip|imageView2/2/w/288/format/webp)

下面是一个立体几何的例子。

如图，AD与BC是四面体ABCD中相互垂直的棱，BC=2，若AD=2c，且AB+BD=AC+CD=2a，其中a、c为常数，则[四面体的体积的最大值](https://www.jianshu.com/p/db2e28201835)是____。



![img](https://upload-images.jianshu.io/upload_images/13250940-68a40454e4150c38.png?imageMogr2/auto-orient/strip|imageView2/2/w/166/format/webp)

虽然不完美，我们还是可以用Matplotlib画出在BC变化的情况下，四面体的变化情况，从而推测出四面体体积的最大值应该出现在中间位置，为求解提供一定的方向。



![img](https://upload-images.jianshu.io/upload_images/13250940-ab4fb3aaa7850d4e.gif?imageMogr2/auto-orient/strip|imageView2/2/w/432/format/webp)

下面是另一个立体几何的例子。

如图，在四棱锥S-ABCD中，底面ABCD为正方形，侧棱SD底面ABCD，E、F分别是AB、SC的中点。

[if !supportLists]（1）       [endif]求证：EF//平面SAD

[if !supportLists]（2）       [endif]设SD=2CD，求二面角A-EF-D的大小。



![img](https://upload-images.jianshu.io/upload_images/13250940-bedd573266385b98.png?imageMogr2/auto-orient/strip|imageView2/2/w/169/format/webp)

我们可以用Matplotlib画出上述题目的形状，然后手动调整它的观察位置和角度，全方位了解上述立体图形。



![img](https://upload-images.jianshu.io/upload_images/13250940-1ffabcd6dc649f3f.png?imageMogr2/auto-orient/strip|imageView2/2/w/519/format/webp)



![img](https://upload-images.jianshu.io/upload_images/13250940-70b60878ee292fd4.png?imageMogr2/auto-orient/strip|imageView2/2/w/498/format/webp)

上面第一个图为题中的正视图，我们拖动鼠标旋转，在转到第二个图示，我们发现，似乎两条红边是相等的，这可以通过计算证明。然后就可以找到题中的关键点，即线段EF的中点。从而最终得到此题的答案。

# [结束语](https://www.jianshu.com/p/db2e28201835)

本文介绍了Python的绘图模块Matplotlib以及在深度学习等方面的应用。需要知道，Matplotlib仅仅是一个工具，我们掌握它，为我们要实现的目标更好的服务。