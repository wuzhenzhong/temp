# 组件 | Components

- 贡献者3人

  

超过十多种可重用组件，可以提供图像，下拉菜单，输入组，导航，警报等等。

## **Glyphicons**

### 可用字形

包含 Glyphicon Halflings 集的超过 250 种字体格式的字形。[Glyphicons](http://glyphicons.com/) Halflings 通常不是免费的，但他们的创造者已经把它们免费提供给 Bootstrap。作为对他们的一种感谢与尊重，我们只要求您在可能的情况下包含一个链接到 [Glyphicons ](http://glyphicons.com/)的链接。

[打开 getbootstrap.com 上的示例](https://getbootstrap.com/docs/3.3/components/#glyphicons-glyphs)

### 如何使用

由于性能原因，所有图标都需要一个基类和单个图标类。若要使用，请将以下代码放置在任何位置。一定要在图标和文本之间留出一个空格，以便适当地填充。

##### 不要与其他组件混合

图标类不能直接与其他组件组合。他们不应该与同一元素上的其他类一起使用。相反，添加嵌套`<span>`并将图标类应用于`<span>`。

##### 仅用于空元素

图标类只应用于不包含文本内容且没有子元素的元素。

##### 更改图标字体位置

引导程序假定图标字体文件将位于`../fonts/`目录，相对于已编译的CSS文件。移动或重命名这些字体文件意味着以三种方式之一更新CSS：

- 更改源文件中的`@icon-font-path`和/或`@icon-font-name`变量。

利用 Less 编译器提供的 [URL 相对路径](http://lesscss.org/usage/#command-line-usage-relative-urls) 选项。

- 更改 CSS 中`url()`路径。

使用任何最适合您的特定开发设置的选项。

##### 可访问图标

现代版的辅助技术将导致生成CSS的内容以及特定的Unicode字符。为了避免在屏幕阅读器中出现意外和混乱的输出（特别是当图标纯粹用于装饰时），我们用`aria-hidden="true"`属性隐藏它们。

如果您使用图标来传达意义（而不仅仅是作为一种装饰元素），请确保将这一意义传达给辅助技术 - 例如，包含额外的内容，这些内容在视觉上隐藏在`.sr-only`类中。

如果您创建的控件没有其他文本（例如`<button>`只包含图标），则应始终提供替代内容以确定控件的用途，以便对辅助技术的用户有意义。在这种情况下，您可以在控件本身添加一个属性`aria-label`。

```javascript
<span class="glyphicon glyphicon-search" aria-hidden="true"></span>
```

### 实例

在按钮、工具栏按钮组、导航或预置表单输入中使用它们。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#glyphicons-examples)

```javascript
<button type="button" class="btn btn-default" aria-label="Left Align">
  <span class="glyphicon glyphicon-align-left" aria-hidden="true"></span>
</button>

<button type="button" class="btn btn-default btn-lg">
  <span class="glyphicon glyphicon-star" aria-hidden="true"></span> Star
</button>
```

警报中使用的图标表示这是一条错误消息，并附加`.sr-only`文字向辅助技术的用户传达此提示。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#glyphicons-examples)

```javascript
<div class="alert alert-danger" role="alert">
  <span class="glyphicon glyphicon-exclamation-sign" aria-hidden="true"></span>
  <span class="sr-only">Error:</span>
  Enter a valid email address
</div>
```

## 下拉菜单

用于显示链接列表的可切换的上下文菜单。与下拉式JavaScript插件交互。

### 示例

将下拉列表中的触发器和下拉菜单`.dropdown`或其他声明的元素包裹起来`position: relative;`。然后添加菜单的HTML。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#dropdowns-example)

```javascript
<div class="dropdown">
  <button class="btn btn-default dropdown-toggle" type="button" id="dropdownMenu1" data-toggle="dropdown" aria-haspopup="true" aria-expanded="true">
    Dropdown
    <span class="caret"></span>
  </button>
  <ul class="dropdown-menu" aria-labelledby="dropdownMenu1">
    <li><a href="#">Action</a></li>
    <li><a href="#">Another action</a></li>
    <li><a href="#">Something else here</a></li>
    <li role="separator" class="divider"></li>
    <li><a href="#">Separated link</a></li>
  </ul>
</div>
```

通过添加`.dropup`到父级，可以将下拉菜单更改为向上扩展（而不是向下扩展）。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#dropdowns-example)

```javascript
<div class="dropup">
  <button class="btn btn-default dropdown-toggle" type="button" id="dropdownMenu2" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    Dropup
    <span class="caret"></span>
  </button>
  <ul class="dropdown-menu" aria-labelledby="dropdownMenu2">
    <li><a href="#">Action</a></li>
    <li><a href="#">Another action</a></li>
    <li><a href="#">Something else here</a></li>
    <li role="separator" class="divider"></li>
    <li><a href="#">Separated link</a></li>
  </ul>
</div>
```

### 对齐

默认情况下，下拉菜单自动从父级的顶部和左侧定位100％。添加`.dropdown-menu-right`到`.dropdown-menu`右侧对齐下拉菜单。

##### 可能需要额外的定位

下拉菜单通过CSS在文档的正常流程中自动定位。这意味着下拉菜单可能会由具有某些`overflow`属性的父母裁剪或出现在视口范围之外。在出现问题时自行解决这些问题。

##### 弃用`.pull-right`对齐

在3.1.0版本中，我们不建议使用`.pull-right`下拉菜单上。若要向右对齐菜单，请使用`.dropdown-menu-right`.导航栏中的对齐导航组件使用这个类的混合版本来自动对齐菜单。若要重写它，请使用`.dropdown-menu-left`...

```javascript
<ul class="dropdown-menu dropdown-menu-right" aria-labelledby="dLabel">
  ...
</ul>
```

### 标头

在任何下拉菜单中添加一个标题来标记操作的部分。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#dropdowns-headers)

```javascript
<ul class="dropdown-menu" aria-labelledby="dropdownMenu3">
  ...
  <li class="dropdown-header">Dropdown header</li>
  ...
</ul>
```

### 分频器

在下拉菜单中添加一个分隔符来分隔一系列链接。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#dropdowns-divider)

```javascript
<ul class="dropdown-menu" aria-labelledby="dropdownMenuDivider">
  ...
  <li role="separator" class="divider"></li>
  ...
</ul>
```

### 禁用菜单项

添加`.disabled`到`<li>`下拉菜单中禁用链接。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#dropdowns-disabled)

```javascript
<ul class="dropdown-menu" aria-labelledby="dropdownMenu4">
  <li><a href="#">Regular link</a></li>
  <li class="disabled"><a href="#">Disabled link</a></li>
  <li><a href="#">Another link</a></li>
</ul>
```

## 按钮组

用按钮组将一系列按钮组合在一行上。使用我们的按钮插件添加可选的JavaScript广播和复选框样式行为。

##### 按钮组中的工具提示和弹出需要特殊设置

当在`.btn-group`中的元素上使用工具提示或弹出窗口时，必须指定选项`container: 'body'`以避免不必要的副作用（例如，当工具提示或弹出窗口被触发时元素会变得更宽和/或失去圆角）。

##### 确保正确`role`并提供一个标签

为了让辅助技术（如屏幕阅读器）传达一系列按钮的分组，`role`需要提供适当的属性。对于按钮组，这将是`role="group"`，而工具栏应该有一个`role="toolbar"`。

一个例外是只包含单个控件的组（例如，带有`<button>`元素的对齐按钮组）或下拉菜单。

另外，应该给组和工具栏一个明确的标签，因为尽管存在正确的`role`属性，但大多数辅助技术将不会公布它们。在这里提供的例子中，我们使用了`aria-label`，但`aria-labelledby`也可以使用替代方法。

### 基本实例

用在`.btn-group`中的`.btn` 包裹一系列按钮

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#btn-groups-single)

```javascript
<div class="btn-group" role="group" aria-label="...">
  <button type="button" class="btn btn-default">Left</button>
  <button type="button" class="btn btn-default">Middle</button>
  <button type="button" class="btn btn-default">Right</button>
</div>
```

### 按钮工具栏

对于更复杂的组件，组合集`<div class="btn-group">`变成`<div class="btn-toolbar">`。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#btn-groups-toolbar)

```javascript
<div class="btn-toolbar" role="toolbar" aria-label="...">
  <div class="btn-group" role="group" aria-label="...">...</div>
  <div class="btn-group" role="group" aria-label="...">...</div>
  <div class="btn-group" role="group" aria-label="...">...</div>
</div>
```

### 调整大小

不要将按钮大小类应用于组中的每个按钮，只需添加`.btn-group-*`致每一个`.btn-group`，包括嵌套多个组时。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#btn-groups-sizing)

```javascript
<div class="btn-group btn-group-lg" role="group" aria-label="...">...</div>
<div class="btn-group" role="group" aria-label="...">...</div>
<div class="btn-group btn-group-sm" role="group" aria-label="...">...</div>
<div class="btn-group btn-group-xs" role="group" aria-label="...">...</div>
```

### 嵌套

当你想要下拉菜单与一系列按钮混合在一起时，放置`.btn-group`在另一个`.btn-group`之内。



[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#btn-groups-nested)

```javascript
<div class="btn-group" role="group" aria-label="...">
  <button type="button" class="btn btn-default">1</button>
  <button type="button" class="btn btn-default">2</button>

  <div class="btn-group" role="group">
    <button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
      Dropdown
      <span class="caret"></span>
    </button>
    <ul class="dropdown-menu">
      <li><a href="#">Dropdown link</a></li>
      <li><a href="#">Dropdown link</a></li>
    </ul>
  </div>
</div>
```

### 垂直变化

使一组按钮出现垂直堆叠而不是水平叠加。**这里不支持拆分按钮下拉。**

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#btn-groups-vertical)

```javascript
<div class="btn-group-vertical" role="group" aria-label="...">
  ...
</div>
```

### 对齐按钮组

使一组按钮按相同的大小拉伸，以跨越父节点的整个宽度。也适用于按钮组中的按钮下拉。

##### 处理边界

由于用来证明按钮（即`display: table-cell`）的特定HTML和CSS ，它们之间的边界加倍。在常规的按钮组中，`margin-left: -1px`用于堆叠边框而不是将其删除。但是，`margin`不适用`display: table-cell`。因此，根据您对Bootstrap的定制，您可能希望删除或重新添加边框颜色。

##### IE8和边界

Internet Explorer 8不会在对齐按钮组中的按钮上呈现边框，无论该按钮是处于打开状态`<a>`还是处于`<button>`元素上。为了解决这个问题，将每个按钮换成另一个按钮`.btn-group`。

请参阅[＃12476](https://github.com/twbs/bootstrap/issues/12476)了解更多信息。

##### 带着`<a>`元素

只是包装一系列的`.btn`s `.btn-group.btn-group-justified`。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#callout-btn-group-ie8-border)

```javascript
<div class="btn-group btn-group-justified" role="group" aria-label="...">
  ...
</div>
```

##### 作为按钮的链接

如果`<a>`元素用作按钮 - 触发页内功能，而不是导航到当前页面中的另一个文档或部分 - 则应该给它们适当的值`role="button"`。

##### 带着`<button>`元素

使用对齐按钮组`<button>`元素，**必须将每个按钮包装在按钮组中。**大多数浏览器正确地将css应用于`<button>`元素，但由于我们支持按钮下拉，我们可以绕过它。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#callout-btn-group-anchor-btn)

```javascript
<div class="btn-group btn-group-justified" role="group" aria-label="...">
  <div class="btn-group" role="group">
    <button type="button" class="btn btn-default">Left</button>
  </div>
  <div class="btn-group" role="group">
    <button type="button" class="btn btn-default">Middle</button>
  </div>
  <div class="btn-group" role="group">
    <button type="button" class="btn btn-default">Right</button>
  </div>
</div>
```

## 按钮下拉

使用任意按钮触发下拉菜单，方法是将下拉菜单放置在`.btn-group`并提供正确的菜单标记。

##### 插件依赖性

按钮下拉列表要求下拉插件包括在你的Bootstrap版本中。

### 单按钮下拉

通过一些基本的标记更改，将按钮转换为下拉按钮。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#btn-dropdowns-single)

```javascript
<!-- Single button -->
<div class="btn-group">
  <button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    Action <span class="caret"></span>
  </button>
  <ul class="dropdown-menu">
    <li><a href="#">Action</a></li>
    <li><a href="#">Another action</a></li>
    <li><a href="#">Something else here</a></li>
    <li role="separator" class="divider"></li>
    <li><a href="#">Separated link</a></li>
  </ul>
</div>
```

### 拆分按钮下拉

类似地，创建带有相同标记更改的拆分按钮下拉列表，只使用单独的按钮。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#btn-dropdowns-split)

```javascript
<!-- Split button -->
<div class="btn-group">
  <button type="button" class="btn btn-danger">Action</button>
  <button type="button" class="btn btn-danger dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    <span class="caret"></span>
    <span class="sr-only">Toggle Dropdown</span>
  </button>
  <ul class="dropdown-menu">
    <li><a href="#">Action</a></li>
    <li><a href="#">Another action</a></li>
    <li><a href="#">Something else here</a></li>
    <li role="separator" class="divider"></li>
    <li><a href="#">Separated link</a></li>
  </ul>
</div>
```

### 改变大小

按钮下拉式适用于各种大小的按钮。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#btn-dropdowns-sizing)

```javascript
<!-- Large button group -->
<div class="btn-group">
  <button class="btn btn-default btn-lg dropdown-toggle" type="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    Large button <span class="caret"></span>
  </button>
  <ul class="dropdown-menu">
    ...
  </ul>
</div>

<!-- Small button group -->
<div class="btn-group">
  <button class="btn btn-default btn-sm dropdown-toggle" type="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    Small button <span class="caret"></span>
  </button>
  <ul class="dropdown-menu">
    ...
  </ul>
</div>

<!-- Extra small button group -->
<div class="btn-group">
  <button class="btn btn-default btn-xs dropdown-toggle" type="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    Extra small button <span class="caret"></span>
  </button>
  <ul class="dropdown-menu">
    ...
  </ul>
</div>
```

### 丢弃变化

通过添加`.dropup`到父级，触发元素上方的下拉菜单。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#btn-dropdowns-dropup)

```javascript
<div class="btn-group dropup">
  <button type="button" class="btn btn-default">Dropup</button>
  <button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    <span class="caret"></span>
    <span class="sr-only">Toggle Dropdown</span>
  </button>
  <ul class="dropdown-menu">
    <!-- Dropdown menu links -->
  </ul>
</div>
```

## 输入组

通过在任何基于文本的文本之前，之后或两侧添加文本或按钮来扩展表单控件`<input>`。`.input-group`与`.input-group-addon`or `.input-group-btn`一起使用可以预先添加或附加元素`.form-control`。

##### `<input>`仅限文本

避免使用`<select>`元素，因为它们不能在WebKit浏览器中完全样式化。

避免在这里使用`<textarea>`元素，因为在某些情况下`rows`属性不受支持。

##### 输入组中的工具提示和弹出需要特殊设置

当在一个元素中使用工具提示或弹出元素时`.input-group`，您必须指定选项`container: 'body'`以避免不必要的副作用（例如，当工具提示或弹出窗口被触发时，元素会变宽和/或失去圆角）。

##### 不要与其他部件混合

不要直接将表单组或网格列类与输入组混合。相反，将输入组嵌套在窗体组或与网格相关的元素中。

##### 总是添加标签

如果您不为每个输入包含一个标签，屏幕阅读器就会遇到表单问题。对于这些输入组，确保向辅助技术传递任何附加的标签或功能。

要使用的确切技术（可视`<label>`元素，`<label>`使用隐藏要素`.sr-only`类，或者使用的`aria-label`，`aria-labelledby`，`aria-describedby`，`title`或`placeholder`属性）和哪些附加信息需要传达将取决于你实现接口小窗口的确切类型而有所不同。本节中的示例提供了一些建议的特定于案例的方法。

### 基本实例

在输入的两边放置一个加载项或按钮.。您也可以在输入的两边放置一个。

**我们不支持**在单面上**添加多个附加组件（\****`.input-group-addon` ** **或** **.input-group-btn\****）。**

**我们不支持单个输入组中的多个窗体控件。**

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#input-groups-basic)

```javascript
<div class="input-group">
  <span class="input-group-addon" id="basic-addon1">@</span>
  <input type="text" class="form-control" placeholder="Username" aria-describedby="basic-addon1">
</div>

<div class="input-group">
  <input type="text" class="form-control" placeholder="Recipient's username" aria-describedby="basic-addon2">
  <span class="input-group-addon" id="basic-addon2">@example.com</span>
</div>

<div class="input-group">
  <span class="input-group-addon">$</span>
  <input type="text" class="form-control" aria-label="Amount (to the nearest dollar)">
  <span class="input-group-addon">.00</span>
</div>

<label for="basic-url">Your vanity URL</label>
<div class="input-group">
  <span class="input-group-addon" id="basic-addon3">https://example.com/users/</span>
  <input type="text" class="form-control" id="basic-url" aria-describedby="basic-addon3">
</div>
```

### 调整大小

将相对窗体调整大小类添加到`.input-group`它本身和内部的内容将自动调整大小-不需要重复每个元素上的窗体控件大小类。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#input-groups-sizing)

```javascript
<div class="input-group input-group-lg">
  <span class="input-group-addon" id="sizing-addon1">@</span>
  <input type="text" class="form-control" placeholder="Username" aria-describedby="sizing-addon1">
</div>

<div class="input-group">
  <span class="input-group-addon" id="sizing-addon2">@</span>
  <input type="text" class="form-control" placeholder="Username" aria-describedby="sizing-addon2">
</div>

<div class="input-group input-group-sm">
  <span class="input-group-addon" id="sizing-addon3">@</span>
  <input type="text" class="form-control" placeholder="Username" aria-describedby="sizing-addon3">
</div>
```

### 复选框和单选框

将任何复选框或单选框放在输入组的插件中，而不是文本。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#input-groups-checkboxes-radios)

```javascript
<div class="row">
  <div class="col-lg-6">
    <div class="input-group">
      <span class="input-group-addon">
        <input type="checkbox" aria-label="...">
      </span>
      <input type="text" class="form-control" aria-label="...">
    </div><!-- /input-group -->
  </div><!-- /.col-lg-6 -->
  <div class="col-lg-6">
    <div class="input-group">
      <span class="input-group-addon">
        <input type="radio" aria-label="...">
      </span>
      <input type="text" class="form-control" aria-label="...">
    </div><!-- /input-group -->
  </div><!-- /.col-lg-6 -->
</div><!-- /.row -->
```

### 按钮加载项

输入组中的按钮有点不同，需要额外的嵌套级别。而不是`.input-group-addon`，您需要使用`.input-group-btn`把纽扣包起来。这是必需的，因为默认浏览器样式不能被覆盖。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#input-groups-buttons)

```javascript
<div class="row">
  <div class="col-lg-6">
    <div class="input-group">
      <span class="input-group-btn">
        <button class="btn btn-default" type="button">Go!</button>
      </span>
      <input type="text" class="form-control" placeholder="Search for...">
    </div><!-- /input-group -->
  </div><!-- /.col-lg-6 -->
  <div class="col-lg-6">
    <div class="input-group">
      <input type="text" class="form-control" placeholder="Search for...">
      <span class="input-group-btn">
        <button class="btn btn-default" type="button">Go!</button>
      </span>
    </div><!-- /input-group -->
  </div><!-- /.col-lg-6 -->
</div><!-- /.row -->
```

### 带下垂的按钮

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#input-groups-buttons-dropdowns)

```javascript
<div class="row">
  <div class="col-lg-6">
    <div class="input-group">
      <div class="input-group-btn">
        <button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">Action <span class="caret"></span></button>
        <ul class="dropdown-menu">
          <li><a href="#">Action</a></li>
          <li><a href="#">Another action</a></li>
          <li><a href="#">Something else here</a></li>
          <li role="separator" class="divider"></li>
          <li><a href="#">Separated link</a></li>
        </ul>
      </div><!-- /btn-group -->
      <input type="text" class="form-control" aria-label="...">
    </div><!-- /input-group -->
  </div><!-- /.col-lg-6 -->
  <div class="col-lg-6">
    <div class="input-group">
      <input type="text" class="form-control" aria-label="...">
      <div class="input-group-btn">
        <button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">Action <span class="caret"></span></button>
        <ul class="dropdown-menu dropdown-menu-right">
          <li><a href="#">Action</a></li>
          <li><a href="#">Another action</a></li>
          <li><a href="#">Something else here</a></li>
          <li role="separator" class="divider"></li>
          <li><a href="#">Separated link</a></li>
        </ul>
      </div><!-- /btn-group -->
    </div><!-- /input-group -->
  </div><!-- /.col-lg-6 -->
</div><!-- /.row -->
```

### 分段按钮

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#input-groups-buttons-segmented)

```javascript
<div class="input-group">
  <div class="input-group-btn">
    <!-- Button and dropdown menu -->
  </div>
  <input type="text" class="form-control" aria-label="...">
</div>

<div class="input-group">
  <input type="text" class="form-control" aria-label="...">
  <div class="input-group-btn">
    <!-- Button and dropdown menu -->
  </div>
</div>
```

### 多个按钮

虽然每个边只能有一个外接程序，但是可以在单个内有多个按钮。`.input-group-btn`...

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#input-groups-buttons-multiple)

```javascript
<div class="input-group">
  <div class="input-group-btn">
    <!-- Buttons -->
  </div>
  <input type="text" class="form-control" aria-label="...">
</div>

<div class="input-group">
  <input type="text" class="form-control" aria-label="...">
  <div class="input-group-btn">
    <!-- Buttons -->
  </div>
</div>
```

## Navs

引导程序中可用的NAVA具有共享标记，首先从基础开始`.nav`类以及共享状态。交换修饰符类以在每种样式之间切换。

##### 为选项卡面板使用NAVS需要JavaScript选项卡插件

对于具有可选项卡区域的选项卡，必须使用制表符JavaScript插件标记还将需要额外的`role`和aria属性-参见插件%27s示例标记更多细节。

##### 使导航成为可访问的

如果您正在使用导航条来提供导航条，请确保添加`role="navigation"`对象的最逻辑父容器。`<ul>`，或将一个`<nav>`元素环绕整个导航。不要将角色添加到`<ul>`这本身就阻止了辅助技术将其作为一个实际清单公布。

### 制表符

注意`.nav-tabs`类需要`.nav`基类。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#nav-tabs)

```javascript
<ul class="nav nav-tabs">
  <li role="presentation" class="active"><a href="#">Home</a></li>
  <li role="presentation"><a href="#">Profile</a></li>
  <li role="presentation"><a href="#">Messages</a></li>
</ul>
```

Pills

使用相同的HTML，但请使用`.nav-pills`相反：

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#nav-pills)

```javascript
<ul class="nav nav-pills">
  <li role="presentation" class="active"><a href="#">Home</a></li>
  <li role="presentation"><a href="#">Profile</a></li>
  <li role="presentation"><a href="#">Messages</a></li>
</ul>
```

pill添加`.nav-stacked`后变为垂直样式

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#nav-pills)

```javascript
<ul class="nav nav-pills nav-stacked">
  ...
</ul>
```

原因

很容易使标签或药丸在屏幕上的宽度相等，屏幕宽度大于768 px。`.nav-justified`.在较小的屏幕上，导航链接被堆放起来。

**目前不支持正确的导航链接。**

##### 狩猎和反应正确的导航系统

在9.1.2版本中，Safari显示了一个bug，在这个bug中，水平调整浏览器会导致对齐NAV中的呈现错误，这些错误在刷新时被清除。此错误还显示在[正确的NAV示例](https://getbootstrap.com/docs/3.3/examples/justified-nav/)...

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#callout-navs-justified-safari)

```javascript
<ul class="nav nav-tabs nav-justified">
  ...
</ul>
<ul class="nav nav-pills nav-justified">
  ...
</ul>
```

### 禁用链接

对于任何NAV组件（标签）中，添加`.disabled`为**灰色链接和没有悬停效果**。

##### 链接功能未受影响

类将只更改`<a>`的外观，而不是它的功能。在这里使用自定义JavaScript禁用链接。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#callout-navs-anchor-disabled)

```javascript
<ul class="nav nav-pills">
  ...
  <li role="presentation" class="disabled"><a href="#">Disabled link</a></li>
  ...
</ul>
```

### 使用下拉

添加带有少量额外HTML的下拉菜单，下拉JavaScript插件...

#### 带下拉选项卡

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#nav-dropdowns)

```javascript
<ul class="nav nav-tabs">
  ...
  <li role="presentation" class="dropdown">
    <a class="dropdown-toggle" data-toggle="dropdown" href="#" role="button" aria-haspopup="true" aria-expanded="false">
      Dropdown <span class="caret"></span>
    </a>
    <ul class="dropdown-menu">
      ...
    </ul>
  </li>
  ...
</ul>
```

#### 弃用pills

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#nav-dropdowns)

```javascript
<ul class="nav nav-pills">
  ...
  <li role="presentation" class="dropdown">
    <a class="dropdown-toggle" data-toggle="dropdown" href="#" role="button" aria-haspopup="true" aria-expanded="false">
      Dropdown <span class="caret"></span>
    </a>
    <ul class="dropdown-menu">
      ...
    </ul>
  </li>
  ...
</ul>
```

## 导航栏

### 默认导航条

导航栏是响应式元组件，可用作应用程序或站点的导航标题。它们在移动视图中开始折叠（并且可切换），并随着可用视口宽度的增加而变为水平。

**目前不支持正确的导航链接。**

##### 溢出量

由于Bootstrap不知道导航栏中的内容需要多少空间，所以您可能会遇到将内容包装到第二行的问题。要解决这个问题，您可以：

1. 减少导航栏项目的数量或宽度。

\2. 使用响应式实用程序类以某些屏幕大小隐藏某些导航栏项目。

3.更改导航栏在折叠和水平模式之间切换的点。自定义`@grid-float-breakpoint`变量或添加您自己的媒体查询。

##### 需要JavaScript插件

如果禁用了JavaScript，并且viewport足够窄，使导航条崩溃，则不可能展开Navbar并查看`.navbar-collapse`...

响应导航栏需要崩溃插件包括在你的Bootstrap版本中。

##### 更改折叠的移动导航条断点

当视口比视图窄时，导航条就会折叠到垂直移动视图中。`@grid-float-breakpoint`，并扩展到至少当视口为非移动视图时的水平非移动视图。`@grid-float-breakpoint`宽度。在“较少”源中调整此变量，以控制导航条何时折叠/展开。默认值是`768px`%28最小的“小”或“平板”屏幕%29。

##### 使导航条可访问

确保使用`<nav>`元素，或者，如果使用更通用的元素，如`<div>`，添加`role="navigation"`每个导航栏都明确地将其确定为一个具有里程碑意义的区域，供辅助技术的用户使用。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#callout-navbar-role)

```javascript
<nav class="navbar navbar-default">
  <div class="container-fluid">
    <!-- Brand and toggle get grouped for better mobile display -->
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="#">Brand</a>
    </div>

    <!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav">
        <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li role="separator" class="divider"></li>
            <li><a href="#">Separated link</a></li>
            <li role="separator" class="divider"></li>
            <li><a href="#">One more separated link</a></li>
          </ul>
        </li>
      </ul>
      <form class="navbar-form navbar-left">
        <div class="form-group">
          <input type="text" class="form-control" placeholder="Search">
        </div>
        <button type="submit" class="btn btn-default">Submit</button>
      </form>
      <ul class="nav navbar-nav navbar-right">
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li role="separator" class="divider"></li>
            <li><a href="#">Separated link</a></li>
          </ul>
        </li>
      </ul>
    </div><!-- /.navbar-collapse -->
  </div><!-- /.container-fluid -->
</nav>
```

### 品牌形象

通过交换文本来替换导航栏和自己的图像`<img>`。由于它`.navbar-brand`有自己的填充和高度，因此可能需要根据图像重写一些CSS。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#navbar-brand-image)

```javascript
<nav class="navbar navbar-default">
  <div class="container-fluid">
    <div class="navbar-header">
      <a class="navbar-brand" href="#">
        <img alt="Brand" src="...">
      </a>
    </div>
  </div>
</nav>
```

### 表格

表格内容`.navbar-form`正确的垂直对齐和狭窄视口中的折叠行为。使用对齐选项来决定它驻留在导航栏内容中的位置。

作为一个提个醒，`.navbar-form`将其大部分代码与`.form-inline`通过混音。**某些窗体控件，如输入组，可能需要在导航栏中正确显示固定宽度。**

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#navbar-forms)

```javascript
<form class="navbar-form navbar-left" role="search">
  <div class="form-group">
    <input type="text" class="form-control" placeholder="Search">
  </div>
  <button type="submit" class="btn btn-default">Submit</button>
</form>
```

##### 移动设备警告

在移动设备上使用固定元素中的表单控件有一些警告。请参阅我们的浏览器支持文档。关于细节。

##### 总是添加标签

如果您没有为每个输入添加标签，屏幕阅读器将会对您的表单造成麻烦。对于这些内联表单，您可以使用`.sr-only`该类隐藏标签。存在用于辅助技术，如提供一个标签的进一步的替代方法`aria-label`，`aria-labelledby`或者`title`属性。如果这些都不存在，屏幕阅读器可能会诉诸使用该`placeholder`属性（如果存在），但请注意`placeholder`不建议使用替代其他标记方法。

### 纽扣

将该`.navbar-btn`类添加到`<button>`不在a中的元素以将`<form>`其垂直居中放置在导航栏中。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#navbar-buttons)

```javascript
<button type="button" class="btn btn-default navbar-btn">Sign in</button>
```

##### 特定上下文用法

标准按钮类等，`.navbar-btn`可以在使用`<a>`和`<input>`元件。但是，内部的元素都不`.navbar-btn`应该使用标准按钮类。`<a>.navbar-nav`

### 文本

在元素中用`.navbar-text`，通常在`<p>`标签，以正确的领导和颜色。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#navbar-text)

```javascript
<p class="navbar-text">Signed in as Mark Otto</p>
```

### 非导航链接

对于使用常规导航组件中不存在的标准链接的人员，请使用`.navbar-link`类为默认和逆导航条选项添加适当的颜色。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#navbar-links)

```javascript
<p class="navbar-text navbar-right">Signed in as <a href="#" class="navbar-link">Mark Otto</a></p>
```

### 组件对齐

使用`.navbar-left`或`.navbar-right`实用程序类。两个类都将按照指定的方向添加CSS浮点数。例如，若要对齐导航链接，请将它们放在单独的`<ul>`应用了相应的实用程序类。

这些类是混合版本的`.pull-left`和`.pull-right`，但它们的作用域为媒体查询，以便更容易地处理跨设备大小的导航条组件。

##### 对齐多个分量

Navbar目前对多个`.navbar-right`类有限制。为了正确地分隔内容，我们在最后一个`.navbar-right`元素上使用负边距。当有多个使用该类的元素时，这些边距不能按预期工作。

当我们可以在v4中重写该组件时，我们将重新考虑这个问题。

### 固定到顶部

添加`.navbar-fixed-top`并包含一个`.container`或`.container-fluid`中心并填充导航栏内容。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#navbar-fixed-top)

```javascript
<nav class="navbar navbar-default navbar-fixed-top">
  <div class="container">
    ...
  </div>
</nav>
```

##### 需要体垫

固定的导航栏将覆盖您的其他内容，除非您添加`padding`到顶部`<body>`尝试你自己的价值观，或者使用下面的片段。提示：默认情况下，导航条高50 px。

```javascript
body { padding-top: 70px; }
```

一定要包括这个**后**核心引导CSS。

### 固定到底部

加`.navbar-fixed-bottom`并包括一个`.container`或`.container-fluid`以中心和垫肚脐的内容。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#navbar-fixed-bottom)

```javascript
<nav class="navbar navbar-default navbar-fixed-bottom">
  <div class="container">
    ...
  </div>
</nav>
```

##### 需要填充body

固定的导航栏将覆盖您的其他内容，除非您添加`padding`的底部`<body>`尝试你自己的价值观，或者使用下面的片段。提示：默认情况下，导航条高50 px。

```javascript
body { padding-bottom: 70px; }
```

一定要包括这个**后**核心引导CSS。

### 静态顶

创建一个全宽度的导航条，通过添加`.navbar-static-top`并包括一个`.container`或`.container-fluid`以中心和垫肚脐的内容。

与`.navbar-fixed-*`类不同，你不需要改变任何填充`body`。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#navbar-static-top)

```javascript
<nav class="navbar navbar-default navbar-static-top">
  <div class="container">
    ...
  </div>
</nav>
```

### 倒置的导航栏

通过添加来修改导航栏的外观`.navbar-inverse`。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#navbar-inverted)

```javascript
<nav class="navbar navbar-inverse">
  ...
</nav>
```

## Breadcrumbs



指示导航层次结构内当前页面的位置。

分隔符在css中自动添加`:before`和`content`...

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#breadcrumbs)

```javascript
<ol class="breadcrumb">
  <li><a href="#">Home</a></li>
  <li><a href="#">Library</a></li>
  <li class="active">Data</li>
</ol>
```

## 分页

为您的站点或应用程序提供多页分页组件的分页链接，或使用更简单的分页组件。寻呼机备选方案...

### 默认分页

简单的分页灵感来自Rdio，很适合应用程序和搜索结果。大块很难错过，很容易扩展，并且提供了大的点击区域。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#pagination-default)

```javascript
<nav aria-label="Page navigation">
  <ul class="pagination">
    <li>
      <a href="#" aria-label="Previous">
        <span aria-hidden="true">&laquo;</span>
      </a>
    </li>
    <li><a href="#">1</a></li>
    <li><a href="#">2</a></li>
    <li><a href="#">3</a></li>
    <li><a href="#">4</a></li>
    <li><a href="#">5</a></li>
    <li>
      <a href="#" aria-label="Next">
        <span aria-hidden="true">&raquo;</span>
      </a>
    </li>
  </ul>
</nav>
```

##### 对分页组件进行标记

分页组件应包装在`<nav>`元素将其标识为用于筛选阅读器和其他辅助技术的导航部分。此外，由于页面可能已经有多个这样的导航部分，例如头中的主导航，或者侧栏导航%29，因此建议提供一个描述性的导航部分。`aria-label`为`<nav>`这反映了它的目的。例如，如果分页组件用于在一组搜索结果之间导航，则可以使用适当的标签`aria-label="Search results pages"`...

#### 禁用和活动状态

链接可以根据不同的情况定制。使用`.disabled`对于不可点击的链接和`.active`若要指示当前页，请执行以下操作。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#callout-pagination-label)

```javascript
<nav aria-label="...">
  <ul class="pagination">
    <li class="disabled"><a href="#" aria-label="Previous"><span aria-hidden="true">&laquo;</span></a></li>
    <li class="active"><a href="#">1 <span class="sr-only">(current)</span></a></li>
    ...
  </ul>
</nav>
```

我们建议您将活动的或禁用的锚交换为`<span>`，或者在使用前一个/下一个箭头的情况下省略锚，以删除单击功能，同时保留预期的样式。

```javascript
<nav aria-label="...">
  <ul class="pagination">
    <li class="disabled">
      <span>
        <span aria-hidden="true">&laquo;</span>
      </span>
    </li>
    <li class="active">
      <span>1 <span class="sr-only">(current)</span></span>
    </li>
    ...
  </ul>
</nav>
```

#### 改变大小

更大或更小的分页？添加`.pagination-lg`或`.pagination-sm`

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#callout-pagination-label)

```javascript
<nav aria-label="..."><ul class="pagination pagination-lg">...</ul></nav>
<nav aria-label="..."><ul class="pagination">...</ul></nav>
<nav aria-label="..."><ul class="pagination pagination-sm">...</ul></nav>
```

### 传呼机

使用浅色标记和样式的简单分页实现的快速上一个和下一个链接。对于博客或杂志等简单网站来说，这非常棒。

#### 默认示例

默认情况下，寻呼机以链接为中心。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#pagination-pager)

```javascript
<nav aria-label="...">
  <ul class="pager">
    <li><a href="#">Previous</a></li>
    <li><a href="#">Next</a></li>
  </ul>
</nav>
```

#### 对齐链接

或者，您可以将每个链接对齐到两侧：

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#pagination-pager)

```javascript
<nav aria-label="...">
  <ul class="pager">
    <li class="previous"><a href="#"><span aria-hidden="true">&larr;</span> Older</a></li>
    <li class="next"><a href="#">Newer <span aria-hidden="true">&rarr;</span></a></li>
  </ul>
</nav>
```

#### 可选禁用状态

寻呼机链接也使用通用`.disabled`从分页中提取的实用程序类。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#pagination-pager)

```javascript
<nav aria-label="...">
  <ul class="pager">
    <li class="previous disabled"><a href="#"><span aria-hidden="true">&larr;</span> Older</a></li>
    <li class="next"><a href="#">Newer <span aria-hidden="true">&rarr;</span></a></li>
  </ul>
</nav>
```

## 标签

### 例

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#labels)

```javascript
<h3>Example heading <span class="label label-default">New</span></h3>
```

### 可用变体

添加下面提到的任何修饰符类以更改标签的外观。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#labels)

```javascript
<span class="label label-default">Default</span>
<span class="label label-primary">Primary</span>
<span class="label label-success">Success</span>
<span class="label label-info">Info</span>
<span class="label label-warning">Warning</span>
<span class="label label-danger">Danger</span>
```

##### 有很多标签吗？

如果在一个狭窄的容器中有许多内联标签，每个标签都包含自己的`inline-block`元素（如图标），则可能会出现渲染问题。解决方法是设置`display: inline-block;`。有关上下文和示例，[请参阅＃13219](https://github.com/twbs/bootstrap/issues/13219)。

## 徽章

通过添加`<span class="badge">`链接，引导导航，等等。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#badges)

```javascript
<a href="#">Inbox <span class="badge">42</span></a>

<button class="btn btn-primary" type="button">
  Messages <span class="badge">4</span>
</button>
```

##### 自塌陷

当没有新的或未读的项目时，`:empty`如果没有内容存在，徽章将简单地崩溃（通过CSS的选择器）。

##### 跨浏览器兼容性

徽章不会在Internet Explorer 8中自行崩溃，因为它缺少对`:empty`选择器的支持。

##### 适应主动导航状态

包括内置样式用于将徽章置于导航中的活动状态。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#callout-badges-ie8-empty)

```javascript
<ul class="nav nav-pills" role="tablist">
  <li role="presentation" class="active"><a href="#">Home <span class="badge">42</span></a></li>
  <li role="presentation"><a href="#">Profile</a></li>
  <li role="presentation"><a href="#">Messages <span class="badge">3</span></a></li>
</ul>
```

## **Jumbotron**



一个轻量级的、灵活的组件，它可以选择扩展整个视口以展示站点上的关键内容。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#jumbotron)

```javascript
<div class="jumbotron">
  <h1>Hello, world!</h1>
  <p>...</p>
  <p><a class="btn btn-primary btn-lg" href="#" role="button">Learn more</a></p>
</div>
```

为了使jumbotron完整宽度，并且没有圆角，将它放在所有`.container`s 之外，而是添加一个`.container`。

```javascript
<div class="jumbotron">
  <div class="container">
    ...
  </div>
</div>
```

## 页头

一个简单的外壳，用于在页面`h1`上适当地分隔和分割内容部分。它可以利用`h1`默认`small`元素以及大多数其他组件（附加样式）。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#page-header)

```javascript
<div class="page-header">
  <h1>Example page header <small>Subtext for header</small></h1>
</div>
```

## 缩略图

使用缩略图组件扩展Bootstrap的网格系统，轻松显示图像，视频，文本等网格。

如果您正在寻找像Pinterest一样呈现不同高度和/或宽度的缩略图，则需要使用第三方插件，如[砌体](http://masonry.desandro.com/)，[同位素](http://isotope.metafizzy.co/)或[Salvattore](http://salvattore.com/)。

### 默认示例

默认情况下，Bootstrap的缩略图旨在以最少的必需标记显示链接的图像。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#thumbnails-default)

```javascript
<div class="row">
  <div class="col-xs-6 col-md-3">
    <a href="#" class="thumbnail">
      <img src="..." alt="...">
    </a>
  </div>
  ...
</div>
```

### 定制内容

有了额外的标记，可以将任何类型的HTML内容添加到缩略图中，例如标题，段落或按钮。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#thumbnails-custom-content)

```javascript
<div class="row">
  <div class="col-sm-6 col-md-4">
    <div class="thumbnail">
      <img src="..." alt="...">
      <div class="caption">
        <h3>Thumbnail label</h3>
        <p>...</p>
        <p><a href="#" class="btn btn-primary" role="button">Button</a> <a href="#" class="btn btn-default" role="button">Button</a></p>
      </div>
    </div>
  </div>
</div>
```

## 警报

为典型的用户操作提供上下文反馈消息，并提供少量可用和灵活的警报消息。

### 实例

对于基本警报消息`.alert`，将四个上下文类（例如，`.alert-success`）中的任何文本和可选的解雇按钮和其中一个包装起来。

##### 没有默认类

警报没有默认类，只有基础类和修饰符类。默认的灰色警报没有多大意义，所以您需要通过上下文类指定类型。选择成功，信息，警告或危险。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#callout-alerts-no-default)

```javascript
<div class="alert alert-success" role="alert">...</div>
<div class="alert alert-info" role="alert">...</div>
<div class="alert alert-warning" role="alert">...</div>
<div class="alert alert-danger" role="alert">...</div>
```

### 可撤销警报

在任何警报上添加一个可选的`.alert-dismissible`还有关闭按钮。

##### 需要JavaScript警报插件

若要使警报充分发挥作用，则必须使用警报JavaScript插件...

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#callout-alerts-dismiss-plugin)

```javascript
<div class="alert alert-warning alert-dismissible" role="alert">
  <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span></button>
  <strong>Warning!</strong> Better check yourself, you're not looking too good.
</div>
```

##### 确保所有设备的正确行为

确保使用`<button>`元素的`data-dismiss="alert"`数据属性

### 警报链接

使用`.alert-link`实用工具类，以便在任何警报中快速提供匹配的彩色链接。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#alerts-links)

```javascript
<div class="alert alert-success" role="alert">
  <a href="#" class="alert-link">...</a>
</div>
<div class="alert alert-info" role="alert">
  <a href="#" class="alert-link">...</a>
</div>
<div class="alert alert-warning" role="alert">
  <a href="#" class="alert-link">...</a>
</div>
<div class="alert alert-danger" role="alert">
  <a href="#" class="alert-link">...</a>
</div>
```

## 进度条

用简单而灵活的进度条提供有关工作流或操作进度的最新反馈。

##### 跨浏览器兼容性

进度条使用CSS 3转换和动画来实现它们的一些效果。这些特性在InternetExplorer 9或更低版本或更旧版本的Firefox中不受支持。歌剧12不支持动画。

##### 内容安全策略（CSP）兼容性

如果您的网站具有[内容安全策略（CSP）](https://developer.mozilla.org/en-US/docs/Web/Security/CSP)不允许`style-src 'unsafe-inline'`，那么您将无法使用内联`style`属性来设置进度条宽度，如下面的示例所示。用于设置与严格CSP兼容的宽度的其他方法包括使用少量自定义JavaScript（设置`element.style.width`）或使用自定义CSS类。

### 基本实例

默认进度条。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#progress-basic)

```javascript
<div class="progress">
  <div class="progress-bar" role="progressbar" aria-valuenow="60" aria-valuemin="0" aria-valuemax="100" style="width: 60%;">
    <span class="sr-only">60% Complete</span>
  </div>
</div>
```

### 带标签

移除`<span>`带着`.sr-only`从进度栏中初始化以显示可见百分比。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#progress-label)

```javascript
<div class="progress">
  <div class="progress-bar" role="progressbar" aria-valuenow="60" aria-valuemin="0" aria-valuemax="100" style="width: 60%;">
    60%
  </div>
</div>
```

为了确保标签文本即使在低百分比的情况下仍可阅读，请考虑添加`min-width`到进度栏。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#progress-label)

```javascript
<div class="progress">
  <div class="progress-bar" role="progressbar" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100" style="min-width: 2em;">
    0%
  </div>
</div>
<div class="progress">
  <div class="progress-bar" role="progressbar" aria-valuenow="2" aria-valuemin="0" aria-valuemax="100" style="min-width: 2em; width: 2%;">
    2%
  </div>
</div>
```

### 上下文选择

进度条使用一些相同的按钮和警报类，以获得一致的样式。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#progress-alternatives)

```javascript
<div class="progress">
  <div class="progress-bar progress-bar-success" role="progressbar" aria-valuenow="40" aria-valuemin="0" aria-valuemax="100" style="width: 40%">
    <span class="sr-only">40% Complete (success)</span>
  </div>
</div>
<div class="progress">
  <div class="progress-bar progress-bar-info" role="progressbar" aria-valuenow="20" aria-valuemin="0" aria-valuemax="100" style="width: 20%">
    <span class="sr-only">20% Complete</span>
  </div>
</div>
<div class="progress">
  <div class="progress-bar progress-bar-warning" role="progressbar" aria-valuenow="60" aria-valuemin="0" aria-valuemax="100" style="width: 60%">
    <span class="sr-only">60% Complete (warning)</span>
  </div>
</div>
<div class="progress">
  <div class="progress-bar progress-bar-danger" role="progressbar" aria-valuenow="80" aria-valuemin="0" aria-valuemax="100" style="width: 80%">
    <span class="sr-only">80% Complete (danger)</span>
  </div>
</div>
```

### 条纹

使用渐变创建条纹效果。IE9及以下未提供。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#progress-striped)

```javascript
<div class="progress">
  <div class="progress-bar progress-bar-success progress-bar-striped" role="progressbar" aria-valuenow="40" aria-valuemin="0" aria-valuemax="100" style="width: 40%">
    <span class="sr-only">40% Complete (success)</span>
  </div>
</div>
<div class="progress">
  <div class="progress-bar progress-bar-info progress-bar-striped" role="progressbar" aria-valuenow="20" aria-valuemin="0" aria-valuemax="100" style="width: 20%">
    <span class="sr-only">20% Complete</span>
  </div>
</div>
<div class="progress">
  <div class="progress-bar progress-bar-warning progress-bar-striped" role="progressbar" aria-valuenow="60" aria-valuemin="0" aria-valuemax="100" style="width: 60%">
    <span class="sr-only">60% Complete (warning)</span>
  </div>
</div>
<div class="progress">
  <div class="progress-bar progress-bar-danger progress-bar-striped" role="progressbar" aria-valuenow="80" aria-valuemin="0" aria-valuemax="100" style="width: 80%">
    <span class="sr-only">80% Complete (danger)</span>
  </div>
</div>
```

### 动画片

加`.active`到`.progress-bar-striped`将条纹从右向左动画。IE9及以下未提供。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#progress-animated)

```javascript
<div class="progress">
  <div class="progress-bar progress-bar-striped active" role="progressbar" aria-valuenow="45" aria-valuemin="0" aria-valuemax="100" style="width: 45%">
    <span class="sr-only">45% Complete</span>
  </div>
</div>
```

### 堆叠

将多个条放入同一个`.progress`把它们堆起来。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#progress-stacked)

```javascript
<div class="progress">
  <div class="progress-bar progress-bar-success" style="width: 35%">
    <span class="sr-only">35% Complete (success)</span>
  </div>
  <div class="progress-bar progress-bar-warning progress-bar-striped" style="width: 20%">
    <span class="sr-only">20% Complete (warning)</span>
  </div>
  <div class="progress-bar progress-bar-danger" style="width: 10%">
    <span class="sr-only">10% Complete (danger)</span>
  </div>
</div>
```

## 媒体对象

用于构建各种类型组件（如博客评论，Tweets等）的抽象对象样式，其中包含文本内容旁边的左侧或右侧对齐图像。

### 默认媒体

默认媒体在内容块的左侧或右侧显示媒体对象（图像，视频，音频）。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#media-default)

```javascript
<div class="media">
  <div class="media-left">
    <a href="#">
      <img class="media-object" src="..." alt="...">
    </a>
  </div>
  <div class="media-body">
    <h4 class="media-heading">Media heading</h4>
    ...
  </div>
</div>
```

类`.pull-left`和`.pull-right`也存在与先前用作介质部件的一部分，但不建议使用用于该用途作为V3.3.0的。它们大约等于`.media-left`和`.media-right`，除了`.media-right`应该放在`.media-body`html之后。

### 媒体对齐

图像或其他媒体可以对齐顶部、中部或底部。默认值为顶对齐。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#media-alignment)

```javascript
<div class="media">
  <div class="media-left media-middle">
    <a href="#">
      <img class="media-object" src="..." alt="...">
    </a>
  </div>
  <div class="media-body">
    <h4 class="media-heading">Middle aligned media</h4>
    ...
  </div>
</div>
```

### 媒体名单

有了额外的标记，您可以使用列表中的媒体（对评论主题或文章列表有用）。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#media-list)

```javascript
<ul class="media-list">
  <li class="media">
    <div class="media-left">
      <a href="#">
        <img class="media-object" src="..." alt="...">
      </a>
    </div>
    <div class="media-body">
      <h4 class="media-heading">Media heading</h4>
      ...
    </div>
  </li>
</ul>
```

## 列表组

列表组是一个灵活而强大的组件，不仅可以显示简单的元素列表，而且可以显示具有自定义内容的复杂元素。

### 基本实例

最基本的列表组只是一个带有列表项的无序列表，以及适当的类。在此基础上添加以下选项，或根据需要使用您自己的CSS。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#list-group-basic)

```javascript
<ul class="list-group">
  <li class="list-group-item">Cras justo odio</li>
  <li class="list-group-item">Dapibus ac facilisis in</li>
  <li class="list-group-item">Morbi leo risus</li>
  <li class="list-group-item">Porta ac consectetur ac</li>
  <li class="list-group-item">Vestibulum at eros</li>
</ul>
```

### 徽章

将徽章组件添加到任何列表组项中，它将自动定位在右侧。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#list-group-badges)

```javascript
<ul class="list-group">
  <li class="list-group-item">
    <span class="badge">14</span>
    Cras justo odio
  </li>
</ul>
```

### 链接项目

通过使用锚标记而不是列表项来链接列表组项目（这也意味着父项`<div>`而不是一项`<ul>`）。每个元素周围都不需要个别父母。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#list-group-linked)

```javascript
<div class="list-group">
  <a href="#" class="list-group-item active">
    Cras justo odio
  </a>
  <a href="#" class="list-group-item">Dapibus ac facilisis in</a>
  <a href="#" class="list-group-item">Morbi leo risus</a>
  <a href="#" class="list-group-item">Porta ac consectetur ac</a>
  <a href="#" class="list-group-item">Vestibulum at eros</a>
</div>
```

### 按钮项目

列表组项目可以是按钮而不是列表项目（这也意味着一个父代`<div>`而不是一个`<ul>`）。每个元素周围都不需要个别父母。**不要** **在这里使用标准类**`**.btn**`

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#list-group-buttons)

```javascript
<div class="list-group">
  <button type="button" class="list-group-item">Cras justo odio</button>
  <button type="button" class="list-group-item">Dapibus ac facilisis in</button>
  <button type="button" class="list-group-item">Morbi leo risus</button>
  <button type="button" class="list-group-item">Porta ac consectetur ac</button>
  <button type="button" class="list-group-item">Vestibulum at eros</button>
</div>
```

### 禁用项目

加`.disabled`转到`.list-group-item`将其灰色化以显示为禁用。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#list-group-disabled)

```javascript
<div class="list-group">
  <a href="#" class="list-group-item disabled">
    Cras justo odio
  </a>
  <a href="#" class="list-group-item">Dapibus ac facilisis in</a>
  <a href="#" class="list-group-item">Morbi leo risus</a>
  <a href="#" class="list-group-item">Porta ac consectetur ac</a>
  <a href="#" class="list-group-item">Vestibulum at eros</a>
</div>
```

### 上下文类

使用上下文类样式列表项，默认或链接。还包括`.active`状态。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#list-group-contextual-classes)

```javascript
<ul class="list-group">
  <li class="list-group-item list-group-item-success">Dapibus ac facilisis in</li>
  <li class="list-group-item list-group-item-info">Cras sit amet nibh libero</li>
  <li class="list-group-item list-group-item-warning">Porta ac consectetur ac</li>
  <li class="list-group-item list-group-item-danger">Vestibulum at eros</li>
</ul>
<div class="list-group">
  <a href="#" class="list-group-item list-group-item-success">Dapibus ac facilisis in</a>
  <a href="#" class="list-group-item list-group-item-info">Cras sit amet nibh libero</a>
  <a href="#" class="list-group-item list-group-item-warning">Porta ac consectetur ac</a>
  <a href="#" class="list-group-item list-group-item-danger">Vestibulum at eros</a>
</div>
```

### 定制内容

在其中添加几乎任何HTML，即使是下面这样的链接列表组也是如此。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#list-group-custom-content)

```javascript
<div class="list-group">
  <a href="#" class="list-group-item active">
    <h4 class="list-group-item-heading">List group item heading</h4>
    <p class="list-group-item-text">...</p>
  </a>
</div>
```

## 面板

虽然并不总是必要的，但有时您需要将DOM放在一个盒子中。对于这些情况，请尝试面板组件。

### 基本实例

默认情况下，所有`.panel`DO是应用一些基本边框和填充来包含某些内容。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#panels-basic)

```javascript
<div class="panel panel-default">
  <div class="panel-body">
    Basic panel example
  </div>
</div>
```

### 带标题的面板

使用轻松添加标题容器到面板`.panel-heading`。你也可以包括任何`<h1>`- `<h6>`用`.panel-title`课程添加一个预先设计的标题。然而，字体大小`<h1>`- `<h6>`被覆盖`.panel-heading`。

为了正确链接着色，请务必将链接放置在标题中`.panel-title`。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#panels-heading)

```javascript
<div class="panel panel-default">
  <div class="panel-heading">Panel heading without title</div>
  <div class="panel-body">
    Panel content
  </div>
</div>

<div class="panel panel-default">
  <div class="panel-heading">
    <h3 class="panel-title">Panel title</h3>
  </div>
  <div class="panel-body">
    Panel content
  </div>
</div>
```

### 带页脚的面板

将按钮或辅助文本包装在`.panel-footer`注意面板页脚**不要**在使用上下文变体时继承颜色和边框，因为它们不应该在前台。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#panels-footer)

```javascript
<div class="panel panel-default">
  <div class="panel-body">
    Panel content
  </div>
  <div class="panel-footer">Panel footer</div>
</div>
```

### 上下文选择

与其他组件一样，通过添加任何上下文状态类，可以轻松地使面板对特定的上下文更有意义。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#panels-alternatives)

```javascript
<div class="panel panel-primary">...</div>
<div class="panel panel-success">...</div>
<div class="panel panel-info">...</div>
<div class="panel panel-warning">...</div>
<div class="panel panel-danger">...</div>
```

### 使用表格

添加任何无边界的`.table`在面板内进行无缝设计。如果有一个`.panel-body`，我们在表的顶部添加一个额外的边框以进行分离。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#panels-tables)

```javascript
<div class="panel panel-default">
  <!-- Default panel contents -->
  <div class="panel-heading">Panel heading</div>
  <div class="panel-body">
    <p>...</p>
  </div>

  <!-- Table -->
  <table class="table">
    ...
  </table>
</div>
```

如果没有面板主体，则组件从面板标题移动到表格而不会中断。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#panels-tables)

```javascript
<div class="panel panel-default">
  <!-- Default panel contents -->
  <div class="panel-heading">Panel heading</div>

  <!-- Table -->
  <table class="table">
    ...
  </table>
</div>
```

### 与列表组

容易包括全宽度列表组在任何面板内。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#panels-list-group)

```javascript
<div class="panel panel-default">
  <!-- Default panel contents -->
  <div class="panel-heading">Panel heading</div>
  <div class="panel-body">
    <p>...</p>
  </div>

  <!-- List group -->
  <ul class="list-group">
    <li class="list-group-item">Cras justo odio</li>
    <li class="list-group-item">Dapibus ac facilisis in</li>
    <li class="list-group-item">Morbi leo risus</li>
    <li class="list-group-item">Porta ac consectetur ac</li>
    <li class="list-group-item">Vestibulum at eros</li>
  </ul>
</div>
```

## 响应嵌入

允许浏览器根据其包含块的宽度来确定视频或幻灯片尺寸，方法是创建一个在任何设备上适当缩放的内在比率。

规则被直接施加到`<iframe>`，`<embed>`，`<video>`，和`<object>`元件; `.embed-responsive-item`如果要为其他属性的样式进行匹配，可以使用明确的后代类。

**专家提示！**你不需要包含`frameborder="0"`在你的`<iframe>`中，因为我们会为你覆盖它。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#responsive-embed)

```javascript
<!-- 16:9 aspect ratio -->
<div class="embed-responsive embed-responsive-16by9">
  <iframe class="embed-responsive-item" src="..."></iframe>
</div>

<!-- 4:3 aspect ratio -->
<div class="embed-responsive embed-responsive-4by3">
  <iframe class="embed-responsive-item" src="..."></iframe>
</div>
```

## Wells

默认 well

使用well作为对元素的简单效果以赋予它一个插入效果。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#wells)

```javascript
<div class="well">...</div>
```

### 可选类别

使用两个可选修饰符类控制填充和圆角。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#wells)

```javascript
<div class="well well-lg">...</div>
```

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/components/#wells)

```javascript
<div class="well well-sm">...</div>
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18