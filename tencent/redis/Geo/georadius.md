# georadius

```javascript
GEORADIUS key longitude latitude radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]
```

**自3.2.0起可用。**

**时间复杂度：** O（N + log（M））其中N是由中心和半径界定的圆形区域的边界框内的元素的数量，M 是索引内的项目的数量。

使用 GEOADD 返回填充了地理空间信息的已排序集合的成员，这些位于由中心位置指定的区域的边界内，并且与中心的最大距离（半径）一致。

[纠错](javascript:;)

本手册页还涵盖了`GEORADIUS_RO`和`GEORADIUSBYRANGE_RO`变种（参见下面的更多信息部分）。

此命令的常见用例是检索指定点附近的地理空间项目，并且不超过指定数量的米（或其他单位）。例如，这允许向附近的应用程序的移动用户建议位置。

半径以下列单位之一指定：

- **m** 为米。

- **km** 为千米。

- **mi** 为英里。

- **ft** 为英尺。

该命令有选择地使用以下选项返回附加信息：

- `WITHDIST`：还要返回指定中心返回物品的距离。距离以与指定为命令的半径参数的单位相同的单位返回。

- `WITHCOORD`：还返回匹配项目的经度，纬度坐标。

- `WITHHASH`：还以52位无符号整数的形式返回项目的原始 geohash 编码的有序集合分数。这只对低级别的黑客或调试很有用，对于普通用户来说这很有趣。

该命令的默认设置是返回未排序的项目。使用以下两个选项可以调用两种不同的排序方法：

- `ASC`：将返回的项目从最近的到最远的，相对于中心排序。

- `DESC`：从最远到最近的相对于中心的返回项目排序。

默认情况下会返回所有匹配的项目。通过使用 **COUNT** **<count>**选项，可以将结果限制为前 N 个匹配项。但是请注意，在内部，命令需要执行与匹配指定区域的项目数量成比例的努力，因此，`COUNT`即使只返回几个结果，使用非常小的选项查询非常大的区域也可能会很慢。另一方面，`COUNT`如果通常只使用第一个结果，则可以成为减少带宽使用的非常有效的方法。

## 返回值

[阵列回复](https://redis.io/topics/protocol#array-reply)，具体为：

- 没有`WITH`指定任何选项，该命令只返回一个线性数组，如“纽约”，“米兰”，“巴黎”。

- 如果`WITHCOORD`，`WITHDIST`或者`WITHHASH`指定了选项，该命令将返回阵列，其中每个子阵列表示单个项目的阵列。

当附加信息作为每个项目的数组数组返回时，子数组中的第一项始终是返回项目的名称。其他信息按以下顺序作为子数组的连续元素返回。

\1. 与中心的距离作为浮点数，与半径中指定的单位相同。

\2. geohash 整数。

\3. 坐标作为两个项目的 x，y 数组（经度，纬度）。

例如，命令`GEORADIUS Sicily 15 37 200 km WITHCOORD WITHDIST`将以下列方式返回每个项目：

```javascript
["Palermo","190.4424",["13.361389338970184","38.115556395496299"]]
```

## 只读变量

由于 GEORADIUS 和 GEORADIUSBYMEMBER 有一个`STORE`和`STOREDIST`选择，他们在技术上标记为在 Redis 的命令表写入命令。因为这个原因，只读从属会标记它们，即使连接处于只读模式，Redis 集群从属也会将它们重定向到主实例（请参阅 Redis 集群的 READONLY 命令）。

打破与过去的兼容性被认为是被拒绝的，至少对于 Redis 4.0 来说是这样，所以添加了两个只读的命令变体。他们完全像原来的命令，但拒绝`STORE`和`STOREDIST`选项。这两个变量被称为`GEORADIUS_RO`和`GEORADIUSBYMEMBER_RO`，并能安全地从设备中使用。

这两个命令分别在 Redis 3.2.10 和 Redis 4.0.0 中引入。

## 例子

redis> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania" `(integer) 2` redis> GEORADIUS Sicily 15 37 200 km WITHDIST `1) 1) "Palermo" 2) "190.4424" 2) 1) "Catania" 2) "56.4413"` redis> GEORADIUS Sicily 15 37 200 km WITHCOORD `1) 1) "Palermo" 2) 1) "13.36138933897018433" 2) "38.11555639549629859" 2) 1) "Catania" 2) 1) "15.08726745843887329" 2) "37.50266842333162032"` redis> GEORADIUS Sicily 15 37 200 km WITHDIST WITHCOORD `1) 1) "Palermo" 2) "190.4424" 3) 1) "13.36138933897018433" 2) "38.11555639549629859" 2) 1) "Catania" 2) "56.4413" 3) 1) "15.08726745843887329" 2) "37.50266842333162032"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18