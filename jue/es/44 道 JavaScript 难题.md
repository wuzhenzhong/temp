# 44 道 JavaScript 难题（JavaScript Puzzlers!）

这是一套很经典的JavaScript题了，做之前一些题我也觉得稀奇古怪，但一道一道做，记下错题，去查解释，做完感觉真的很值得，有点像回到高中时候，就想到了沙耶加。如果在学习路上疲惫了，安利你们[《垫底辣妹》](https://link.juejin.im/?target=http%3A%2F%2Fwww.iqiyi.com%2Fv_19rrmep9co.html%3Fvfm%3D2008_aldbd) 。这里是这套题的[原文链接](https://link.juejin.im/?target=http%3A%2F%2Fjavascript-puzzlers.herokuapp.com%2F)，可以不看答案一道一道去做。

[下一篇《一站到底 ---前端基础之网络》](https://juejin.im/post/5b3357556fb9a00e5a4b63df)

#### 1. ["1", "2", "3"].map(parseInt)

```
答案：[1, NaN, NaN]
解析：parseInt (val, radix) ：两个参数，val值，radix基数（就是多少进制转换）
     map 能传进回调函数 3参数 (element, index, array)
     parseInt('1', 0);  //0代表10进制
     parseInt('2', 1);  //没有1进制，不合法
     parseInt('3', 2);  //2进制根本不会有3
巩固：["1", "1", "11","5"].map(parseInt) //[1, NaN, 3, NaN]
      parseInt('13',2)    // 1 ,
      计算机在二进制只认识0，1，parseInt转换时就当作不认识的字符忽略了
      parseInt('18str')     //18   10进制能认识到9
      parseInt(1/0，19)    // 18  
      1/0 == Infinity 19 进制计算机能认识最后一个字符是i 
      详细解析在下面的链接
复制代码
```

[巩固题详细解释在stackoverflow](https://link.juejin.im/?target=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F11340673%2Fwhy-does-parseint1-0-19-return-18)

#### 2. [typeof null, null instanceof Object]

```
答案：["object", false]
解析：null代表空对象指针，所以typeof判断成一个对象。可以说JS设计上的一个BUG
     instanceof 实际上判断的是对象上构造函数，null是空当然不可能有构造函数
巩固：null == undefined //true    null === undefined //flase
复制代码
```

#### 3. [ [3,2,1].reduce(Math.pow), [].reduce(Math.pow) ]

```
答案：an error
解析：Math.pow (x , y)  x 的 y 次幂的值
     reduce（fn,total）
     fn (total, currentValue, currentIndex, arr) 
         如果一个函数不传初始值，数组第一个组默认为初始值.
         [3,2,1].reduce(Math.pow)
         Math.pow(3,2) //9
         Math.pow(9,1) //9
巩固：[].reduce(Math.pow)       //空数组会报TypeError
     [1].reduce(Math.pow)      //只有初始值就不会执行回调函数，直接返回1
     [].reduce(Math.pow,1)     //只有初始值就不会执行回调函数，直接返回1
     [2].reduce(Math.pow,3)    //传入初始值，执行回调函数，返回9
复制代码
```

#### 4.

```
 var val = 'smtg';
 console.log('Value is ' + (val === 'smtg') ? 'Something' : 'Nothing');
复制代码
```

这段代码的执行结果？

```
答案：Something
解析：字符串连接比三元运算有更高的优先级 
     所以原题等价于 'Value is true' ? 'Somthing' : 'Nonthing' 
     而不是 'Value   is' + (true ? 'Something' : 'Nonthing')
巩固：
    1 || fn() && fn()   //1  
    1 || 1 ? 2 : 3 ;    //2  
   巩固的解释请看下面这篇文章
复制代码
```

[Like Sunday, Like Rain - JavaScript运算符优先级之谜](https://juejin.im/post/5b22a5a36fb9a00e43465e2c/)

#### 5.

```
var name = 'World!';
(function () {
if (typeof name === 'undefined') {
    var name = 'Jack';
    console.log('Goodbye ' + name);
} else {
    console.log('Hello ' + name);
}
})();
复制代码
```

这段代码的执行结果？

```
答案：Goodbye Jack
解析：（1）typeof时 name变量提升。 在函数内部之声明未定义
     （2）typeof优先级高于===
巩固：
    var str = 'World!';   
    (function (name) {
    if (typeof name === 'undefined') {
        var name = 'Jack';
        console.log('Goodbye ' + name);
    } else {
        console.log('Hello ' + name);
    }
    })(str);
    答案：Hello World 因为name已经变成函数内局部变量
复制代码
```

#### 6.

```
var END = Math.pow(2, 53);
var START = END - 100;
var count = 0;
for (var i = START; i <= END; i++) {
    count++;
}
console.log(count);
复制代码
```

这段代码的执行结果？

```
答案：other ,不是101
解析：js中可以表示的最大整数不是2的53次方，而是1.7976931348623157e+308。2的53次方不是js能表示的最大整数而应该是能正确计算且不失精度的最大整数，
巩固：
     var END = 1234567635;
     var START = END - 1024;
     var c = count = 0;
     for (var i = START; i <= END; i++) {
        c = count++;
     }
     console.log(count);   //1025
     console.log(c);       //1024
复制代码
```

#### 7.

```
var ary = [0,1,2];
ary[10] = 10;
ary.filter(function(x) { return x === undefined;}); 
复制代码
```

这段代码的执行结果？

```
答案：[]
解析：filter() 不会对空数组进行检测。会跳过那些空元素
巩固：
      var ary = [0,1,2,undefined,undefined,undefined,null];
      ary.filter(function(x) { return x === undefined;});
      // [undefined, undefined, undefined] 
复制代码
```

#### 8.

```
var two   = 0.2
var one   = 0.1
var eight = 0.8
var six   = 0.6
[two - one == one, eight - six == two]
复制代码
```

这段代码的执行结果？

```
答案：[true, false]
解析：IEEE 754标准中的浮点数并不能精确地表达小数
巩固：var two   = 0.2;
     var one   = 0.1;
     var eight = 0.8;
     var six   = 0.6;
     ( eight - six ).toFixed(4) == two 
     //true
复制代码
```

#### 9.

```
function showCase(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase(new String('A'));
复制代码
```

这段代码的执行结果？

```
答案：Do not know!
解析：switch判断的是全等（===） ，new String(x)是个对象
巩固：var a =  new String('A') ;
      a.__proto__
     // String.prototype 实例的原型指向构造函数的原型对象
复制代码
```

#### 10.

```
function showCase2(value) {
    switch(value) {
    case 'A':
        console.log('Case A');
        break;
    case 'B':
        console.log('Case B');
        break;
    case undefined:
        console.log('undefined');
        break;
    default:
        console.log('Do not know!');
    }
}
showCase2(String('A'));
复制代码
```

这段代码的执行结果？

```
答案：Case A
解析：String('A')就是返回一个字符串
巩固: var a2 =  'A';
      a2.__proto__                  // String.prototype 
      a1.__proto__ === a2.__proto__  // true 上一题的a.__proto__
      那字符串不是对象为啥也指向String.prototype？
解析：b是基本类型的值，逻辑上不应该有原型和方法。为了便于操作，有一种特殊的引用类型（基本包装类型）String。其实读取时，后台会自动完成下面的操作：
      var str = new String("A"); //创建实例
      str.__proto__;             //调用指定属性和方法
      str = null;                //销毁实例
      所以 a1.__proto__ === a2.__proto__
      但注意基本包装类型特殊就在于它对象（str）的生命周期，只存在于一行代码(a1.__proto__ === a2.__proto__)的执行瞬间。
      这也就解释了为啥字符串也能操作属性和方法但不能添加。基本包装类型有三个（String,Number,Boolean）
     （详情请看《js高程》 5.6基本包类型 P119）
复制代码
```

#### 11.

```
function isOdd(num) {
    return num % 2 == 1;
}
function isEven(num) {
    return num % 2 == 0;
}
function isSane(num) {
    return isEven(num) || isOdd(num);
}
var values = [7, 4, '13', -9, Infinity];
values.map(isSane);
复制代码
```

这段代码的执行结果？

```
答案：[true, true, true, false, false]
解析：%如果不是数值会调用Number（）去转化
     '13' % 2       // 1
      Infinity % 2  //NaN  Infinity 是无穷大
      -9 % 2        // -1
巩固： 9 % -2        // 1   余数的正负号随第一个操作数
复制代码
```

#### 12.

```
parseInt(3, 8)
parseInt(3, 2)
parseInt(3, 0)
复制代码
```

这段代码的执行结果？

```
答案：3  NaN  3
解析：2进制不可能有3
复制代码
```

#### 13.

```
Array.isArray( Array.prototype )
复制代码
```

这段代码的执行结果？

```
答案：true
解析：Array.prototype是一个数组
     数组的原型是数组，对象的原型是对象，函数的原型是函数
复制代码
```

#### 14.

```
var a = [0];
if ([0]) {
  console.log(a == true);
} else {
  console.log("wut");
}
复制代码
```

这段代码的执行结果？

```
答案：false
解析：[0]的boolean值是true
      console.log(a == true); // 转换为数字进行比较， a转换先toString,转化成'0',再Number('0') 转化成数值0
      Number(true) => 1 ,所有是false
复制代码
```

#### 15.[]==[]

```
答案：false
解析：两个引用类型， ==比较的是引用地址
巩固：[]== ![] 
     (1)! 的优先级高于== ，右边Boolean([])是true,取返等于 false
     (2)一个引用类型和一个值去比较 把引用类型转化成值类型，左边0
     (3)所以 0 == false  答案是true
复制代码
```

#### 16.

```
'5' + 3
'5' - 3
复制代码
```

这段代码的执行结果？

```
答案：53  2
解析：加号有拼接功能，减号就是逻辑运算
巩固：typeof (+"1")   // "number" 对非数值+—常被用来做类型转换相当于Number()
复制代码
```

#### 17. 1 + - + + + - + 1

```
答案：2
解析：+-又是一元加和减操作符号，就是数学里的正负号。负负得正哈。 
巩固： 一元运算符还有一个常用的用法就是将自执行函数的function从函数声明变成表达式。
      常用的有 + - ～ ！ void
      + function () { }
      - function () { }
      ~ function () { }
      void function () { }
复制代码
```

#### 18.

```
var ary = Array(3);
ary[0]=2
ary.map(function(elem) { return '1'; });
复制代码
```

这段代码的执行结果？

```
答案：["1", empty × 2]
解析：如过没有值，map会跳过不会执行回调函数
复制代码
```

#### 19.

```
function sidEffecting(ary) {
  ary[0] = ary[2];
}
function bar(a,b,c) {
  c = 10
  sidEffecting(arguments);
  return a + b + c;
}
bar(1,1,1) 
复制代码
```

这段代码的执行结果？

```
答案：21, 
解析：arguments会和函数参数绑定。
巩固：但如果es6付给初始值则无法修改，因为es6编译后用了严格模式
      function sidEffecting(ary) {
        ary[0] = ary[2];
      }
      function bar(a=1,b,c) {
        c = 10
        sidEffecting(arguments);
        return a + b + c;
    }
       bar(1,1,1)
       //12
    这里总结一下：严格模式和非严格模式
    （1）严格模式arguments对象是传入函数内实参列表的静态副本；非严格模式下，指向同一个值的引用 
    （2）严格模式变量必须先声明，才能使用
    （3）严格模式中 call apply传入null undefined保持原样不被转换为window
复制代码
```

#### 20.

```
var a = 111111111111111110000,
b = 1111;
a + b;
复制代码
```

这段代码的执行结果？

```
答案：11111111111111111000
解析：在JavaScript中number类型在JavaScript中以64位（8byte）来存储。这64位中有符号位1位、指数位11位、实数位52位。2的53次方时，是最大值。其值为：9007199254740992（0x20000000000000）。超过这个值的话，运算的结果就会不对.
复制代码
```

#### 21.

```
var x = [].reverse;
x();
复制代码
```

这段代码的执行结果？

```
答案：error
解析：原来答案是window
      x = [].reverse  是把reverse函数赋值给x
      reverse 函数处理的是调用它的this
      比如 [1,2,3].reverse()时，它的this是[1,2,3]
      以前reverse是非严格模式的函数下，没传this会默认为window
      现在的reverse使用严格模式编写,应该是undefined，所以会报类型转换错误
      更多解释可以看下面"fe-baidu"的评论
复制代码
```

#### 22.Number.MIN_VALUE > 0

```
答案：true
解析：MIN_VALUE 属性是 JavaScript 中可表示的最小的数（接近 0 ，但不是负数）。它的近似值为 5 x 10-324。
复制代码
```

#### 23.[1 < 2 < 3, 3 < 2 < 1]

```
答案：[true,true]
解析： 1 < 2    =>  true;
      true < 3 =>  1 < 3 => true;
      
      3 < 2     => false;
      false < 1 => 0 < 1 => true;
复制代码
```

#### 24. 2 == [[[2]]]

```
答案：true
解析：值和引用类型去比较,把引用类型转话成值类型
     [[[2]]]）//2
巩固：++[[]][+[]]+[+[]] //"10"
     （1）(++([[]][+[]])) + [+[]]  //运算符权重判断，安利一下第四题下面的文章
     （2）(++([[]][0])) + [0]      // 16题中我们讲过+用来做类型转换Number([]) ===0
     （3）+([] + 1) + [0]            //[[]]数组的第0项就是[],++代表自增+1
       *******  注意这一步不是 (++[]) + [0] 这样是错误的   **********
     （4）+([] + 1) + [0]           // 前面+将"1"转成数字1 后边，+是拼接 "0" 所以是字符串"10"
      这题的详细解释在下面的链接中。高票答案解释的非常赞，推荐阅读
复制代码
```

##### [巩固题详细解释在stackoverflow](https://link.juejin.im/?target=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F7202157%2Fwhy-does-return-the-string-10%2F7202287%237202287)

#### 25.

```
3.toString()
3..toString()
3...toString()
复制代码
```

这段代码的执行结果？

```
答案：error, "3", error
解析：因为在 js 中 1.1, 1., .1 都是合法的数字. 那么在解析 3.toString 的时候这个 . 到底是属于这个数字还是函数调用呢? 只能是数字, 因为3.合法啊!
复制代码
```

#### 26.

```
(function(){
  var x = y = 1;
})();
console.log(y);
console.log(x);
复制代码
```

这段代码的执行结果？

```
答案：1, error
解析：y 被赋值成全局变量，等价于
     y = 1 ;
     var x = y;
复制代码
```

#### 27.

```
var a = /123/,
b = /123/;
a == b
a === b
复制代码
```

这段代码的执行结果？

```
答案：false, false
解析：正则是对象，引用类型，相等（==）和全等（===）都是比较引用地址
复制代码
```

#### 28.

```
var a = [1, 2, 3],
b = [1, 2, 3],
c = [1, 2, 4]
a ==  b
a === b
a >   c
a <   c
复制代码
```

这段代码的执行结果？

```
答案：false, false, false, true
解析：相等（==）和全等（===）还是比较引用地址
     引用类型间比较大小是按照字典序比较，就是先比第一项谁大，相同再去比第二项。
复制代码
```

#### 29.

```
var a = {}, b = Object.prototype;
[a.prototype === b, Object.getPrototypeOf(a) === b]    
复制代码
```

这段代码的执行结果？

```
答案：false, true
解析：Object 的实例是 a，a上并没有prototype属性
     a的__poroto__ 指向的是Object.prototype，也就是Object.getPrototypeOf(a)。a的原型对象是b
复制代码
```

#### 30.

```
function f() {}
var a = f.prototype, b = Object.getPrototypeOf(f);
a === b         
复制代码
```

这段代码的执行结果？

```
答案：false
解析：a是构造函数f的原型 ： {constructor: ƒ}
     b是实例f的原型对象 ： ƒ () { [native code] }
复制代码
```

#### 31.

```
function foo() { }
var oldName = foo.name;
foo.name = "bar";
[oldName, foo.name]     
复制代码
```

这段代码的执行结果？

```
答案：["foo", "foo"]
解析：函数的名字不可变.
复制代码
```

#### 32."1 2 3".replace(/\d/g, parseInt)

```
答案："1 NaN 3"
解析：replace() 回调函数的四个参数:
      1、匹配项  
      2、与模式中的子表达式匹配的字符串  
      3、出现的位置  
      4、stringObject 本身 。
如果没有与子表达式匹配的项，第二参数为出现的位置.所以第一个参数是匹配项，第二个参数是位置
 parseInt('1', 0)
 parseInt('2', 2)  //2进制中不可能有2
 parseInt('3', 4)
巩固：
   "And the %1".replace(/%([1-8])/g,function(match,a , b ,d){
      console.log(match +"  "+ a + " "+ b +" "+d )
    });
   //%1  1 8 And the %1 
复制代码
```

#### 33.

```
function f() {}
var parent = Object.getPrototypeOf(f);
f.name // ?
parent.name // ?
typeof eval(f.name) // ?
typeof eval(parent.name) //  ?  
复制代码
```

这段代码的执行结果？

```
答案："f", "Empty", "function", error
解析：f的函数名就是f
     parent是f原型对象的名字为"" ,
     先计算eval(f.name) 为 f,f的数据类型是function
     eval(parent.name) 为undefined, "undefined"
复制代码
```

#### 34.

```
var lowerCaseOnly =  /^[a-z]+$/;
lowerCaseOnly.test(null), lowerCaseOnly.test()]
复制代码
```

这段代码的执行结果？

```
答案：[true, true]
解析：这里 test 函数会将参数转为字符串. 'nul', 'undefined' 自然都是全小写了
复制代码
```

#### 35.[,,,].join(",")

```
答案：",,"
解析：因为javascript 在定义数组的时候允许最后一个元素后跟一个,
     所以这个数组长度是3，
巩固： [,,1,].join(".").length //  3 
复制代码
```

#### 36.

```
var a = {class: "Animal", name: 'Fido'};
a.class   
复制代码
```

这段代码的执行结果？

```
答案：other
解析：这取决于浏览器。类是一个保留字，但是它被Chrome、Firefox和Opera接受为属性名。在另一方面，每个人都会接受大多数其他保留词（int，私有，抛出等）作为变量名，而类是VordBoint。
复制代码
```

#### 37.var a = new Date("epoch")

```
答案：other
解析：您得到“无效日期”，这是一个实际的日期对象（一个日期的日期为true）。但无效。这是因为时间内部保持为一个数字，在这种情况下，它是NA。
      在chrome上是undefined 
      正确的是格式是var d = new Date(year, month, day, hours, minutes, seconds, milliseconds);
复制代码
```

#### 38.

```
var a = Function.length,
b = new Function().length
a === b
复制代码
```

这段代码的执行结果？

```
答案：false
解析：首先new在函数带（）时运算优先级和.一样所以从左向右执行
     new Function() 的函数长度为0
巩固：function fn () {
         var a = 1;
      }
      console.log(fn.length) 
      //0 fn和new Function()一样
复制代码
```

#### 39.

```
var a = Date(0);
var b = new Date(0);
var c = new Date();
[a === b, b === c, a === c]
复制代码
```

这段代码的执行结果？

```
答案：[false, false, false]
解析：当日期被作为构造函数调用时，它返回一个相对于划时代的对象（JAN 01 1970）。当参数丢失时，它返回当前日期。当它作为函数调用时，它返回当前时间的字符串表示形式。
a是字符串   a === b // 数据类型都不同，肯定是false
b是对象     b === c // 引用类型，比的是引用地址
c也是对象   a === c // 数据类型都不同，肯定是false
巩固：  var a = Date(2018);
       var b = Date(2001);
       [a ===b ]
       //[true] Date() 方法获得当日的日期,作为函数调用不需要，返回的同一个字符串
       "Tue Jun 12 2018 14:36:24 GMT+0800 (CST)" 当然如果a,b执行时间相差1秒则为false
复制代码
```

#### 40.

```
var min = Math.min(), max = Math.max()
min < max
复制代码
```

这段代码的执行结果？

```
答案：false
解析： Math.min 不传参数返回 Infinity, Math.max 不传参数返回 -Infinity ，Infinity应该大于-Infinity,所以是false
     
巩固：Number.MAX_VALUE  > Number.MIN_VALUE  //true
复制代码
```

#### 41.

```
function captureOne(re, str) {
  var match = re.exec(str);
  return match && match[1];
}
var numRe  = /num=(\d+)/ig,
    wordRe = /word=(\w+)/i,
    a1 = captureOne(numRe,  "num=1"),
    a2 = captureOne(wordRe, "word=1"),
    a3 = captureOne(numRe,  "NUM=2"),
    a4 = captureOne(wordRe,  "WORD=2");
[a1 === a2, a3 === a4]
复制代码
```

这段代码的执行结果？

```
答案：[true, false]
解析： ／g有一个属性叫lastIndex，每次匹配如果没有匹配到，它将重置为0，如果匹配到了，他将记录匹配的位置。我们看一个简单的例子吧。
        var numRe  = /num=(\d)/g;
         numRe.test("num=1abcwewe") //true
         numRe.lastIndex            //5     匹配到num=1后在5的索引位置
         numRe.exec("num=1")        //fales 这次要从5的索引位置，开始匹配
         numRe.lastIndex            //0     上一次匹配失败了numRe.lastIndex重制为0
复制代码
```

#### 42.

```
var a = new Date("2014-03-19"),
b = new Date(2014, 03, 19);
[a.getDay() === b.getDay(), a.getMonth() === b.getMonth()]
复制代码
```

这段代码的执行结果？

```
答案：[false, false]
解析： var a = new Date("2014-03-19")    //能够识别这样的字符串，返回想要的日期
      Wed Mar 19 2014 08:00:00 GMT+0800 (CST)
      b = new Date(2014, 03, 19);       //参数要按照索引来
      Sat Apr 19 2014 00:00:00 GMT+0800 (CST)
      月是从0索引，日期是从1 
      getDay()是获取星期几
      getMonth()是获取月份所以都不同
巩固： [a.getDate() === b.getDate()] //true
复制代码
```

#### 43.

```
 if ('http://giftwrapped.com/picture.jpg'.match('.gif')) {
    'a gif file'
  } else {
    'not a gif file'
  }
复制代码
```

这段代码的执行结果？

```
答案：'a gif file'
解析： String.prototype.match 接受一个正则, 如果不是, 按照 new RegExp(obj) 转化. 所以 . 并不会转义 。 那么 /gif 就匹配了 /.gif/
巩固： if ('http://giftwrapped.com/picture.jpg'.indexOf('.gif')) {
        'a gif file'
      } else {
        'not a gif file'
      }
      //indexOf如果匹配不到返回是-1   所以是 'a gif file' 
复制代码
```

#### 44.

```
function foo(a) {
  var a;
  return a;
}
function bar(a) {
    var a = 'bye';
    return a;
}
[foo('hello'), bar('hello')]
复制代码
```

这段代码的执行结果？

```
答案：["hello", "bye"]
解析：最后一题很简单吧，变量声明
```