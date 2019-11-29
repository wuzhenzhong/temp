# Range()

返回一个 Seq.dexed（从）`start`（包括）到`end`（排除），by `step`，`start`默认为0，`step`到1，`end`到无穷大。当`start`等于`end`，则返回空范围。

```javascript
Range(start?: number, end?: number, step?: number): Seq.Indexed<number>
```

#### 示例

```javascript
Range() // [0,1,2,3,...]
Range(10) // [10,11,12,13,...]
Range(10,15) // [10,11,12,13,14]
Range(10,30,5) // [10,15,20,25]
Range(30,10,5) // [30,25,20,15]
Range(30,30,5) // []
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-04-11