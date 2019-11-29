# Lang

### _.castArray(value)

`value`如果不是数组，则将其转换为数组。

##### Since

4.4.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

*（Array）*：返回cast数组。

##### Example

```javascript
_.castArray(1);
// => [1]
 
_.castArray({ 'a': 1 });
// => [{ 'a': 1 }]
 
_.castArray('abc');
// => ['abc']
 
_.castArray(null);
// => [null]
 
_.castArray(undefined);
// => [undefined]
 
_.castArray();
// => []
 

var array = [1, 2, 3];

console.log(_.castArray(array) === array);
// => true
```

### _.clone(value)

创建一个浅层克隆`value`。

注意：此方法松散地基于结构化克隆算法，并支持克隆数组，数组缓冲区，布尔值，日期对象，映射，数字，对象对象，正则表达式，集合，字符串，符号和类型化数组。 参数对象的自身枚举属性被克隆为普通对象。 返回一个空对象用于不可克隆的值，例如错误对象，函数，DOM节点和WeakMaps。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：克隆的值。

##### Returns

*（\*）*：返回克隆值。

##### Example

```javascript
var objects = [{ 'a': 1 }, { 'b': 2 }];
 

var shallow = _.clone(objects);

console.log(shallow[0] === objects[0]);
// => true
```

### _.cloneDeep(value)

这个方法就像_.clone，只不过它递归地克隆了值。

##### Since

1.0.0

##### Arguments

1. `value` *（\*）*：递归克隆的值。

##### Returns

*（\*）*：返回深度克隆值。

##### Example

```javascript
var objects = [{ 'a': 1 }, { 'b': 2 }];
 

var deep = _.cloneDeep(objects);

console.log(deep[0] === objects[0]);
// => false
```

### _.cloneDeepWith(value, customizer)

这个方法就像_.cloneWith，只不过它递归地克隆了值。

##### Since

4.0.0

##### Arguments

1. `value` *（\*）*：递归克隆的值。
2. `[customizer]` *（功能）*：定制克隆的功能。

##### Returns

*（\*）*：返回深度克隆值。

##### Example

```javascript
function customizer(value) {
  if (_.isElement(value)) {
    return value.cloneNode(true);
  }
}
 

var el = _.cloneDeepWith(document.body, customizer);
 

console.log(el === document.body);
// => false

console.log(el.nodeName);
// => 'BODY'

console.log(el.childNodes.length);
// => 20
```

### _.cloneWith(value, customizer)

此方法与_.clone相似，只是它接受调用以产生克隆值的定制程序。 如果定制程序返回未定义，则克隆由该方法处理。 定制器最多可以调用四个参数; （值，索引|键，对象，堆栈）。

##### Since

4.0.0

##### Arguments

1. `value` *（\*）*：克隆的值。
2. `[customizer]` *（功能）*：定制克隆的功能。

##### Returns

*（\*）*：返回克隆值。

##### Example

```javascript
function customizer(value) {
  if (_.isElement(value)) {
    return value.cloneNode(false);
  }
}
 

var el = _.cloneWith(document.body, customizer);
 

console.log(el === document.body);
// => false

console.log(el.nodeName);
// => 'BODY'

console.log(el.childNodes.length);
// => 0
```

### _.conformsTo(object, source)

通过使用对象的相应属性值调用源的谓词属性来检查对象是否符合源。

注：当部分应用源时，此方法等同于_.conforms。

##### Since

4.14.0

##### Arguments

1. `object` *（对象）*：要检查的对象。
2. `source` *（Object）*：属性谓词符合的对象。

##### Returns

（boolean）：如果对象符合则返回true，否则返回false。

##### Example

```javascript
var object = { 'a': 1, 'b': 2 };
 
_.conformsTo(object, { 'b': function(n) { return n > 1; } });
// => true
 
_.conformsTo(object, { 'b': function(n) { return n > 2; } });
// => false
```

### _.eq(value, other)

执行[`SameValueZero`](http://ecma-international.org/ecma-262/7.0/#sec-samevaluezero) 两个值之间的比较以确定它们是否相等。

##### Since

4.0.0

##### Arguments

1. `value` *（\*）*：要比较的值。
2. `other` *（\*）*：要比较的另一个值。

##### Returns

（boolean）：如果值相等，则返回true，否则返回false。

##### Example

```javascript
var object = { 'a': 1 };

var other = { 'a': 1 };
 
_.eq(object, object);
// => true
 
_.eq(object, other);
// => false
 
_.eq('a', 'a');
// => true
 
_.eq('a', Object('a'));
// => false
 
_.eq(NaN, NaN);
// => true
```

### _.gt(value, other)

检查是否`value`大于`other`。

##### Since

3.9.0

##### Arguments

1. `value` *（\*）*：要比较的值。
2. `other` *（\*）*：要比较的另一个值。

##### Returns

（布尔值）：如果值大于其他值，则返回true，否则返回false。

##### Example

```javascript
_.gt(3, 1);
// => true
 
_.gt(3, 3);
// => false
 
_.gt(1, 3);
// => false
```

### _.gte(value, other)

检查是否`value`大于或等于`other`。

##### Since

3.9.0

##### Arguments

1. `value` *（\*）*：要比较的值。
2. `other` *（\*）*：要比较的另一个值。

##### Returns

（boolean）：如果value大于或等于other，则返回true，否则返回false。

##### Example

```javascript
_.gte(3, 1);
// => true
 
_.gte(3, 3);
// => true
 
_.gte(1, 3);
// => false
```

### _.isArguments(value)

检查是否`value`可能是一个`arguments`对象。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个参数对象，则返回true，否则返回false。

##### Example

```javascript
_.isArguments(function() { return arguments; }());
// => true
 
_.isArguments([1, 2, 3]);
// => false
```

### _.isArray(value)

检查值是否被分类为数组对象。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个数组，则返回true，否则返回false。

##### Example

```javascript
_.isArray([1, 2, 3]);
// => true
 
_.isArray(document.body.children);
// => false
 
_.isArray('abc');
// => false
 
_.isArray(_.noop);
// => false
```

### _.isArrayBuffer(value)

检查值是否被分类为ArrayBuffer对象。

##### Since

4.3.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个数组缓冲区，则返回true，否则返回false。

##### Example

```javascript
_.isArrayBuffer(new ArrayBuffer(2));
// => true
 
_.isArrayBuffer(new Array(2));
// => false
```

### _.isArrayLike(value)

检查值是否类似数组。 如果一个值不是一个函数，并且value.length是一个大于或等于0且小于或等于Number.MAX_SAFE_INTEGER的整数，则该值被认为是类似数组的。

##### Since

4.0.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

boolean）：如果value是类数组，则返回true，否则返回false。

##### Example

```javascript
_.isArrayLike([1, 2, 3]);
// => true
 
_.isArrayLike(document.body.children);
// => true
 
_.isArrayLike('abc');
// => true
 
_.isArrayLike(_.noop);
// => false
```

### _.isArrayLikeObject(value)

这个方法就像_.isArrayLike，只是它也检查值是否是一个对象。

##### Since

4.0.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个类似数组的对象，则返回true，否则返回false。

##### Example

```javascript
_.isArrayLikeObject([1, 2, 3]);
// => true
 
_.isArrayLikeObject(document.body.children);
// => true
 
_.isArrayLikeObject('abc');
// => false
 
_.isArrayLikeObject(_.noop);
// => false
```

### _.isBoolean(value)

检查值是否被分类为布尔基元或对象。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个布尔值，则返回true，否则返回false。

##### Example

```javascript
_.isBoolean(false);
// => true
 
_.isBoolean(null);
// => false
```

### _.isBuffer(value)

检查是否`value`是缓冲区。

##### Since

4.3.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个缓冲区，则返回true，否则返回false。

##### Example

```javascript
_.isBuffer(new Buffer(2));
// => true
 
_.isBuffer(new Uint8Array(2));
// => false
```

### _.isDate(value)

检查值是否被分类为Date对象。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是日期对象，则返回true，否则返回false。

##### Example

```javascript
_.isDate(new Date);
// => true
 
_.isDate('Mon April 23 2012');
// => false
```

### _.isElement(value)

检查值是否可能是DOM元素。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个DOM元素，则返回true，否则返回false。

##### Example

```javascript
_.isElement(document.body);
// => true
 
_.isElement('<body>');
// => false
```

### _.isEmpty(value)

检查是否`value`为空对象，collection, map,或 set.



如果对象没有自己的可枚举字符串键控属性，则它们被认为是空的。

如果参数对象，数组，缓冲区，字符串或类似jQuery的集合的长度为0，则它们将被视为空。类似地，如果地图和集合的大小为0，则它们将被视为空。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value为空则返回true，否则返回false。

##### Example

```javascript
_.isEmpty(null);
// => true
 
_.isEmpty(true);
// => true
 
_.isEmpty(1);
// => true
 
_.isEmpty([1, 2, 3]);
// => false
 
_.isEmpty({ 'a': 1 });
// => false
```

### _.isEqual(value, other)

在两个值之间进行深入比较以确定它们是否相等。

**注意：**此方法支持比较数组，数组缓冲区，布尔值，日期对象，错误对象，映射，数字， `Object`对象，正则表达式，集合，字符串，符号和类型数组。`Object`对象通过它们自己的而不是继承的可枚举属性进行比较。函数和DOM节点通过严格的等式进行比较，即`===`。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要比较的值。
2. `other` *（\*）*：要比较的另一个值。

##### Returns

（boolean）：如果值相等，则返回true，否则返回false。

##### Example

```javascript
var object = { 'a': 1 };

var other = { 'a': 1 };
 
_.isEqual(object, other);
// => true
 
object === other;
// => false
```

### _.isEqualWith(value, other, customizer)

此方法与_.isEqual类似，只是它接受调用以比较值的定制程序。 如果定制程序返回未定义，则比较由该方法处理。 定制器最多可以调用6个参数：（objValue，othValue，index | key，object，other，stack）。

##### Since

4.0.0

##### Arguments

1. `value` *（\*）*：要比较的值。
2. `other` *（\*）*：要比较的另一个值。
3. `[customizer]` *（功能）*：自定义比较的功能。

##### Returns

（boolean）：如果值相等，则返回true，否则返回false。

##### Example

```javascript
function isGreeting(value) {
  return /^h(?:i|ello)$/.test(value);
}
 

function customizer(objValue, othValue) {
  if (isGreeting(objValue) && isGreeting(othValue)) {
    return true;
  }
}
 

var array = ['hello', 'goodbye'];

var other = ['hi', 'goodbye'];
 
_.isEqualWith(array, other, customizer);
// => true
```

### _.isError(value)

检查值是否为Error，EvalError，RangeError，ReferenceError，SyntaxError，TypeError或URIError对象。

##### Since

3.0.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个错误对象，则返回true，否则返回false。

##### Example

```javascript
_.isError(new Error);
// => true
 
_.isError(Error);
// => false
```

### _.isFinite(value)

检查是否`value`是有限的原始数字。

**注意：**此方法基于[`Number.isFinite`](https://mdn.io/Number/isFinite)。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是有限数字，则返回true，否则返回false。

##### Example

```javascript
_.isFinite(3);
// => true
 
_.isFinite(Number.MIN_VALUE);
// => true
 
_.isFinite(Infinity);
// => false
 
_.isFinite('3');
// => false
```

### _.isFunction(value)

检查值是否被分类为Function对象。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个函数，则返回true，否则返回false。

##### Example

```javascript
_.isFunction(_);
// => true
 
_.isFunction(/abc/);
// => false
```

### _.isInteger(value)

检查是否`value`是整数。

**注意：**此方法基于[`Number.isInteger`](https://mdn.io/Number/isInteger)。

##### Since

4.0.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个整数，则返回true，否则返回false。

##### Example

```javascript
_.isInteger(3);
// => true
 
_.isInteger(Number.MIN_VALUE);
// => false
 
_.isInteger(Infinity);
// => false
 
_.isInteger('3');
// => false
```

### _.isLength(value)

检查值是否是一个有效的类似数组的长度。

**注意：**这个方法松散地基于[`ToLength`](http://ecma-international.org/ecma-262/7.0/#sec-tolength)。

##### Since

4.0.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是有效长度，则返回true，否则返回false。

##### Example

```javascript
_.isLength(3);
// => true
 
_.isLength(Number.MIN_VALUE);
// => false
 
_.isLength(Infinity);
// => false
 
_.isLength('3');
// => false
```

### _.isMap(value)

检查值是否被分类为Map对象。

##### Since

4.3.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个映射，则返回true，否则返回false。

##### Example

```javascript
_.isMap(new Map);
// => true
 
_.isMap(new WeakMap);
// => false
```

### _.isMatch(object, source)

在对象和源之间执行部分深层比较以确定对象是否包含等效属性值。

注意：当部分应用源时，此方法等同于_.matches。

部分比较将分别将空数组和空对象源值与任何数组或对象值进行匹配。 请参阅_.isEqual以获取支持的值比较列表。

##### Since

3.0.0

##### Arguments

1. `object` *（对象）*：要检查的对象。
2. `source` *（Object）*：要匹配的属性值的对象。

##### Returns

（boolean）：如果object匹配，则返回true，否则返回false。

##### Example

```javascript
var object = { 'a': 1, 'b': 2 };
 
_.isMatch(object, { 'b': 2 });
// => true
 
_.isMatch(object, { 'b': 1 });
// => false
```

### _.isMatchWith(object, source, customizer)

此方法与_.isMatch类似，不同之处在于它接受调用以比较值的定制程序。 如果定制程序返回未定义，则比较由该方法处理。 定制器使用五个参数调用：（objValue，srcValue，index | key，object，source）。

##### Since

4.0.0

##### Arguments

1. `object` *（对象）*：要检查的对象。
2. `source` *（Object）*：要匹配的属性值的对象。
3. `[customizer]` *（功能）*：自定义比较的功能。

##### Returns

（boolean）：如果object匹配，则返回true，否则返回false。

##### Example

```javascript
function isGreeting(value) {
  return /^h(?:i|ello)$/.test(value);
}
 

function customizer(objValue, srcValue) {
  if (isGreeting(objValue) && isGreeting(srcValue)) {
    return true;
  }
}
 

var object = { 'greeting': 'hello' };

var source = { 'greeting': 'hi' };
 
_.isMatchWith(object, source, customizer);
// => true
```

### _.isNaN(value)

检查值是否为NaN。

注意：此方法基于Number.isNaN，与全局isNaN不同，后者对未定义的值和其他非数值返回true。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果值为NaN，则返回true，否则返回false。

##### Example

```javascript
_.isNaN(NaN);
// => true
 
_.isNaN(new Number(NaN));
// => true
 

isNaN(undefined);
// => true
 
_.isNaN(undefined);
// => false
```

### _.isNative(value)

检查是否`value`是原始的本机功能。

**注意：**由于core-js绕过了这种检测，因此此方法无法可靠地检测到存在core-js包时的本机函数。尽管有多个请求，core-js维护人员已经明确表示：任何修复检测的尝试都会受到阻碍。因此，我们别无选择，只能抛出一个错误。不幸的是，这也会影响包，比如[babel-polyfill](https://www.npmjs.com/package/babel-polyfill)，它依赖于core-js。

##### Since

3.0.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是本地函数，则返回true，否则返回false。

##### Example

```javascript
_.isNative(Array.prototype.push);
// => true
 
_.isNative(_);
// => false
```

### _.isNil(value)

检查是否`value`为`null`或`undefined`。

##### Since

4.0.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value为nullish，则返回true，否则返回false。

##### Example

```javascript
_.isNil(null);
// => true
 
_.isNil(void 0);
// => true
 
_.isNil(NaN);
// => false
```

### _.isNull(value)

检查是否`value`是`null`。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value为null，则返回true，否则返回false。

##### Example

```javascript
_.isNull(null);
// => true
 
_.isNull(void 0);
// => false
```

### _.isNumber(value)

检查是否`value`被分类为`Number`基元或对象。

**注意：**要排除`Infinity`，`-Infinity`和`NaN`分类为数字，请使用该`_.isFinite`方法。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个数字，则返回true，否则返回false。

##### Example

```javascript
_.isNumber(3);
// => true
 
_.isNumber(Number.MIN_VALUE);
// => true
 
_.isNumber(Infinity);
// => true
 
_.isNumber('3');
// => false
```

### _.isObject(value)

检查是否`value`是[语言类](http://www.ecma-international.org/ecma-262/7.0/#sec-ecmascript-language-types)的`Object`。 *（例如数组，函数，对象，正则表达式，* `*new Number(0)*`*__和* `*new String('')*`*__）*

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个对象，则返回true，否则返回false。

##### Example

```javascript
_.isObject({});
// => true
 
_.isObject([1, 2, 3]);
// => true
 
_.isObject(_.noop);
// => true
 
_.isObject(null);
// => false
```

### _.isObjectLike(value)

检查是否`value`像对象一样。如果值不是 “对象” `null`，并且`typeof`结果为“对象”，则值为对象。

##### Since

4.0.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果值是对象状态，则返回true，否则返回false。

##### Example

```javascript
_.isObjectLike({});
// => true
 
_.isObjectLike([1, 2, 3]);
// => true
 
_.isObjectLike(_.noop);
// => false
 
_.isObjectLike(null);
// => false
```

### _.isPlainObject(value)

检查是否`value`是一个普通的对象，即，由所述创建的对象`Object`构造器或是用`[[Prototype]]`的`null`。

##### Since

0.8.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个普通对象，则返回true，否则返回false。

##### Example

```javascript
function Foo() {
  this.a = 1;
}
 
_.isPlainObject(new Foo);
// => false
 
_.isPlainObject([1, 2, 3]);
// => false
 
_.isPlainObject({ 'x': 0, 'y': 0 });
// => true
 
_.isPlainObject(Object.create(null));
// => true
```

### _.isRegExp(value)

检查是否`value`被分类为`RegExp`对象。

##### Since

0.1.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

（boolean）：如果value是一个正则表达式，则返回true，否则返回false。

##### Example

```javascript
_.isRegExp(/abc/);
// => true
 
_.isRegExp('/abc/');
// => false
```

### _.isSafeInteger(value)

检查是否`value`是安全整数。一个整数是安全的，如果它是一个IEEE-754双精度数字，而不是一个四舍五入的不安全整数的结果。

**注意：**此方法基于[`Number.isSafeInteger`](https://mdn.io/Number/isSafeInteger)。

##### Since

4.0.0

##### Arguments

1. `value` *（\*）*：要检查的值。

##### Returns

*（boolean）*：如果 `value`为安全整数，返回`true`，否则返回`false`。

##### Example

```javascript
_.isSafeInteger(3);
// => true
 
_.isSafeInteger(Number.MIN_VALUE);
// => false
 
_.isSafeInteger(Infinity);
// => false
 
_.isSafeInteger('3');
// => false
```

### _.isSet(value)

检查是否`value`被分类为`Set`对象。

##### Since

4.3.0

##### Arguments

- `value` *（\*）*：要检查的值。

##### Returns

*（boolean）*：返回`true`是否`value`是一个集合，否则`false`。

##### Example

```javascript
_.isSet(new Set);
// => true
 
_.isSet(new WeakSet);
// => false
```

### _.isString(value)

检查`value`是否被分类为`String`基元或对象。

##### Since

0.1.0

##### Arguments

- `value` *(\*)*：要检查的值。

##### Returns

*（boolean）*：返回true当value是符号的时候，否则返回false。



##### Example

```javascript
_.isString('abc');
// => true
 
_.isString(1);
// => false
```

### _.isSymbol(value)

检查`value`是否被分类为`Symbol`基元或对象。

##### Since

4.0.0

##### Arguments

- `value` *(\*)*：要检查的值。

##### Returns

*（boolean）*：返回`true`当`value`是符号的时候，否则返回`false`。

##### Example

```javascript
_.isSymbol(Symbol.iterator);
// => true
 
_.isSymbol('abc');
// => false
```

### _.isTypedArray(value)

检查`value`是否被分类为类型数组。

##### Since

3.0.0

##### Arguments

- `value` *(\*)*：要检查的值。

##### Returns

*（boolean）*：返回`true`当`value`是符号的时候，否则返回`false`。



##### Example

```javascript
_.isTypedArray(new Uint8Array);
// => true
 
_.isTypedArray([]);
// => false
```

### _.isUndefined(value)

检查`value`值是否是`nudefined`。

##### Since

0.1.0

##### Arguments

- `value` *(\*)*：要检查的值。

##### Returns

*（boolean）*：返回`true`当`value`是符号的时候，否则返回`false`。



##### Example

```javascript
_.isUndefined(void 0);
// => true
 
_.isUndefined(null);
// => false
```

### _.isWeakMap(value)

检查`value`是否被分类为`WeakMap`对象。

##### Since

4.3.0

##### Arguments

- `value` *（\*）*：要检查的值。

##### Returns

*（boolean）*：返回`true`如果当`value`是弱映射，否则为`false`。

##### Example

```javascript
_.isWeakMap(new WeakMap);
// => true
 
_.isWeakMap(new Map);
// => false
```

### _.isWeakSet(value)

检查`value`是否被分类为`WeakSet`对象。

##### Since

4.3.0

##### Arguments

- `value` *（\*）*：要检查的值。

##### Returns

*（boolean）*：返回`true`如果`value`是弱集，否则为`false`。

##### Example

```javascript
_.isWeakSet(new WeakSet);
// => true
 
_.isWeakSet(new Set);
// => false
```

### _.lt(value, other)

检查是否`value`小于`other`。

##### Since

3.9.0

##### Arguments

1. `value` *(\*)*：要比较的值。
2. `other` *(\*)*：要比较的另一个值。

##### Returns

*（boolean）*：返回`true如果value`小于`other`否则返回`false`。

##### Example

```javascript
_.lt(1, 3);
// => true
 
_.lt(3, 3);
// => false
 
_.lt(3, 1);
// => false
```

### _.lte(value, other)

检查是否`value`小于或等于`other`。

##### Since

3.9.0

##### Arguments

1. `value` *(\*)*：要比较的值。
2. `other` *(\*)*：要比较的另一个值。

##### Returns

*（boolean）*：返回`true`如果`value`小于或等于`other`否则为`false`。

##### Example

```javascript
_.lte(1, 3);
// => true
 
_.lte(3, 3);
// => true
 
_.lte(3, 1);
// => false
```

### _.toArray(value)

将`value`转换为数组。

##### Since

0.1.0

##### Arguments

- `value` *(\*)*：要转换的值。

##### Returns

*（Array）*：返回转换后的数组。

##### Example

```javascript
_.toArray({ 'a': 1, 'b': 2 });
// => [1, 2]
 
_.toArray('abc');
// => ['a', 'b', 'c']
 
_.toArray(1);
// => []
 
_.toArray(null);
// => []
```

### _.toFinite(value)

转换`value`为有限数字。

##### Since

4.12.0

##### Arguments

- `value` (**)*：要转换的值。

##### Returns

*(number)*：返回转换后的数字。

##### Example

```javascript
_.toFinite(3.2);
// => 3.2
 
_.toFinite(Number.MIN_VALUE);
// => 5e-324
 
_.toFinite(Infinity);
// => 1.7976931348623157e+308
 
_.toFinite('3.2');
// => 3.2
```

### _.toInteger(value)

转换`value`为整数。

**注意：**这个方法松散地基于[`ToInteger`](http://www.ecma-international.org/ecma-262/7.0/#sec-tointeger)。

##### Since

4.0.0

##### Arguments

- `value` *(\*)*：要转换的值。

##### Returns

*(number)*：返回转换后的整数。

##### Example

```javascript
_.toInteger(3.2);
// => 3
 
_.toInteger(Number.MIN_VALUE);
// => 0
 
_.toInteger(Infinity);
// => 1.7976931348623157e+308
 
_.toInteger('3.2');
// => 3
```

### _.toLength(value)

转换`value`为适合用作类似数组的对象的长度的整数。

**注意：** 此方法基于[`ToLength`](http://ecma-international.org/ecma-262/7.0/#sec-tolength)。

##### Since

4.0.0

##### Arguments

1. `value` *(\*)*：要转换的值。

##### Returns

*(number)*：返回转换后的整数。

##### Example

```javascript
_.toLength(3.2);
// => 3
 
_.toLength(Number.MIN_VALUE);
// => 0
 
_.toLength(Infinity);
// => 4294967295
 
_.toLength('3.2');
// => 3
```

### _.toNumber(value)

转换`value`为数字。

##### Since

4.0.0

##### Arguments

- `value` *(\*)*：要处理的值。

##### Returns

*(number)*：返回数值。

##### Example

```javascript
_.toNumber(3.2);
// => 3.2
 
_.toNumber(Number.MIN_VALUE);
// => 5e-324
 
_.toNumber(Infinity);
// => Infinity
 
_.toNumber('3.2');
// => 3.2
```

### _.toPlainObject(value)

将`value`简单展开为继承可枚举字符串键属性，以`value`为自己的属性。

##### Since

3.0.0

##### Arguments

- `value` *(\*）*：要转换的值。

##### Returns

*（Object）*：返回转换后的普通对象。

##### Example

```javascript
function Foo() {
  this.b = 2;
}
 
Foo.prototype.c = 3;
 
_.assign({ 'a': 1 }, new Foo);
// => { 'a': 1, 'b': 2 }
 
_.assign({ 'a': 1 }, _.toPlainObject(new Foo));
// => { 'a': 1, 'b': 2, 'c': 3 }
```

### _.toSafeInteger(value)

转换`value`为安全整数。一个安全的整数可以被正确比较和表示。

##### Since

4.0.0

##### Arguments

- `value` *(\*)*：要转换的值。

##### Returns

*(number)*：返回转换后的整数。

##### Example

```javascript
_.toSafeInteger(3.2);
// => 3
 
_.toSafeInteger(Number.MIN_VALUE);
// => 0
 
_.toSafeInteger(Infinity);
// => 9007199254740991
 
_.toSafeInteger('3.2');
// => 3
```

### _.toString(value)

转换`value`为字符串。返回空字符串`null`和`undefined` 值。`-0`为保存的标志。

##### Since

4.0.0

##### Arguments

- `value` *(\*)*：要转换的值。

##### Returns

*(string)*：返回转换后的字符串。

##### Example

```javascript
_.toString(null);
// => ''
 
_.toString(-0);
// => '-0'
 
_.toString([1, 2, 3]);
// => '1,2,3'
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-07