# restore

```javascript
RESTORE key ttl serialized-value [REPLACE]
```

**自2.6.0起可用。**

**时间复杂度：** O（1）创建新的密钥和附加的O（N * M）来重建序列化的值，其中N是组成该值的Redis对象的数量，M是它们的平均大小。对于小字符串值，时间复杂度因此是O（1）+ O（1 * M），其中M很小，因此简单地为O（1）。然而，对于排序集合值，复杂度为O（N * M * log（N）），因为将值插入到有序集合中的时间为O（log（N））。

创建一个与通过反序列化提供的序列化值（通过 DUMP 获取）获得的值关联的键。

如果`ttl`为0，则密钥创建时不会过期，否则设置指定的到期时间（以毫秒为单位）。

`key`除非使用`REPLACE`修饰符（Redis 3.0或更高版本），否则RESTORE 将在已存在时返回“目标键名称正忙”错误。

RESTORE 检查 RDB 版本和数据校验和。如果它们不匹配，则返回错误。

## 返回值

[简单字符串回复](https://redis.io/topics/protocol#simple-string-reply)：该命令在成功时返回OK。

## 例子

```javascript
redis> DEL mykey
0
redis> RESTORE mykey 0 "\n\x17\x17\x00\x00\x00\x12\x00\x00\x00\x03\x00\
                        x00\xc0\x01\x00\x04\xc0\x02\x00\x04\xc0\x03\x00\
                        xff\x04\x00u#<\xc0;.\xe9\xdd"
OK
redis> TYPE mykey
list
redis> LRANGE mykey 0 -1
1) "1"
2) "2"
3) "3"
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18


  