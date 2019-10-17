# 【JS 口袋书】第 6 章：JS 中的闭包与模块

> 作者：valentinogagliardi
>
> 译者：前端小智
>
> 来源：github

------

阿里云最近在做活动，低至2折，有兴趣可以看看： [promotion.aliyun.com/ntms/yunpar…](https://promotion.aliyun.com/ntms/yunparter/invite.html?userCode=pxuujn3r)

------

**为了保证的可读性，本文采用意译而非直译。**

全局变量使用容易引发bug，咱们经常教导尽量不要使用全局变量，尽管全局变量在某些情况下是有用的。 例如，在浏览器中使用JS时，咱们可以访问全局`window`对象，`window`中有很多有用的方法，比如：

```
window.alert('Hello world'); // Shows an alert
window.setTimeout(callback, 3000); // Delay execution
window.fetch(someUrl); // make XHR requests
window.open(); // Opens a new tab
复制代码
```

这些方法也像这样使用：

```
alert('Hello world'); // Shows an alert
setTimeout(callback, 3000); // Delay execution
fetch(someUrl); // make XHR requests
open(); // Opens a new tab
复制代码
```

这是方便的。`Redux`是另一个“好”全局变量的例子:整个应用程序的状态存储在一个JS对象中，这个对象可以从整个应用程序(通过Redux)访问。但是当在一个团队如果同时有50个编写代码时，以如何处理这样的代码：

```
var arr = [];

function addToArr(element) {
  arr.push(element);
  return element + " added!";
}
复制代码
```

咱们同事在另一个文件中创建一个名为`arr`的新全局数组的几率有多大?我觉得非常高。JS中的全局变量非常糟糕的另一个原因是引擎足够友好，可以为咱们创建全局变量。如果忘了在变量名前加上`var`，就像这样：

```
name = "Valentino";
复制代码
```

JS引擎为会创建一个全局变量，更糟糕的是，可以在函数中创建了“非预期”变量：

```
function doStuff() {
  name = "Valentino";
}

doStuff();

console.log(name); // "Valentino"
复制代码
```

无辜的功能最终污染了全球环境。 幸运的是，可以用“严格模式”来消除这种行为， 在每个JS文件使用`“use strict”`足以避免愚蠢的错误：

```
"use strict";

function doStuff() {
  name = "Valentino";
}

doStuff();

console.log(name); // ReferenceError: name is not defined
复制代码
```

但一直使用严格模式也是一个问题，并不确实每个开发人员都会使用严格模式，因此，咱们必须找到一种解决“全局变量污染”问题的方法，幸运的是，JS 一直有一个内置的机制来解决这个问题。

## 揭秘闭包

那么，咱们如何保护全局变量不被污染?让咱们从一个简单的解开始，把`arr`移动到一个函数中：

```
function addToArr(element) {
  var arr = [];
  arr.push(element);
  return element + " added to " + arr;
}
复制代码
```

似乎合理，但结果不是咱们所期望的：

```
var firstPass = addToArr("a");
var secondPass = addToArr("b");
console.log(firstPass); // a added to a
console.log(secondPass); // b added to b
复制代码
```

`arr`在每次函数调用时都会被重置，现在它成了一个局部变量，而在第一个例子中咱们声明的`arr`是全局变量。 全局变量是“实时的”，不会被重围。 局部变量在函数执行完后就会被销毁了似乎没有办法防止局部变量被破坏？ 闭包会有帮助吗？ 但是什么是 闭包呢？

JS函数可以包含其他函数，这到现在是很常见的，如下所示：

```
function addToArr(element) {
  var arr = [];

  function push() {
    arr.push(element);
  }

  return element + " added to " + arr;
}
复制代码
```

但如果咱们直接把 `push` 函数返回，又会怎么样呢？如下所示：

```
function addToArr(element) {
  var arr = [];

  return function push() {
    arr.push(element);
    console.log(arr);
  };

  //return element + " added to " + arr;
}
复制代码
```

外部函数变成一个容器，返回另一个函数。第二个`return`语句被注释，因为该代码永远不会被执行。此时，咱们知道函数调用的结果可以保存在变量中。

```
var result = addToArr();
复制代码
```

现在`result`变成了一个可执行的JS函数:

```
var result = addToArr();
result("a");
result("b");
复制代码
```

只需修复一下，将参数“`element`”从外部函数移动到内部函数：

```
function addToArr() {
  var arr = [];

  return function push(element) {
    arr.push(element);
    console.log(arr);
  };

  //return element + " added to " + arr;
}
复制代码
```

神奇的现象出现了，完整代码如下：

```
function addToArr() {
  var arr = [];

  return function push(element) {
    arr.push(element);
    console.log(arr);
  };

  //return element + " added to " + arr;
}

var result = addToArr();
result("a"); // [ 'a' ]
result("b"); // [ 'a', 'b' ]
复制代码
```

这种被称为JS闭包:一个能够记住其环境变量的函数。为此，内部函数必须是一个封闭(外部)函数的返回值。这种也称为**工厂函数**。代码可以稍作调整，变更可以取更好的命名，内部函数可以是匿名的:

```
function addToArr() {
  var arr = [];

  return function(element) {
    arr.push(element);
    return element + " added to " + arr;
  };
}

var closure = addToArr();
console.log(closure("a")); // a added to a
console.log(closure("b")); // b added to a,b
复制代码
```

现在应该清楚了，“`闭包`”是内部函数。但有一个问题需要解决:咱们为什么要这样做?JS闭包的真正目的是什么?

## 闭包的需要

除了纯粹的“学术”知识之外，JS闭包还有很多用处：

- 提供私有的全局变量
- 在函数调用之间保存变量(状态)

JS中闭包最有趣的应用程序之一是`模块模式`。在ES6之前，除了将变量和方法封装在函数中之外，没有其他方法可以模块化JS代码并提供私有变量与方法”。闭包与立即调用的函数表达式相结合 是至今通用解决方案。

```
var Person = (function(){
  // do something
})()
复制代码
```

在模块中可以有“私有”变量和方法:

```
var Person = (function() {
  var person = {
    name: "",
    age: 0
  };

  function setName(personName) {
    person.name = personName;
  }

  function setAge(personAge) {
    person.age = personAge;
  }
})();
复制代码
```

从外部咱们无法访问`person.name`或`person.age`。咱们也不能调用`setName`或`setAge`。模块内的所有内容都是“私有的”。如果想公开咱们的方法，我们可以返回一个包含对私有方法引用的对象。

```
var Person = (function() {
  var person = {
    name: "",
    age: 0
  };

  function setName(personName) {
    person.name = personName;
  }

  function setAge(personAge) {
    person.age = personAge;
  }

  return {
    setName: setName,
    setAge: setAge
  };
})();
复制代码
```

如果想获取`person`对象，添加一个获取 `person` 对象的方法并返回即可。

```
var Person = (function() {
  var person = {
    name: "",
    age: 0
  };

  function setName(personName) {
    person.name = personName;
  }

  function setAge(personAge) {
    person.age = personAge;
  }

  function getPerson() {
    return person.name + " " + person.age;
  }

  return {
    setName: setName,
    setAge: setAge,
    getPerson: getPerson
  };
})();

Person.setName("Tom");
Person.setAge(44);
var person = Person.getPerson();
console.log(person); // Tom 44
复制代码
```

这种方式，外部获取不到 `person` 对象：

```
console.log(Person.person); // undefined
复制代码
```

模块模式不是构造JS代码的唯一方式。 使用对象，咱们可以实现相同的结果：

```
var Person = {
  name: "",
  age: 0,
  setName: function(personName) {
    this.name = personName;
  }
  // other methods here
};
复制代码
```

但是这样，内部属性就不在是私有的了：

```
var Person = {
  name: "",
  age: 0,
  setName: function(personName) {
    this.name = personName;
  }
  // other methods here
};

Person.setName("Tom");

console.log(Person.name); // Tom
复制代码
```

这是模块的主要卖点之一。 另一个好处是，模块有助于组织代码，使其具有重用性和可读性。 如，开发人员看到以下的代码就大概知道是做什么的：

```
"use strict";

var Person = (function() {
  var person = {
    name: "",
    age: 0
  };

  function setName(personName) {
    person.name = personName;
  }

  function setAge(personAge) {
    person.age = personAge;
  }

  function getPerson() {
    return person.name + " " + person.age;
  }

  return {
    setName: setName,
    setAge: setAge,
    getPerson: getPerson
  };
})();
复制代码
```

## 总结

全局变量很容易引发bug，咱们应该尽可能地避免它们。 有时全局变量是有用的，需要格外小心使用，因为JS引擎可以自由地创建全局变量。

这些年来出现了许多模式来管理全局变量，模块模式就是其中之一。 模块模式建立在闭包上，这是JS的固有特性。 JS 中的闭包是一种能够“记住”其变量环境的函数，即使在后续函数调用之间也是如此。 当咱们从另一个函数返回一个函数时，会创建一个闭包，这个模式也称为**“工厂函数**”。

## 思考

- 什么是闭包？
- 使用全局变量有哪些不好的方面？
- 什么是 JS 模块，为什么要使用它？

**代码部署后可能存在的BUG没法实时知道，事后为了解决这些BUG，花了大量的时间进行log 调试，这边顺便给大家推荐一个好用的BUG监控工具 Fundebug。**

原文：[github.com/valentinoga…](https://github.com/valentinogagliardi/Little-JavaScript-Book/blob/v1.0.0/manuscript/chapter4.md)

## 交流（欢迎加入群，群工作日都会发红包，互动讨论技术）

阿里云最近在做活动，低至2折，有兴趣可以看看：[promotion.aliyun.com/ntms/yunpar…](https://promotion.aliyun.com/ntms/yunparter/invite.html?userCode=pxuujn3r)

干货系列文章汇总如下，觉得不错点个Star，欢迎 加群 互相学习。

> [github.com/qq449245884…](https://github.com/qq449245884/xiaozhi)

因为篇幅的限制，今天的分享只到这里。如果大家想了解更多的内容的话，可以去扫一扫每篇文章最下面的二维码，然后关注咱们的微信公众号，了解更多的资讯和有价值的内容。