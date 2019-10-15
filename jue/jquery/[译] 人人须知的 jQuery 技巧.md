# [译] 人人须知的 jQuery 技巧

阅读 7364

收藏 471

2016-03-11

原文链接：[github.com](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner%2Fblob%2Fmaster%2FTODO%2FjQuery-Tips-Everyone-Should-Know.md)

[LARK巡洋计划-首届开发者大赛，更有机会获得20万元人民币现金奖励！open.larksuite.com](https://open.larksuite.com/contest?utm_source=juejin)

> - 原文链接 : [jquery-tips-everyone-should-know](https://link.juejin.im/?target=http%3A%2F%2Fgold.xitu.io%2Fentry%2F563f79f300b0ee7f57ad9c6d)
> - 原文作者 : [AllThingsSmitty (Matt Smith)](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2FAllThingsSmitty)
> - 译文出自 : [掘金翻译计划](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fxitu%2Fgold-miner)
> - 译者 : [Yves X](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2FYves-X)
> - 校对者: [sqrthree(根号三)](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fsqrthree)
> - 状态 : 完成

这里收集了一些简单的窍门，助你玩转 jQuery。

1. [检查 jQuery 是否加载](https://juejin.im/entry/56e1a95b731956005da35c24#检查-jQuery-是否加载)
2. [返回顶部按钮](https://juejin.im/entry/56e1a95b731956005da35c24#返回顶部按钮)
3. [预加载图片](https://juejin.im/entry/56e1a95b731956005da35c24#预加载图片)
4. [判断图片是否加载完成](https://juejin.im/entry/56e1a95b731956005da35c24#判断图片是否加载完成)
5. [自动修复失效图片](https://juejin.im/entry/56e1a95b731956005da35c24#自动修复失效图片)
6. [鼠标悬停切换 class](https://juejin.im/entry/56e1a95b731956005da35c24#鼠标悬停切换-class)
7. [禁用输入字段](https://juejin.im/entry/56e1a95b731956005da35c24#禁用输入字段)
8. [阻止链接加载](https://juejin.im/entry/56e1a95b731956005da35c24#阻止链接加载)
9. [缓存 jQuery 选择器](https://juejin.im/entry/56e1a95b731956005da35c24#缓存-jQuery-选择器)
10. [切换淡出 / 滑动](https://juejin.im/entry/56e1a95b731956005da35c24#切换淡出--滑动)
11. [简单的手风琴效果](https://juejin.im/entry/56e1a95b731956005da35c24#简单的手风琴效果)
12. [使两个 div 等高](https://juejin.im/entry/56e1a95b731956005da35c24#使两个-div-等高)
13. [在新标签页 / 新窗口打开外部链接](https://juejin.im/entry/56e1a95b731956005da35c24#在新标签页--新窗口打开外部链接)
14. [通过文本查找元素](https://juejin.im/entry/56e1a95b731956005da35c24#通过文本查找元素)
15. [在 visibility 属性变化时触发](https://juejin.im/entry/56e1a95b731956005da35c24#在-visibility-属性变化时触发)
16. [Ajax 调用错误处理](https://juejin.im/entry/56e1a95b731956005da35c24#Ajax-调用错误处理)
17. [链式插件调用](https://juejin.im/entry/56e1a95b731956005da35c24#链式插件调用)

### 检查 jQuery 是否加载

在使用 jQuery 进行任何操作之前，你需要先确认它已经加载：

```javascript
if (typeof jQuery == 'undefined') {
  console.log('jQuery hasn\'t loaded');
} else {
  console.log('jQuery has loaded');
}
```

### 返回顶部按钮

利用 jQuery 中的 `animate` 和 `scrollTop` 方法，你无需插件就可以创建简单的 scroll up 效果:

```javascript
// 返回顶部
$('a.top').click(function (e) {
  e.preventDefault();
  $(document.body).animate({scrollTop: 0}, 800);
});
<!-- 设置锚 -->
<a class="top" href="#">Back to top</a>
```

调整 `scrollTop` 的值即可改变滚动着陆位置。你实际所做的是在 800 毫秒内不断设置文档主体的位置，直到它滚动到顶部。

### 预加载图片

如果你的网页使用了大量并非立即可见的图片（例如悬停鼠标触发的图片），那么预加载这些图片就显得很有意义了:

```javascript
$.preloadImages = function () {
  for (var i = 0; i < arguments.length; i++) {
    $('<img>').attr('src', arguments[i]);
  }
};

$.preloadImages('img/hover-on.png', 'img/hover-off.png');
```

### 判断图片是否加载完成

在有些情况下，为了继续执行脚本，你需要检查图片是否已经完全加载:

```javascript
$('img').load(function () {
  console.log('image load successful');
});
```

同样，换用一个带有 id 或者 class 属性的 `<img>` 标签，你也可以检查特定图片是否加载完成。

### 自动修复失效图片

如果你在你的网站上发现了失效的图片链接，逐个去替换它们将会是个苦差。这段简单的代码可以很大程度地减轻痛苦：

```javascript
$('img').on('error', function () {
  if(!$(this).hasClass('broken-image')) {
    $(this).prop('src', 'img/broken.png').addClass('broken-image');
  }
});
```

即使你暂无任何失效的链接，添加这段代码也不会有任何损失。

### 鼠标悬停切换 class

如果你希望在用户将鼠标悬停在某个可点击元素上时改变它的视觉效果，你可以在该元素被悬停时给它添加一个 class，当鼠标不再悬停时，移除这个 class：

```javascript
$('.btn').hover(function () {
  $(this).addClass('hover');
}, function () {
  $(this).removeClass('hover');
});
```

如果你还寻求*更简单*的途径，可以使用 `toggleClass` 方法，仅需添加必要的 CSS：

```javascript
$('.btn').hover(function () {
  $(this).toggleClass('hover');
});
```

**注**：在这种情况下，使用 CSS 或许是一个更快速的解决方案，但这种方法仍然值得稍作了解。

### 禁用输入字段

有时，你可能希望在用户完成特定操作（例如，勾选“我已阅读条例”的确认框）前禁用表单的提交按钮或禁用其中某个输入框。你可以在你的输入字段上添加 `disabled` 属性，而后你能在需要时启用它：

```javascript
$('input[type="submit"]').prop('disabled', true);
```

你只需在输入字段上再次运行 `prop` 方法, 但是这一次把 `disabled` 值改为 `false`：

```javascript
$('input[type="submit"]').prop('disabled', false);
```

### 阻止链接加载

有时你不希望链接到指定页面或者重载当前页面，而是想让它们干些别的，例如触发其它脚本。这需要在阻止默认动作上做些文章：

```javascript
$('a.no-link').click(function (e) {
  e.preventDefault();
});
```

### 缓存 jQuery 选择器

想想你在项目中一次又一次地写了多少相同的选择器吧。每个 `$('.element')` 都必须查询一次整个 DOM,不管它是否曾这样执行过。作为代替，我们只运行一次选择器，并把结果储存在一个变量中：

```javascript
var blocks = $('#blocks').find('li');
```

现在你能在任何地方使用 `blocks` 变量而无需每次查询 DOM 了:

```javascript
$('#hideBlocks').click(function () {
  blocks.fadeOut();
});

$('#showBlocks').click(function () {
  blocks.fadeIn();
});
```

缓存 jQuery 的选择器是种简单的性能提升。

### 切换淡出 / 滑动

淡出和滑动都是我们在 jQuery 中大量使用的效果。你可能只想在用户点击后展现某个元素，此时用 `fadeIn` 和 `slideDown` 方法就很完美。但是如果你希望这个元素在首次点击时出现，在再次点击时消失，这段代码就很有用了：

```javascript
// 淡出
$('.btn').click(function () {
  $('.element').fadeToggle('slow');
});

// 切换
$('.btn').click(function () {
  $('.element').slideToggle('slow');
});
```

### 简单的手风琴效果

这是一个快速实现手风琴效果的简单方法:

```javascript
// 关闭所有面板
$('#accordion').find('.content').hide();

// 手风琴效果
$('#accordion').find('.accordion-header').click(function () {
  var next = $(this).next();
  next.slideToggle('fast');
  $('.content').not(next).slideUp('fast');
  return false;
});
```

通过添加这段脚本，你实际要做的只是提供必要的 HTML 元素以便它正常运行。

### 使两个 div 等高

有时你希望无论两个 div 各自包含什么内容，它们总有相同的高度：

```javascript
$('.div').css('min-height', $('.main-div').height());
```

这个例子设置了 `min-height`，意味着高度可以大于主 div 而不能小于它。然而，更灵活的方法是遍历一组元素，然后将高度设置为最高元素的高度：

```javascript
var $columns = $('.column');
var height = 0;
$columns.each(function () {
  if ($(this).height() > height) {
    height = $(this).height();
  }
});
$columns.height(height);
```

如果你希望*所有*列高度相同：

```javascript
var $rows = $('.same-height-columns');
$rows.each(function () {
  $(this).find('.column').height($(this).height());
});
```

### 在新标签页 / 新窗口打开外部链接

在一个新的浏览器标签页或窗口中打开外部链接，并确保相同来源的链接在同一个标签页或者窗口中打开：

```javascript
$('a[href^="http"]').attr('target', '_blank');
$('a[href^="//"]').attr('target', '_blank');
$('a[href^="' + window.location.origin + '"]').attr('target', '_self');
```

**注：** `window.location.origin` 在 IE10 中不可用. [这个修复方案](https://link.juejin.im/?target=http%3A%2F%2Ftosbourn.com%2Fa-fix-for-window-location-origin-in-internet-explorer%2F) 正是关注于该问题。

### 通过文本查找元素

通过使用 jQuery 的 `contains()` 选择器，你能够查找元素内容中的文本。若文本不存在，该元素将被隐藏：

```javascript
var search = $('#search').val();
$('div:not(:contains("' + search + '"))').hide();
```

### 在 visibility 属性变化时触发

当用户的焦点离开或者重新回到某个标签页时，触发 Javasrcipt：

```javascript
$(document).on('visibilitychange', function (e) {
  if (e.target.visibilityState === "visible") {
    console.log('Tab is now in view!');
  } else if (e.target.visibilityState === "hidden") {
    console.log('Tab is now hidden!');
  }
});
```

### Ajax 调用错误处理

当一个 Ajax 调用返回 404 或 500 错误时，错误处理程序将被执行。若错误处理未被定义，其它 jQuery 代码可能不再有效。所以定义一个全局的 Ajax 错误处理：

```javascript
$(document).ajaxError(function (e, xhr, settings, error) {
  console.log(error);
});
```

### 链式插件调用

jQuery 允许通过“链式”插件调用的方法，来缓解反复查询 DOM 和创建多个 jQuery 对象的过程。例如，下面的代码代表着你的插件调用：

```javascript
$('#elem').show();
$('#elem').html('bla');
$('#elem').otherStuff();
```

通过使用链式操作，有了显著的改善:

```javascript
$('#elem')
  .show()
  .html('bla')
  .otherStuff();
```

另一种方法是在变量（以 `$` 为前缀）中，对元素进行缓存：

```javascript
var $elem = $('#elem');
$elem.hide();
$elem.html('bla');
$elem.otherStuff();
```

无论是链式操作，还是缓存元素，都是 jQuery 中用以简化和优化代码的最佳实践。