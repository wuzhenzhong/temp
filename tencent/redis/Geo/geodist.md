# geodist

```javascript
GEODIST key member1 member2 [unit]
```

**自3.2.0起可用。**

**时间复杂度：** O（log（N））

返回排序集合表示的地理空间索引中两个成员之间的距离。

给定一个表示地理空间索引的有序集合，该集合使用 GEOADD 命令填充，该命令返回指定单元中两个指定成员之间的距离。

如果一个或两个成员都缺失，则该命令返回 NULL 。

该单位必须是以下之一，并且默认为米：

- **m** 为米。

- **km** 为千米。

- **mi** 为英里。

- **ft** 为英尺。

假设地球是一个完美的球体，计算距离，因此在边缘情况下可能出现高达0.5％的误差。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)，具体为：

该命令以指定单位的双精度值（以字符串表示）返回距离，如果缺少一个或两个元素，则返回 NULL 。

## 例子

redis> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania" `(integer) 2` redis> GEODIST Sicily Palermo Catania `"166274.1516"` redis> GEODIST Sicily Palermo Catania km `"166.2742"` redis> GEODIST Sicily Palermo Catania mi `"103.3182"` redis> GEODIST Sicily Foo Bar `(nil)`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18