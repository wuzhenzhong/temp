# Matplotlib

[MA木易YA](https://www.jianshu.com/u/e62fdbf2ffd9)关注

0.8052019.03.06 17:06:03字数 374阅读 866

> ​    Matplotlib 可能是 Python 2D-绘图领域使用最广泛的套件。它能让使用者很轻松地将数据图形化，并且提供多样化的输出格式

## matplotlib三层结构

### 1. 容器层

> 对画布进行创建，定义相关属性

- 画板层Canvas
- 画布层Figure（可指定画布属性，大小、清晰度等）
- 绘图区/坐标系（可指定多区域、坐标系显示，通过figure、axes对象）

### 2. 辅助显示层

> 增加相关显示功能、描述

- 修改x、y轴刻度（plt.x/yticks()）
- 添加描述信息（plt.x/ylabel();plt.title()）
- 添加网格（plt.grid()）

### 3. 图像层

> 具体描绘的图像风格、种类等等

**I. 折线图**

```css
plt.plot()
```

- 设置画布属性以及保存图像
  `figsize：大小 dpi：清晰度`
- [多个坐标系显示](https://matplotlib.org/api/axes_api.html)

```undefined
plt.subplots
figure, axes = plt.subplots(nrows=1, ncols=2, **fig_kw)

axes[0].方法名()
```

**II. 散点图**

```css
plt.scatter（x，y）
```

**III. 柱状图**

```bash
matplotlib.pylot.bar(x,y, width, align='center', **kwargs)
```

**IV. 直方图**

```rust
matplotlib.pylot.hist(x,y, bins=None, normed=None, **kwargs)

bins（组数） = (max(x)-min(x))//(组距)
```

**V. 饼图**

```undefined
plt.pie(x, labels=autopct=,color=)
```

- x,数量，自动计算占比
- labels，每部分名称
- actopct 占比显示指定%1.2f%%
- color 每部分颜色
- plt.axis('equal')——保证饼图圆形，保证长宽一致

## 总结

​    **总得来说，Matplotlib绘图过程无非几步：**
**1. 准备数据**
**2. 创建画布**
**3. 绘制图像（根据不同图像类别调用不同方法）**
**4. 辅助绘制（刻度、图例等）**
**5. 图像显示/保存**

- 更多相关方法可以去官网的api上查询文档：matplotlib：

### 示例（ 温度变化折线图）

```python
import matplotlib.pyplot as plt
import random


if __name__ == '__main__':
    #准备数据
    x = range(60)
    y_shanghai = [random.uniform(15, 18) for i in x]
    #创建画布
    plt.figure(figsize=(20, 8), dpi=80)

    #绘制图像
    plt.plot(x, y_shanghai, color='b', linestyle='--', label='上海')

    #显示图例
    plt.legend()

    #准备x、y的刻度以及刻度说明
    x_label = ["11点{}分".format(i) for i in x]
    plt.xticks(x[::5], x_label[::5])
    plt.yticks(range(0, 40, 5))

    #添加网格
    plt.grid(linestyle='--', alpha=0.5)

    plt.xlabel("时间变化")
    plt.ylabel("温度变化")
    plt.title("某城市11点到12点温度变化")

    plt.show()
```