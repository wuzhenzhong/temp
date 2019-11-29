# Number

### _.clamp(number, lower, upper)

对`number`获取`lower`和`upper`之间的数字

##### 版本

4.0.0

##### 参数

1. `number` *（数字）*：要钳位的数字。
2. `[lower]` *（数字）*：下限。
3. `upper` *（数字）*：上限。

##### 返回

*（number）*：返回数字。

##### 例

```javascript
_.clamp(-10, -5, 5);
// => -5
 
_.clamp(10, -5, 5);
// => 5
```

### _.inRange(number, start=0, end)

检查是否`n`在`start`和之间，但不包括，`end`。如果 `end`未指定，则设置为`start`，`start`然后设置为`0`。如果`start`大于`end`参数交换来支持负范围。

##### 版本

3.3.0

##### 参数

1. `number` *（号码）*：要检查的号码。
2. `[start=0]` *（数字）*：范围的开始。
3. `end` *（数字）*：范围的结尾。

##### 返回

*（boolean）*：返回`true`是否`number`在范围内，否则`false`。

##### 例

```javascript
_.inRange(3, 2, 4);
// => true
 
_.inRange(4, 8);
// => true
 
_.inRange(4, 2);
// => false
 
_.inRange(2, 2);
// => false
 
_.inRange(1.2, 2);
// => true
 
_.inRange(5.2, 4);
// => false
 
_.inRange(-3, -2, -6);
// => true
```

### _.random（lower = 0，upper = 1，floating）

在包含`lower`和`upper`边界之间产生一个随机数。如果只提供一个参数`0`，则返回给定数字之间的数字。如果`floating`是 `true`，或者是`lower`或者`upper`是浮点数，则返回一个浮点数而不是整数。

**注意：** JavaScript遵循IEEE-754标准来解决可能产生意外结果的浮点值。

**版本**

0.7.0

##### 参数

1. `[lower=0]` *(number)*: The lower bound.
2. `[upper=1]` *(number)*: The upper bound.
3. `[floating]` *(boolean)*: Specify returning a floating-point number.

##### 返回

*(number)*: Returns the random number.

##### 例

```javascript
_.random(0, 5);
// => an integer between 0 and 5
 
_.random(5);
// => also an integer between 0 and 5
 
_.random(5, true);
// => a floating-point number between 0 and 5
 
_.random(1.2, 5.2);
// => a floating-point number between 1.2 and 5.2
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-07