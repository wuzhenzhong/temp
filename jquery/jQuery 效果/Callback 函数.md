# jQuery Callback 函数



**Callback 函数在当前动画 100% 完成之后执行。**

## jQuery 动画的问题

许多 jQuery 函数涉及动画。这些函数也许会将 *speed* 或 *duration* 作为可选参数。

例子：*$("p").hide("slow")*

*speed* 或 *duration* 参数可以设置许多不同的值，比如 "slow", "fast", "normal" 或毫秒。

### 实例

```js
$("button").click(function(){
$("p").hide(1000);
});
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=jquery_hide_slow)

由于 JavaScript 语句（指令）是逐一执行的 - 按照次序，动画之后的语句可能会产生错误或页面冲突，因为动画还没有完成。

为了避免这个情况，您可以以参数的形式添加 Callback 函数。

## jQuery Callback 函数

当动画 100% 完成后，即调用 Callback 函数。

### 典型的语法：

```
$(selector).hide(speed,callback)
```

*callback* 参数是一个在 hide 操作完成后被执行的函数。

### 错误（没有 callback）

```js
$("p").hide(1000);
alert("The paragraph is now hidden");
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=jquery_hide_slow_wrong)

### 正确（有 callback）

```js
$("p").hide(1000,function(){
alert("The paragraph is now hidden");
});
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=jquery_hide_slow_right)

**结论：**如果您希望在一个涉及动画的函数之后来执行语句，请使用 callback 函数。