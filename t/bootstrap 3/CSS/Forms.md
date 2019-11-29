# 表单 | Forms

## Forms

### 基本示例

单独的表单控件自动接收一些全局样式。所有文本`<input>`，`<textarea>`和`<select>`元素默认`.form-control`设置为`width: 100%;`。将标签和控件包裹起来以`.form-group`获得最佳间距。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#forms-example)

```javascript
<form>
  <div class="form-group">
    <label for="exampleInputEmail1">Email address</label>
    <input type="email" class="form-control" id="exampleInputEmail1" placeholder="Email">
  </div>
  <div class="form-group">
    <label for="exampleInputPassword1">Password</label>
    <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
  </div>
  <div class="form-group">
    <label for="exampleInputFile">File input</label>
    <input type="file" id="exampleInputFile">
    <p class="help-block">Example block-level help text here.</p>
  </div>
  <div class="checkbox">
    <label>
      <input type="checkbox"> Check me out
    </label>
  </div>
  <button type="submit" class="btn btn-default">Submit</button>
</form>
```

##### 不要将表单组与输入组混合

不要将表单组与[输入组](https://getbootstrap.com/components/#input-groups)直接混合。而是将输入组嵌套在表单组中。

### 内联表格

添加`.form-inline`到您的表单（这不一定是 `<form>`）用于左对齐和内联块控件。**这仅适用于宽度至少为768像素的视口内的表单。**

##### 可能需要自定义宽度

输入和选择`width: 100%;`在Bootstrap中默认应用。在内联表单中，我们将其重置为`width: auto;`使多个控件可以位于同一行上。根据您的布局，可能需要额外的自定义宽度。

##### 总是添加标签

如果您没有为每个输入添加标签，屏幕阅读器将会对您的表单造成麻烦。对于这些内联表单，您可以使用`.sr-only`该类隐藏标签。存在用于辅助技术，如提供一个标签的进一步的替代方法`aria-label`，`aria-labelledby`或者`title`属性。如果这些都不存在，屏幕阅读器可能会诉诸使用该`placeholder`属性（如果存在），但请注意`placeholder`不建议使用替代其他标记方法。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-inline-form-labels)

```javascript
<form class="form-inline">
  <div class="form-group">
    <label for="exampleInputName2">Name</label>
    <input type="text" class="form-control" id="exampleInputName2" placeholder="Jane Doe">
  </div>
  <div class="form-group">
    <label for="exampleInputEmail2">Email</label>
    <input type="email" class="form-control" id="exampleInputEmail2" placeholder="jane.doe@example.com">
  </div>
  <button type="submit" class="btn btn-default">Send invitation</button>
</form>
```

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-inline-form-labels)

```javascript
<form class="form-inline">
  <div class="form-group">
    <label class="sr-only" for="exampleInputEmail3">Email address</label>
    <input type="email" class="form-control" id="exampleInputEmail3" placeholder="Email">
  </div>
  <div class="form-group">
    <label class="sr-only" for="exampleInputPassword3">Password</label>
    <input type="password" class="form-control" id="exampleInputPassword3" placeholder="Password">
  </div>
  <div class="checkbox">
    <label>
      <input type="checkbox"> Remember me
    </label>
  </div>
  <button type="submit" class="btn btn-default">Sign in</button>
</form>
```

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-inline-form-labels)

```javascript
<form class="form-inline">
  <div class="form-group">
    <label class="sr-only" for="exampleInputAmount">Amount (in dollars)</label>
    <div class="input-group">
      <div class="input-group-addon">$</div>
      <input type="text" class="form-control" id="exampleInputAmount" placeholder="Amount">
      <div class="input-group-addon">.00</div>
    </div>
  </div>
  <button type="submit" class="btn btn-primary">Transfer cash</button>
</form>
```

### 水平形式

使用Bootstrap的预定义网格类，通过添加`.form-horizontal`到表单（不一定是 `<form>`）来将标签和表单控件组以水平布局对齐。这样做会更改`.form-group`为表现为网格行，所以不需要`.row`。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#forms-horizontal)

```javascript
<form class="form-horizontal">
  <div class="form-group">
    <label for="inputEmail3" class="col-sm-2 control-label">Email</label>
    <div class="col-sm-10">
      <input type="email" class="form-control" id="inputEmail3" placeholder="Email">
    </div>
  </div>
  <div class="form-group">
    <label for="inputPassword3" class="col-sm-2 control-label">Password</label>
    <div class="col-sm-10">
      <input type="password" class="form-control" id="inputPassword3" placeholder="Password">
    </div>
  </div>
  <div class="form-group">
    <div class="col-sm-offset-2 col-sm-10">
      <div class="checkbox">
        <label>
          <input type="checkbox"> Remember me
        </label>
      </div>
    </div>
  </div>
  <div class="form-group">
    <div class="col-sm-offset-2 col-sm-10">
      <button type="submit" class="btn btn-default">Sign in</button>
    </div>
  </div>
</form>
```

### 支持的控件

示例表单布局中支持的标准表单控件的示例。

#### 输入

最常用的表单控件，基于文本的输入字段。包括所有类型的HTML5支持：`text`，`password`，`datetime`，`datetime-local`，`date`，`month`，`time`，`week`，`number`，`email`，`url`，`search`，`tel`，和`color`。

##### 需要类型声明

只有在`type`声明适当的情况下，输入才会完全符合样式。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-input-needs-type)

```javascript
<input type="text" class="form-control" placeholder="Text input">
```

##### 输入组

要在任何基于文本的文本之前和/或之后添加集成文本或按钮`<input>`，请查看输入组组件。

#### 多行文本

表单控件支持多行文本。`rows`根据需要更改属性。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-xref-input-group)

```javascript
<textarea class="form-control" rows="3"></textarea>
```

#### 复选框和单选框

复选框用于在列表中选择一个或多个选项，而无线电则用于从多个选项中选择一个选项。

不支持复选框和单选框，而是提供一个“不被允许”光标父的悬停`<label>`，你需要的添加`.disabled`类到父`.radio`，`.radio-inline`，`.checkbox`，或`.checkbox-inline`。

##### 默认（堆叠）

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-xref-input-group)

```javascript
<div class="checkbox">
  <label>
    <input type="checkbox" value="">
    Option one is this and that&mdash;be sure to include why it's great
  </label>
</div>
<div class="checkbox disabled">
  <label>
    <input type="checkbox" value="" disabled>
    Option two is disabled
  </label>
</div>

<div class="radio">
  <label>
    <input type="radio" name="optionsRadios" id="optionsRadios1" value="option1" checked>
    Option one is this and that&mdash;be sure to include why it's great
  </label>
</div>
<div class="radio">
  <label>
    <input type="radio" name="optionsRadios" id="optionsRadios2" value="option2">
    Option two can be something else and selecting it will deselect option one
  </label>
</div>
<div class="radio disabled">
  <label>
    <input type="radio" name="optionsRadios" id="optionsRadios3" value="option3" disabled>
    Option three is disabled
  </label>
</div>
```

##### Inline checkboxes and radios

使用一系列复选框或单选框上的`.checkbox-inline`或`.radio-inline`类来显示同一行上的控件。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-xref-input-group)

```javascript
<label class="checkbox-inline">
  <input type="checkbox" id="inlineCheckbox1" value="option1"> 1
</label>
<label class="checkbox-inline">
  <input type="checkbox" id="inlineCheckbox2" value="option2"> 2
</label>
<label class="checkbox-inline">
  <input type="checkbox" id="inlineCheckbox3" value="option3"> 3
</label>

<label class="radio-inline">
  <input type="radio" name="inlineRadioOptions" id="inlineRadio1" value="option1"> 1
</label>
<label class="radio-inline">
  <input type="radio" name="inlineRadioOptions" id="inlineRadio2" value="option2"> 2
</label>
<label class="radio-inline">
  <input type="radio" name="inlineRadioOptions" id="inlineRadio3" value="option3"> 3
</label>
```

##### 没有标签文字复选框和单选框

如果你没有文字`<label>`，输入的位置就像你期望的那样。**目前只适用于非内联复选框和收音机。**请记住仍然提供某种形式的辅助技术标签（例如，使用`aria-label`）。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-xref-input-group)

```javascript
<div class="checkbox">
  <label>
    <input type="checkbox" id="blankCheckbox" value="option1" aria-label="...">
  </label>
</div>
<div class="radio">
  <label>
    <input type="radio" name="blankRadio" id="blankRadio1" value="option1" aria-label="...">
  </label>
</div>
```

#### 选择

请注意，许多原生选择菜单（即Safari和Chrome）都具有不能通过`border-radius`属性修改的圆角。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-xref-input-group)

```javascript
<select class="form-control">
  <option>1</option>
  <option>2</option>
  <option>3</option>
  <option>4</option>
  <option>5</option>
</select>
```

对于`<select>`具有该`multiple`属性的控件，默认显示多个选项。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-xref-input-group)

```javascript
<select multiple class="form-control">
  <option>1</option>
  <option>2</option>
  <option>3</option>
  <option>4</option>
  <option>5</option>
</select>
```

### 静态控制

当您需要将纯文本放在表单标签旁边的表单中时，请使用`.form-control-static`类`<p>`。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#forms-controls-static)

```javascript
<form class="form-horizontal">
  <div class="form-group">
    <label class="col-sm-2 control-label">Email</label>
    <div class="col-sm-10">
      <p class="form-control-static">email@example.com</p>
    </div>
  </div>
  <div class="form-group">
    <label for="inputPassword" class="col-sm-2 control-label">Password</label>
    <div class="col-sm-10">
      <input type="password" class="form-control" id="inputPassword" placeholder="Password">
    </div>
  </div>
</form>
```

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#forms-controls-static)

```javascript
<form class="form-inline">
  <div class="form-group">
    <label class="sr-only">Email</label>
    <p class="form-control-static">email@example.com</p>
  </div>
  <div class="form-group">
    <label for="inputPassword2" class="sr-only">Password</label>
    <input type="password" class="form-control" id="inputPassword2" placeholder="Password">
  </div>
  <button type="submit" class="btn btn-default">Confirm identity</button>
</form>
```

### 焦点状态

我们删除`outline`某些表单控件的默认样式并`box-shadow`在其位置应用`:focus`。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#forms-control-focus)

##### Demo `:focus` state

上述示例输入在我们的文档中使用自定义样式来演示`:focus`状态`.form-control`。

### 禁用状态

`disabled`在输入上添加布尔属性以防止用户交互。禁用的输入显示得较浅并添加一个`not-allowed`光标。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#forms-control-disabled)

```javascript
<input class="form-control" id="disabledInput" type="text" placeholder="Disabled input here..." disabled>
```

#### 禁用的字段集

将该`disabled`属性添加到一个`<fieldset>`以立即禁用所有控件`<fieldset>`。

##### 注意关于链接功能 `<a>`

默认情况下，浏览器将把所有本地表单控件（`<input>`，`<select>`和`<button>`单元）内`<fieldset disabled>`为禁用，防止对他们的键盘和鼠标交互。但是，如果您的表单也包含`<a ... class="btn btn-*">`元素，则这些元素只会被赋予一种风格`pointer-events: none`。正如关于按钮的禁用状态（特别是锚点元素的子节）中所述的那样，此CSS属性尚未标准化，Opera 18及更低版本或Internet Explorer 11中并未完全支持此属性，不会阻止键盘用户能够关注或激活这些链接。为了安全起见，请使用自定义JavaScript来禁用此类链接。

##### 跨浏览器兼容性

虽然Bootstrap将在所有浏览器中应用这些样式，但Internet Explorer 11及其下的版本不完全支持a上的`disabled`属性`<fieldset>`。使用自定义JavaScript来禁用这些浏览器中的字段集。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-fieldset-disabled-ie)

```javascript
<form>
  <fieldset disabled>
    <div class="form-group">
      <label for="disabledTextInput">Disabled input</label>
      <input type="text" id="disabledTextInput" class="form-control" placeholder="Disabled input">
    </div>
    <div class="form-group">
      <label for="disabledSelect">Disabled select menu</label>
      <select id="disabledSelect" class="form-control">
        <option>Disabled select</option>
      </select>
    </div>
    <div class="checkbox">
      <label>
        <input type="checkbox"> Can't check this
      </label>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
  </fieldset>
</form>
```

### 只读状态

`readonly`在输入上添加布尔属性以防止修改输入值。只读输入显示为较亮（与禁用的输入类似），但保留标准光标。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#forms-control-readonly)

```javascript
<input class="form-control" type="text" placeholder="Readonly input here…" readonly>
```

### 帮助文字

窗体控件的块级帮助文本。

##### 将帮助文本与表单控件相关联

帮助文本应与使用该`aria-describedby`属性相关的表单控件显式关联。这将确保辅助技术（如屏幕阅读器）在用户关注或输入控件时宣布此帮助文本。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-help-text-accessibility)

```javascript
<label class="sr-only" for="inputHelpBlock">Input with help text</label>
<input type="text" id="inputHelpBlock" class="form-control" aria-describedby="helpBlock">
...
<span id="helpBlock" class="help-block">A block of help text that breaks onto a new line and may extend beyond one line.</span>
```

### 验证状态

Bootstrap包含表单控件上的错误，警告和成功状态的验证样式。要使用，添加`.has-warning`，`.has-error`或`.has-success`父元素。任何`.control-label`，`.form-control`并且`.help-block`在该元素内都会收到验证样式。

##### 将验证状态传递给辅助技术和色盲用户

使用这些验证样式来表示表单控件的状态仅提供基于颜色的可视化指示，不会将其传递给辅助技术的用户（如屏幕阅读器），也不会将其用于色盲用户。

确保还提供了状态的替代指示。例如，您可以在窗体控件的`<label>`文本本身中包含一个有关状态的提示（如以下代码示例中的情况），包括一个Glyphicon（使用`.sr-only`该类的相应替代文本- 请参阅Glyphicon示例），或者通过提供一个额外的帮助文本块。特别是对于辅助技术，无效表单控件也可以分配一个`aria-invalid="true"`属性。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-form-validation-state-accessibility)

```javascript
<div class="form-group has-success">
  <label class="control-label" for="inputSuccess1">Input with success</label>
  <input type="text" class="form-control" id="inputSuccess1" aria-describedby="helpBlock2">
  <span id="helpBlock2" class="help-block">A block of help text that breaks onto a new line and may extend beyond one line.</span>
</div>
<div class="form-group has-warning">
  <label class="control-label" for="inputWarning1">Input with warning</label>
  <input type="text" class="form-control" id="inputWarning1">
</div>
<div class="form-group has-error">
  <label class="control-label" for="inputError1">Input with error</label>
  <input type="text" class="form-control" id="inputError1">
</div>
<div class="has-success">
  <div class="checkbox">
    <label>
      <input type="checkbox" id="checkboxSuccess" value="option1">
      Checkbox with success
    </label>
  </div>
</div>
<div class="has-warning">
  <div class="checkbox">
    <label>
      <input type="checkbox" id="checkboxWarning" value="option1">
      Checkbox with warning
    </label>
  </div>
</div>
<div class="has-error">
  <div class="checkbox">
    <label>
      <input type="checkbox" id="checkboxError" value="option1">
      Checkbox with error
    </label>
  </div>
</div>
```

#### 带有可选图标

您还可以添加可选的反馈图标，并添加`.has-feedback`右图标。

**反馈图标仅适用于文本** **元素。<input class="form-control">**

##### 图标，标签和输入组

不带标签的输入需要手动定位反馈图标，而右侧需要附加的输入组。强烈建议您为可访问性原因提供所有输入的标签。如果您希望防止显示标签，请将其隐藏在`.sr-only`课程中。如果您必须没有标签，请调整`top`反馈图标的值。对于输入组，`right`根据插件的宽度将值调整为适当的像素值。

##### 将图标的含义传递给辅助技术

为了确保辅助技术（如屏幕阅读器）正确传达图标的含义，应该在`.sr-only`类中包含额外的隐藏文本，并明确将其与相关的表单控件相关联`aria-describedby`。或者，确保其意义（例如，对特定文本输入字段存在警告的事实）以其他某种形式传送，如更改`<label>`与窗体控件关联的实际文本。

尽管以下示例已经在`<label>`文本本身中提及了它们各自表单控件的验证状态，但上述技术（使用`.sr-only`文本和`aria-describedby`）已经被包含用于说明目的。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-has-feedback-icon-accessibility)

```javascript
<div class="form-group has-success has-feedback">
  <label class="control-label" for="inputSuccess2">Input with success</label>
  <input type="text" class="form-control" id="inputSuccess2" aria-describedby="inputSuccess2Status">
  <span class="glyphicon glyphicon-ok form-control-feedback" aria-hidden="true"></span>
  <span id="inputSuccess2Status" class="sr-only">(success)</span>
</div>
<div class="form-group has-warning has-feedback">
  <label class="control-label" for="inputWarning2">Input with warning</label>
  <input type="text" class="form-control" id="inputWarning2" aria-describedby="inputWarning2Status">
  <span class="glyphicon glyphicon-warning-sign form-control-feedback" aria-hidden="true"></span>
  <span id="inputWarning2Status" class="sr-only">(warning)</span>
</div>
<div class="form-group has-error has-feedback">
  <label class="control-label" for="inputError2">Input with error</label>
  <input type="text" class="form-control" id="inputError2" aria-describedby="inputError2Status">
  <span class="glyphicon glyphicon-remove form-control-feedback" aria-hidden="true"></span>
  <span id="inputError2Status" class="sr-only">(error)</span>
</div>
<div class="form-group has-success has-feedback">
  <label class="control-label" for="inputGroupSuccess1">Input group with success</label>
  <div class="input-group">
    <span class="input-group-addon">@</span>
    <input type="text" class="form-control" id="inputGroupSuccess1" aria-describedby="inputGroupSuccess1Status">
  </div>
  <span class="glyphicon glyphicon-ok form-control-feedback" aria-hidden="true"></span>
  <span id="inputGroupSuccess1Status" class="sr-only">(success)</span>
</div>
```

##### 水平和内联表单中的可选图标

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-has-feedback-icon-accessibility)

```javascript
<form class="form-horizontal">
  <div class="form-group has-success has-feedback">
    <label class="control-label col-sm-3" for="inputSuccess3">Input with success</label>
    <div class="col-sm-9">
      <input type="text" class="form-control" id="inputSuccess3" aria-describedby="inputSuccess3Status">
      <span class="glyphicon glyphicon-ok form-control-feedback" aria-hidden="true"></span>
      <span id="inputSuccess3Status" class="sr-only">(success)</span>
    </div>
  </div>
  <div class="form-group has-success has-feedback">
    <label class="control-label col-sm-3" for="inputGroupSuccess2">Input group with success</label>
    <div class="col-sm-9">
      <div class="input-group">
        <span class="input-group-addon">@</span>
        <input type="text" class="form-control" id="inputGroupSuccess2" aria-describedby="inputGroupSuccess2Status">
      </div>
      <span class="glyphicon glyphicon-ok form-control-feedback" aria-hidden="true"></span>
      <span id="inputGroupSuccess2Status" class="sr-only">(success)</span>
    </div>
  </div>
</form>
```

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-has-feedback-icon-accessibility)

```javascript
<form class="form-inline">
  <div class="form-group has-success has-feedback">
    <label class="control-label" for="inputSuccess4">Input with success</label>
    <input type="text" class="form-control" id="inputSuccess4" aria-describedby="inputSuccess4Status">
    <span class="glyphicon glyphicon-ok form-control-feedback" aria-hidden="true"></span>
    <span id="inputSuccess4Status" class="sr-only">(success)</span>
  </div>
</form>
<form class="form-inline">
  <div class="form-group has-success has-feedback">
    <label class="control-label" for="inputGroupSuccess3">Input group with success</label>
    <div class="input-group">
      <span class="input-group-addon">@</span>
      <input type="text" class="form-control" id="inputGroupSuccess3" aria-describedby="inputGroupSuccess3Status">
    </div>
    <span class="glyphicon glyphicon-ok form-control-feedback" aria-hidden="true"></span>
    <span id="inputGroupSuccess3Status" class="sr-only">(success)</span>
  </div>
</form>
```

##### 带有隐藏`.sr-only`标签的可选图标

如果您使用`.sr-only`该类来隐藏表单控件`<label>`（而不是使用其他标签选项，如`aria-label`属性），Bootstrap会在添加图标后自动调整图标的位置。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#callout-has-feedback-icon-accessibility)

```javascript
<div class="form-group has-success has-feedback">
  <label class="control-label sr-only" for="inputSuccess5">Hidden label</label>
  <input type="text" class="form-control" id="inputSuccess5" aria-describedby="inputSuccess5Status">
  <span class="glyphicon glyphicon-ok form-control-feedback" aria-hidden="true"></span>
  <span id="inputSuccess5Status" class="sr-only">(success)</span>
</div>
<div class="form-group has-success has-feedback">
  <label class="control-label sr-only" for="inputGroupSuccess4">Input group with success</label>
  <div class="input-group">
    <span class="input-group-addon">@</span>
    <input type="text" class="form-control" id="inputGroupSuccess4" aria-describedby="inputGroupSuccess4Status">
  </div>
  <span class="glyphicon glyphicon-ok form-control-feedback" aria-hidden="true"></span>
  <span id="inputGroupSuccess4Status" class="sr-only">(success)</span>
</div>
```

### 控制大小

使用像类这样的类来设置高度`.input-lg`，并使用类似的网格列类来设置宽度`.col-lg-*`。

#### 身高尺寸

创建匹配按钮大小的较高或较短的表单控件。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#forms-control-sizes)

```javascript
<input class="form-control input-lg" type="text" placeholder=".input-lg">
<input class="form-control" type="text" placeholder="Default input">
<input class="form-control input-sm" type="text" placeholder=".input-sm">

<select class="form-control input-lg">...</select>
<select class="form-control">...</select>
<select class="form-control input-sm">...</select>
```

#### 水平表单组大小

`.form-horizontal`通过添加`.form-group-lg`或快速缩放标签和表单控件`.form-group-sm`。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#forms-control-sizes)

```javascript
<form class="form-horizontal">
  <div class="form-group form-group-lg">
    <label class="col-sm-2 control-label" for="formGroupInputLarge">Large label</label>
    <div class="col-sm-10">
      <input class="form-control" type="text" id="formGroupInputLarge" placeholder="Large input">
    </div>
  </div>
  <div class="form-group form-group-sm">
    <label class="col-sm-2 control-label" for="formGroupInputSmall">Small label</label>
    <div class="col-sm-10">
      <input class="form-control" type="text" id="formGroupInputSmall" placeholder="Small input">
    </div>
  </div>
</form>
```

#### 列大小

将网格列或任何自定义父元素中的输入包裹起来，以便轻松执行所需的宽度。

[Open example on getbootstrap.com](https://getbootstrap.com/docs/3.3/css/#forms-control-sizes)

```javascript
<div class="row">
  <div class="col-xs-2">
    <input type="text" class="form-control" placeholder=".col-xs-2">
  </div>
  <div class="col-xs-3">
    <input type="text" class="form-control" placeholder=".col-xs-3">
  </div>
  <div class="col-xs-4">
    <input type="text" class="form-control" placeholder=".col-xs-4">
  </div>
</div>
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-09-28