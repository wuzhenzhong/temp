# 辅助类 | Helper classes

## Helper classes

#### 上下文颜色

用一些强调实用程序类, 通过颜色传达意义。这些也可能被应用到链接上, 并且在悬停时会变暗, 就像我们的默认链接样式一样。



[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#helper-classes-colors)

```javascript
<p class="text-muted">...</p>
<p class="text-primary">...</p>
<p class="text-success">...</p>
<p class="text-info">...</p>
<p class="text-warning">...</p>
<p class="text-danger">...</p>
```

##### 处理特殊性

由于其他选择器的特殊性，有时强调类不能应用。在大多数情况下，一个充分的解决方法是将您的文本封装`<span>`在类中。

##### 将意义传递给辅助技术

使用颜色来增加意义只能提供一种视觉指示，而不会传达给辅助技术的用户 - 如屏幕阅读器。确保由颜色表示的信息对于内容本身来说是显而易见的（上下文颜色仅用于强化已经存在于文本/标记中的含义），或者通过其他方式包含，例如隐藏在`.sr-only`类中的附加文本。

#### 上下文背景

与上下文文本颜色类相似，可以轻松地将元素的背景设置为任何上下文类。就像文本类一样，锚组件会在悬停时变暗。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#helper-classes-backgrounds)

```javascript
<p class="bg-primary">...</p>
<p class="bg-success">...</p>
<p class="bg-info">...</p>
<p class="bg-warning">...</p>
<p class="bg-danger">...</p>
```

##### 处理特殊性

由于其他选择器的特殊性，有时上下文背景类不能应用。在某些情况下，一个充分的解决方法是将元素的内容封装`<div>`在类中。

##### 将意义传递给辅助技术

与上下文颜色一样，确保通过颜色传达的任何意义也以非纯粹表示的格式传达。

#### 关闭图标

使用通用的关闭图标来解除模式和警报等内容。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#helper-classes-close)

```javascript
<button type="button" class="close" aria-label="Close"><span aria-hidden="true">&times;</span></button>
```

#### 插入符号

使用插入符号来指示下拉功能和方向。请注意，默认插入符将在下拉菜单中自动反转。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#helper-classes-carets)

```javascript
<span class="caret"></span>
```

#### 快速浮动

用一个类向左或向右浮动一个元素。`!important`包含在内以避免特殊性问题。类也可以用作mixin。

```javascript
<div class="pull-left">...</div>
<div class="pull-right">...</div>
// Classes
.pull-left {
  float: left !important;
}
.pull-right {
  float: right !important;
}

// Usage as mixins
.element {
  .pull-left();
}
.another-element {
  .pull-right();
}
```

##### 不适用于导航条

要将navbars中的组件与实用程序类对齐，请使用`.navbar-left`或`.navbar-right`替代。有关详细信息，请参阅导航栏文档。

#### 中心内容块

将元素设置为`display: block`并通过`margin`居中。可被mixin和类调用。

```javascript
<div class="center-block">...</div>
// Class
.center-block {
  display: block;
  margin-left: auto;
  margin-right: auto;
}

// Usage as a mixin
.element {
  .center-block();
}
```

#### Clearfix

`float`通过添加`.clearfix` **到父元素可**轻松清除 。利用Nicolas Gallagher推广[的micro clearfix](http://nicolasgallagher.com/micro-clearfix-hack/)。也可以用作混音。

```javascript
<!-- Usage as a class -->
<div class="clearfix">...</div>
// Mixin itself
.clearfix() {
  &:before,
  &:after {
    content: " ";
    display: table;
  }
  &:after {
    clear: both;
  }
}

// Usage as a mixin
.element {
  .clearfix();
}
```

#### 显示和隐藏内容

使用.`show`和`.hidden`类强制元素显示或隐藏（**包括屏幕阅读器**）。这些类用`!important`避免特殊性冲突，就像快速浮动一样。它们只能用于块级切换。它们也可以用作mixin。

`.hide`是可用的，但它并不总是影响屏幕阅读器，从v3.0.1开始**已弃用**。使用`.hidden`或`.sr-only`替代。

此外，`.invisible`只能用于切换元素的可见性，这意味着它`display`不会被修改，元素仍然会影响文档的流向。

```javascript
<div class="show">...</div>
<div class="hidden">...</div>
// Classes
.show {
  display: block !important;
}
.hidden {
  display: none !important;
}
.invisible {
  visibility: hidden;
}

// Usage as mixins
.element {
  .show();
}
.another-element {
  .hidden();
}
```

#### 屏幕阅读器和键盘导航内容

隐藏所有设备的元素**，除了屏幕阅读器**使用`.sr-only`。`.sr-only`与`.sr-only-focusable`焦点合并时再次显示元素（例如，通过键盘用户）。必须遵循以下可访问性最佳实践。也可以用作mixin。

```javascript
<a class="sr-only sr-only-focusable" href="#content">Skip to main content</a>
// Usage as a mixin
.skip-navigation {
  .sr-only();
  .sr-only-focusable();
}
```

#### 图像替换

利用`.text-hide`类或mixin帮助用背景图像替换元素的文本内容。

```javascript
<h1 class="text-hide">Custom heading</h1>
// Usage as a mixin
.heading {
  .text-hide();
}
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-10-08