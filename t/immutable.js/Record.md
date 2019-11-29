# Record

创建一个生成 Record 实例的新类。记录类似于 JS 对象，但强制允许一组特定的字符串键，并具有默认值。

#### 示例

```javascript
var ABRecord = Record({a:1, b:2})
var myRecord = new ABRecord({b:3})
```

记录始终具有它们定义的键的值。`remove`从记录中取出一个密钥只需将其重置为该密钥的默认值即可。

```javascript
myRecord.size // 2
myRecord.get('a') // 1
myRecord.get('b') // 3
myRecordWithoutB = myRecord.remove('b')
myRecordWithoutB.get('b') // 2
myRecordWithoutB.size // 2
```

提供给记录类型中找不到的构造函数的值将被忽略。例如，在这种情况下，即使仅定义了“a”和“b”，ABRecord 也被提供了键“x”。此记录将忽略“x”的值。

```javascript
var myRecord = new ABRecord({b:3, x:10})
myRecord.get('x') // undefined
```

由于记录有一组已知的字符串键，属性获取访问按预期工作，但属性集将引发错误。

注意：IE8不支持属性访问。仅`get()`在支持 IE8时使用。

```javascript
myRecord.b // 3
myRecord.b = 5 // throws Error
```

记录类也可以扩展，允许记录上的自定义方法。这不是功能环境中的常见模式，但在许多 JS 程序中。

注意：TypeScript 不支持这种类型的子类。

```javascript
class ABRecord extends Record({a:1,b:2}) {
  getAB() {
    return this.a + this.b;
  }
}

var myRecord = new ABRecord({b: 3})
myRecord.getAB() // 4
```

## 建设

### Record()

```javascript
Record(defaultValues: {[key: string]: any}, name?: string): Record.Class
```

## 类型

Record.Class

```javascript
class Record.Class
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-04-11