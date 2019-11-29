# JavaScript

- 贡献者2人

  

通过十几种自定义jQuery插件让Bootstrap的组件成为现实。轻松地将它们全部或逐一包含在内。

## 概述

### 独立使用或编译

插件可以单独包含（使用Bootstrap的单个`*.js`文件），或者一次全部使用（使用`bootstrap.js`或缩小`bootstrap.min.js`）。

##### 使用已编译的JavaScript

`bootstrap.js`和`bootstrap.min.js`所有插件包含在 一个 文件中

##### 插件依赖关系

一些插件和CSS组件依赖于其他插件。如果您单独包含插件，请确保在文档中检查这些依赖关系。另外请注意，所有插件都依赖于jQuery（这意味着jQuery必须包含**在**插件文件**之前**）。

### 数据属性

您可以纯粹通过标记API使用所有Bootstrap插件，无需编写单行JavaScript代码。这是Bootstrap的一流API，应该是您使用插件时的首要考虑事项。

也就是说，在某些情况下，关闭此功能可能是可取的。因此，我们还提供了通过取消绑定命名空间的文档上的所有事件来禁用数据属性api的功能。`data-api`.这个看起来是这样的：

```javascript
$(document).off('.data-api')
```

另外，要针对特定的插件，只需将plugin%27的名称与Data-API命名空间一起包括在下面这样的名称空间中：

```javascript
$(document).off('.alert.data-api')
```

##### 通过数据属性，每个元素只有一个插件

不要在同一元素上使用来自多个插件的数据属性。例如，一个按钮不能同时具有工具提示和切换模式。要做到这一点，请使用包装元素。

### 编程API

我们还认为，您应该能够完全通过JavaScriptAPI使用所有的Bootstrap插件。所有公共API都是单一的、可链接的方法，并返回所操作的集合。

```javascript
$('.btn.danger').button('toggle').addClass('fat')
```

所有的方法都应该接受一个可选的选项对象，一个针对特定方法的字符串，或者什么都不接受（它用默认行为启动一个插件）：

```javascript
$('#myModal').modal()                      // initialized with defaults
$('#myModal').modal({ keyboard: false })   // initialized with no keyboard
$('#myModal').modal('show')                // initializes and invokes show immediately
```

每个插件还在一个`Constructor`属性上公开它的原始构造函数：`$.fn.popover.Constructor`。如果你想获得一个特定的插件实例，直接从一个元素中获取：`$('[rel="popover"]').data('popover')`。

##### 默认设置

您可以通过修改插件的`Constructor.DEFAULTS`对象来更改插件的默认设置：

```javascript
$.fn.modal.Constructor.DEFAULTS.keyboard = false // changes default for the modal plugin's `keyboard` option to false
```

### 无冲突

有时，需要在其他UI框架中使用引导插件。在这种情况下，名称空间冲突可能会偶尔发生。如果发生这种情况，你可以打电话给`.noConflict`的值。

```javascript
var bootstrapButton = $.fn.button.noConflict() // return $.fn.button to previously assigned value
$.fn.bootstrapBtn = bootstrapButton            // give $().bootstrapBtn the Bootstrap functionality
```

### 事件

Bootstrap为大多数插件的独特操作提供自定义事件。一般来说，这些都是不定式和过去分词形式 - 不定式（ex。`show`）在事件开始时触发，其过去分词形式（ex。`shown`）在动作完成时触发。

从3.0.0开始，所有的引导事件都是命名空间的。

所有不定式事件`preventDefault`功能。这提供了在动作开始之前停止执行的能力。

```javascript
$('#myModal').on('show.bs.modal', function (e) {
  if (!data) return e.preventDefault() // stops modal from being shown
})
```

### 版本号

每个Bootstrap的jQuery插件的版本都可以通过`VERSION`插件构造函数的属性来访问。例如，对于工具提示插件：

```javascript
$.fn.tooltip.Constructor.VERSION // => "3.3.7"
```

### 禁用JavaScript时没有连锁反应

当JavaScript禁用时，Bootstrap的插件不会特别优雅地回退。如果您在这种情况下关心用户体验，请向用户[``](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/noscript)解释情况（以及如何重新启用JavaScript），和/或添加您自己的自定义回退。

##### 第三方图书馆

**引导程序不正式支持第三方JavaScript库。**比如Prototype或jQueryUI。尽管`.noConflict`以及命名空间事件，可能存在需要自己解决的兼容性问题。

## 过渡效果 transition.js

### 关于 transition.js

对于简单的过渡效果，`transition.js`一次只包含其他JS文件。如果您使用编译过的（或缩小版）`bootstrap.js`，则不需要包含它 - 它已经存在。

### 里面是什么？

js 是` transitionEnd` 事件的基本助手, 也是 CSS 转换仿真器。其他插件使用它来检查 CSS 过渡支持和捕捉挂起的过渡。

### 禁用过渡

可以使用以下JavaScript代码段全局禁用转换，该代码段必须在加载之后`transition.js`（或`bootstrap.js`或者`bootstrap.min.js`，视情况而定）加载：

```javascript
$.support.transition = false
```

## modals modal.js

MODALS是流线型的，但很灵活，对话框提示具有最低要求的功能和智能的默认设置。

##### 不支持多个开放模式

确保不要打开一个模式，而另一个仍然是可见的。一次显示多个模式需要自定义代码。

##### 模态标记放置

始终尝试将模式的 HTML 代码放在文档的顶级位置, 以避免影响模式外观和/或功能的其他组件。

##### 移动设备警告

关于在移动设备上使用MODEL有一些警告。请参阅我们的浏览器支持文档。关于细节。

**由于HTML 5如何定义其语义，autofocusHTML属性在引导模式中没有任何影响。**要达到同样的效果，请使用一些自定义JavaScript：

```javascript
$('#myModal').on('shown.bs.modal', function () {
  $('#myInput').focus()
})
```

### 实例

#### 静态示例

页脚中具有页眉、正文和动作集的呈现模式。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#modals-examples)

```javascript
<div class="modal fade" tabindex="-1" role="dialog">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title">Modal title</h4>
      </div>
      <div class="modal-body">
        <p>One fine body&hellip;</p>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div><!-- /.modal-content -->
  </div><!-- /.modal-dialog -->
</div><!-- /.modal -->
```

#### 现场演示

通过单击下面的按钮，通过JavaScript切换一个模式。它将从页面顶部向下滑动并逐渐消失。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#modals-examples)

```javascript
<!-- Button trigger modal -->
<button type="button" class="btn btn-primary btn-lg" data-toggle="modal" data-target="#myModal">
  Launch demo modal
</button>

<!-- Modal -->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title" id="myModalLabel">Modal title</h4>
      </div>
      <div class="modal-body">
        ...
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
```

##### 使调制解调器可访问

一定要加`role="dialog"`和`aria-labelledby="..."`，引用模式称号，`.modal`以及`role="document"`对`.modal-dialog`本身。

另外，你可以用`aria-describedby`来描述你的模态对话框`.modal`。

##### 嵌入YouTube视频

在MODALS中嵌入YouTube视频需要额外的JavaScript，而不是引导，以自动停止播放等等。[请参阅此有用的堆栈溢出帖子。](https://stackoverflow.com/questions/18622508/bootstrap-3-and-youtube-in-modal)想了解更多信息。

### 可选尺寸

Modals有两个可选的尺寸，可通过修饰符类放置在a上`.modal-dialog`。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#modals-sizes)

```javascript
<!-- Large modal -->
<button type="button" class="btn btn-primary" data-toggle="modal" data-target=".bs-example-modal-lg">Large modal</button>

<div class="modal fade bs-example-modal-lg" tabindex="-1" role="dialog" aria-labelledby="myLargeModalLabel">
  <div class="modal-dialog modal-lg" role="document">
    <div class="modal-content">
      ...
    </div>
  </div>
</div>

<!-- Small modal -->
<button type="button" class="btn btn-primary" data-toggle="modal" data-target=".bs-example-modal-sm">Small modal</button>

<div class="modal fade bs-example-modal-sm" tabindex="-1" role="dialog" aria-labelledby="mySmallModalLabel">
  <div class="modal-dialog modal-sm" role="document">
    <div class="modal-content">
      ...
    </div>
  </div>
</div>
```

### 移除动画

对于只出现而不是淡入查看的MODALL，请删除`.fade`从您的模式标记中初始化。

```javascript
<div class="modal" tabindex="-1" role="dialog" aria-labelledby="...">
  ...
</div>
```

### 使用网格系统

要利用模态内的引导网格系统，只需嵌套`.row`在`.modal-body`然后使用普通网格系统类。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#modals-grid-system)

```javascript
<div class="modal fade" tabindex="-1" role="dialog" aria-labelledby="gridSystemModalLabel">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title" id="gridSystemModalLabel">Modal title</h4>
      </div>
      <div class="modal-body">
        <div class="row">
          <div class="col-md-4">.col-md-4</div>
          <div class="col-md-4 col-md-offset-4">.col-md-4 .col-md-offset-4</div>
        </div>
        <div class="row">
          <div class="col-md-3 col-md-offset-3">.col-md-3 .col-md-offset-3</div>
          <div class="col-md-2 col-md-offset-4">.col-md-2 .col-md-offset-4</div>
        </div>
        <div class="row">
          <div class="col-md-6 col-md-offset-3">.col-md-6 .col-md-offset-3</div>
        </div>
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
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div><!-- /.modal-content -->
  </div><!-- /.modal-dialog -->
</div><!-- /.modal -->
```

### 基于触发按钮改变模态内容

有一堆按钮都触发相同的模式，只是内容稍有不同？使用`event.relatedTarget`和[HTML `data-*`属性](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_data_attributes)（可能[通过jQuery](http://api.jquery.com/data/)）根据点击哪个按钮来改变模式的内容。有关详细信息`relatedTarget`，请参阅Modal Events文档，

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#modals-related-target)

```javascript
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal" data-whatever="@mdo">Open modal for @mdo</button>
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal" data-whatever="@fat">Open modal for @fat</button>
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal" data-whatever="@getbootstrap">Open modal for @getbootstrap</button>
...more buttons...

<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title" id="exampleModalLabel">New message</h4>
      </div>
      <div class="modal-body">
        <form>
          <div class="form-group">
            <label for="recipient-name" class="control-label">Recipient:</label>
            <input type="text" class="form-control" id="recipient-name">
          </div>
          <div class="form-group">
            <label for="message-text" class="control-label">Message:</label>
            <textarea class="form-control" id="message-text"></textarea>
          </div>
        </form>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Send message</button>
      </div>
    </div>
  </div>
</div>
$('#exampleModal').on('show.bs.modal', function (event) {
  var button = $(event.relatedTarget) // Button that triggered the modal
  var recipient = button.data('whatever') // Extract info from data-* attributes
  // If necessary, you could initiate an AJAX request here (and then do the updating in a callback).
  // Update the modal's content. We'll use jQuery here, but you could use a data binding library or other methods instead.
  var modal = $(this)
  modal.find('.modal-title').text('New message to ' + recipient)
  modal.find('.modal-body input').val(recipient)
})
```

### 使用

模式插件通过数据属性或JavaScript来按需切换隐藏内容。它还增加`.modal-open`了`<body>`覆盖默认的滚动行为，并生成一个`.modal-backdrop`提供点击区域来解除在模态外单击时显示的模态。

#### 通过数据属性

无需编写JavaScript即可激活模式。`data-toggle="modal"`在一个控制器元素上设置，就像一个按钮一起，`data-target="#foo"`或者`href="#foo"`将一个特定的模式作为切换目标。

```javascript
<button type="button" data-toggle="modal" data-target="#myModal">Launch modal</button>
```

#### 通过JavaScript

`myModal`用一行JavaScript 调用带id的模式：

```javascript
$('#myModal').modal(options)
```

#### 备选方案

选项可以通过数据属性或JavaScript传递。对于数据属性，将选项名追加到`data-`，如`data-backdrop=""`...

| 名字     | 类型                           | 默认  | 详细信息                                                     |
| :------- | :----------------------------- | :---- | :----------------------------------------------------------- |
| backdrop | boolean or the string 'static' | true  | Includes a modal-backdrop element. Alternatively, specify static for a backdrop which doesn't close the modal on click. |
| keyboard | boolean                        | true  | Closes the modal when escape key is pressed                  |
| show     | boolean                        | true  | Shows the modal when initialized.                            |
| remote   | path                           | false | This option is deprecated since v3.3.0 and has been removed in v4. We recommend instead using client-side templating or a data binding framework, or calling jQuery.load yourself. If a remote URL is provided, content will be loaded one time via jQuery's load method and injected into the .modal-content div. If you're using the data-api, you may alternatively use the href attribute to specify the remote source. An example of this is shown below: <a data-toggle="modal" href="remote.html" data-target="#modal">Click me</a> |

#### 方法

##### `.modal(options)`

激活您的内容作为模式。接受可选的选项`object`。

```javascript
$('#myModal').modal({
  keyboard: false
})
```

##### `.modal('toggle')`

手动切换模式。**返回到之前的模态主叫方实际上已显示或隐藏**（IE浏览器之前`shown.bs.modal`或`hidden.bs.modal`事件发生时）。

```javascript
$('#myModal').modal('toggle')
```

##### `.modal('show')`

手动打开模式。**在实际显示模态**之前（即`shown.bs.modal`事件发生之前）**返回给调用者**。

```javascript
$('#myModal').modal('show')
```

##### `.modal('hide')`

手动隐藏模式。**在模态被隐藏**之前（即`hidden.bs.modal`事件发生之前）**返回给调用者**。

```javascript
$('#myModal').modal('hide')
```

##### `.modal('handleUpdate')`

重新调整模式的位置以对抗滚动条，以防出现滚动条，这会使模式跳转到左侧。

只有当模态的高度变化时，它才需要打开。

```javascript
$('#myModal').modal('handleUpdate')
```

#### 事件

Bootstrap公开了一些挂钩到模态功能的事件。

所有的模态事件都是在模态本身（即在`<div class="modal">`）处被触发的。

| 事件类型        | 细节描述                                                     |
| :-------------- | :----------------------------------------------------------- |
| show.bs.modal   | This event fires immediately when the show instance method is called. If caused by a click, the clicked element is available as the relatedTarget property of the event. |
| shown.bs.modal  | This event is fired when the modal has been made visible to the user (will wait for CSS transitions to complete). If caused by a click, the clicked element is available as the relatedTarget property of the event. |
| hide.bs.modal   | This event is fired immediately when the hide instance method has been called. |
| hidden.bs.modal | This event is fired when the modal has finished being hidden from the user (will wait for CSS transitions to complete). |
| loaded.bs.modal | This event is fired when the modal has loaded content using the remote option. |

```javascript
$('#myModal').on('hidden.bs.modal', function (e) {
  // do something...
})
```

## Dropdowns dropdown.js

### 实例

使用这个简单的插件将下拉菜单添加到几乎任何东西，包括导航栏，选项卡和pill。

#### 在导航栏内

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#dropdowns-examples)

#### 丸内

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#dropdowns-examples)

### 使用

通过数据属性或JavaScript，下拉插件通过切换`.open`父列表项上的类来切换隐藏内容（下拉菜单）。

在移动设备上，打开下拉列表将添加`.dropdown-backdrop`当点击菜单外部时，作为关闭下拉菜单的一个点击区，需要适当的IOS支持。**这意味着从打开的下拉菜单切换到不同的下拉菜单需要额外的移动点击。**

注：`data-toggle="dropdown"`属性用于在应用程序级别关闭下拉菜单，因此始终使用它是一个好主意。

#### 通过数据属性

加`data-toggle="dropdown"`切换到链接或按钮以切换下拉菜单。

```javascript
<div class="dropdown">
  <button id="dLabel" type="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    Dropdown trigger
    <span class="caret"></span>
  </button>
  <ul class="dropdown-menu" aria-labelledby="dLabel">
    ...
  </ul>
</div>
```

要使链接按钮保持不变，请使用`data-target`属性来代替`href="#"`。

```javascript
<div class="dropdown">
  <a id="dLabel" data-target="#" href="http://example.com" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">
    Dropdown trigger
    <span class="caret"></span>
  </a>

  <ul class="dropdown-menu" aria-labelledby="dLabel">
    ...
  </ul>
</div>
```

#### 通过JavaScript

通过JavaScript调用下拉列表：

```javascript
$('.dropdown-toggle').dropdown()
```

仍需`data-toggle="dropdown"`

无论您是通过JavaScript调用下拉列表还是使用data-api，`data-toggle="dropdown"`总是需要在下拉的触发器元素中出现。

#### 备选方案

*无*

#### 方法

##### `$().dropdown('toggle')`

切换给定导航条或选项卡导航的下拉菜单。

#### 事件

所有的下拉事件都是在`.dropdown-menu`父元素上触发的。

所有下拉事件都有一个`relatedTarget`属性，其值是切换锚点元素。

| 事件               | 详细说明                                                     |
| :----------------- | :----------------------------------------------------------- |
| show.bs.dropdown   | This event fires immediately when the show instance method is called. |
| shown.bs.dropdown  | This event is fired when the dropdown has been made visible to the user (will wait for CSS transitions, to complete). |
| hide.bs.dropdown   | This event is fired immediately when the hide instance method has been called. |
| hidden.bs.dropdown | This event is fired when the dropdown has finished being hidden from the user (will wait for CSS transitions, to complete). |

```javascript
$('#myDropdown').on('show.bs.dropdown', function () {
  // do something…
})
```

## ScrollSpy scrollspy.js

### 导航栏中的示例

ScrollSpy插件用于基于滚动位置自动更新导航目标。滚动导航栏下面的区域，并观察活动类的变化。下拉子项也将被突出显示。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#scrollspy-examples)

### 使用

##### 需要引导导航

当前需要使用自举导航元件用于正确突出活动链接。

##### 所需可解析ID目标

Navbar链接必须有可分辨的id目标。例如，`<a href="#home">home</a>`必须对应于DOM中的某些内容，如`<div id="home"></div>`...

##### 非-`:visible`忽略目标元素

不是的目标元素[`:visible`根据jQuery](http://api.jquery.com/visible-selector/)将被忽略，其相应的导航项将永远不会突出显示。

#### 需要相对定位

无论实现方法如何，滚动间谍都需要使用`position: relative;`在你监视的元素上。在大多数情况下，这是`<body>`.当滚动监视除`<body>`，一定要有一个`height`设置和`overflow-y: scroll;`申请。

#### 通过数据属性

要轻松地将滚动行为添加到您的顶栏导航中，请添加`data-spy="scroll"`到您想要监视的元素（最典型的情况是这将是`<body>`）。然后将该`data-target`属性添加到任何Bootstrap `.nav`组件的父元素的ID或类中。

```javascript
body {
  position: relative;
}
<body data-spy="scroll" data-target="#navbar-example">
  ...
  <div id="navbar-example">
    <ul class="nav nav-tabs" role="tablist">
      ...
    </ul>
  </div>
  ...
</body>
```

#### 通过JavaScript

添加`position: relative;`后，通过JavaScript调用scrollspy：

```javascript
$('body').scrollspy({ target: '#navbar-example' })
```

#### 方法

##### `.scrollspy('refresh')`

当使用scrollspy与添加或从DOM中删除元素相结合时，您需要调用refresh方法，如下所示：

```javascript
$('[data-spy="scroll"]').each(function () {
  var $spy = $(this).scrollspy('refresh')
})
```

#### 备选方案

选项可以通过数据属性或JavaScript传递。对于数据属性，请将选项名称附加到中`data-`，如`data-offset=""`。

| Name   | type   | default | description                                                  |
| :----- | :----- | :------ | :----------------------------------------------------------- |
| offset | number | 10      | Pixels to offset from top when calculating position of scroll. |

#### 事件

| Event Type            | Description                                                  |
| :-------------------- | :----------------------------------------------------------- |
| activate.bs.scrollspy | This event fires whenever a new item becomes activated by the scrollspy. |

```javascript
$('#myScrollspy').on('activate.bs.scrollspy', function () {
  // do something…
})
```

## 可切换选项卡tab.js

### 示例选项卡

添加快速的，动态的标签功能，以过渡通过窗格的本地内容，甚至通过下拉菜单。**不支持嵌套选项卡。**

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#tabs-examples)

##### 扩展选项卡导航

这个插件扩展了选项卡导航组件若要添加可表区域，请执行以下操作。

### 使用

通过JavaScript启用可放置标签（每个标签需要单独激活）：

```javascript
$('#myTabs a').click(function (e) {
  e.preventDefault()
  $(this).tab('show')
})
```

您可以通过以下几种方式激活各个选项卡：

```javascript
$('#myTabs a[href="#profile"]').tab('show') // Select tab by name
$('#myTabs a:first').tab('show') // Select first tab
$('#myTabs a:last').tab('show') // Select last tab
$('#myTabs li:eq(2) a').tab('show') // Select third tab (0-indexed)
```

#### 标记

您可以通过简单地指定`data-toggle="tab"`或`data-toggle="pill"`在元素上书写任何JavaScript来激活制表符或导航。将选项`nav`和`nav-tabs`类添加到选项卡`ul`将应用Bootstrap选项卡样式，而添加`nav`和`nav-pills`类将应用pill样式。

```javascript
<div>

  <!-- Nav tabs -->
  <ul class="nav nav-tabs" role="tablist">
    <li role="presentation" class="active"><a href="#home" aria-controls="home" role="tab" data-toggle="tab">Home</a></li>
    <li role="presentation"><a href="#profile" aria-controls="profile" role="tab" data-toggle="tab">Profile</a></li>
    <li role="presentation"><a href="#messages" aria-controls="messages" role="tab" data-toggle="tab">Messages</a></li>
    <li role="presentation"><a href="#settings" aria-controls="settings" role="tab" data-toggle="tab">Settings</a></li>
  </ul>

  <!-- Tab panes -->
  <div class="tab-content">
    <div role="tabpanel" class="tab-pane active" id="home">...</div>
    <div role="tabpanel" class="tab-pane" id="profile">...</div>
    <div role="tabpanel" class="tab-pane" id="messages">...</div>
    <div role="tabpanel" class="tab-pane" id="settings">...</div>
  </div>

</div>
```

#### 褪色效应

若要使选项卡淡入，请添加`.fade`致每一个`.tab-pane`。第一个选项卡窗格还必须具有`.in`若要使初始内容可见，请执行以下操作。

```javascript
<div class="tab-content">
  <div role="tabpanel" class="tab-pane fade in active" id="home">...</div>
  <div role="tabpanel" class="tab-pane fade" id="profile">...</div>
  <div role="tabpanel" class="tab-pane fade" id="messages">...</div>
  <div role="tabpanel" class="tab-pane fade" id="settings">...</div>
</div>
```

#### 方法

##### `$().tab`

激活选项卡元素和内容容器。选项卡应该有一个`data-target`或者`href`以DOM中的容器节点为目标。在上面的示例中，选项卡是`<a>`S与...`data-toggle="tab"`属性。

##### `.tab('show')`

选择给定的选项卡并显示其关联的内容。之前选择的任何其他选项卡将变为未选中状态，并且其关联的内容将被隐藏。**在选项卡窗格实际显示**之前（即`shown.bs.tab`事件发生之前）**返回给调用者**。

```javascript
$('#someTab').tab('show')
```

#### 事件

当显示一个新选项卡时，事件按以下顺序触发：

\1. `hide.bs.tab` （在当前活动选项卡上）

2.`show.bs.tab` （在待显示的选项卡上）

3.`hidden.bs.tab`（在上一个活动选项卡上，与`hide.bs.tab`事件相同）

4.`shown.bs.tab`（在新近活动的刚显示的标签上，与`show.bs.tab`事件相同）

如果没有选项卡已处于活动状态，则`hide.bs.tab`和`hidden.bs.tab`事件不会被触发。

| 事件类型      | 详细信息                                                     |
| :------------ | :----------------------------------------------------------- |
| show.bs.tab   | This event fires on tab show, but before the new tab has been shown. Use event.target and event.relatedTarget to target the active tab and the previous active tab (if available) respectively. |
| shown.bs.tab  | This event fires on tab show after a tab has been shown. Use event.target and event.relatedTarget to target the active tab and the previous active tab (if available) respectively. |
| hide.bs.tab   | This event fires when a new tab is to be shown (and thus the previous active tab is to be hidden). Use event.target and event.relatedTarget to target the current active tab and the new soon-to-be-active tab, respectively. |
| hidden.bs.tab | This event fires after a new tab is shown (and thus the previous active tab is hidden). Use event.target and event.relatedTarget to target the previous active tab and the new active tab, respectively. |

```javascript
$('a[data-toggle="tab"]').on('shown.bs.tab', function (e) {
  e.target // newly activated tab
  e.relatedTarget // previous active tab
})
```

## Tooltips tooltip.js

灵感来自jQuery.tipsy插件，它由JasonFrame编写；工具提示是一个更新版本，它不依赖于图像，使用CSS 3进行动画，使用数据属性存储本地标题。

没有显示零长度标题的工具提示。

### 实例

在下面的链接上悬停以查看工具提示：

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#tooltips-examples)

#### 静态tooltip

有四个选项可用：上、右、底和左对齐。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#tooltips-examples)

#### 四个方向

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#tooltips-examples)

```javascript
<button type="button" class="btn btn-default" data-toggle="tooltip" data-placement="left" title="Tooltip on left">Tooltip on left</button>

<button type="button" class="btn btn-default" data-toggle="tooltip" data-placement="top" title="Tooltip on top">Tooltip on top</button>

<button type="button" class="btn btn-default" data-toggle="tooltip" data-placement="bottom" title="Tooltip on bottom">Tooltip on bottom</button>

<button type="button" class="btn btn-default" data-toggle="tooltip" data-placement="right" title="Tooltip on right">Tooltip on right</button>
```

##### 选择功能

由于性能原因，工具提示和Popover数据-api是选择的，意思是**你必须自己初始化**...

初始化页面上所有工具提示的一种方法是通过`data-toggle`属性：

```javascript
$(function () {
  $('[data-toggle="tooltip"]').tooltip()
})
```

### 使用

工具提示插件按需生成内容和标记，默认情况下将工具提示放在触发器元素之后。

通过JavaScript触发工具提示：

```javascript
$('#example').tooltip(options)
```

#### 标记

工具提示所需的标记只是一个`data`属性，`title`在HTML元素上你希望有一个工具提示。生成的工具提示标记相当简单，但它确实需要一个位置（默认情况下，`top`由插件设置）。

```javascript
<!-- HTML to write -->
<a href="#" data-toggle="tooltip" title="Some tooltip text!">Hover over me</a>

<!-- Generated markup by the plugin -->
<div class="tooltip top" role="tooltip">
  <div class="tooltip-arrow"></div>
  <div class="tooltip-inner">
    Some tooltip text!
  </div>
</div>
```

##### 多线链路

有时，您希望将工具提示添加到包装多行的超链接中。工具提示插件的默认行为是将其水平和垂直地居中。加`white-space: nowrap;`为了避免这件事。

##### 按钮组、输入组和表中的工具提示需要特殊设置

当内的元件使用工具提示`.btn-group`或`.input-group`，或上表相关的元素（`<td>`，`<th>`，`<tr>`，`<thead>`，`<tbody>`，`<tfoot>`），就必须指定选项`container: 'body'`（下面记载），以避免不想要的副作用（如元件日益加大和/或者在触发工具提示时丢失其圆角）。

##### 不要试图在隐藏的元素上显示工具提示

调用`$(...).tooltip('show')`当目标元素是`display: none;`将导致工具提示的位置不正确。

##### 键盘和辅助技术用户可访问的工具提示

对于使用键盘导航的用户，特别是使用辅助技术的用户，您应该只向可用于键盘焦点的元素添加工具提示，例如链接、窗体控件或任何具有`tabindex="0"`属性。

##### 关于禁用元素的工具提示需要包装器元素。

若要将工具提示添加到`disabled`或`.disabled`元素中的元素。`<div>`并将工具提示应用到`<div>`相反。

#### 备选方案

选项可以通过数据属性或JavaScript传递。对于数据属性，将选项名追加到`data-`，如`data-animation=""`.

| 名称      | 类型                         | 默认值                                                       | 细节                                                         |
| :-------- | :--------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| animation | boolean                      | true                                                         | Apply a CSS fade transition to the tooltip                   |
| container | string \| false              | false                                                        | Appends the tooltip to a specific element. Example: container: 'body'. This option is particularly useful in that it allows you to position the tooltip in the flow of the document near the triggering element - which will prevent the tooltip from floating away from the triggering element during a window resize. |
| delay     | number \| object             | 0                                                            | Delay showing and hiding the tooltip (ms) - does not apply to manual trigger type If a number is supplied, delay is applied to both hide/show Object structure is: delay: { "show": 500, "hide": 100 } |
| html      | boolean                      | false                                                        | Insert HTML into the tooltip. If false, jQuery's text method will be used to insert content into the DOM. Use text if you're worried about XSS attacks. |
| placement | string \| function           | 'top'                                                        | How to position the tooltip - top \| bottom \| left \| right \| auto.When "auto" is specified, it will dynamically reorient the tooltip. For example, if placement is "auto left", the tooltip will display to the left when possible, otherwise it will display right. When a function is used to determine the placement, it is called with the tooltip DOM node as its first argument and the triggering element DOM node as its second. The this context is set to the tooltip instance. |
| selector  | string                       | false                                                        | If a selector is provided, tooltip objects will be delegated to the specified targets. In practice, this is used to enable dynamic HTML content to have tooltips added. See this and an informative example. |
| template  | string                       | '<div class="tooltip" role="tooltip"><div class="tooltip-arrow"></div><div class="tooltip-inner"></div></div>' | Base HTML to use when creating the tooltip. The tooltip's title will be injected into the .tooltip-inner. .tooltip-arrow will become the tooltip's arrow. The outermost wrapper element should have the .tooltip class. |
| title     | string \| function           | ''                                                           | Default title value if title attribute isn't present. If a function is given, it will be called with its this reference set to the element that the tooltip is attached to. |
| trigger   | string                       | 'hover focus'                                                | How tooltip is triggered - click \| hover \| focus \| manual. You may pass multiple triggers; separate them with a space. manual cannot be combined with any other trigger. |
| viewport  | string \| object \| function | { selector: 'body', padding: 0 }                             | Keeps the tooltip within the bounds of this element. Example: viewport: '#viewport' or { "selector": "#viewport", "padding": 0 } If a function is given, it is called with the triggering element DOM node as its only argument. The this context is set to the tooltip instance. |

##### 个别工具提示的数据属性

可以通过使用数据属性来指定单个工具提示的选项，如上文所述。

#### 方法

##### `$().tooltip(options)`

将工具提示处理程序附加到元素集合。

##### `.tooltip('show')`

揭示元素的工具提示。**在实际显示工具提示**之前（即在`shown.bs.tooltip`事件发生之前）**返回给调用者**。这被认为是工具提示的“手动”触发。从不显示零长度标题的工具提示。

```javascript
$('#element').tooltip('show')
```

##### `.tooltip('hide')`

隐藏元素的工具提示。**在工具提示实际上已被隐藏**之前（即`hidden.bs.tooltip`事件发生之前）**返回给调用者**。这被认为是工具提示的“手动”触发。

```javascript
$('#element').tooltip('hide')
```

##### `.tooltip('toggle')`

切换元素的工具提示。**返回到之前提示主叫方实际上已显示或隐藏**（IE浏览器之前`shown.bs.tooltip`或`hidden.bs.tooltip`事件发生时）。这被认为是工具提示的“手动”触发。

```javascript
$('#element').tooltip('toggle')
```

##### `.tooltip('destroy')`

隐藏和销毁元素的工具提示。使用委托（使用`selector`选项创建的）的工具提示不能在后代触发器元素上单独销毁。

```javascript
$('#element').tooltip('destroy')
```

#### 事件

| Event Type          | Description                                                  |
| :------------------ | :----------------------------------------------------------- |
| show.bs.tooltip     | This event fires immediately when the show instance method is called. |
| shown.bs.tooltip    | This event is fired when the tooltip has been made visible to the user (will wait for CSS transitions to complete). |
| hide.bs.tooltip     | This event is fired immediately when the hide instance method has been called. |
| hidden.bs.tooltip   | This event is fired when the tooltip has finished being hidden from the user (will wait for CSS transitions to complete). |
| inserted.bs.tooltip | This event is fired after the show.bs.tooltip event when the tooltip template has been added to the DOM. |

```javascript
$('#myTooltip').on('hidden.bs.tooltip', function () {
  // do something…
})
```

## popover popover.js

在任何用于容纳次要信息的元素中添加小的内容覆盖，就像iPad上的内容一样。

标题和内容都为零长度的弹出式永远不会显示。

##### 插件依赖性

爆米花需要工具提示插件包括在你的Bootstrap版本中。

##### 选择功能

由于性能原因，工具提示和Popover数据-api是选择的，意思是**你必须自己初始化**...

初始化页面上所有弹出框的一种方法是根据它们的`data-toggle`属性：

```javascript
$(function () {
  $('[data-toggle="popover"]').popover()
})
```

##### 按钮组、输入组和表中的弹出需要特殊设置

当内的元件使用popovers `.btn-group`或`.input-group`，或上表相关的元素（`<td>`，`<th>`，`<tr>`，`<thead>`，`<tbody>`，`<tfoot>`），就必须指定选项`container: 'body'`（下面记载），以避免不想要的副作用（如元件日益加大和/或者在弹出窗口被触发时丢失其圆角）。

##### 不要试图在隐藏的元素上显示弹出窗口

调用`$(...).popover('show')`当目标元素是`display: none;`会导致弹出器的位置不正确。

##### 禁用元素的弹出需要包装元素。

若要向`disabled`或`.disabled`元素中的元素。`<div>`然后把波波夫应用到那个`<div>`相反。

##### 多线链路

有时，您想要向一个包装多行的超链接中添加一个弹出窗口。Popover插件的默认行为是将其水平和垂直地居中。加`white-space: nowrap;`为了避免这件事。

### 实例

#### 静态 **popover**

有四个选项可用：上、右、底和左对齐。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#popovers-examples)

#### 现场演示

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#popovers-examples)

```javascript
<button type="button" class="btn btn-lg btn-danger" data-toggle="popover" title="Popover title" data-content="And here's some amazing content. It's very engaging. Right?">Click to toggle popover</button>
```

##### 四个方向

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#popovers-examples)

```javascript
<button type="button" class="btn btn-default" data-container="body" data-toggle="popover" data-placement="left" data-content="Vivamus sagittis lacus vel augue laoreet rutrum faucibus.">
  Popover on left
</button>

<button type="button" class="btn btn-default" data-container="body" data-toggle="popover" data-placement="top" data-content="Vivamus sagittis lacus vel augue laoreet rutrum faucibus.">
  Popover on top
</button>

<button type="button" class="btn btn-default" data-container="body" data-toggle="popover" data-placement="bottom" data-content="Vivamus
sagittis lacus vel augue laoreet rutrum faucibus.">
  Popover on bottom
</button>

<button type="button" class="btn btn-default" data-container="body" data-toggle="popover" data-placement="right" data-content="Vivamus sagittis lacus vel augue laoreet rutrum faucibus.">
  Popover on right
</button>
```

##### 下一次点击解散

使用`focus`触发器，以在用户下一次单击时取消弹出。

##### 下一次单击“解散”所需的特定标记

对于正确的跨浏览器和跨平台行为，必须使用`<a>`标签，*不*大`<button>`标记，并且您还必须包括`role="button"`和[`tabindex`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes#tabindex)属性。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#callout-popover-dismiss-click)

```javascript
<a tabindex="0" class="btn btn-lg btn-danger" role="button" data-toggle="popover" data-trigger="focus" title="Dismissible popover" data-content="And here's some amazing content. It's very engaging. Right?">Dismissible popover</a>
```

### 使用

通过JavaScript启用弹出：

```javascript
$('#example').popover(options)
```

#### 备选方案

选项可以通过数据属性或JavaScript传递。对于数据属性，请将选项名称附加到中`data-`，如`data-animation=""`。

| 名称      | 类型                         | 默认值                                                       | 描述                                                         |
| :-------- | :--------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| animation | boolean                      | true                                                         | Apply a CSS fade transition to the popover                   |
| container | string \| false              | false                                                        | Appends the popover to a specific element. Example: container: 'body'. This option is particularly useful in that it allows you to position the popover in the flow of the document near the triggering element - which will prevent the popover from floating away from the triggering element during a window resize. |
| content   | string \| function           | ''                                                           | Default content value if data-content attribute isn't present. If a function is given, it will be called with its this reference set to the element that the popover is attached to. |
| delay     | number \| object             | 0                                                            | Delay showing and hiding the popover (ms) - does not apply to manual trigger type If a number is supplied, delay is applied to both hide/show Object structure is: delay: { "show": 500, "hide": 100 } |
| html      | boolean                      | false                                                        | Insert HTML into the popover. If false, jQuery's text method will be used to insert content into the DOM. Use text if you're worried about XSS attacks. |
| placement | string \| function           | 'right'                                                      | How to position the popover - top \| bottom \| left \| right \| auto.When "auto" is specified, it will dynamically reorient the popover. For example, if placement is "auto left", the popover will display to the left when possible, otherwise it will display right. When a function is used to determine the placement, it is called with the popover DOM node as its first argument and the triggering element DOM node as its second. The this context is set to the popover instance. |
| selector  | string                       | false                                                        | If a selector is provided, popover objects will be delegated to the specified targets. In practice, this is used to enable dynamic HTML content to have popovers added. See this and an informative example. |
| template  | string                       | '<div class="popover" role="tooltip"><div class="arrow"></div><h3 class="popover-title"></h3><div class="popover-content"></div></div>' | Base HTML to use when creating the popover. The popover's title will be injected into the .popover-title. The popover's content will be injected into the .popover-content. .arrow will become the popover's arrow. The outermost wrapper element should have the .popover class. |
| title     | string \| function           | ''                                                           | Default title value if title attribute isn't present. If a function is given, it will be called with its this reference set to the element that the popover is attached to. |
| trigger   | string                       | 'click'                                                      | How popover is triggered - click \| hover \| focus \| manual. You may pass multiple triggers; separate them with a space. manual cannot be combined with any other trigger. |
| viewport  | string \| object \| function | { selector: 'body', padding: 0 }                             | Keeps the popover within the bounds of this element. Example: viewport: '#viewport' or { "selector": "#viewport", "padding": 0 } If a function is given, it is called with the triggering element DOM node as its only argument. The this context is set to the popover instance. |

##### 单个弹出式的数据属性

如上文所述，可以通过使用数据属性来指定单个弹出选项。

#### 方法

##### `$().popover(options)`

初始化元素集合的弹出。

##### `.popover('show')`

揭示元素的弹出窗口。**在实际显示弹出窗口**之前（即在`shown.bs.popover`事件发生之前）**返回给调用者**。这被认为是弹出式的“手动”触发。永远不会显示标题和内容均为零的Popovers。

```javascript
$('#element').popover('show')
```

##### `.popover('hide')`

隐藏元素的弹出窗口。**在弹出窗口实际上被隐藏**之前（即在`hidden.bs.popover`事件发生之前）**返回给调用者**。这被认为是弹出式的“手动”触发。

```javascript
$('#element').popover('hide')
```

##### `.popover('toggle')`

切换元素的弹出窗口。**返回之前，主叫方实际上已显示或隐藏**（IE浏览器之前`shown.bs.popover`或`hidden.bs.popover`事件发生时）。这被认为是弹出式的“手动”触发。

```javascript
$('#element').popover('toggle')
```

##### `.popover('destroy')`

隐藏和销毁元素的弹出窗口。使用委托（使用`selector`选项创建）的弹出窗口不能单独销毁在后代触发器元素上。

```javascript
$('#element').popover('destroy')
```

#### 事件

| Event Type          | Description                                                  |
| :------------------ | :----------------------------------------------------------- |
| show.bs.popover     | This event fires immediately when the show instance method is called. |
| shown.bs.popover    | This event is fired when the popover has been made visible to the user (will wait for CSS transitions to complete). |
| hide.bs.popover     | This event is fired immediately when the hide instance method has been called. |
| hidden.bs.popover   | This event is fired when the popover has finished being hidden from the user (will wait for CSS transitions to complete). |
| inserted.bs.popover | This event is fired after the show.bs.popover event when the popover template has been added to the DOM. |

```javascript
$('#myPopover').on('hidden.bs.popover', function () {
  // do something…
})
```

## 警报消息

### 示例警报

添加解散功能到所有警告消息与此插件。

当使用`.close`按钮，它必须是`.alert-dismissible`在标记中，文本内容也不可能出现在它之前。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#alerts-examples)

### 使用

只要加上`data-dismiss="alert"`到“关闭”按钮自动提供警报关闭功能。关闭警报将其从DOM中删除。

```javascript
<button type="button" class="close" data-dismiss="alert" aria-label="Close">
  <span aria-hidden="true">&times;</span>
</button>
```

若要让警报在关闭时使用动画，请确保它们具有`.fade`和`.in`类已经应用于它们。

#### 方法

##### `$().alert()`

使警报侦听具有该`data-dismiss="alert"`属性的后代元素上的点击事件。（使用data-api的自动初始化时不需要）。

##### `$().alert('close')`

通过从DOM中删除警报来关闭警报。如果`.fade`和`.in`类存在于元素上，警报在删除之前将逐渐消失。

#### 事件

引导程序的警报插件公开了一些事件，以便挂接到警报功能。

| Event Type      | Description                                                  |
| :-------------- | :----------------------------------------------------------- |
| close.bs.alert  | This event fires immediately when the close instance method is called. |
| closed.bs.alert | This event is fired when the alert has been closed (will wait for CSS transitions to complete). |

```javascript
$('#myAlert').on('closed.bs.alert', function () {
  // do something…
})
```

## **Buttons button.js**

用纽扣做更多。控制按钮状态或为更多的组件(如工具栏)创建按钮组。

##### 跨浏览器兼容性

[Firefox在页面加载时保持表单控制状态（禁用和检查）](https://github.com/twbs/bootstrap/issues/793)。解决方法是使用`autocomplete="off"`。参见[Mozilla错误＃654072](https://bugzilla.mozilla.org/show_bug.cgi?id=654072)。

### 有状态

加`data-loading-text="Loading..."`若要在按钮上使用加载状态，请执行以下操作。

**这个特性从v3.3.5开始就不再受欢迎，并且在v4中被删除了。**

##### 使用你喜欢的任何一种状态！

为了演示，我们正在使用`data-loading-text`和`$().button('loading')`，但这不是您可以使用的唯一状态。在`$().button(string)`文档中查看下面的更多信息。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#callout-buttons-state-names)

```javascript
<button type="button" id="myButton" data-loading-text="Loading..." class="btn btn-primary" autocomplete="off">
  Loading state
</button>

<script>
  $('#myButton').on('click', function () {
    var $btn = $(this).button('loading')
    // business logic...
    $btn.button('reset')
  })
</script>
```

### 单一切换

添加`data-toggle="button"`以激活单个按钮的切换。

##### 预切换按钮`.active`和`aria-pressed="true"`

对于预切换按钮，必须添加`.active`类和`aria-pressed="true"`属性的`button`。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#callout-buttons-single-pretoggled)

```javascript
<button type="button" class="btn btn-primary" data-toggle="button" aria-pressed="false" autocomplete="off">
  Single toggle
</button>
```

### 复选框/单选按钮

添加`data-toggle="buttons"`到`.btn-group`包含复选框或收音机输入以启用其各自样式的切换。

##### 预选选项需要`.active`

对于预先选择的选项，您必须将该`.active`类添加到您`label`自己的输入中。

##### 只在单击时更新可视选中状态

如果在不触发`click`按钮上的事件的情况下更新复选框按钮的选中状态（例如，通过`<input type="reset">`或通过设置`checked`输入的属性），您将需要自行切换`.active`输入的类`label`。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#callout-buttons-only-click)

```javascript
<div class="btn-group" data-toggle="buttons">
  <label class="btn btn-primary active">
    <input type="checkbox" autocomplete="off" checked> Checkbox 1 (pre-checked)
  </label>
  <label class="btn btn-primary">
    <input type="checkbox" autocomplete="off"> Checkbox 2
  </label>
  <label class="btn btn-primary">
    <input type="checkbox" autocomplete="off"> Checkbox 3
  </label>
</div>
```

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#callout-buttons-only-click)

```javascript
<div class="btn-group" data-toggle="buttons">
  <label class="btn btn-primary active">
    <input type="radio" name="options" id="option1" autocomplete="off" checked> Radio 1 (preselected)
  </label>
  <label class="btn btn-primary">
    <input type="radio" name="options" id="option2" autocomplete="off"> Radio 2
  </label>
  <label class="btn btn-primary">
    <input type="radio" name="options" id="option3" autocomplete="off"> Radio 3
  </label>
</div>
```

### 方法

##### `$().button('toggle')`

切换推进状态。使按钮具有已激活的外观。

##### `$().button('reset')`

重置按钮状态-将文本切换为原始文本。**此方法是异步的，并在重置实际完成之前返回。**

##### `$().button(string)`

将文本转换为任何数据定义的文本状态。

```javascript
<button type="button" id="myStateButton" data-complete-text="finished!" class="btn btn-primary" autocomplete="off">
  ...
</button>

<script>
  $('#myStateButton').on('click', function () {
    $(this).button('complete') // button text will be "finished!"
  })
</script>
```

## **Collapse collapse.js**



灵活的插件，利用少数类，以方便切换行为。

##### 插件依赖性

折叠需要过渡插件包括在你的Bootstrap版本中。

### 例

单击下面的按钮，通过类更改显示并隐藏另一个元素：

- `.collapse`隐藏内容

- `.collapsing`在过渡期间应用

- `.collapse.in`显示内容

您可以使用与`href`属性，或带有`data-target`属性。在这两种情况下，`data-toggle="collapse"`是必需的。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#collapse-example)

```javascript
<a class="btn btn-primary" role="button" data-toggle="collapse" href="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
  Link with href
</a>
<button class="btn btn-primary" type="button" data-toggle="collapse" data-target="#collapseExample" aria-expanded="false" aria-controls="collapseExample">
  Button with data-target
</button>
<div class="collapse" id="collapseExample">
  <div class="well">
    ...
  </div>
</div>
```

### Accordion 示例

扩展默认的折叠行为以创建带有面板组件的手风琴。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#collapse-example-accordion)

```javascript
<div class="panel-group" id="accordion" role="tablist" aria-multiselectable="true">
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingOne">
      <h4 class="panel-title">
        <a role="button" data-toggle="collapse" data-parent="#accordion" href="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
          Collapsible Group Item #1
        </a>
      </h4>
    </div>
    <div id="collapseOne" class="panel-collapse collapse in" role="tabpanel" aria-labelledby="headingOne">
      <div class="panel-body">
        Anim pariatur cliche reprehenderit, enim eiusmod high life accusamus terry richardson ad squid. 3 wolf moon officia aute, non cupidatat skateboard dolor brunch. Food truck quinoa nesciunt laborum eiusmod. Brunch 3 wolf moon tempor, sunt aliqua put a bird on it squid single-origin coffee nulla assumenda shoreditch et. Nihil anim keffiyeh helvetica, craft beer labore wes anderson cred nesciunt sapiente ea proident. Ad vegan excepteur butcher vice lomo. Leggings occaecat craft beer farm-to-table, raw denim aesthetic synth nesciunt you probably haven't heard of them accusamus labore sustainable VHS.
      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingTwo">
      <h4 class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordion" href="#collapseTwo" aria-expanded="false" aria-controls="collapseTwo">
          Collapsible Group Item #2
        </a>
      </h4>
    </div>
    <div id="collapseTwo" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingTwo">
      <div class="panel-body">
        Anim pariatur cliche reprehenderit, enim eiusmod high life accusamus terry richardson ad squid. 3 wolf moon officia aute, non cupidatat skateboard dolor brunch. Food truck quinoa nesciunt laborum eiusmod. Brunch 3 wolf moon tempor, sunt aliqua put a bird on it squid single-origin coffee nulla assumenda shoreditch et. Nihil anim keffiyeh helvetica, craft beer labore wes anderson cred nesciunt sapiente ea proident. Ad vegan excepteur butcher vice lomo. Leggings occaecat craft beer farm-to-table, raw denim aesthetic synth nesciunt you probably haven't heard of them accusamus labore sustainable VHS.
      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading" role="tab" id="headingThree">
      <h4 class="panel-title">
        <a class="collapsed" role="button" data-toggle="collapse" data-parent="#accordion" href="#collapseThree" aria-expanded="false" aria-controls="collapseThree">
          Collapsible Group Item #3
        </a>
      </h4>
    </div>
    <div id="collapseThree" class="panel-collapse collapse" role="tabpanel" aria-labelledby="headingThree">
      <div class="panel-body">
        Anim pariatur cliche reprehenderit, enim eiusmod high life accusamus terry richardson ad squid. 3 wolf moon officia aute, non cupidatat skateboard dolor brunch. Food truck quinoa nesciunt laborum eiusmod. Brunch 3 wolf moon tempor, sunt aliqua put a bird on it squid single-origin coffee nulla assumenda shoreditch et. Nihil anim keffiyeh helvetica, craft beer labore wes anderson cred nesciunt sapiente ea proident. Ad vegan excepteur butcher vice lomo. Leggings occaecat craft beer farm-to-table, raw denim aesthetic synth nesciunt you probably haven't heard of them accusamus labore sustainable VHS.
      </div>
    </div>
  </div>
</div>
```

这也是可能的交换. `.panel-body`与`.list-group`

##### 使展开/折叠控件可访问

一定要加上`aria-expanded`控制元素。此属性显式定义可折叠元素的当前状态，以筛选读取器和类似的辅助技术。如果默认情况下可折叠元素关闭，则它的值应为`aria-expanded="false"`如果%27 ve将折叠元素默认设置为打开，则使用`in`类，集合`aria-expanded="true"`而不是控制。插件将根据是否打开或关闭可折叠元素自动切换此属性。

此外，如果控件元素针对的是单个可折叠元素-即`data-target`属性指向`id`选择器-您可以添加额外的`aria-controls`属性设置为控件元素，其中包含`id`可折叠元素的。现代屏幕阅读器和类似的辅助技术利用这个属性为用户提供了更多的快捷方式，可以直接导航到可折叠元素本身。

### 使用

崩溃插件使用几个类来处理繁重的工作：

- `.collapse`隐藏内容

- `.collapse.in`显示内容

- `.collapsing`在过渡开始时添加，并在完成时删除。

这些类可以在`component-animations.less`找到

#### 通过数据属性

只要加上`data-toggle="collapse"`和一个`data-target`到要自动分配可折叠元素的控件的元素。大`data-target`属性接受要将折叠应用到的CSS选择器。一定要添加类`collapse`到可折叠元素。如果%27D喜欢默认打开，则添加附加类`in`...

若要向可折叠控件中添加类似于即插即用的组管理，请添加数据属性。`data-parent="#selector"`请参考演示以查看此操作。

#### 通过JavaScript

手动启用：

```javascript
$('.collapse').collapse()
```

#### 备选方案

选项可以通过数据属性或JavaScript传递。对于数据属性，将选项名追加到`data-`，如`data-parent=""`.

| Name   | type     | default | description                                                  |
| :----- | :------- | :------ | :----------------------------------------------------------- |
| parent | selector | false   | If a selector is provided, then all collapsible elements under the specified parent will be closed when this collapsible item is shown. (similar to traditional accordion behavior - this is dependent on the panel class) |
| toggle | boolean  | true    | Toggles the collapsible element on invocation                |

#### 方法

##### `.collapse(options)`

将内容激活为可折叠元素。接受可选选项`object`...

```javascript
$('#myCollapsible').collapse({
  toggle: false
})
```

##### `.collapse('toggle')`

切换可显示或隐藏的可折叠元素。**返回到可折叠元件之前呼叫者实际上已显示或隐藏**（即在之前`shown.bs.collapse`或`hidden.bs.collapse`事件发生时）。

##### `.collapse('show')`

显示可折叠的元素。**在可折叠元素实际显示**之前（即`shown.bs.collapse`事件发生之前）**返回给调用者**。

##### `.collapse('hide')`

隐藏可折叠元素。**在可折叠元素实际隐藏之前返回给调用方。**%28i.e.。在`hidden.bs.collapse`事件发生%29。

#### 事件

引导%27折叠类公开了一些事件，用于链接到折叠功能。

| Event Type         | Description                                                  |
| :----------------- | :----------------------------------------------------------- |
| show.bs.collapse   | This event fires immediately when the show instance method is called. |
| shown.bs.collapse  | This event is fired when a collapse element has been made visible to the user (will wait for CSS transitions to complete). |
| hide.bs.collapse   | This event is fired immediately when the hide method has been called. |
| hidden.bs.collapse | This event is fired when a collapse element has been hidden from the user (will wait for CSS transitions to complete). |

```javascript
$('#myCollapsible').on('hidden.bs.collapse', function () {
  // do something…
})
```

## **Carousel carousel.js**



一种幻灯片组件，用于遍历元素，如旋转木马。**不支持嵌套旋转木马。**

### 实例

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#carousel-examples)

```javascript
<div id="carousel-example-generic" class="carousel slide" data-ride="carousel">
  <!-- Indicators -->
  <ol class="carousel-indicators">
    <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
    <li data-target="#carousel-example-generic" data-slide-to="1"></li>
    <li data-target="#carousel-example-generic" data-slide-to="2"></li>
  </ol>

  <!-- Wrapper for slides -->
  <div class="carousel-inner" role="listbox">
    <div class="item active">
      <img src="..." alt="...">
      <div class="carousel-caption">
        ...
      </div>
    </div>
    <div class="item">
      <img src="..." alt="...">
      <div class="carousel-caption">
        ...
      </div>
    </div>
    ...
  </div>

  <!-- Controls -->
  <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
    <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </a>
  <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
    <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </a>
</div>
```

##### 无障碍问题

旋转木马组件通常不符合可访问性标准。如果您需要遵守，请考虑其他选项，以显示您的内容。

##### InternetExplorer 8和9中不支持的转换动画

Bootstrap专门为其动画使用CSS3，但Internet Explorer 8和9不支持必需的CSS属性。因此，使用这些浏览器时没有幻灯片切换动画。我们故意决定不为过渡包含基于jQuery的后备。

##### 初始活动元件

该`.active`课程需要添加到其中一张幻灯片中。否则，传送带将不可见。

##### 不需要糖图标

在`.glyphicon .glyphicon-chevron-left`和`.glyphicon .glyphicon-chevron-right`班不一定需要的控件。Bootstrap提供了`.icon-prev`并且`.icon-next`是普通的unicode替代品。

#### 可选标题

将标题轻松添加到幻灯片中。`.carousel-caption`元素中的`.item`在里面放置几乎所有可选的HTML，它将自动对齐和格式化。

[打开getbootstrap.com上的示例](https://getbootstrap.com/docs/3.3/javascript/#callout-carousel-without-glyphicons)

```javascript
<div class="item">
  <img src="..." alt="...">
  <div class="carousel-caption">
    <h3>...</h3>
    <p>...</p>
  </div>
</div>
```

### 使用

#### 多重传送带

传送带需要使用`id`最外面的容器（`.carousel`）来传送传送带控制才能正常工作。添加多个轮播或更换轮播时`id`，请务必更新相关控件。

#### 通过数据属性

使用数据属性可以轻松控制传送带的位置。`data-slide`接受关键字`prev`或者`next`相对于其当前位置改变滑动位置。或者，使用`data-slide-to`将原始幻灯片索引传递给旋转木马`data-slide-to="2"`，将幻灯片位置移至以特定索引开头的位置`0`。

该`data-ride="carousel"`属性用于在转页加载时将转盘标记为动画。**它不能与相同轮播的显式JavaScript初始化（冗余和不必要的）组合使用。**

#### 通过JavaScript

手动调用旋转木马：

```javascript
$('.carousel').carousel()
```

#### 备选方案

选项可以通过数据属性或JavaScript传递。对于数据属性，将选项名追加到`data-`，如`data-interval=""`.

| Name     | type           | default | description                                                  |
| :------- | :------------- | :------ | :----------------------------------------------------------- |
| interval | number         | 5000    | The amount of time to delay between automatically cycling an item. If false, carousel will not automatically cycle. |
| pause    | string \| null | "hover" | If set to "hover", pauses the cycling of the carousel on mouseenter and resumes the cycling of the carousel on mouseleave. If set to null, hovering over the carousel won't pause it. |
| wrap     | boolean        | true    | Whether the carousel should cycle continuously or have hard stops. |
| keyboard | boolean        | true    | Whether the carousel should react to keyboard events.        |

#### 方法

##### `.carousel(options)`

使用可选选项初始化旋转木马。`object`开始骑自行车穿过物品。

```javascript
$('.carousel').carousel({
  interval: 2000
})
```

##### `.carousel('cycle')`

从左到右循环旋转木马项目。

##### `.carousel('pause')`

阻止carousel通过物品循环。

##### `.carousel(number)`

将传送带循环到特定帧（基于0，类似于数组）。

##### `.carousel('prev')`

循环到上一项。

##### `.carousel('next')`

循环到下一项。

#### 事件

Bootstrap的carousel类公开了两个事件用于挂接轮播功能。

这两个事件都具有以下附加属性：

- `direction` carousel滑动的方向`"left"`或`"right"`。

- `relatedTarget`：作为活动项正在滑动的DOM元素。

所有carousel事件都是在carousel本身（即在`<div class="carousel">`）执行的。

| Event Type        | Description                                                  |
| :---------------- | :----------------------------------------------------------- |
| slide.bs.carousel | This event fires immediately when the slide instance method is invoked. |
| slid.bs.carousel  | This event is fired when the carousel has completed its slide transition. |

```javascript
$('#myCarousel').on('slide.bs.carousel', function () {
  // do something…
})
```

## 词缀

### 例

词缀插件`position: fixed;`断断续续地模仿[`position: sticky;`](https://developer.mozilla.org/en-US/docs/Web/CSS/position#Sticky_positioning).右边的子导航是词缀插件的现场演示。

### 使用

通过数据属性或手动使用您自己的JavaScript插件。**在这两种情况下，您必须提供CSS的位置和宽度的附加内容。**

注意：不要对包含在相对位置元素中的元素使用词缀插件，例如拉出或推送列，因为[Safari渲染错误](https://github.com/twbs/bootstrap/issues/12126)...

#### 通过CSS定位

缀三个类之间切换插件，每个代表一个特定的状态：`.affix`，`.affix-top`，和`.affix-bottom`。您必须为这些类自己（独立于此插件）提供样式（除`position: fixed;`on 之外`.affix`）来处理实际位置。

以下是词缀插件的工作原理：

1. 首先，插件添加了`.affix-top`以指示元素位于其最顶端。此时不需要CSS定位。

\2. 滚动要粘贴的元素应触发实际粘贴。这是`.affix`替换`.affix-top`和设置的地方`position: fixed;`（由Bootstrap的CSS提供）。

\3. 如果定义了底部偏移量，则滚动过它应替换`.affix`带着`.affix-bottom`因为偏移量是可选的，设置偏移量需要设置适当的CSS。在本例中，添加`position: absolute;`必要的时候。插件使用Data属性或JavaScript选项来确定元素的位置。

按照上述步骤为下面的使用选项中的任何一个设置CSS。

#### 通过数据属性

要轻松地向任何元素添加词缀行为，只需添加`data-spy="affix"`你想监视的元素。使用偏移量来定义何时切换元素的固定。

```javascript
<div data-spy="affix" data-offset-top="60" data-offset-bottom="200">
  ...
</div>
```

#### 通过JavaScript

通过JavaScript调用词缀插件：

```javascript
$('#myAffix').affix({
  offset: {
    top: 100,
    bottom: function () {
      return (this.bottom = $('.footer').outerHeight(true))
    }
  }
})
```

#### 备选方案

选项可以通过数据属性或JavaScript传递。对于数据属性，将选项名追加到`data-`，如`data-offset-top="200"`...

| Name   | type                               | default           | description                                                  |
| :----- | :--------------------------------- | :---------------- | :----------------------------------------------------------- |
| offset | number \| function \| object       | 10                | Pixels to offset from screen when calculating position of scroll. If a single number is provided, the offset will be applied in both top and bottom directions. To provide a unique, bottom and top offset just provide an object offset: { top: 10 } or offset: { top: 10, bottom: 5 }. Use a function when you need to dynamically calculate an offset. |
| target | selector \| node \| jQuery element | the window object | Specifies the target element of the affix.                   |

#### 方法

##### `.affix(options)`

激活您的内容作为附加内容。接受可选的选项`object`。

```javascript
$('#myAffix').affix({
  offset: 15
})
```

##### `.affix('checkPosition')`

根据相关元素的尺寸，位置和滚动位置重新计算词缀的状态。`.affix`，`.affix-top`和`.affix-bottom`类添加到或根据新的状态下从贴内容删除。只要贴上内容或目标元素的尺寸发生变化，就需要调用此方法，以确保贴上内容的正确定位。

```javascript
$('#myAffix').affix('checkPosition')
```

#### 事件

Bootstrap的词缀插件公开了一些事件，以便挂接到词缀功能。

| Event Type              | Description                                                  |
| :---------------------- | :----------------------------------------------------------- |
| affix.bs.affix          | This event fires immediately before the element has been affixed. |
| affixed.bs.affix        | This event is fired after the element has been affixed.      |
| affix-top.bs.affix      | This event fires immediately before the element has been affixed-top. |
| affixed-top.bs.affix    | This event is fired after the element has been affixed-top.  |
| affix-bottom.bs.affix   | This event fires immediately before the element has been affixed-bottom. |
| affixed-bottom.bs.affix | This event is fired after the element has been affixed-bottom. |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-10-09