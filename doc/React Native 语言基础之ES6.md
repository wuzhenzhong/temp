# React Native 语言基础之ES6

0.1762017.09.01 18:32:37字数 2573阅读 1105

**React Native 是基于 React 这个前端框架来构建native app的架构。React Native基于ES6（即ECMAScript2015）语言进行开发的。**

------

### ES6基础

我大概在13年的时候学过一阵子的JavaScript，ES5。
如果学JavaScript的话，还是推荐



![《JavaScript高级程序设计》](https://gss2.bdstatic.com/-fo3dSag_xI4khGkpoWK1HF6hhy/baike/c0=baike116,5,5,116,38/sign=1c95ea13ddca7bcb6976cf7ddf600006/b7003af33a87e9505bb120bc1a385343faf2b4b8.jpg)

《JavaScript高级程序设计》

这本书真的是太经典了，学完这个后再去看ECMAScript6，那时真的会发现，6真的是太人性化了，如果一开始就学6，可能不会很难体会到她的那种魅力吧。学习ES6，还是看看 阮一峰老师的 [ECMAScript 6 入门](https://link.jianshu.com/?t=http://es6.ruanyifeng.com/)，



![img](https://upload-images.jianshu.io/upload_images/1833901-6f26fc39d64a1bde.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/268/format/webp)

《ECMAScript 6 入门》


不过这本书是基于ES5的。

```jsx
JavaScript时间轴：

下面就是一个简单的JavaScript发展时间轴：

1、1995：JavaScript诞生，它的初始名叫LiveScript。

2、1997：ECMAScript标准确立。

3、1999：ES3出现，与此同时IE5风靡一时。

4、2000–2005： XMLHttpRequest又名AJAX， 在Outlook Web Access (2000)、Oddpost (2002)，Gmail (2004)和Google Maps (2005)大受重用。

5、2009： ES5出现，（就是我们大多数人现在使用的）例如foreach，Object.keys，Object.create和JSON标准。

6、2015：ES6/ECMAScript2015出现。
JS的组成
1)  核心（ECMAScript）：描述了该语言的语法和基本对象。担当的是一个翻译的角色；是一个解释器；帮助计算机来读懂我们写的程序；实现加减乘除, 定义变量；
2) 文档对象模型（DOM）：描述了处理网页内容的方法和接口。文档指的就是网页；把网页变成一个JS可以操作的对象；给了JS可以操作页面元素的能力；
3) 浏览器对象模型（BOM）：描述了与浏览器进行交互的方法和接口。给了JS操作浏览器的能力；
```

**我这里从ES6的十大特性出发整理一下ES6**,整理来自[前端开发者不得不知的ES6十大特性](https://link.jianshu.com/?t=http://www.alloyteam.com/2016/03/es6-front-end-developers-will-have-to-know-the-top-ten-properties/)和 [ECMAScript 6 入门](https://link.jianshu.com/?t=http://es6.ruanyifeng.com/) ， 这里在内容上做了丰富。

##### 1) Block-Scoped Constructs Let and Const（块作用域构造Let and Const）

首先我们来看看这个例子：

```jsx
var name = 'Emily'

while (true) {
    var name = 'Anthony'
    console.log(name)  //Anthony
    break
}

console.log(name)  //Anthony
let name = 'Emily'

while (true) {
    let name = 'Anthony'
    console.log(name)  //Anthony
    break
}

console.log(name)  //Emily
```

使用var两次输出都是"Anthony"，这是因为ES5只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。第一种场景就是你现在看到的内层变量覆盖外层变量。而let则实际上为JavaScript新增了块级作用域。用它所声明的变量，只在let命令所在的代码块内有效。

那么你可能就会理解下面的代码为什么用let取代var了。

```jsx
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```

const也用来声明变量，但是声明的是常量。一旦声明，常量的值就不能改变。

```jsx
const PI = Math.PI

PI = 23 //Module build failed: SyntaxError: /es6/app.js: "PI" is read-only
```

const有一个很好的应用场景，就是当我们引用第三方库的时声明的变量，用const来声明可以避免未来不小心重命名而导致出现bug：

```jsx
const monent = require('moment')
```

##### 2) Default Parameters（默认参数） in ES6

同样是对比，ES5中我们这样指定默认值。

```jsx
var link = function (height, color, url) {
    var height = height || 50;
    var color = color || 'red';
    var url = url || 'http://azat.co';
    ...
}
```

现在呢，ES6中，我们可以直接指定默认值：

```jsx
var link = function(height = 50, color = 'red', url = 'http://azat.co') {
  ...
}
```

还可以这样：

```jsx
function animals(...types){
    console.log(types)
}
animals('cat', 'dog', 'fish') //["cat", "dog", "fish"]
```

##### 3) Template Literals （模板文本）in ES6

在ES5和Java语言中，我们可以这样组合一个字符串：

```csharp
var name = 'Your name is ' + first + ' ' + last + '.';
var url = 'http://localhost:3000/api/messages/' + id;
```

在ES6中，我们可以使用新的语法$ {NAME}，并把它放在反引号里：

```jsx
var name = `Your name is ${first} ${last}. `;
var url = `http://localhost:3000/api/messages/${id}`;
```

##### 4) Multi-line Strings （多行字符串）in ES6

ES6的多行字符串是一个非常实用的功能。在ES5和Java中，我们不得不使用以下方法来表示多行字符串：

```go
var roadPoem = 'Then took the other, as just as fair,nt'
    + 'And having perhaps the better claimnt'
    + 'Because it was grassy and wanted wear,nt'
    + 'Though as for that the passing therent'
    + 'Had worn them really about the same,nt';
var fourAgreements = 'You have the right to be you.n
    You can only be you when you do your best.';
```

然而在ES6中，仅仅用反引号就可以解决了：

```php
var roadPoem = `Then took the other, as just as fair,
    And having perhaps the better claim
    Because it was grassy and wanted wear,
    Though as for that the passing there
    Had worn them really about the same,`;
var fourAgreements = `You have the right to be you.
    You can only be you when you do your best.`;
```

##### 5) Destructuring Assignment （解构赋值）in ES6

ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

```bash
let cat = 'ken'
let dog = 'lili'
let zoo = {cat: cat, dog: dog}
console.log(zoo)  //Object {cat: "ken", dog: "lili"}
```

用ES6完全可以像下面这么写：

```bash
let cat = 'ken'
let dog = 'lili'
let zoo = {cat, dog}
console.log(zoo)  //Object {cat: "ken", dog: "lili"}
```

反过来可以这么写：

```tsx
let dog = {type: 'animal', many: 2}
let { type, many} = dog
console.log(type, many)   //animal 2
```

##### 6 ) Arrow Functions （箭头函数）in ES6

这个恐怕是ES6最最常用的一个新特性了，用它来写function比原来的写法要简洁清晰很多:

```php
function(i){ return i + 1; } //ES5
(i) => i + 1 //ES6
```

简直是简单的不像话对吧...
如果方程比较复杂，则需要用{}把代码包起来：

```php
function(x, y) { 
    x++;
    y--;
    return x + y;
}
(x, y) => {x++; y--; return x+y}
```

除了看上去更简洁以外，arrow function还有一项超级无敌的功能！
长期以来，JavaScript语言的this对象一直是一个令人头痛的问题，在对象方法中使用this，必须非常小心。例如：

```jsx
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout(function(){
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}

 var animal = new Animal()
 animal.says('hi')  //undefined says hi
```

运行上面的代码会报错，这是因为setTimeout中的this指向的是全局对象。所以为了让它能够正确的运行，传统的解决方法有两种：

第一种是将this传给self,再用self来指代this

```jsx
 says(say){
     var self = this;
     setTimeout(function(){
         console.log(self.type + ' says ' + say)
     }, 1000)
```

2.第二种方法是用bind(this),即

```jsx
 says(say){
     setTimeout(function(){
         console.log(self.type + ' says ' + say)
     }.bind(this), 1000)
```

但现在我们有了箭头函数，就不需要这么麻烦了：

```jsx
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        setTimeout( () => {
            console.log(this.type + ' says ' + say)
        }, 1000)
    }
}
 var animal = new Animal()
 animal.says('hi')  //animal says hi
```

当我们使用箭头函数时，函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，它的this是继承外面的，因此内部的this就是外层代码块的this。

##### 7) Promises in ES6

> **JS本身是单线程的语言，它要实现异步都是通过回调函数来实现的。**
> Promises象征着一个异步操作的最终结果。Promises交互主要通过它的then方法，then方法接受一个回调函数，这个回调函数接受执行成功的返回值或执行失败的错误原因，错误原因一般是Error对象。需要注意的是，then方法执行的返回值是一个Promise对象，而then方法接受的回调函数的返回值则可以是任意的JavaScript对象，包括Promises。基于这种机制，Promise对象的链式调用就起作用了。

在ES6中有标准的Promise实现。
下面是一个简单的用setTimeout()实现的异步延迟加载函数:

```jsx
setTimeout(function(){
  console.log('Yay!');
}, 1000);
```

在ES6中，我们可以用promise重写:

```jsx
var wait1000 =  new Promise(function(resolve, reject) {
  setTimeout(resolve, 1000);
}).then(function() {
  console.log('Yay!');
});
```

或者用ES6的箭头函数：

```jsx
var wait1000 =  new Promise((resolve, reject)=> {
  setTimeout(resolve, 1000);
}).then(()=> {
  console.log('Yay!');
});
```

到目前为止，代码的行数从三行增加到五行，并没有任何明显的好处。确实，如果我们有更多的嵌套逻辑在setTimeout()回调函数中，我们将发现更多好处：

```jsx
setTimeout(function(){
  console.log('Yay!');
  setTimeout(function(){
    console.log('Wheeyee!');
  }, 1000)
}, 1000);
```

在ES6中我们可以用promises重写：

```jsx
var wait1000 =  ()=> new Promise((resolve, reject)=> {setTimeout(resolve, 1000)});
wait1000()
    .then(function() {
        console.log('Yay!')
        return wait1000()
    })
    .then(function() {
        console.log('Wheeyee!')
    });
```

推荐文章：
[ES6 JavaScript Promise的感性认知](https://link.jianshu.com/?t=http://www.zhangxinxu.com/wordpress/2014/02/es6-javascript-promise-感性认知/)

[初识JavaScript Promises](https://link.jianshu.com/?t=http://www.cnblogs.com/1000/p/getting-started-with-promises.html)

[Introduction to ES6 Promises – The Four Functions You Need To Avoid Callback Hell
](https://link.jianshu.com/?t=http://jamesknelson.com/grokking-es6-promises-the-four-functions-you-need-to-avoid-callback-hell/)

##### 8) Classes（类） in ES6

class, extends, super这三个特性涉及了ES5中最令人头疼的的几个部分：原型、构造函数，继承...你还在为它们复杂难懂的语法而烦恼吗？你还在为指针到底指向哪里而纠结万分吗？

有了ES6我们不再烦恼！

ES6提供了更接近传统语言的写法，引入了Class（类）这个概念。新的class写法让对象原型的写法更加清晰、更像面向对象编程的语法，也更加通俗易懂。

```jsx
class Animal {
    constructor(){
        this.type = 'animal'
    }
    says(say){
        console.log(this.type + ' says ' + say)
    }
}

let animal = new Animal()
animal.says('hello') //animal says hello

class Cat extends Animal {
    constructor(){
        super()
        this.type = 'cat'
    }
}

let cat = new Cat()
cat.says('hello') //cat says hello
```

上面代码首先用class定义了一个“类”，可以看到里面有一个constructor方法，这就是构造方法，而this关键字则代表实例对象。简单地说，constructor内定义的方法和属性是实例对象自己的，而constructor外定义的方法和属性则是所有实力对象可以共享的。

Class之间可以通过extends关键字实现继承，这比ES5的通过修改原型链实现继承，要清晰和方便很多。上面定义了一个Cat类，该类通过extends关键字，继承了Animal类的所有属性和方法。

super关键字，它指代父类的实例（即父类的this对象）。子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其进行加工。如果不调用super方法，子类就得不到this对象。

ES6的继承机制，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this。
P.S 如果你写react的话，就会发现以上三个东西在最新版React中出现得很多。创建的每个component都是一个继承React.Component
的类。[详见react文档](https://link.jianshu.com/?t=https://facebook.github.io/react/docs/reusable-components.html)

##### 9) Modules（模块） in ES6

众所周知，在ES6以前JavaScript并不支持本地的模块。人们想出了AMD，RequireJS，CommonJS以及其它解决方法。现在ES6中可以用模块import 和export 操作了。
在ES5中，你可以在 <script>中直接写可以运行的代码（简称IIFE），或者一些库像AMD。然而在ES6中，你可以用export导入你的类。下面举个例子，在ES5中,module.js有port变量和getAccounts 方法:

```jsx
module.exports = {
  port: 3000,
  getAccounts: function() {
    ...
  }
}
```

在ES5中，main.js需要依赖require('module') 导入module.js：

```jsx
var service = require('module.js');
console.log(service.port); // 3000 
```

但在ES6中，我们将用export and import。例如，这是我们用ES6 写的module.js文件库：

```jsx
export var port = 3000;
export function getAccounts(url) {
  ...
}
```

如果用ES6来导入到文件main.js中，我们需用import {name} from 'my-module'语法，例如：

```jsx
import {port, getAccounts} from 'module';
console.log(port); // 3000
```

或者我们可以在main.js中把整个模块导入, 并命名为 service：

```jsx
import * as service from 'module';
console.log(service.port); // 3000
```

从我个人角度来说，我觉得ES6模块是让人困惑的。但可以肯定的事，它们使语言更加灵活了。
更多的信息和例子关于ES6模块，请看 [this text](https://link.jianshu.com/?t=http://exploringjs.com/es6/ch_modules.html)。不管怎样，请写模块化的JavaScript。

##### 10) Enhanced Object Literals （增强的对象文本）in

ES6
具体可以看文章：[前端开发者不得不知的ES6十大特性](https://link.jianshu.com/?t=http://web.jobbole.com/86984/)

> JavaScript是基于原型的面对象语言
> 理解这一点，对使用JS开发还是比较重要的。
> 像Java，Objective C，C＋＋都是基于类的面向对象语言。
> 面向对象语言有两个:
> **基于类的面向对象语言**
> 主要有两个概念
> 类(class):定义了一组具有某一类特征的事务。类是抽象的，比如鸟类
> 实例（instance）:实体是类的实体话提现，比如一只鸟
> **基于原型的面向对象**
> 基于原型的面向对象语言并不存在这种区别，基于原型的面向对象语言所有的都是对象。基于原型的面向对象语言有一个概念叫做原型对象,古代有一种东西叫做活字印刷术，那一个个字的模版就是这里的原型对象。

下一篇文章是React Native 语言基础之React ，敬请期待。