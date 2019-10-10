# jQuery

0.2122016.06.05 15:56:29字数 4548阅读 1755

请记得在进行JQuery类库的运用时，加入JQuery类库，并且要保证先写文档就绪函数

- $(document).ready(handler)
- $().ready(handler) (this is not recommended)
- $(handler)

```jsx
$(document).ready(function() {
// Handler for .ready() called.
});
$(function() {
// Handler for .ready() called.
});
```

[JQuery库](https://link.jianshu.com/?t=http://www.bootcdn.cn/jquery/)

### 基础

###### jQuery 库包含以下特性：

- HTML 元素选取
- HTML 元素操作
- CSS 操作
- HTML 事件函数
- JavaScript 特效和动画
- HTML DOM 遍历和修改
- AJAX
- Utilities

------

###### 使用谷歌或微软的 **jQuery**，有一个很大的优势：

许多用户在访问其他站点时，已经从谷歌或微软加载过jQuery。所有结果是，当他们访问您的站点时，会从缓存中加载jQuery这样可以减少加载时间。同时，大多数CDN都可以确保当用户向其请求文件时，会从离用户最近的服务器上返回响应，这样也可以提高加载速度。

------

###### jQuery 语法

基础语法是：**$(selector).action()**

- 美元符号定义 jQuery
- 选择符（selector）“查询”和“查找” HTML 元素
- jQuery 的 action() 执行对元素的操作
  </br>

###### 文档就绪函数

```jsx
$(document).ready(function(){
          --- jQuery functions go here ----
  });
```

这是为了防止文档在完全加载（就绪）之前运行 jQuery 代码。
如果在文档没有完全加载之前就运行函数，操作可能失败。下面是两个具体的例子：

- 试图隐藏一个不存在的元素
- 获得未完全加载的图像的大小

------

**jQuery是为事件处理特别设计的**
由于 jQuery 是为处理 HTML 事件而特别设计的，那么当您遵循以下原则时，您的代码会更恰当且更易维护：

- 把所有 jQuery 代码置于事件处理函数中
- 把所有事件处理函数置于文档就绪事件处理器中
- 把 jQuery 代码置于单独的 .js 文件中
- 如果存在名称冲突，则重命名 jQuery 库
  eg:用jq代替$符号

```csharp
var jq=jQuery.noConflict()
```

------

</br>
</br>

### 效果

隐藏/显示

```jsx
$(selector).hide(speed,callback);
$(selector).show(speed,callback);
```

可选的 speed 参数规定隐藏/显示的速度，可以取以下值："slow"、"fast" 或毫秒。
可选的 callback 参数是隐藏或显示完成后所执行的函数名称。
*jQuery toggle()*
通过 jQuery，您可以使用 toggle() 方法来切换 hide() 和 show() 方法。

```jsx
$(selector).toggle(speed,callback);
```

------

通过 jQuery，您可以实现元素的淡入淡出效果。
jQuery拥有以下四种fade方法

- fadeIn()
- fadeOut()
- fadeTaggle()
- fadeTo()
  *jQuery fadeIn() 方法*
     jQuery fadeIn() 用于淡入已隐藏的元素。

```jsx
$(selector).fadeIn(speed,callback);
```

可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
可选的 callback 参数是 fading 完成后所执行的函数名称。
*jQuery fadeOut() 方法*
   jQuery fadeOut() 方法用于淡出可见元素。

```jsx
$(selector).fadeOut(speed,callback);
```

同上。
*jQuery fadeToggle() 方法*
jQuery fadeToggle() 方法可以在 fadeIn() 与 fadeOut() 方法之间进行切换。
 如果元素已淡出，则 fadeToggle() 会向元素添加淡入效果。
 如果元素已淡入，则 fadeToggle() 会向元素添加淡出效果。

```jsx
$(selector).fadeToggle(speed,callback);
```

同上。
*jQuery fadeTo() 方法*
 jQuery fadeTo() 方法允许渐变为给定的不透明度（值介于 0 与 1 之间）。

```jsx
$(selector).fadeTo(speed,opacity,callback);
```

必需的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
fadeTo() 方法中必需的 opacity 参数将淡入淡出效果设置为给定的不透明度（值介于 0 与 1 之间）。
可选的 callback 参数是该函数完成后所执行的函数名称。

------

通过 jQuery，您可以在元素上创建滑动效果。
jQuery 拥有以下滑动方法：

- slideDown()
- slideUp()
- slideToggle()
  *jQuery slideDown() 方法*
   jQuery slideDown() 方法用于向下滑动元素。

```jsx
$(selector).slideDown(speed,callback);
```

可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
可选的 callback 参数是滑动完成后所执行的函数名称。
*jQuery slideUp() 方法*
 jQuery slideUp() 方法用于向上滑动元素。

```jsx
$(selector).slideUp(speed,callback);
```

同上
*jQuery slideToggle() 方法*
 jQuery slideToggle() 方法可以在 slideDown() 与 slideUp() 方法之间进行切换。
如果元素向下滑动，则 slideToggle() 可向上滑动它们。
如果元素向上滑动，则 slideToggle() 可向下滑动它们。

```jsx
$(selector).slideToggle(speed,callback);
```

同上

------

**jQuery 动画 - animate() 方法**
jQuery animate() 方法用于创建自定义动画。

```csharp
$(selector).animate({params},speed,callback);
```

必需的 params 参数定义形成动画的 CSS 属性。
可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
可选的 callback 参数是动画完成后所执行的函数名称。
*提示*：默认地，所有 HTML 元素都有一个静态位置，且无法移动。
如需对位置进行操作，要记得首先把元素的 CSS position 属性设置为 relative、fixed 或 absolute！
*提示*：可以用 animate() 方法来操作所有 CSS 属性吗？
是的，几乎可以！不过，需要记住一件重要的事情：当使用 animate() 时，必须使用 Camel 标记法书写所有的属性名，比如，必须使用 paddingLeft 而不是 padding-left，使用 marginRight 而不是 margin-right，等等。
同时，色彩动画并不包含在核心 jQuery 库中。
如果需要生成颜色动画，您需要从 jQuery.com 下载 Color Animations 插件。
*jQuery animate() - 使用相对值*
也可以定义相对值（该值相对于元素的当前值）。需要在值的前面加上 += 或 -=：
*jQuery animate() - 使用预定义的值*
您甚至可以把属性的动画值设置为 "show"、"hide" 或 "toggle"：
*jQuery animate() - 使用队列功能*
默认地，jQuery 提供针对动画的队列功能。
这意味着如果您在彼此之后编写多个 animate() 调用，jQuery 会创建包含这些方法调用的“内部”队列。然后逐一运行这些 animate 调用。

------

**jQuery 停止动画**
jQuery stop() 方法用于停止动画或效果，在它们完成之前。
stop() 方法适用于所有 jQuery 效果函数，包括滑动、淡入淡出和自定义动画。

```jsx
$(selector).stop(stopAll,goToEnd);
```

可选的 stopAll 参数规定是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行。
可选的 goToEnd 参数规定是否立即完成当前动画。默认是 false。
因此，默认地，stop() 会清除在被选元素上指定的当前动画。

------

**jQuery Callback 函数**
Callback 函数在当前动画 100% 完成之后执行

```jsx
$(selector).hide(speed,callback);
```

callback参数是一个在 hide 操作完成后被执行的函数。

------

**jQuery - Chaining**
有一种名为链接（chaining）的技术，允许我们在相同的元素上运行多条 jQuery 命令，一条接着另一条。

> 当进行链接时，代码行会变得很差。不过，jQuery 在语法上不是很严格；您可以按照希望的格式来写，包含折行和缩进。

------

</br>
</br>

### HTML

**获得内容和属性**
jQuery 中非常重要的部分，就是操作 DOM 的能力。
jQuery 提供一系列与 DOM 相关的方法，这使访问和操作元素和属性变得很容易。
*提示：DOM = Document Object Model（文档对象模型）*
DOM 定义访问 HTML 和 XML 文档的标准：
“W3C 文档对象模型独立于平台和语言的界面，允许程序和脚本动态访问和更新文档的内容、结构以及样式。”

三个简单实用的用于 DOM 操作的 jQuery 方法：

- text() - 设置或返回所选元素的文本内容
- html() - 设置或返回所选元素的内容（包括 HTML 标记）
- val() - 设置或返回表单字段的值
  获取属性 - attr()
  jQuery attr() 方法用于获取属性值。

------

**jQuery - 设置内容和属性**
同上
上面的三个 jQuery 方法：text()、html() 以及 val()，同样拥有回调函数。回调函数由两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值。然后以函数新值返回您希望使用的字符串。
jQuery attr() 方法也用于设置/改变属性值。

------

**jQuery - 添加元素**
我们将学习用于添加新内容的四个 jQuery 方法：

- append() - 在被选元素的结尾插入内容
- prepend() - 在被选元素的开头插入内容
- after() - 在被选元素之后插入内容
- before() - 在被选元素之前插入内容
  *jQuery append() 方法*
   jQuery append() 方法在被选元素的结尾插入内容。

```go
$("p").append("Some appended text.");
```

*jQuery prepend() 方法
 jQuery prepend() 方法在被选元素的开头插入内容。

```jsx
$("p").prepend("Some prepended text.");
```

通过 append() 和 prepend() 方法添加若干新元素
在上面的例子中，我们只在被选元素的开头/结尾插入文本/HTML。
不过，append() 和 prepend() 方法能够通过参数接收无限数量的新元素。可以通过 jQuery 来生成文本/HTML（就像上面的例子那样），或者通过 JavaScript 代码和 DOM 元素。
*jQuery after() 和 before() 方法*
 jQuery after() 方法在被选元素之后插入内容。
 jQuery before() 方法在被选元素之前插入内容。

```jsx
$(img).afer("Some text after");
$(img).befor("Some text before");
```

通过 after() 和 before() 方法添加若干新元素
after() 和 before() 方法能够通过参数接收无限数量的新元素。可以通过 text/HTML、jQuery 或者 JavaScript/DOM 来创建新元素。

------

**jQuery - 删除元素**
如需删除元素和内容，一般可使用以下两个 jQuery 方法：

- remove() - 删除被选元素（及其子元素）
- empty() - 从被选元素中删除子元素

```jsx
$(selector).remove();
$(selector).empty();
```

*过滤被删除的元素*
jQuery remove() 方法也可接受一个参数，允许您对被删元素进行过滤。
该参数可以是任何 jQuery 选择器的语法。

------

**jQuery - 获取并设置 CSS 类**
jQuery 拥有若干进行 CSS 操作的方法。我们将学习下面这些：

- addClass() - 向被选元素添加一个或多个类
- removeClass() - 从被选元素删除一个或多个类
- toggleClass() - 对被选元素进行添加/删除类的切换操作
- css() - 设置或返回样式属性

------

**jQuery - css() 方法**
css() 方法设置或返回被选元素的一个或多个样式属性。
*返回CSS属性*

```bash
css("propertyname");
```

*设置CSS属性*

```bash
css("propertyname","value");
```

*设置多个CSS属性*

```bash
css("propertyname":"value","propertyname":"value");
```

------

**jQuery - 尺寸**
jQuery 提供多个处理尺寸的重要方法：

- width()
- height()
- innerWidth()
- innerHeight()
- outerWidth()
- outerHeight()
  *jQuery width() 和 height() 方法*
  width() 方法设置或返回元素的宽度（不包括内边距、边框或外边距）。
  height() 方法设置或返回元素的高度（不包括内边距、边框或外边距）。
  *jQuery innerWidth() 和 innerHeight() 方法*
  innerWidth() 方法返回元素的宽度（包括内边距）。
  innerHeight() 方法返回元素的高度（包括内边距）。
  *jQuery outerWidth() 和 outerHeight() 方法*
  outerWidth() 方法返回元素的宽度（包括内边距和边框）。
  outerHeight() 方法返回元素的高度（包括内边距和边框）。
  outerWidth(true) 方法返回元素的宽度（包括内边距padding、边框border和外边距margin）。
  outerHeight(true) 方法返回元素的高度（包括内边距、边框和外边距margin）。
  下面的例子设置指定的 <div> 元素的宽度和高度：

```php
  $(button).click(function(){
        $(#div1).weigth(500).height(500);
  });
```

------

</br>
</br>

### jQuery 遍历

jQuery 遍历，意为“移动”，用于根据其相对于其他元素的关系来“查找”（或选取）HTML 元素。以某项选择开始，并沿着这个选择移动，直到抵达您期望的元素为止。
遍历方法中最大的种类是树遍历（tree-traversal）。

------

**jQuery 遍历 - 祖先**
用于向上遍历 DOM 树：

- parent()
- parents()
- parentsUntil()
  jQuery parent() 方法

*parent() 方法返回被选元素的直接父元素。*
 该方法只会向上一级对 DOM 树进行遍历。
*jQuery parents() 方法*
 parents() 方法返回被选元素的所有祖先元素，它一路向上直到文档的根元素 (<html>)。
*jQuery parentsUntil() 方法*
 parentsUntil() 方法返回介于两个给定元素之间的所有祖先元素。

------

**jQuery 遍历 - 后代**
下面是两个用于向下遍历 DOM 树的 jQuery 方法：

- children()
- find()

*jQuery children() 方法*
children() 方法返回被选元素的所有直接子元素。
该方法只会向下一级对 DOM 树进行遍历。
*jQuery find() 方法*
find() 方法返回被选元素的后代元素，一路向下直到最后一个后代。

------

**jQuery 遍历 - 同胞**
有许多有用的方法让我们在 DOM 树进行水平遍历：

- siblings()
- next()
- nextAll()
- nextUntil()
- prev()
- prevAll()
- prevUntil()

*jQuery siblings() 方法*
siblings() 方法返回被选元素的所有同胞元素。
*jQuery next() 方法*
next() 方法返回被选元素的下一个同胞元素。
*jQuery nextAll() 方法*
nextAll() 方法返回被选元素的所有跟随的同胞元素。
*jQuery nextUntil() 方法*
nextUntil() 方法返回介于两个给定参数之间的所有跟随的同胞元素。
*jQuery prev(), prevAll() & prevUntil() 方法*
prev(), prevAll() 以及 prevUntil() 方法的工作方式与上面的方法类似，只*不过方向相反而已：它们返回的是前面的同胞元素（在 DOM 树中沿着同胞元素向后遍历，而不是向前）。

------

**jQuery 遍历 - 过滤**
三个最基本的过滤方法是：first(), last() 和 eq()，它们允许您基于其在一组元素中的位置来选择一个特定的元素。
其他过滤方法，比如 filter() 和 not() 允许您选取匹配或不匹配某项指定标准的元素。
*jQuery first() 方法*
first() 方法返回被选元素的首个元素。
*jQuery last() 方法*
last() 方法返回被选元素的最后一个元素。
*jQuery eq() 方法*
eq() 方法返回被选元素中带有指定索引号的元素。
索引号从 0 开始，因此首个元素的索引号是 0 而不是 1。
*jQuery filter() 方法*
filter() 方法允许您规定一个标准。不匹配这个标准的元素会被从集合中删除，匹配的元素会被返回。
*jQuery not() 方法*
not() 方法返回不匹配标准的所有元素。
*提示*：not() 方法与 filter() 相反。

------

</br>
</br>

### jQuery - AJAX 简介

> AJAX = 异步 JavaScript 和 XML（Asynchronous JavaScript and XML）。

简短地说，在不重载整个网页的情况下，AJAX 通过后台加载数据，并在网页上进行显示。
使用 AJAX 的应用程序案例：谷歌地图、腾讯微博、优酷视频、人人网等等。
jQuery 提供多个与 AJAX 有关的方法。
通过 jQuery AJAX 方法，您能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON - 同时您能够把这些外部数据直接载入网页的被选元素中。
提示：如果没有 jQuery，AJAX 编程还是有些难度的。
编写常规的 AJAX 代码并不容易，因为不同的浏览器对 AJAX 的实现并不相同。这意味着您必须编写额外的代码对浏览器进行测试。不过，*jQuery 团队为我们解决了这个难题，我们只需要一行简单的代码，就可以实现 AJAX 功能。*

------

**jQuery - AJAX load() 方法**
jQuery load() 方法
jQuery load() 方法是简单但强大的 AJAX 方法。
load() 方法从服务器加载数据，并把返回的数据放入被选元素中。

```jsx
$(selector).load(url,data,callback);
```

必需的 *URL* 参数规定您希望加载的 URL。
可选的 *data* 参数规定与请求一同发送的查询字符串键/值对集合。
可选的 *callback* 参数是 load() 方法完成后所执行的函数名称。
可选的 callback 参数规定当 load() 方法完成后所要允许的回调函数。回调函数可以设置不同的参数：

- *responseTxt* - 包含调用成功时的结果内容
- *statusTXT* - 包含调用的状态
- *xhr* - 包含 XMLHttpRequest 对象

------

**jQuery - AJAX get() 和 post() 方法**
*jQuery get() 和 post() 方法用于通过 HTTP GET 或 POST 请求从服务器请求数据。*
两种在客户端和服务器端进行请求-响应的常用方法是：GET 和 POST。

- *GET* - 从指定的资源请求数据
- *POST* - 向指定的资源提交要处理的数据

GET 基本上用于从服务器获得（取回）数据。注释：GET 方法可能返回缓存数据。
POST 也可用于从服务器获取数据。不过，POST 方法不会缓存数据，并且常用于连同请求一起发送数据。
*jQuery $.get() 方法*
$.get() 方法通过 HTTP GET 请求从服务器上请求数据。

```jsx
$.get(URL,callback);
```

必需的 *URL* 参数规定您希望请求的 URL。
可选的 *callback* 参数是请求成功后所执行的函数名。
*jQuery $.post() 方法*
$.post() 方法通过 HTTP POST 请求从服务器上请求数据。

```jsx
$.post(URL,data,callback);
```

必需的 *URL* 参数规定您希望请求的 URL。
可选的 *data* 参数规定连同请求发送的数据。
可选的 *callback* 参数是请求成功后所执行的函数名。

------

</br>
</br>

### JQuery 杂项

*jQuery noConflict() 方法*
noConflict() 方法会释放会 $ 标识符的控制，这样其他脚本就可以使用它了。
您仍然可以通过全名替代简写的方式来使用 jQuery
您也可以创建自己的简写。noConflict() 可返回对 jQuery 的引用，您可以把它存入变量，以供稍后使用。

```jsx
var jq = $.noConflict();
```

如果你的 jQuery 代码块使用 $ 简写，并且您不愿意改变这个快捷方式，那么您可以把 $ 符号作为变量传递给 ready 方法。这样就可以在函数内使用 $ 符号了 - 而在函数外，依旧不得不使用 "jQuery"：

```jsx
$.noConflict();
jQuery(document).ready(function($){
  $("button").click(function(){
    $("p").text("jQuery 仍在运行！");
  });
});
```