# 05-Scipy

[![img](https://upload.jianshu.io/users/upload_avatars/14575314/733eddc9-d8a1-45a4-8e7d-080f7e6e225e.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/bbb36adcc81d)

[郑元吉](https://www.jianshu.com/u/bbb36adcc81d)关注

0.0452018.12.14 21:02:38字数 320阅读 136

### 一.Scipy简介

> -Scipy依赖于Numpy
> -Scipy提供了真正的矩阵
> -Scipy包含的功能：最优化、线性代数、积分、插值、拟合、特殊函数、快速傅里叶变换、信号处理、图像处理、常微分方程求解器等
> -Scipy是高端科学计算工具包
> -Scipy由一些特定功能的子模块组成



![img](https://upload-images.jianshu.io/upload_images/14575314-ba68266e09f92d0c.png?imageMogr2/auto-orient/strip|imageView2/2/w/287/format/webp)

Scipy.png

### 二.高数积分

##### 2.1 什么是积分?



![img](https://upload-images.jianshu.io/upload_images/14575314-06aaae928f38a762.png?imageMogr2/auto-orient/strip|imageView2/2/w/1054/format/webp)

积分.png

##### 2.2 定积分计算



![img](https://upload-images.jianshu.io/upload_images/14575314-3364241f737d559e.png?imageMogr2/auto-orient/strip|imageView2/2/w/172/format/webp)

定积分公式.png



![img](https://upload-images.jianshu.io/upload_images/14575314-1b8cca809ca98c59.png?imageMogr2/auto-orient/strip|imageView2/2/w/1039/format/webp)

定积分半圆.png

##### 2.3 绘制圆

```python
f = lambda x : (1 - x**2)**0.5
import numpy as np
import matplotlib.pyplot as plt
x = np.linspace(-1,1,1000)
plt.figure(figsize = (4,4))
plt.plot(x,f(x),'-',x,-f(x),'-',color = 'r')

plt.show()
```



![img](https://upload-images.jianshu.io/upload_images/14575314-02f9508f5a7bff16.png?imageMogr2/auto-orient/strip|imageView2/2/w/294/format/webp)

定积分圆.png

##### 2.4 使用Scipy.integrate.quad()来进行计算

```python
from scipy import integrate
def g(x):
    return (1- x**2)**0.5
pi_2,err = integrate.quad(g,-1,1)
print(pi_2,err)
```

### 三.Scipy文件输入/输出

##### 3.1 输入

```python
from scipy import io as spio
import numpy as np
a = np.ones((3,3))
#mat文件是标准的二进制文件
spio.savemat('./data/file.mat',mdict={'a':a})
```

- 读取图片

```python
#读取图片
from scipy import misc
data = misc.imread('./data/moon.png')
```

##### 3.2 输出

```kotlin
#读取保存的文件
data = spio.loadmat('./data/file.mat')
data['a']
```

- 保存图片

```bash
#保存图片
misc.imsave('./data/save.png',arr=data)
```

##### 3.3 scipy.misc

```dart
模糊，轮廓，细节，edge_enhance，edge_enhance_more， 浮雕，find_edges，光滑，smooth_more，锐化
# 滤镜
# 'blur', 'contour', 'detail', 'edge_enhance', 'edge_enhance_more',
       # 'emboss', 'find_edges', 'smooth', 'smooth_more', 'sharpen'.
cat_filter = misc.imfilter(cat, 'sharpen' )
plt.figure(figsize=(12,9))
plt.imshow(cat_filter)
```

### 四.图片处理ndimage

##### 4.1 使用Scipy中的ndimage/misc进行处理

- 导包提取数据处理数据

```python
from scipy import misc,ndimage
#原始图片
face = misc.face(gray=True)
#移动图片坐标
shifted_face = ndimage.shift(face, (50, 50))
#移动图片坐标，并且指定模式
shifted_face2 = ndimage.shift(face, (-200, 0), mode='wrap')
#旋转图片
rotated_face = ndimage.rotate(face, -30)
#切割图片
cropped_face = face[10:-10, 50:-50]
#对图片进行缩放
zoomed_face = ndimage.zoom(face, 0.5)
faces = [shifted_face,shifted_face2,rotated_face,cropped_face,zoomed_face]
```

- 绘制图片

```bash
plt.figure(figsize = (12,12))
for i,face in enumerate(faces):
    plt.subplot(1,5,i+1)
    plt.imshow(face,cmap = plt.cm.gray)
    plt.axis('off')
```



![img](https://upload-images.jianshu.io/upload_images/14575314-d5dc49be60ddf73b.png?imageMogr2/auto-orient/strip|imageView2/2/w/729/format/webp)

绘图.png

##### 4.2 图片过滤

- 导包处理滤波

```python
from scipy import misc,ndimage
import numpy as np
import matplotlib.pyplot as plt
face = misc.face(gray=True)
face = face[:512, -512:]  # 做成正方形

noisy_face = np.copy(face).astype(np.float)
#噪声图片
noisy_face += face.std() * 0.5 * np.random.standard_normal(face.shape)
#高斯过滤
blurred_face = ndimage.gaussian_filter(noisy_face, sigma=1)
#中值滤波
median_face = ndimage.median_filter(noisy_face, size=5)

#signal中维纳滤波
from scipy import signal
wiener_face = signal.wiener(noisy_face, (5, 5))

titles = ['noisy','gaussian','median','wiener']
faces = [noisy_face,blurred_face,median_face,wiener_face]
```

- 绘制图片

```bash
plt.figure(figsize=(12,12))
plt.subplot(141)
plt.imshow(noisy_face,cmap = 'gray')
plt.title('noisy')
plt.subplot(142)
plt.imshow(blurred_face,cmap = 'gray')
plt.title('gaussian')
plt.subplot(143)
plt.imshow(median_face,cmap = 'gray')
plt.title('median')
plt.subplot(144)
plt.imshow(wiener_face,cmap = 'gray')
plt.title('wiener')
plt.show()
```



![img](https://upload-images.jianshu.io/upload_images/14575314-2d40d2f83b591205.png?imageMogr2/auto-orient/strip|imageView2/2/w/747/format/webp)

滤波绘图.png

### 五.pandas绘图函数

##### 5.1 线型图

- 简单的Series图标示例

```jsx
import numpy as np
import pandas as pd
from pandas import Series,DataFrame
import matplotlib.pyplot as plt
np.random.seed(0)
s = Series(np.random.randn(10).cumsum(),index = np.arange(0,100,10))
s.plot()
plt.show(s.plot())
```



![img](https://upload-images.jianshu.io/upload_images/14575314-281fbc66418b34ee.png?imageMogr2/auto-orient/strip|imageView2/2/w/457/format/webp)

Series线型图.png

- 简单的DataFrame图标示例

```bash
np.random.seed(0)
df = DataFrame(np.random.randn(10,4).cumsum(0),
              columns= ['A','B','C','D'],
              index = np.arange(0,100,10))
plt.show(df.plot())
```



![img](https://upload-images.jianshu.io/upload_images/14575314-6b3fe7ed0ef5a908.png?imageMogr2/auto-orient/strip|imageView2/2/w/470/format/webp)

DataFrame线型图.png

##### 5.2 柱状图

- 水平和垂直柱状图

```kotlin
fig,axes = plt.subplots(2,1)
data = Series(np.random.rand(16),index = list('abcdefghijklmnop'))
data.plot(kind = 'bar',ax = axes[0],color = 'b',alpha = 0.9)
data.plot(kind = 'barh',ax = axes[1],color = 'b',alpha = 0.9)
```



![img](https://upload-images.jianshu.io/upload_images/14575314-7d320df4cd412f8d.png?imageMogr2/auto-orient/strip|imageView2/2/w/428/format/webp)

水平和垂直柱状图.png

- DataFrame柱状图示例

```bash
df = DataFrame(np.random.rand(6,4),
              index = ['one','two','three','four','five','six'],
              columns = pd.Index(['A','B','C','D'],name = 'Genus'))
plt.show(df.plot(kind = 'bar'))
```



![img](https://upload-images.jianshu.io/upload_images/14575314-463fd5f2ef1a5e63.png?imageMogr2/auto-orient/strip|imageView2/2/w/423/format/webp)

柱状图1.png

```php
df = DataFrame(np.random.rand(6,4),
              index = ['one','two','three','four','five','six'],
              columns = pd.Index(['A','B','C','D'],name = 'Genus'))
plt.show(df.plot(kind = 'bar',stacked = True))
```



![img](https://upload-images.jianshu.io/upload_images/14575314-462e4e6ca532645a.png?imageMogr2/auto-orient/strip|imageView2/2/w/426/format/webp)

柱状图2.png

##### 5.3 直方图和密度图

- random随机数百分比的直方图

```bash
a = np.random.random(10)
b = a/a.sum()
s = Series(b)
plt.show(s.hist(bins = 100)) #bins直方图的柱数
```



![img](https://upload-images.jianshu.io/upload_images/14575314-ccb30baeb5973df1.png?imageMogr2/auto-orient/strip|imageView2/2/w/428/format/webp)

随机直方图.png

- random随机数百分比的密度图

```bash
a = np.random.random(10)
b = a/a.sum()
s = Series(b)
plt.show(s.plot(kind = 'kde'))
```



![img](https://upload-images.jianshu.io/upload_images/14575314-32011fc248e9943e.png?imageMogr2/auto-orient/strip|imageView2/2/w/443/format/webp)

随机密度图.png

- 带有密度估计的规格化直方图

```kotlin
%matplotlib inline
comp1 = np.random.normal(0,1,size = 200)
comp2 = np.random.normal(10,2,size = 200)
values = Series(np.concatenate([comp1,comp2]))
p1 = values.hist(bins = 100,alpha = 0.3,color = 'k',normed = True)

p2 = values.plot(kind = 'kde',style = '--',color = 'r')
```



![img](https://upload-images.jianshu.io/upload_images/14575314-037ad2c1ff8a5278.png?imageMogr2/auto-orient/strip|imageView2/2/w/449/format/webp)

规格化直方图.png

##### 5.4 散布图

- 一张简单散布图

```bash
df = DataFrame(DataFrame({'A': np.random.randn(1000), 'B': np.random.randn(1000), 'C': np.random.randn(1000) , 'D': np.random.randn(1000)})

df.plot('A','B',kind = 'scatter',title = 'x Vs y')
```



![img](https://upload-images.jianshu.io/upload_images/14575314-be79dacdc476679b.png?imageMogr2/auto-orient/strip|imageView2/2/w/420/format/webp)

简单散布图.png

- 散布图矩阵

```jsx
import numpy as np
import pandas as pd
from pandas import Series,DataFrame
%matplotlib inline
df = DataFrame(np.random.randn(200).reshape(50,4),columns = ['A','B','C','D'])
pd.plotting.scatter_matrix(df,diagonal = 'kde',color = 'k')
```