# Query - css() 方法



## jQuery css() 方法

css() 方法设置或返回被选元素的一个或多个样式属性。

## 返回 CSS 属性

如需返回指定的 CSS 属性的值，请使用如下语法：

```js
css("propertyname");
```

下面的例子将返回首个匹配元素的 background-color 值：

### 实例

```js
$("p").css("background-color");
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=jquery_css_getcolor)

## 设置 CSS 属性

如需设置指定的 CSS 属性，请使用如下语法：

```
css("propertyname","value");
```

下面的例子将为所有匹配元素设置 background-color 值：

### 实例

```js
$("p").css("background-color","yellow");
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=jquery_css_setcolor)

## 设置多个 CSS 属性

如需设置多个 CSS 属性，请使用如下语法：

```js
css({"propertyname":"value","propertyname":"value",...});
```

下面的例子将为所有匹配元素设置 background-color 和 font-size：

### 实例

```js
$("p").css({"background-color":"yellow","font-size":"200%"});
```

[亲自试一试](https://www.w3school.com.cn/tiy/t.asp?f=jquery_css_set_multiple)

## jQuery HTML 参考手册

如需有关 jQuery CSS 方法的完整内容，请访问我们的 [jQuery CSS 操作参考手册](https://www.w3school.com.cn/jquery/jquery_ref_css.asp)