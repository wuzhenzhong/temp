# Repeat()

返回一个`value`的 Seq.Indexed 重复`times`次数。当`times`没有定义时，返回一个无限`Seq`的`value`。

```javascript
Repeat<T>(value: T, times?: number): Seq.Indexed<T>
```

#### 示例

```javascript
Repeat('foo') // ['foo','foo','foo',...]
Repeat('bar',4) // ['bar','bar','bar','bar']
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-04-11