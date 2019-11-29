# 按钮 | Buttons

## Buttons

### 按钮标签

使用一个按钮类`<a>`，`<button>`或`<input>`元素。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#buttons-tags)

```javascript
<a class="btn btn-default" href="#" role="button">Link</a>
<button class="btn btn-default" type="submit">Button</button>
<input class="btn btn-default" type="button" value="Input">
<input class="btn btn-default" type="submit" value="Submit">
```

##### 上下文特定的用法

虽然按钮类可以用在`<a>`与`<button>`元素，只有`<button>`元素被我们的导航和导航栏组件内的支持。

##### 作为按钮的链接

如果`<a>`元素用作按钮 - 触发页内功能，而不是导航到当前页面中的另一个文档或部分 - 则应该给它们适当的值`role="button"`。

##### 跨浏览器呈现

作为最佳做法，**我们强烈建议** **尽可能使用<button>元素**以确保匹配的跨浏览器呈现。

除此之外，还有[在Firefox <30的错误](https://bugzilla.mozilla.org/show_bug.cgi?id=697451)阻止我们设定`line-height`的`<input>`基础的按钮，使他们无法在Firefox上其他按钮的高度完全匹配。

### 选项

使用任何可用的按钮类来快速创建一个样式化的按钮。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#buttons-options)

```javascript
<!-- Standard button -->
<button type="button" class="btn btn-default">Default</button>

<!-- Provides extra visual weight and identifies the primary action in a set of buttons -->
<button type="button" class="btn btn-primary">Primary</button>

<!-- Indicates a successful or positive action -->
<button type="button" class="btn btn-success">Success</button>

<!-- Contextual button for informational alert messages -->
<button type="button" class="btn btn-info">Info</button>

<!-- Indicates caution should be taken with this action -->
<button type="button" class="btn btn-warning">Warning</button>

<!-- Indicates a dangerous or potentially negative action -->
<button type="button" class="btn btn-danger">Danger</button>

<!-- Deemphasize a button by making it look like a link while maintaining button behavior -->
<button type="button" class="btn btn-link">Link</button>
```

##### 将意义传递给辅助技术

使用颜色为按钮添加含义仅提供可视指示，不会将其传递给辅助技术的用户 - 例如屏幕阅读器。确保由颜色表示的信息从内容本身（按钮的可见文本）中显而易见，或者通过其他方式包含在内，例如隐藏在`.sr-only`课程中的其他文本。

### 尺寸

想要更大或更小的按钮？ 添加`.btn-lg`，`.btn-sm`或`.btn-xsxu`

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#buttons-sizes)

```javascript
<p>
  <button type="button" class="btn btn-primary btn-lg">Large button</button>
  <button type="button" class="btn btn-default btn-lg">Large button</button>
</p>
<p>
  <button type="button" class="btn btn-primary">Default button</button>
  <button type="button" class="btn btn-default">Default button</button>
</p>
<p>
  <button type="button" class="btn btn-primary btn-sm">Small button</button>
  <button type="button" class="btn btn-default btn-sm">Small button</button>
</p>
<p>
  <button type="button" class="btn btn-primary btn-xs">Extra small button</button>
  <button type="button" class="btn btn-default btn-xs">Extra small button</button>
</p>
```

通过添加创建块级别按钮 - 即跨越父级的全部宽度的按钮`.btn-block`。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#buttons-sizes)

```javascript
<button type="button" class="btn btn-primary btn-lg btn-block">Block level button</button>
<button type="button" class="btn btn-default btn-lg btn-block">Block level button</button>
```

### 活动状态

活动时，按钮将显示为按下（背景较暗，边框较暗并且嵌入阴影）。对于`<button>`元素，这是通过`:active`。对于`<a>`元素来说，它已经完成了`.active`。但是，如果需要以编程方式复制活动状态，则可以`.active`在`<button>`上使用（并包含`aria-pressed="true"`属性）。

#### 按钮元素

不需要添加，`:active`因为它是一个伪类，但如果您需要强制使用相同的外观，请继续并添加`.active`。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#buttons-active)

```javascript
<button type="button" class="btn btn-primary btn-lg active">Primary button</button>
<button type="button" class="btn btn-default btn-lg active">Button</button>
```

#### 固定元素

将该`.active`类添加到`<a>`按钮。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#buttons-active)

```javascript
<a href="#" class="btn btn-primary btn-lg active" role="button">Primary link</a>
<a href="#" class="btn btn-default btn-lg active" role="button">Link</a>
```

### 禁用状态

通过将按钮淡出，使按钮看起来不可点击`opacity`。

#### 按钮元素

将该`disabled`属性添加到`<button>`按钮。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#buttons-disabled)

```javascript
<button type="button" class="btn btn-lg btn-primary" disabled="disabled">Primary button</button>
<button type="button" class="btn btn-default btn-lg" disabled="disabled">Button</button>
```

##### 跨浏览器兼容性

如果您将`disabled`属性添加到 `<button>`，则Internet Explorer 9及以下版本将呈现带有我们无法修复的令人讨厌的文字阴影的文字灰色。

#### 固定元素

将该`.disabled`类添加到`<a>`按钮。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-buttons-ie-disabled)

```javascript
<a href="#" class="btn btn-primary btn-lg disabled" role="button">Primary link</a>
<a href="#" class="btn btn-default btn-lg disabled" role="button">Link</a>
```

我们`.disabled`在这里用作工具类，类似于普通`.active`类，所以不需要前缀。

##### 链接功能警告

该类用于`pointer-events: none`尝试禁用`<a>`的链接功能，但CSS属性尚未标准化，在Opera 18及更低版本或Internet Explorer 11中不完全支持。此外，即使在支持的浏览器中`pointer-events: none`，键盘导航仍然不受影响，这意味着有远见的键盘用户和辅助技术用户仍然可以激活这些链接。为了安全起见，请使用自定义JavaScript来禁用此类链接。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-09-28