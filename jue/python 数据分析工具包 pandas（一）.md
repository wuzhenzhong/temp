# python 数据分析工具包 pandas（一）



![img](https://user-gold-cdn.xitu.io/2019/9/10/16d1895946d940cf?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## 1. 简单介绍

pandas 是专为 python 编程语言设计的高性能，简单易用的**数据结构**和**数据分析**工具库，它建立在 numpy 之上，可以许多第三方库完美集成在同一个科学计算环境中。pandas 被广泛应用于金融，统计，社会科学和许多工程技术领域，处理典型数据分析案例。

## 2. 安装

pandas 支持 conda 和 pip 两种方式安装。

conda 安装：

```
conda install pandas
复制代码
```

pip 安装：

```
pip install pandas
复制代码
```

截至本文写作之时，官方最新发布版本是 v0.25.1，发布时间为2019年8月22日。最新版本是 0.25.x 系列的bug修复版，建议更新。更新方式如下：

```
pip install --upgrade pandas
复制代码
```

## 3. 数据结构

pandas 有两种主要的数据结构：`Series`（1维）和 `DataFrame` (2维)。

下面分别介绍这两种数据结构，首先在我们的 python 脚本或 jupyter notebook 中导入 pandas，业界惯例缩写为 pd。

```
import pandas as pd
复制代码
```

### 3.1 Series

Series 是一维标记数组，能够保存任何数据类型（整数，字符串，浮点数，Python对象等）。 轴标签统称为索引。

#### 3.1.1 创建 Series

通过列表创建：

```
data = [1, 2, 3]
pd.Series(data)
复制代码
0    1
1    2
2    3
dtype: int64
复制代码
```

通过字典创建：

```
data = {'a': 1, 'b': 2, 'c': 3}
pd.Series(data)
复制代码
a    1
b    2
c    3
dtype: int64
复制代码
```

通过 index 参数设置索引(标签):

```
data = [1, 2, 3]
index = ['a', 'b', 'c']
pd.Series(data, index=index)
复制代码
a    1
b    2
c    3
dtype: int64
复制代码
```

通过标量创建(相同值)，并设置索引(标签，不能重复)：

```
data = 0
index = ['a', 'b', 'c']
pd.Series(data, index=index)
复制代码
a    0
b    0
c    0
dtype: int64
复制代码
```

#### 3.1.2 访问 Series

```
s = pd.Series([10, 100, 1000], index=['a', 'b', 'c'])
s
复制代码
a      10
b     100
c    1000
dtype: int64
复制代码
```

数组方式访问：

```
print(s[0], s[1], s[2])
复制代码
10 100 1000
复制代码
```

字典方式访问：

```
print(s['a'], s['b'], s['c'])
复制代码
10 100 1000
复制代码
```

可见两种访问方式之间的对应关系：

```
print(s[0] == s['a'], s[1] == s['b'], s[2] == s['c'])
复制代码
True True True
复制代码
```

### 3.2 DataFrame

DataFrame 是一个二维标记数据结构，具有可能不同类型的列。 可以将其类比于电子表格或 SQL 表，或 Series 对象的字典。 它也是最常用的 pandas 对象。

#### 3.2.1 创建 DataFrame

通过列表字典创建：

```
data = {
    'col1': [1, 2, 3],
    'col2': [4, 5, 6],
    'col3': [7, 8, 9]
}

pd.DataFrame(data)
复制代码
```

|      | col1 | col2 | col3 |
| ---- | ---- | ---- | ---- |
| 0    | 1    | 4    | 7    |
| 1    | 2    | 5    | 8    |
| 2    | 3    | 6    | 9    |

通过 Series 字典创建：

```
s1 = pd.Series([1, 2, 3], index=['row1', 'row2', 'row3'])
s2 = pd.Series([4, 5, 6], index=['row2', 'row3', 'row4'])
s3 = pd.Series([7, 8, 9], index=['row3', 'row4', 'row5'])

data = {
    'col1': s1,
    'col2': s2,
    'col3': s3
}

pd.DataFrame(data)
复制代码
```

|      | col1 | col2 | col3 |
| ---- | ---- | ---- | ---- |
| row1 | 1.0  | NaN  | NaN  |
| row2 | 2.0  | 4.0  | NaN  |
| row3 | 3.0  | 5.0  | 7.0  |
| row4 | NaN  | 6.0  | 8.0  |
| row5 | NaN  | NaN  | 9.0  |

通过字典列表创建:

```
data = [
    {'col1': 1, 'col2': 2, 'col3': 3},
    {'col1': 2, 'col2': 3, 'col3': 4},
    {'col1': 3, 'col2': 4, 'col3': 5}
]

pd.DataFrame(data, index=['row1', 'row2', 'row3'])
复制代码
```

|      | col1 | col2 | col3 |
| ---- | ---- | ---- | ---- |
| row1 | 1    | 2    | 3    |
| row2 | 2    | 3    | 4    |
| row3 | 3    | 4    | 5    |

通过二维列表创建：

```
data = [
    [1, 2, 3],
    [2, 3, 4],
    [3, 4, 5]
]

pd.DataFrame(data, index=['row1', 'row2', 'row3'], columns=['col1', 'col2', 'col3'])
复制代码
```

|      | col1 | col2 | col3 |
| ---- | ---- | ---- | ---- |
| row1 | 1    | 2    | 3    |
| row2 | 2    | 3    | 4    |
| row3 | 3    | 4    | 5    |

#### 3.2.2 访问 DataFrame

```
df = pd.DataFrame([[1, 4, 7], [2, 5, 8], [3, 6, 9]],
                  index=['row1', 'row2', 'row3'],
                  columns=['col1', 'col2', 'col3'])
df
复制代码
```

|      | col1 | col2 | col3 |
| ---- | ---- | ---- | ---- |
| row1 | 1    | 4    | 7    |
| row2 | 2    | 5    | 8    |
| row3 | 3    | 6    | 9    |

通过列标签访问列:

```
df['col1']
复制代码
row1    1
row2    2
row3    3
Name: col1, dtype: int64
复制代码
```

通过行标签访问行:

```
df.loc['row1']
复制代码
col1    1
col2    4
col3    7
Name: row1, dtype: int64
复制代码
```

通过整数访问行：

```
df.iloc[0]
复制代码
col1    1
col2    4
col3    7
Name: row1, dtype: int64
复制代码
```

通过切片选择行:

```
df[1:]
复制代码
```

|      | col1 | col2 | col3 |
| ---- | ---- | ---- | ---- |
| row2 | 2    | 5    | 8    |
| row3 | 3    | 6    | 9    |

#### 3.2.3 转置 DataFrame

将行列互换，类似线性代数中矩阵的转置。

```
df.T
复制代码
```

|      | row1 | row2 | row3 |
| ---- | ---- | ---- | ---- |
| col1 | 1    | 2    | 3    |
| col2 | 4    | 5    | 6    |
| col3 | 7    | 8    | 9    |

## 猜你喜欢

- [1] [python 科学计算的基石 numpy（一）](https://juejin.im/post/5d783205e51d4561f64a0894)
- [2] [python 数据可视化工具包 matplotlib](https://juejin.im/post/5d74c9a351882535db6de92b)

------

坚持写专栏不易，如果觉得本文对你有帮助，记得点个赞。感谢支持！

- 个人网站: [kenblog.top](https://juejin.im/post/kenblog.top)
- github 站点: [kenblikylee.github.io](https://kenblikylee.github.io/)