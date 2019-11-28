# georadiusbymember

```javascript
GEORADIUSBYMEMBER key member radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]
```

**自3.2.0起可用。**

**时间复杂度：** O（N + log（M））其中N是由中心和半径界定的圆形区域的边界框内的元素的数量，M 是索引内的项目的数量。

该命令与 GEORADIUS 完全相同，唯一的区别在于，它不是以查询区域的中心为经度和纬度值，而是采用已存在于有序集合所代表的地理空间索引内的成员的名称。

指定成员的位置用作查询的中心。

请查看下面的示例和 GEORADIUS 文档以获取有关该命令及其选项的更多信息。

请注意，`GEORADIUSBYMEMBER_RO`自 Redis 3.2.10 和 Redis 4.0.0 以来，为了提供可用于从服务器的只读命令，此功能也可用。有关更多信息，请参阅 GEORADIUS 页面。

## 例子

redis> GEOADD Sicily 13.583333 37.316667 "Agrigento" `(integer) 1` redis> GEOADD Sicily 13.361389 38.115556 "Palermo" 15.087269 37.502669 "Catania" `(integer) 2` redis> GEORADIUSBYMEMBER Sicily Agrigento 100 km `1) "Agrigento" 2) "Palermo"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18