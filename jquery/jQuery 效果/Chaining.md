# jQuery - Chaining



**通过 jQuery，您可以把动作/方法链接起来。**

**Chaining 允许我们在一条语句中允许多个 jQuery 方法（在相同的元素上）。**

## jQuery 方法链接

直到现在，我们都是一次写一条 jQuery 语句（一条接着另一条）。

不过，有一种名为链接（chaining）的技术，允许我们在相同的元素上运行多条 jQuery 命令，一条接着另一条。

**提示：**这样的话，浏览器就不必多次查找相同的元素。

如需链接一个动作，您只需简单地把该动作追加到之前的动作上。

### 例子 1

下面的例子把 css(), slideUp(), and slideDown() 链接在一起。"p1" 元素首先会变为红色，然后向上滑动，然后向下滑动：

```js
$("#p1").css("color","red").slideUp(2000).slideDown(2000);
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=jquery_chaining)

如果需要，我们也可以添加多个方法调用。

**提示：**当进行链接时，代码行会变得很差。不过，jQuery 在语法上不是很严格；您可以按照希望的格式来写，包含折行和缩进。

### 例子 2

这样写也可以运行：

```js
$("#p1").css("color","red")
  .slideUp(2000)
  .slideDown(2000);
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=jquery_chaining2)

jQuery 会抛掉多余的空格，并按照一行长代码来执行上面的代码行。