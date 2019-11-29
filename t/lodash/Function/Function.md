# Function

### _.after(n, func)

与_.before的相反; 这个方法创建一个调用func的函数，一旦它被调用n次或更多次。

##### 初始

0.1.0

##### 参数

1. `n` *（number）*：调用前`func`的调用次数。
2. `func` *（功能）*：限制功能。

##### 返回

*（功能）*：返回新的限制功能。

##### 例

```javascript
var saves = ['profile', 'settings'];
 

var done = _.after(saves.length, function() {
  console.log('done saving!');
});
 
_.forEach(saves, function(type) {
  asyncSave({ 'type': type, 'complete': done });
});
// => Logs 'done saving!' after the two async saves have completed.
```

### _.ary(func, n=func.length)

创建一个函数，调用`func`最多`n`参数，忽略任何附加参数。

##### 初始

3.0.0

##### 参数

1. `func` *（函数）*：为参数加上参数的函数。
2. `[n=func.length]` *（数字）*：上限。

##### 返回

*（功能）*：返回新的上限功能。

##### 例

```javascript
_.map(['6', '8', '10'], _.ary(parseInt, 1));
// => [6, 8, 10]
```

### _.before(n, func)

创建一个函数，该函数使用创建的函数`func`的`this`绑定和参数调用，但调用时间少于`n`次数。随后对创建函数的`func`调用返回上次调用的结果。

##### 初始

3.0.0

##### 参数

1. `n` *（序号）*：`func`不再被调用的呼叫数量。
2. `func` *（功能）*：限制功能。

##### 返回

*（功能）*：返回新的限制功能。

##### 例

```javascript
jQuery(element).on('click', _.before(5, addContactToList));
// => Allows adding up to 4 contacts to the list.
```

### _.bind(func, thisArg, partials)

创建一个函数，`func`用它接收的参数的`this`绑定`thisArg`和 `partials`前缀进行调用。

该`_.bind.placeholder`值默认为`_`单体版本，可用作部分应用参数的占位符。

注意：与原生函数Function#bind绑定不同，此方法不会设置绑定函数的“长度”属性。

##### 初始

0.1.0

##### 参数

1. `func` *（功能）*：绑定的功能。
2. `thisArg` *（\*）*：的`this`绑定`func`。
3. `[partials]` *（... \*）*：部分应用的参数。

##### 返回

*（函数）*：返回新的绑定函数。

##### 例

```javascript
function greet(greeting, punctuation) {
  return greeting + ' ' + this.user + punctuation;
}
 

var object = { 'user': 'fred' };
 

var bound = _.bind(greet, object, 'hi');

bound('!');
// => 'hi fred!'
 
// Bound with placeholders.

var bound = _.bind(greet, object, _, '!');

bound('hi');
// => 'hi fred!'
```

### _.bindKey(object, key, partials)

创建一个函数，该函数调用前面的方法，`object[key]`并在其`partials`前面接收参数。

此方法不同于`_.bind`允许绑定函数引用可能被重新定义或尚不存在的方法。

该`_.bindKey.placeholder`值默认为`_`单体版本，可用作部分应用参数的占位符。

##### 初始

0.10.0

##### 参数

1. `object` *（Object）*：调用该方法的对象。
2. `key` *（字符串）*：方法的关键。
3. `[partials]` *（... \*）*：部分应用的参数。

##### 返回

*（函数）*：返回新的绑定函数。

##### 例

```javascript
var object = {
  'user': 'fred',

  'greet': function(greeting, punctuation) {
    return greeting + ' ' + this.user + punctuation;
  }
};
 

var bound = _.bindKey(object, 'greet', 'hi');

bound('!');
// => 'hi fred!'
 
object.greet = function(greeting, punctuation) {
  return greeting + 'ya ' + this.user + punctuation;
};
 

bound('!');
// => 'hiya fred!'
 
// Bound with placeholders.

var bound = _.bindKey(object, 'greet', _, '!');

bound('hi');
// => 'hiya fred!'
```

### _.curry(func, arity=func.length)

创建一个函数，该函数接受func的参数，并调用func返回其结果，如果至少提供了arity参数数量，或者返回接受剩余func参数的函数，依此类推。 如果func.length不够，func的arity可能被指定。

_.curry.placeholder值，默认为单个构建中的_，可用作所提供参数的占位符。

**注意：**此方法不会设置curried函数的“length”属性。

##### 初始

2.0.0

##### 参数

1. `func` *（功能）*：当前的功能。
2. `[arity=func.length]` *（数字）*：`func`的元数

##### 返回

*（功能）*：返回新的当前功能。

##### 例

```javascript
var abc = function(a, b, c) {
  return [a, b, c];
};
 

var curried = _.curry(abc);
 

curried(1)(2)(3);
// => [1, 2, 3]
 

curried(1, 2)(3);
// => [1, 2, 3]
 

curried(1, 2, 3);
// => [1, 2, 3]
 
// Curried with placeholders.

curried(1)(_, 3)(2);
// => [1, 2, 3]
```

### _.curryRight(func, arity=func.length)

此方法与_.curry类似，只是参数以_.partialRight而不是_.partial的方式应用于func。

_.curryRight.placeholder值默认为单个构建中的_，可用作所提供参数的占位符。

**注意：**此方法不会设置curried函数的“length”属性。

##### 初始

3.0.0

##### 参数

1. `func` *（功能）*：当前的功能。
2. `[arity=func.length]` *（数字）*：`func`的元数

##### 返回

*（功能）*：返回新的当前功能。

##### 例

```javascript
var abc = function(a, b, c) {
  return [a, b, c];
};
 

var curried = _.curryRight(abc);
 

curried(3)(2)(1);
// => [1, 2, 3]
 

curried(2, 3)(1);
// => [1, 2, 3]
 

curried(1, 2, 3);
// => [1, 2, 3]
 
// Curried with placeholders.

curried(3)(1, _)(2);
// => [1, 2, 3]
```

### _.debounce(func, wait=0, options={})

创建一个去抖动函数，该函数会延迟调用func，直到自上次调用去抖函数后等待几毫秒后。 去抖动函数带有一个取消方法来取消延迟的func调用和一个flush方法来立即调用它们。 提供选项以指示是否应在等待超时的前沿和/或后沿调用func。 func被提供给去抖动函数的最后一个参数调用。 随后调用debounced函数返回最后一次func调用的结果。

注意：如果前导和尾随选项为true，则只有在等待超时期间多次调用debounced函数时，才会在超时的后沿调用func。

如果wait为0且leading为false，则func调用被推迟到下一个tick，类似于超时值为0的setTimeout。

##### 初始

0.1.0

##### 参数

1. `func` *（功能）*：去抖功能。
2. `[wait=0]` *（数字）*：延迟的毫秒数。
3. `[options={}]` *（对象）*：选项对象。
4. `[options.leading=false]` *（boolean）*：指定在超时的前沿调用。
5. `[options.maxWait]` *（数字）*：最大时间`func`可以在被调用之前被延迟。
6. `[options.trailing=true]` *（布尔值）*：指定在超时的后沿调用。

##### 返回

*（功能）*：返回新的去抖功能。

##### 例

```javascript
// Avoid costly calculations while the window size is in flux.

jQuery(window).on('resize', _.debounce(calculateLayout, 150));
 
// Invoke `sendMail` when clicked, debouncing subsequent calls.

jQuery(element).on('click', _.debounce(sendMail, 300, {
  'leading': true,

  'trailing': false
}));
 
// Ensure `batchLog` is invoked once after 1 second of debounced calls.

var debounced = _.debounce(batchLog, 250, { 'maxWait': 1000 });

var source = new EventSource('/stream');

jQuery(source).on('message', debounced);
 
// Cancel the trailing debounced invocation.

jQuery(window).on('popstate', debounced.cancel);
```

### _.defer(func, args)

延迟调用func，直到当前调用堆栈已被清除。 任何其他参数在调用时都会提供给func。

##### 初始

0.1.0

##### 参数

1. `func` *（功能）*：延迟的功能。
2. `[args]` *（... \*）*：用来调用的参数`func`。

##### 返回

*（数字）*：返回定时器ID。

##### 例

```javascript
_.defer(function(text) {
  console.log(text);
}, 'deferred');
// => Logs 'deferred' after one millisecond.
```

### _.delay(func, wait, args)

等待毫秒后调用func。 任何其他参数在调用时都会提供给func。

##### 初始

0.1.0

##### 参数

1. `func` *（功能）*：延迟的功能。
2. `wait` *（数字）*：延迟调用的毫秒数。
3. `[args]` *（... \*）*：用来调用的参数`func`。

##### 返回

*（数字）*：返回定时器ID。

##### 例

```javascript
_.delay(function(text) {
  console.log(text);
}, 1000, 'later');
// => Logs 'later' after one second.
```

### _.flip(func)

创建一个调用`func`颠倒参数的函数。

##### 初始

4.0.0

##### 参数

1. `func` *（功能）*：用于翻转参数的功能。

##### 返回

*（功能）*：返回新的翻转功能。

##### 例

```javascript
var flipped = _.flip(function() {
  return _.toArray(arguments);
});
 

flipped('a', 'b', 'c', 'd');
// => ['d', 'c', 'b', 'a']
```

### _.memoize(func, resolver)

创建一个函数来记忆func的结果。 如果提供了解析器，它将根据提供给memoized函数的参数确定用于存储结果的缓存键。 默认情况下，提供给memoized函数的第一个参数用作映射缓存键。 func被这个memoized函数的绑定调用。

注：缓存作为memoized函数的缓存属性公开。 它的创建可以通过将_.memoize.Cache构造函数替换为其实例实现Map方法接口clear，delete，get，has和set的构造函数来定制。

##### 初始

0.1.0

##### 参数

1. `func` *（功能）*：输出记忆功能。
2. `[resolver]` *（功能）*：解析缓存键的功能。

##### 返回

*（功能）*：返回新的记忆功能。

##### 例

```javascript
var object = { 'a': 1, 'b': 2 };

var other = { 'c': 3, 'd': 4 };
 

var values = _.memoize(_.values);

values(object);
// => [1, 2]
 

values(other);
// => [3, 4]
 
object.a = 2;

values(object);
// => [1, 2]
 
// Modify the result cache.
values.cache.set(object, ['a', 'b']);

values(object);
// => ['a', 'b']
 
// Replace `_.memoize.Cache`.
_.memoize.Cache = WeakMap;
```

### _.negate(predicate)

创建一个否定谓词func结果的函数。 func谓词用所创建函数的绑定和参数调用。

##### 初始

3.0.0

##### 参数

1. `predicate` *（函数）*：否定的谓词。

##### 返回

*（函数）*：返回新的否定函数。

##### 例

```javascript
function isEven(n) {
  return n % 2 == 0;
}
 
_.filter([1, 2, 3, 4, 5, 6], _.negate(isEven));
// => [1, 3, 5]
```

### _.once(func)

创建一次只能调用func的函数。 重复调用函数返回第一个调用的值。 函数被创建函数的这个绑定和参数调用。

##### 初始

0.1.0

##### 参数

1. `func` *（功能）*：限制功能。

##### 返回

*（功能）*：返回新的限制功能。

##### 例

```javascript
var initialize = _.once(createApplication);

initialize();

initialize();
// => `createApplication` is invoked once
```

### _.overArgs(func, [transforms=_.identity])

创建一个调用`func`其参数转换的函数。

##### 初始

4.0.0

##### 参数

1. `func` *（功能）*：包装的功能。
2. `[transforms=[_.identity]]` *（...（函数|函数[]））*：参数转换。

##### 返回

*（功能）*：返回新功能。

##### 例

```javascript
function doubled(n) {
  return n * 2;
}
 

function square(n) {
  return n * n;
}
 

var func = _.overArgs(function(x, y) {
  return [x, y];
}, [square, doubled]);
 

func(9, 3);
// => [81, 6]
 

func(10, 5);
// => [100, 10]
```

### _.partial(func, partials)

创建一个调用func的函数，该函数带有前缀给它接收到的参数的部分。 这个方法就像_.bind，只是它不会改变这个绑定。

_.partial.placeholder值（在单体构建中默认为_）可以用作部分应用参数的占位符。

**注意：**此方法不会设置部分应用函数的“length”属性。

##### 初始

0.2.0

##### 参数

1. `func` *（功能）*：部分应用参数的功能。
2. `[partials]` *（... \*）*：部分应用的参数。

##### 返回

*（功能）*：返回新的部分应用功能。

##### 例

```javascript
function greet(greeting, name) {
  return greeting + ' ' + name;
}
 

var sayHelloTo = _.partial(greet, 'hello');

sayHelloTo('fred');
// => 'hello fred'
 
// Partially applied with placeholders.

var greetFred = _.partial(greet, _, 'fred');

greetFred('hi');
// => 'hi fred'
```

### _.partialRight(func, partials)

此方法与_.partial类似，但部分应用的参数会附加到它所接收的参数中。

_.partialRight.placeholder值（默认为单个构建中的_）可用作部分应用参数的占位符。

**注意：**此方法不会设置部分应用函数的“length”属性。

##### 初始

1.0.0

##### 参数

1. `func` *（功能）*：部分应用参数的功能。
2. `[partials]` *（... \*）*：部分应用的参数。

##### 返回

*（功能）*：返回新的部分应用功能。

##### 例

```javascript
function greet(greeting, name) {
  return greeting + ' ' + name;
}
 

var greetFred = _.partialRight(greet, 'fred');

greetFred('hi');
// => 'hi fred'
 
// Partially applied with placeholders.

var sayHelloTo = _.partialRight(greet, 'hello', _);

sayHelloTo('fred');
// => 'hello fred'
```

### _.rearg(func, indexes)

创建一个调用func的函数，其参数按照指定的索引进行排列，其中第一个索引的参数值作为第一个参数，第二个索引的参数值作为第二个参数提供，依此类推。

##### 初始

3.0.0

##### 参数

1. `func` *（函数）*：重新排列参数的函数。
2. `indexes` *（...（number | number []））*：排列的参数索引。

##### 返回

*（功能）*：返回新功能。

##### 例

```javascript
var rearged = _.rearg(function(a, b, c) {
  return [a, b, c];
}, [2, 0, 1]);
 

rearged('b', 'c', 'a')
// => ['a', 'b', 'c']
```

### _.rest(func, start=func.length-1)

创建一个调用func的函数，该函数使用创建函数的此绑定以及作为数组提供的从start和之后的参数。

**注意：**此方法基于[其余参数](https://mdn.io/rest_parameters)。

##### 初始

4.0.0

##### 参数

1. `func` *（功能）*：将休息参数应用于的功能。
2. `[start=func.length-1]` *（数字）*：其余参数的起始位置。

##### 返回

*（功能）*：返回新功能。

##### 例

```javascript
var say = _.rest(function(what, names) {
  return what + ' ' + _.initial(names).join(', ') +
    (_.size(names) > 1 ? ', & ' : '') + _.last(names);
});
 

say('hello', 'fred', 'barney', 'pebbles');
// => 'hello fred, barney, & pebbles'
```

### _.spread(func, start=0)

创建一个调用create function和一个参数数组`func`的`this`绑定的函数[`Function#apply`](http://www.ecma-international.org/ecma-262/7.0/#sec-function.prototype.apply)。

**注意：** 此方法基于[扩展运算符](https://mdn.io/spread_operator)。

##### 初始

3.2.0

##### 参数

1. `func` *（功能）*：将参数传播的功能。
2. `[start=0]` *（数字）*：价差的起始位置。

##### 返回

*（功能）*：返回新功能。

##### 例

```javascript
var say = _.spread(function(who, what) {
  return who + ' says ' + what;
});
 

say(['fred', 'hello']);
// => 'fred says hello'
 

var numbers = Promise.all([
  Promise.resolve(40),

  Promise.resolve(36)
]);
 
numbers.then(_.spread(function(x, y) {
  return x + y;
}));
// => a Promise of 76
```

### _.throttle(func, wait=0, options={})

创建一个throttled函数，每等待毫秒最多只调用一次func。 被扼杀的函数带有一个取消方法来取消延迟的func调用和一个flush方法来立即调用它们。 提供选项以指示是否应在等待超时的前沿和/或后沿调用func。 func被提供给限制函数的最后一个参数调用。 随后调用throttled函数会返回最后一次func调用的结果。

注意：如果前导和尾随选项为true，则只有在等待超时期间调用被阻止的函数多次时，才会在超时的后沿调用func。

如果wait为0且leading为false，则func调用被推迟到下一个tick，类似于超时值为0的setTimeout。

##### 初始

0.1.0

##### 参数

1. `func` *（功能）*：节流的功能。
2. `[wait=0]` *（数字）*：将调用限制为的毫秒数。
3. `[options={}]` *（对象）*：选项对象。
4. `[options.leading=true]` *（boolean）*：指定在超时的前沿调用。
5. `[options.trailing=true]` *（布尔值）*：指定在超时的后沿调用。

##### 返回

*（功能）*：返回新的限制功能。

##### 例

```javascript
// Avoid excessively updating the position while scrolling.

jQuery(window).on('scroll', _.throttle(updatePosition, 100));
 
// Invoke `renewToken` when the click event is fired, but not more than once every 5 minutes.

var throttled = _.throttle(renewToken, 300000, { 'trailing': false });

jQuery(element).on('click', throttled);
 
// Cancel the trailing throttled invocation.

jQuery(window).on('popstate', throttled.cancel);
```

### _.unary(func)

创建一个最多接受一个参数的函数，忽略任何其他参数。

##### 初始

4.0.0

##### 参数

1. `func` *（函数）*：为参数加上参数的函数。

##### 返回

*（功能）*：返回新的上限功能。

##### 例

```javascript
_.map(['6', '8', '10'], _.unary(parseInt));
// => [6, 8, 10]
```

### _.wrap(value, wrapper=identity)

创建一个为封装器提供值作为其第一个参数的函数。 提供给函数的任何附加参数都会附加到提供给包装器的参数中。 包装被调用与创建的函数的此绑定。

##### 初始

0.1.0

##### 参数

1. `value` *（\*）*：要包装的值。
2. `[wrapper=identity]` *（功能）*：包装功能。

##### 返回

*（功能）*：返回新功能。

##### 例

```javascript
var p = _.wrap(_.escape, function(func, text) {
  return '<p>' + func(text) + '</p>';
});
 

p('fred, barney, & pebbles');
// => '<p>fred, barney, &amp; pebbles</p>'
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-07