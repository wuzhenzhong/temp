# Math

### _.add(augend, addend)

添加两个数字。

##### 初始

3.4.0

##### 参数

1. `augend` *（数字）*：另外的第一个数字。
2. `addend` *（数字）*：另外一个数字。

##### 返回

*（数字）*：返回总数。

##### 例

```javascript
_.add(6, 4);
// => 10
```

### _.ceil(number, precision=0)

计算四舍五入到精度的数字。

##### 初始

3.10.0

##### 参数

1. `number` *（数字）*：收起的数字。
2. `[precision=0]` *（数字）*：精确到四舍五入。

##### 返回

*（数字）*：返回向上取整的数字。

##### 例

```javascript
_.ceil(4.006);
// => 5
 
_.ceil(6.004, 2);
// => 6.01
 
_.ceil(6040, -2);
// => 6100
```

### _.divide(dividend, divisor)

除以两个数字。

##### 初始

4.7.0

##### 参数

1. `dividend` *（数字）*：分部中的第一个数字。
2. `divisor` *（数字）*：分部中的第二个数字。

##### 返回

*（数字）*：返回商。

##### 例

```javascript
_.divide(6, 4);
// => 1.5
```

### _.floor(number, precision=0)

计算舍入到精度的数。

##### 初始

3.10.0

##### 参数

1. `number` *（数字）*：要舍入的数字。
2. `[precision=0]` *（数字）*：向下舍入的精度。

##### 返回

*（数字）*：返回向下舍入的数字。

##### 例

```javascript
_.floor(4.006);
// => 4
 
_.floor(0.046, 2);
// => 0.04
 
_.floor(4060, -2);
// => 4000
```

### _.max(array)

计算数组的最大值。 如果数组为空或falsey，则返回undefined。

##### 初始

0.1.0

##### 参数

1. `array` *（Array）*：迭代的数组。

##### 返回

*（\*）*：返回最大值。

##### 例

```javascript
_.max([4, 2, 8, 6]);
// => 8
 
_.max([]);
// => undefined
```

### _.maxBy(array, iteratee=_.identity)

此方法与_.max类似，不同之处在于它接受针对数组中每个元素调用的iteratee，以生成排序值的标准。 迭代器被调用一个参数：（value）。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：迭代的数组。
2. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（\*）*：返回最大值。

##### 例

```javascript
var objects = [{ 'n': 1 }, { 'n': 2 }];
 
_.maxBy(objects, function(o) { return o.n; });
// => { 'n': 2 }
 
// The `_.property` iteratee shorthand.
_.maxBy(objects, 'n');
// => { 'n': 2 }
```

### _.mean(array)

计算中`array`的值的平均值。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：迭代的数组。

##### 返回

*（数字）*：返回平均值。

##### 例

```javascript
_.mean([4, 2, 8, 6]);
// => 5
```

### _.meanBy(array, iteratee=_.identity)

这个方法就像_.mean，只是它接受为数组中每个元素调用的iteratee来生成要进行平均的值。 迭代器被调用一个参数：（value）。

##### 初始

4.7.0

##### 参数

1. `array` *（Array）*：迭代的数组。
2. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（数字）*：返回平均值。

##### 例

```javascript
var objects = [{ 'n': 4 }, { 'n': 2 }, { 'n': 8 }, { 'n': 6 }];
 
_.meanBy(objects, function(o) { return o.n; });
// => 5
 
// The `_.property` iteratee shorthand.
_.meanBy(objects, 'n');
// => 5
```

### _.min(array)

计算数组的最小值。 如果数组为空或falsey，则返回undefined。

##### 初始

0.1.0

##### 参数

1. `array` *（Array）*：迭代的数组。

##### 返回

*（\*）*：返回最小值。

##### 例

```javascript
_.min([4, 2, 8, 6]);
// => 2
 
_.min([]);
// => undefined
```

### _.minBy(array, iteratee=_.identity)

此方法与_.min类似，不同之处在于它接受针对数组中每个元素调用的迭代器，以生成排序值的标准。 迭代器被调用一个参数：（value）。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：迭代的数组。
2. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（\*）*：返回最小值。

##### 例

```javascript
var objects = [{ 'n': 1 }, { 'n': 2 }];
 
_.minBy(objects, function(o) { return o.n; });
// => { 'n': 1 }
 
// The `_.property` iteratee shorthand.
_.minBy(objects, 'n');
// => { 'n': 1 }
```

### _.multiply(multiplier, multiplicand)

乘以两个数字。

##### 初始

4.7.0

##### 参数

1. `multiplier` *（数字）*：乘法中的第一个数字。
2. `multiplicand` *（数字）*：乘法中的第二个数字。

##### 返回

*（数）*：返回产品。

##### 例

```javascript
_.multiply(6, 4);
// => 24
```

### _.round(number, precision=0)

计算舍入到精度的数字。

##### 初始

3.10.0

##### 参数

1. `number` *（数）*：要循环的数字。
2. `[precision=0]` *（数）*：精确到四舍五入。

##### 返回

*（数字）*：返回舍入的数字。

##### 例

```javascript
_.round(4.006);
// => 4
 
_.round(4.006, 2);
// => 4.01
 
_.round(4060, -2);
// => 4100
```

### _.subtract(minuend, subtrahend)

减去两个数字。

##### 初始

4.0.0

##### 参数

1. `minuend` *（数字）*：减法中的第一个数字。
2. `subtrahend` *（数字）*：减法中的第二个数字。

##### 返回

*（数字）*：返回差异。

##### 例

```javascript
_.subtract(6, 4);
// => 2
```

### _.sum(array)

计算`array`中的值的总和。

##### 初始

3.4.0

##### 参数

1. `array` *（Array）*：迭代的数组。

##### 返回

*（数字）*：返回总和。

##### 例

```javascript
_.sum([4, 2, 8, 6]);
// => 20
```

### _.sumBy(array, iteratee=_.identity)

此方法与_.sum相似，不同之处在于它接受为数组中的每个元素调用的iteratee以生成要进行求和的值。 迭代器被调用一个参数：（value）。

##### 初始

4.0.0

##### 参数

1. `array` *（Array）*：迭代的数组。
2. `[iteratee=_.identity]` *（Function）*：每个元素调用的迭代器。

##### 返回

*（数字）*：返回总和。

##### 例

```javascript
var objects = [{ 'n': 4 }, { 'n': 2 }, { 'n': 8 }, { 'n': 6 }];
 
_.sumBy(objects, function(o) { return o.n; });
// => 20
 
// The `_.property` iteratee shorthand.
_.sumBy(objects, 'n');
// => 20
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-07