# Date

- 贡献者1人

  

### _.now()

获取自 Unix 纪元*（1970年1月1日00:00:00 UTC）*以来经过的毫秒数的时间戳。

##### 以来

2.4.0

##### 返回

*（数字）*：返回时间戳（timestamp）。

##### 示例

```javascript
_.defer(function(stamp) {
  console.log(_.now() - stamp);
}, _.now());
// => Logs the number of milliseconds it took for the deferred invocation.
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-07