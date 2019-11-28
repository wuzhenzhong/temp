# geoadd

```javascript
GEOADD key longitude latitude member [longitude latitude member ...]
```

**自3.2.0起可用。**

**时间复杂度：**添加每个项目的 O（log（N）），其中 N 是排序集合中元素的数量。

将指定的地理空间项目（纬度，经度，名称）添加到指定的键。数据作为一个排序集存储在密钥中，以便以后可以使用带有 GEORADIUS 或 GEORADIUSBYMEMBER 命令的半径查询来检索项目。

该命令采用标准格式 x，y 的参数，因此经度必须在纬度之前指定。可以索引的坐标是有限的：非常靠近极点的区域不可索引。EPSG：900913 / EPSG：3785 / OSGEO：41001的具体限制如下：

- 有效经度从-180到180度。

- 有效纬度从 -85.05112878 到 85.05112878 度。

当用户尝试索引超出指定范围的坐标时，该命令将报告错误。

**注意：**没有 **GEODEL** 命令，因为您可以使用 ZREM 来删除元素。地理索引结构只是一个有序集合。

## 它是如何工作的？

排序集的填充方式是使用称为 [Geohash ](https://en.wikipedia.org/wiki/Geohash)的技术。纬度和经度位被交织以形成唯一的52位整数。我们知道有排序的集合双分数可以表示一个52位整数，而不会失去精度。

此格式允许通过检查覆盖整个半径所需的1 + 8区域并丢弃半径外的元素来进行半径查询。通过计算覆盖框的范围来检查区域，从排序集合评分的较低有效部分中移除足够的位，并计算评分范围以在每个区域的排序集合中查询。

## 它使用什么地球模型？

它只是假设地球是一个球体，因为使用的距离公式是 Haversine 公式。这个公式只适用于地球，这不是一个完美的球体。在需要通过 radius 和大多数其他应用程序进行查询的社交网站上下文中使用引入的错误不是问题。然而，在最糟糕的情况下，错误可能高达0.5％，因此您可能需要考虑其他系统的错误关键应用程序。

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- 添加到已排序集合的元素数量，不包括已更新分数的元素。

## 例子

redis> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania" `(integer) 2` redis> GEODIST Sicily Palermo Catania `"166274.1516"` redis> GEORADIUS Sicily 15 37 100 km `1) "Catania"` redis> GEORADIUS Sicily 15 37 200 km `1) "Palermo" 2) "Catania"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18