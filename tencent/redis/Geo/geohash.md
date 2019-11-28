# geohash

```javascript
GEOHASH key member [member ...]
```

**自3.2.0起可用。**

**时间复杂度：**每个请求成员的 O（log（N）），其中 N 是有序集合中元素的数量。

返回表示地理空间索引（使用 GEOADD 添加元素）的排序集值中一个或多个元素位置的有效 [Geohash ](https://en.wikipedia.org/wiki/Geohash)字符串。

通常，Redis 使用 Geohash 技术的变体来表示元素的位置，其中位置使用52位整数进行编码。编码与标准相比也不同，因为在编码和解码过程中使用的最初的最小和最大坐标是不同的。然而，该命令以[维基百科文章](https://en.wikipedia.org/wiki/Geohash)中所述的形式**返回标准 Geohash** ，并与 [geohash.org ](http://geohash.org/)网站兼容。

## Geohash字符串属性

该命令返回11个字符的 Geohash 字符串，因此与 Redis 内部52位表示相比，没有任何精度损失。返回的 Geohashes 具有以下属性：

\1. 他们可以缩短删除右侧的字符。它会失去精确度，但仍会指向同一区域。

\2. 可以在`geohash.org` URL 中使用它们，例如`http://geohash.org/<geohash-string>`。这是[这种 URL 的](http://geohash.org/sqdtr74hyu0)一个[例子](http://geohash.org/sqdtr74hyu0)。

\3. 带有相似前缀的字符串在附近，但相反的情况并非如此，有可能前缀不同的字符串也在附近。

## 返回值

[阵列回复](https://redis.io/topics/protocol#array-reply)，具体为：

该命令返回一个数组，其中每个元素是与作为参数传递给该命令的每个成员名称对应的 Geohash 。

## 例子

redis> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania" `(integer) 2` redis> GEOHASH Sicily Palermo Catania `1) "sqc8b49rny0" 2) "sqdtr74hyu0"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18