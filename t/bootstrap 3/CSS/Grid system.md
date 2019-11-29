# 网格系统 | Grid system

- 贡献者1人

  

## 网格系统

Bootstrap包含一个响应式移动第一流体网格系统，随着设备或视口尺寸的增加，可适当扩展至12列。它包含预定义的类以实现简单的布局选项，以及用于生成更多语义布局的强大mixin。

### 介绍

网格系统用于通过一系列容纳内容的行和列来创建页面布局。以下是Bootstrap网格系统的工作原理：

- 行必须放置在`.container`（固定宽度）或`.container-fluid`（全宽）范围内才能正确对齐和填充。

- 使用行来创建水平的列组。

- 内容应放置在列中，并且只有列可以是行的直接子节点。

- 预先定义的网格类，如`.row`以及`.col-xs-4`可用于快速进行网格布局。较少的mixin也可用于更多语义布局。

- 列通过padding创建间距。第一列和最后一列的填充通过`.row`s 上的负边界在行中被抵消。

- 负边距是为什么下面的例子是缩进的。它使网格列中的内容与非网格内容排列在一起。

- 通过指定您希望跨越的十二个可用列的数量来创建网格列。例如，三个相等的列将使用三个`.col-xs-4`。

- 如果超过12列放置在一行中，每组额外列将作为一个单位换行到一个新行。

- 网格类适用于屏幕宽度大于或等于断点大小的设备，并覆盖面向较小设备的网格类。因此，例如，`.col-md-*`向任何元素应用任何类别不仅会影响其在中型设备上的样式，而且在`.col-lg-*`不存在类别的情况下也会影响到大型设备上的样式。

请查看将这些原理应用于代码的示例。

### 媒体查询

我们在Less文件中使用以下媒体查询来在我们的网格系统中创建关键断点。

```javascript
/* Extra small devices (phones, less than 768px) */
/* No media query since this is the default in Bootstrap */

/* Small devices (tablets, 768px and up) */
@media (min-width: @screen-sm-min) { ... }

/* Medium devices (desktops, 992px and up) */
@media (min-width: @screen-md-min) { ... }

/* Large devices (large desktops, 1200px and up) */
@media (min-width: @screen-lg-min) { ... }
```

We occasionally expand on these media queries to include a `max-width` to limit CSS to a narrower set of devices.

```javascript
@media (max-width: @screen-xs-max) { ... }
@media (min-width: @screen-sm-min) and (max-width: @screen-sm-max) { ... }
@media (min-width: @screen-md-min) and (max-width: @screen-md-max) { ... }
@media (min-width: @screen-lg-min) { ... }
```

### 网格选项

查看Bootstrap网格系统的各个方面如何在具有便捷表的多个设备上工作。

|                 | Extra small devices Phones (<768px)  | Small devices Tablets (≥768px)                   | Medium devices Desktops (≥992px) | Large devices Desktops (≥1200px) |
| :-------------- | :----------------------------------- | :----------------------------------------------- | :------------------------------- | :------------------------------- |
| Grid behavior   | Horizontal at all times              | Collapsed to start, horizontal above breakpoints |                                  |                                  |
| Container width | None (auto)                          | 750px                                            | 970px                            | 1170px                           |
| Class prefix    | .col-xs-                             | .col-sm-                                         | .col-md-                         | .col-lg-                         |
| of columns      | 12                                   |                                                  |                                  |                                  |
| Column width    | Auto                                 | ~62px                                            | ~81px                            | ~97px                            |
| Gutter width    | 30px (15px on each side of a column) |                                                  |                                  |                                  |
| Nestable        | Yes                                  |                                                  |                                  |                                  |
| Offsets         | Yes                                  |                                                  |                                  |                                  |
| Column ordering | Yes                                  |                                                  |                                  |                                  |

### 示例：堆叠到水平

使用一组`.col-md-*`网格类，您可以创建一个基本的网格系统，在桌面（中型）设备上变为水平之前，该网格系统开始堆叠在移动设备和平板电脑设备上（额外的小到小范围）。将网格列放置在任何位置`.row`。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#grid-example-basic)

```javascript
<div class="row">
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
</div>
<div class="row">
  <div class="col-md-8">.col-md-8</div>
  <div class="col-md-4">.col-md-4</div>
</div>
<div class="row">
  <div class="col-md-4">.col-md-4</div>
  <div class="col-md-4">.col-md-4</div>
  <div class="col-md-4">.col-md-4</div>
</div>
<div class="row">
  <div class="col-md-6">.col-md-6</div>
  <div class="col-md-6">.col-md-6</div>
</div>
```

### 示例：流体容器

将任何固定宽度的网格布局转换为全宽布局，方法是将最外层的网格布局更改`.container`为`.container-fluid`。

```javascript
<div class="container-fluid">
  <div class="row">
    ...
  </div>
</div>
```

### 示例：移动和桌面

不希望您的列只能堆放在较小的设备中？通过添加`.col-xs-*` `.col-md-*`到列中来使用额外的小型和中型设备网格类。请参阅下面的示例以更好地了解它是如何工作的。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#grid-example-mixed)

```javascript
<!-- Stack the columns on mobile by making one full-width and the other half-width -->
<div class="row">
  <div class="col-xs-12 col-md-8">.col-xs-12 .col-md-8</div>
  <div class="col-xs-6 col-md-4">.col-xs-6 .col-md-4</div>
</div>

<!-- Columns start at 50% wide on mobile and bump up to 33.3% wide on desktop -->
<div class="row">
  <div class="col-xs-6 col-md-4">.col-xs-6 .col-md-4</div>
  <div class="col-xs-6 col-md-4">.col-xs-6 .col-md-4</div>
  <div class="col-xs-6 col-md-4">.col-xs-6 .col-md-4</div>
</div>

<!-- Columns are always 50% wide, on mobile and desktop -->
<div class="row">
  <div class="col-xs-6">.col-xs-6</div>
  <div class="col-xs-6">.col-xs-6</div>
</div>
```

### 例如：手机，平板电脑，桌面

通过使用平板电脑`.col-sm-*`类创建更加动态和强大的布局，构建前一个示例。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#grid-example-mixed-complete)

```javascript
<div class="row">
  <div class="col-xs-12 col-sm-6 col-md-8">.col-xs-12 .col-sm-6 .col-md-8</div>
  <div class="col-xs-6 col-md-4">.col-xs-6 .col-md-4</div>
</div>
<div class="row">
  <div class="col-xs-6 col-sm-4">.col-xs-6 .col-sm-4</div>
  <div class="col-xs-6 col-sm-4">.col-xs-6 .col-sm-4</div>
  <!-- Optional: clear the XS cols if their content doesn't match in height -->
  <div class="clearfix visible-xs-block"></div>
  <div class="col-xs-6 col-sm-4">.col-xs-6 .col-sm-4</div>
</div>
```

### 示例：列封装

如果超过12列放置在一行中，每组额外列将作为一个单位换行到一个新行。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#grid-example-wrapping)

```javascript
<div class="row">
  <div class="col-xs-9">.col-xs-9</div>
  <div class="col-xs-4">.col-xs-4<br>Since 9 + 4 = 13 &gt; 12, this 4-column-wide div gets wrapped onto a new line as one contiguous unit.</div>
  <div class="col-xs-6">.col-xs-6<br>Subsequent columns continue along the new line.</div>
</div>
```

### 响应列重置

在四层网格可用的情况下，你肯定会遇到一些问题，在某些断点处，你的专栏并不完全清晰，因为一个人比另一个更高。要解决这个问题，请使用a `.clearfix`和我们的响应式实用程序类的组合。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#grid-responsive-resets)

```javascript
<div class="row">
  <div class="col-xs-6 col-sm-3">.col-xs-6 .col-sm-3</div>
  <div class="col-xs-6 col-sm-3">.col-xs-6 .col-sm-3</div>

  <!-- Add the extra clearfix for only the required viewport -->
  <div class="clearfix visible-xs-block"></div>

  <div class="col-xs-6 col-sm-3">.col-xs-6 .col-sm-3</div>
  <div class="col-xs-6 col-sm-3">.col-xs-6 .col-sm-3</div>
</div>
```

除了响应断点处的列清除外，您可能需要**重置偏移，推送或拉取**。在[网格示例中](https://getbootstrap.com/docs/3.3/examples/grid/)查看此操作。

```javascript
<div class="row">
  <div class="col-sm-5 col-md-6">.col-sm-5 .col-md-6</div>
  <div class="col-sm-5 col-sm-offset-2 col-md-6 col-md-offset-0">.col-sm-5 .col-sm-offset-2 .col-md-6 .col-md-offset-0</div>
</div>

<div class="row">
  <div class="col-sm-6 col-md-5 col-lg-6">.col-sm-6 .col-md-5 .col-lg-6</div>
  <div class="col-sm-6 col-md-5 col-md-offset-2 col-lg-6 col-lg-offset-0">.col-sm-6 .col-md-5 .col-md-offset-2 .col-lg-6 .col-lg-offset-0</div>
</div>
```

### 偏移列

使用`.col-md-offset-*`类将列向右移动。这些类按`*`列增加列的左边距。例如，`.col-md-offset-4`移动`.col-md-4`四列。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#grid-offsetting)

```javascript
<div class="row">
  <div class="col-md-4">.col-md-4</div>
  <div class="col-md-4 col-md-offset-4">.col-md-4 .col-md-offset-4</div>
</div>
<div class="row">
  <div class="col-md-3 col-md-offset-3">.col-md-3 .col-md-offset-3</div>
  <div class="col-md-3 col-md-offset-3">.col-md-3 .col-md-offset-3</div>
</div>
<div class="row">
  <div class="col-md-6 col-md-offset-3">.col-md-6 .col-md-offset-3</div>
</div>
```

您也可以使用`.col-*-offset-0`类覆盖较低网格层的偏移量。

```javascript
<div class="row">
  <div class="col-xs-6 col-sm-4">
  </div>
  <div class="col-xs-6 col-sm-4">
  </div>
  <div class="col-xs-6 col-xs-offset-3 col-sm-4 col-sm-offset-0">
  </div>
</div>
```

### 嵌套列

嵌套使用默认网格您的内容，添加新的`.row`和一组`.col-sm-*`列现有的内`.col-sm-*`柱。嵌套行应包含一组最多不超过12个的列（不要求您使用全部12个可用列）。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#grid-nesting)

```javascript
<div class="row">
  <div class="col-sm-9">
    Level 1: .col-sm-9
    <div class="row">
      <div class="col-xs-8 col-sm-6">
        Level 2: .col-xs-8 .col-sm-6
      </div>
      <div class="col-xs-4 col-sm-6">
        Level 2: .col-xs-4 .col-sm-6
      </div>
    </div>
  </div>
</div>
```

### 列排序

使用`.col-md-push-*`和`.col-md-pull-*`修饰符类轻松更改内置网格列的顺序。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#grid-column-ordering)

```javascript
<div class="row">
  <div class="col-md-9 col-md-push-3">.col-md-9 .col-md-push-3</div>
  <div class="col-md-3 col-md-pull-9">.col-md-3 .col-md-pull-9</div>
</div>
```

### 较少的mixins和变量

除了用于快速布局的预先构建的网格类以外，Bootstrap还包括用于快速生成自己的简单语义布局的较少变量和混合。

#### 变量

变量决定列的数量，排水沟宽度以及开始浮动列的媒体查询点。我们使用这些来生成上面记录的预定义网格类，以及下面列出的自定义混合类。

```javascript
@grid-columns:              12;
@grid-gutter-width:         30px;
@grid-float-breakpoint:     768px;
```

#### 混合

Mixin与网格变量结合使用，为单个网格列生成语义CSS。

```javascript
// Creates a wrapper for a series of columns
.make-row(@gutter: @grid-gutter-width) {
  // Then clear the floated columns
  .clearfix();

  @media (min-width: @screen-sm-min) {
    margin-left:  (@gutter / -2);
    margin-right: (@gutter / -2);
  }

  // Negative margin nested rows out to align the content of columns
  .row {
    margin-left:  (@gutter / -2);
    margin-right: (@gutter / -2);
  }
}

// Generate the extra small columns
.make-xs-column(@columns; @gutter: @grid-gutter-width) {
  position: relative;
  // Prevent columns from collapsing when empty
  min-height: 1px;
  // Inner gutter via padding
  padding-left:  (@gutter / 2);
  padding-right: (@gutter / 2);

  // Calculate width based on number of columns available
  @media (min-width: @grid-float-breakpoint) {
    float: left;
    width: percentage((@columns / @grid-columns));
  }
}

// Generate the small columns
.make-sm-column(@columns; @gutter: @grid-gutter-width) {
  position: relative;
  // Prevent columns from collapsing when empty
  min-height: 1px;
  // Inner gutter via padding
  padding-left:  (@gutter / 2);
  padding-right: (@gutter / 2);

  // Calculate width based on number of columns available
  @media (min-width: @screen-sm-min) {
    float: left;
    width: percentage((@columns / @grid-columns));
  }
}

// Generate the small column offsets
.make-sm-column-offset(@columns) {
  @media (min-width: @screen-sm-min) {
    margin-left: percentage((@columns / @grid-columns));
  }
}
.make-sm-column-push(@columns) {
  @media (min-width: @screen-sm-min) {
    left: percentage((@columns / @grid-columns));
  }
}
.make-sm-column-pull(@columns) {
  @media (min-width: @screen-sm-min) {
    right: percentage((@columns / @grid-columns));
  }
}

// Generate the medium columns
.make-md-column(@columns; @gutter: @grid-gutter-width) {
  position: relative;
  // Prevent columns from collapsing when empty
  min-height: 1px;
  // Inner gutter via padding
  padding-left:  (@gutter / 2);
  padding-right: (@gutter / 2);

  // Calculate width based on number of columns available
  @media (min-width: @screen-md-min) {
    float: left;
    width: percentage((@columns / @grid-columns));
  }
}

// Generate the medium column offsets
.make-md-column-offset(@columns) {
  @media (min-width: @screen-md-min) {
    margin-left: percentage((@columns / @grid-columns));
  }
}
.make-md-column-push(@columns) {
  @media (min-width: @screen-md-min) {
    left: percentage((@columns / @grid-columns));
  }
}
.make-md-column-pull(@columns) {
  @media (min-width: @screen-md-min) {
    right: percentage((@columns / @grid-columns));
  }
}

// Generate the large columns
.make-lg-column(@columns; @gutter: @grid-gutter-width) {
  position: relative;
  // Prevent columns from collapsing when empty
  min-height: 1px;
  // Inner gutter via padding
  padding-left:  (@gutter / 2);
  padding-right: (@gutter / 2);

  // Calculate width based on number of columns available
  @media (min-width: @screen-lg-min) {
    float: left;
    width: percentage((@columns / @grid-columns));
  }
}

// Generate the large column offsets
.make-lg-column-offset(@columns) {
  @media (min-width: @screen-lg-min) {
    margin-left: percentage((@columns / @grid-columns));
  }
}
.make-lg-column-push(@columns) {
  @media (min-width: @screen-lg-min) {
    left: percentage((@columns / @grid-columns));
  }
}
.make-lg-column-pull(@columns) {
  @media (min-width: @screen-lg-min) {
    right: percentage((@columns / @grid-columns));
  }
}
```

#### 用法示例

您可以将变量修改为您自己的自定义值，或者仅将mixins与其默认值一起使用。以下是使用默认设置创建两列之间的间距的示例。

```javascript
.wrapper {
  .make-row();
}
.content-main {
  .make-lg-column(8);
}
.content-secondary {
  .make-lg-column(3);
  .make-lg-column-offset(1);
}
<div class="wrapper">
  <div class="content-main">...</div>
  <div class="content-secondary">...</div>
</div>
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-10-08