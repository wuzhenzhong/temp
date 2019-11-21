# index（指数）

- 贡献者3人

  

Sass是CSS的扩展，为基础语言增添了力量和优雅。它允许您使用变量，嵌套规则，mixins，内联导入等，全部使用完全与CSS兼容的语法。Sass有助于保持大型样式表组织良好，并且可以快速启动小型样式表，特别是在 [Compass样式库 ](http://compass-style.org/)的帮助下。

## 特色功能

- 完全与CSS兼容
- 语言扩展，如变量，嵌套和mixin
- 许多用于处理颜色和其他值的有用功能
- 诸如库的控制指令等高级功能
- 格式良好，可定制的输出

## 语法格式

有两种语法可用于 Sass。第一种称为 SCSS（Sassy CSS），用于整篇引用，是 CSS 语法的扩展。这意味着每个有效的 CSS 样式表都是具有相同含义的有效 SCSS 文件。另外，SCSS 能理解大多数 CSS 黑客和特定于供应商的语法，例如 IE 中旧的[`filter`](http://msdn.microsoft.com/en-us/library/ms530752.aspx)语法[ ](http://msdn.microsoft.com/en-us/library/ms530752.aspx)。这个语法通过下面描述的 Sass 特性得到了增强。使用此语法的文件具有`.scss`扩展名。

第二个和更老的语法，被称为缩进语法（或者有时候只是“Sass”），提供了一种更简洁的编写CSS的方式。它使用缩进而不是括号来指示选择器的嵌套，而使用换行符而不是分号来分隔属性。有些人认为这比SCSS更容易阅读和更快写入。缩进语法具有所有相同的功能，尽管它们中的一些具有稍微不同的语法; 这在[缩进的语法参考](http://sass-lang.com/documentation/file.INDENTED_SYNTAX.html)中进行[了](http://sass-lang.com/documentation/file.INDENTED_SYNTAX.html)描述。使用此语法的文件具有`.sass`扩展名。

任何一种语法的文件都可以导入另一种语法的文件。使用`sass-convert`命令行工具可以自动将文件从一种语法转换为另一种语法：

```javascript
# Convert Sass to SCSS
$ sass-convert style.sass style.scss

# Convert SCSS to Sass
$ sass-convert style.scss style.sass
```

请注意，这个命令*不*生成CSS文件。为此，请使用`sass`命令描述其他地方。

## 使用Sass

Sass可以以三种方式使用：作为命令行工具，作为独立的Ruby模块，以及任何支持Rack的框架（包括Ruby on Rails和Merb）的插件。所有这些的第一步是安装Sass gem：

```javascript
gem install sass
```

如果您使用Windows，则可能需要先[安装Ruby](http://rubyinstaller.org/download.html)。

要从命令行运行Sass，只需使用

```javascript
sass input.scss output.css
```

每当Sass文件发生变化时，您也可以让Sass观看文件并更新CSS：

```javascript
sass --watch input.scss:output.css
```

如果你有一个包含许多Sass文件的目录，你也可以告诉Sass观看整个目录：

```javascript
sass --watch app/sass:public/stylesheets
```

使用`sass --help查看`完整文档。

在Ruby代码中使用Sass非常简单。在安装了Sass之后，你可以通过运行`require "sass"，`使用[Sass :: Engine](http://sass-lang.com/documentation/Sass/Engine.html)就像这样：

```javascript
engine = Sass::Engine.new("#main {background-color: #0000ff}", :syntax => :scss)
engine.render #=> "#main { background-color: #0000ff; }\n"
```

### Rack/Rails/Merb Plugin

在 Rails 3 之前的版本中使用 Sass，需要在`environment.rb`文件中添加：



```javascript
config.gem "sass"
```

对于Rails 3，则需要在Gemfile中添加：

```javascript
gem "sass"
```

要在Merb中启用Sass，请将以下一行添加到`config/dependencies.rb`：

```javascript
dependency "merb-haml"
```

要在Rack应用程序中启用Sass，请将以下行添加到`config.ru`。

```javascript
require 'sass/plugin/rack'
use Sass::Plugin::Rack
```

Sass样式表与视图不一样。它们不包含动态内容，因此只需在Sass文件更新时生成CSS。默认情况下，`.sass`和`.scss`public/stylesheets/ SASS（这可以通过自定义`:template_location`选项修改路径）。然后，在必要时，将它们编译为public/stylesheets中的相应CSS文件。例如，public / stylesheets / sass / main.scss将被编译为public / stylesheets / main.css。

### 缓存

默认情况下，Sass 自动缓存编译后的模板与[partials](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#partials)。这极大地加速了对大量Sass文件的重新编译，尤其在处理由`@import`导入多个子文件的大型项目时



没有框架，Sass会将缓存的模板放入`.sass-cache`目录中。在Rails和Merb中，他们进入`tmp/sass-cache`。该目录可以使用该`:cache_location`选项进行自定义。如果您不希望Sass使用缓存，请将该`:cache`选项设置为`false`。

### 配置选项

配置选项可以通过在Rails中`environment.`或Rack中`rbconfig.ru`设置[Sass :: Plugin＃选项](http://sass-lang.com/documentation/Sass/Plugin/Configuration.html#options-instance_method)哈希来设置。

```javascript
Sass::Plugin.options[:style] = :compact
```

...或通过在Merb中`init.rb`设置`Merb::Plugin.config[:sass]`哈希...

```javascript
Merb::Plugin.config[:sass][:style] = :compact
```

...或通过将选项散列传递给[Sass :: Engine＃initialize](http://sass-lang.com/documentation/Sass/Engine.html#initialize-instance_method)。所有相关选项也可通过标志`sass`和`scss`命令行可执行文件获得。可用的选项有：

＃style-option `:style`：设置CSS输出的样式。请参阅输出样式。

＃syntax-option `:syntax`：输入文件的语法，`:sass`用于缩进语法和`:scss`CSS扩展语法。这只在你自己构造[Sass :: Engine](http://sass-lang.com/documentation/Sass/Engine.html)实例时才有用; 使用[Sass :: Plugin](http://sass-lang.com/documentation/Sass/Plugin.html)时会自动设置。默认为`:sass`。

＃property_syntax-option `:property_syntax`：强制缩进语法文档为属性使用一种语法。如果没有使用正确的语法，则会引发错误。`:new`强制在属性名称后面使用冒号。例如：`color: #0f3`或`width: $main_width`。`:old`在属性名称之前强制使用冒号。例如：`:color #0f3`或`:width $main_width`。默认情况下，任何语法都是有效的。这对SCSS文件没有影响。

＃cache-option `:cache`：是否应该缓存解析的Sass文件，从而提高速度。默认为true。

＃read_cache-option `:read_cache`：如果设置了它并且`:cache`没有设置，那么只有在Sass缓存存在的情况下才能读取，如果不存在，就不要写入。

＃cache_store-option `:cache_store`：如果将其设置为[Sass :: CacheStores :: Base](http://sass-lang.com/documentation/Sass/CacheStores/Base.html)的子类的实例，[则](http://sass-lang.com/documentation/Sass/CacheStores/Base.html)该缓存存储将用于存储和检索缓存的编译结果。默认为使用`:cache_location`选项初始化[Sass :: CacheStores :: Filesystem](http://sass-lang.com/documentation/Sass/CacheStores/Filesystem.html)。

＃never_update-option `:never_update`：即使模板文件改变，CSS文件是否永远不会更新。将其设置为true可能会带来较小的性能提升。它总是默认为false。只在Rack，Ruby on Rails或Merb中有意义。

＃always_update-option `:always_update`：每次访问控制器时是否更新CSS文件，而不是仅在模板被修改时更新。默认为false。只在Rack，Ruby on Rails或Merb中有意义。

＃always_check-option `:always_check`：每次访问控制器时是否应检查更新的Sass模板，而不是仅在服务器启动时进行更新。如果Sass模板已经更新，它将被重新编译并覆盖相应的CSS文件。在生产模式下默认为false，否则为true。只在Rack，Ruby on Rails或Merb中有意义。

＃poll-option `:poll`：如果为true，则始终对[Sass :: Plugin :: Compiler＃watch](http://sass-lang.com/documentation/Sass/Plugin/Compiler.html#watch-instance_method)使用轮询后端，而不是本地文件系统后端。

＃full_exception-option `:full_exception`：Sass代码中的错误是否应该导致Sass在生成的CSS文件中提供详细的描述。如果设置为true，则错误将与行号和源代码片段一起显示为CSS文件中的注释和页面顶部（支持的浏览器中）的注释。否则，将在Ruby代码中引发异常。在生产模式下默认为false，否则为true。

＃template_location-option `:template_location`：您的应用程序的根sass模板目录的路径。如果散列`:css_location`被忽略，并且该选项指定输入和输出目录之间的映射。也可以给出一个2元素列表的列表，而不是散列。默认为`css_location + "/sass"`。只在Rack，Ruby on Rails或Merb中有意义。请注意，如果指定了多个模板位置，则它们全部放置在导入路径中，允许您在它们之间导入。**请注意，由于可能需要多种格式，因此只能直接设置此选项，而不能访问或修改此选项。使用** [**Sass :: Plugin＃template_location_array**](http://sass-lang.com/documentation/Sass/Plugin/Configuration.html#template_location_array-instance_method)**，** [**Sass :: Plugin＃add_template_location**](http://sass-lang.com/documentation/Sass/Plugin/Configuration.html#add_template_location-instance_method)**和** [**Sass :: Plugin＃remove_template_location**](http://sass-lang.com/documentation/Sass/Plugin/Configuration.html#remove_template_location-instance_method) **方法**。

＃css_location-option `:css_location`：CSS输出应写入的路径。当`:template_location`是散列时，该选项将被忽略。默认为`"./public/stylesheets"`。只在Rack，Ruby on Rails或Merb中有意义。

＃cache_location-option `:cache_location`：缓存`sassc`文件应写入的路径。默认使用`"./tmp/sass-cache"`Rails和Merb，`"./.sass-cache"`否则。如果`:cache_store`选项已设置，则忽略此选项。

＃unix_newlines-option `:unix_newlines`：如果为true，则在编写文件时使用Unix风格的换行符。只有在Windows上有意义，并且只有当Sass正在编写文件时（在Rack，Rails或Merb中，直接使用[Sass :: Plugin时](http://sass-lang.com/documentation/Sass/Plugin.html)，或者在使用命令行可执行文件时）。

＃filename-option `:filename`：正在渲染的文件的文件名。这仅用于报告错误，并在使用Rack，Rails或Merb时自动设置。

＃line-option `:line`：Sass模板的第一行的编号。用于报告错误的行号。如果Sass模板嵌入到Ruby文件中，这是非常有用的。

＃load_paths-option `:load_paths`：文件系统路径或导入器的数组，应该搜索使用该`@import`指令导入的Sass模板。这些可能是字符串，`Pathname`对象或[Sass :: Importers :: Base的](http://sass-lang.com/documentation/Sass/Importers/Base.html)子类。这默认为工作目录，在Rack，Rails或Merb中，无论如何`:template_location`。加载路径也由[Sass.load_paths](http://sass-lang.com/documentation/Sass.html#load_paths-class_method)和`SASS_PATH`环境变量通知。

＃filesystem_importer-option `:filesystem_importer`：用于处理纯字符串加载路径的[Sass :: Importers :: Base](http://sass-lang.com/documentation/Sass/Importers/Base.html)子类。这应该从文件系统导入文件。它应该是一个继承自[Sass :: Importers :: Base](http://sass-lang.com/documentation/Sass/Importers/Base.html)的Class对象，它带有一个接受单个字符串参数（加载路径）的构造函数。默认为[Sass :: Importers :: Filesystem](http://sass-lang.com/documentation/Sass/Importers/Filesystem.html)。

＃sourcemap-option `:sourcemap`：控制如何生成源代码图。这些源代码告诉浏览器如何找到导致每个CSS样式生成的Sass样式。这有三个有效值：`**:auto**`***\*** **尽可能使用相对URI，假设源样式表将在您使用的任何服务器上可用，并且它们的相对位置将与本地文件系统上的相同。如果相对URI不可用，则使用“file：”URI。*** ***** `**:file**`***\*** **总是使用“file：”URI，这些URI将在本地工作，但不能部署到远程服务器。*** ***** `**:inline**`***\*** **包括源代码中的完整源代码文本，它是最大可移植的，但可以创建非常大的源代码文件。最后，*****`**:none**`根本不会生成源映射。

＃line_numbers-option `:line_numbers`：设置为true时，将定义选择器的行号和文件作为注释发送到已编译的CSS中。用于调试，特别是在使用导入和混入时。这个选项也可能被调用`:line_comments`。使用`:compressed`输出样式或`:debug_info`/ `:trace_selectors`选项时自动禁用。

＃trace_selectors-option `:trace_selectors`：设置为true时，会在每个选择器之前发出完整的导入和混合跟踪。这对于样式表导入和mixin包含的浏览器内调试很有帮助。该选项取代`:line_comments`选项并被选项取代`:debug_info`。使用`:compressed`输出样式时自动禁用。

＃debug_info-option `:debug_info`：设置为true时，会将定义选择器的行号和文件以浏览器可以理解的格式发送到编译后的CSS中。与[FireSass Firebug扩展](https://addons.mozilla.org/en-US/firefox/addon/103988)一起使用可显示Sass文件名和行号。使用`:compressed`输出样式时自动禁用。

＃custom-option `:custom`：这是一个选项，可供各个应用程序设置为使数据可用于自定义Sass功能。

＃quiet-option `:quiet`：设置为true时，会导致警告被禁用。

### 语法选择

Sass命令行工具将使用文件扩展名来确定您正在使用哪种语法，但并不总是有文件名。在`sass`命令行程序默认为缩进语法，但你可以通过该`--scss`选项，如果输入应该被解释为SCSS语法。或者，您可以使用与`scss`程序完全相同的命令行程序，`sass`但默认使用的语法是SCSS。

### 编码

在Ruby 1.9及更高版本上运行时，Sass知道文档的字符编码。Sass遵循[CSS规范](http://www.w3.org/TR/2013/WD-css-syntax-3-20130919/#determine-the-fallback-encoding)来确定样式表的编码，并回退到Ruby字符串编码。这意味着它首先检查Unicode字节顺序标记，然后检查`@charset`声明，然后检查Ruby字符串编码。如果这些都没有设置，它会认为文件是UTF-8。

要明确指定样式表的编码，`@charset`就像在CSS中一样使用声明。`@charset "encoding-name";`在样式表的开头添加（在任何空格或注释之前），Sass会将其解释为给定的编码。请注意，无论您使用哪种编码，都必须将其转换为Unicode。

Sass将始终将其输出编码为UTF-8。`@charset`当且仅当输出文件包含非ASCII字符时，它才会包含声明。在压缩模式下，使用UTF-8字节顺序标记代替`@charset`声明。

## CSS功能扩展

### 嵌套规则

Sass允许CSS规则互相嵌套。内部规则仅适用于外部规则的选择器内。例如：

```javascript
#main p {
  color: #00ff00;
  width: 97%;

  .redbox {
    background-color: #ff0000;
    color: #000000;
  }
}
```

被编译为：

```javascript
#main p {
  color: #00ff00;
  width: 97%; }
  #main p .redbox {
    background-color: #ff0000;
    color: #000000; }
```

这有助于避免父选择器的重复，并且使得使用大量嵌套选择器的复杂CSS布局更加简单。例如：

```javascript
#main {
  width: 97%;

  p, div {
    font-size: 2em;
    a { font-weight: bold; }
  }

  pre { font-size: 3em; }
}
```

被编译为：

```javascript
#main {
  width: 97%; }
  #main p, #main div {
    font-size: 2em; }
    #main p a, #main div a {
      font-weight: bold; }
  #main pre {
    font-size: 3em; }
```

### 引用父选择器：`&`＃parent-selector

有时候，使用嵌套规则的父选择器的方式与默认方式不同。例如，您可能希望在选择器悬停时或在body元素具有某个类时具有特殊样式。在这些情况下，您可以明确指定使用`&`字符插入父选择器的位置。例如：

```javascript
a {
  font-weight: bold;
  text-decoration: none;
  &:hover { text-decoration: underline; }
  body.firefox & { font-weight: normal; }
}
```

被编译为：

```javascript
a {
  font-weight: bold;
  text-decoration: none; }
  a:hover {
    text-decoration: underline; }
  body.firefox a {
    font-weight: normal; }
```

`&` will be replaced with the parent selector as it appears in the CSS. This means that if you have a deeply nested rule, the parent selector will be fully resolved before the `&` is replaced. For example:

```javascript
#main {
  color: black;
  a {
    font-weight: bold;
    &:hover { color: red; }
  }
}
```

被编译为：

```javascript
#main {
  color: black; }
  #main a {
    font-weight: bold; }
    #main a:hover {
      color: red; }
```

`&`必须出现在复合选择器的开头，但后面可能会加上一个后缀，该后缀将添加到父选择器中。例如：

```javascript
#main {
  color: black;
  &-sidebar { border: 1px solid; }
}
```

被编译为：

```javascript
#main {
  color: black; }
  #main-sidebar {
    border: 1px solid; }
```

如果父选择器不能应用后缀，Sass将抛出错误。

### 属性嵌套

CSS在“名称空间”中有很多属性。例如`font-family`，`font-size`和`font-weight`都在`font`命名空间。在CSS中，如果你想在同一个命名空间中设置一堆属性，你必须每次输入它。Sass为此提供了一个快捷方式：只需写一次名称空间，然后在其中嵌套每个子属性。例如：

```javascript
.funky {
  font: {
    family: fantasy;
    size: 30em;
    weight: bold;
  }
}
```

被编译为：

```javascript
.funky {
  font-family: fantasy;
  font-size: 30em;
  font-weight: bold; }
```

命名空间本身也可以有一个属性值。例如：

```javascript
.funky {
  font: 20px/24px fantasy {
    weight: bold;
  }
}
```

被编译为：

```javascript
.funky {
  font: 20px/24px fantasy;
    font-weight: bold;
}
```

### 占位符选择器： `%foo`

Sass 额外提供了一种特殊类型的选择器：占位符选择器 (placeholder selector)。与常用的 id 与 class 选择器写法相似，只是`#`或`.`替换成了`%`。必须通过@extend指令调用，更多介绍请查阅`@extend`-Only Selectors。



当占位符选择器单独使用时（未通过`@extend`调用），不会编译到 CSS 文件中。

## 注释：`/* */`和`//`#comments

Sass支持标准的多行CSS注释`/* */`，以及单行注释`//`。多行注释尽可能保留在CSS输出中，而单行注释则被删除。例如：

```javascript
/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */
body { color: black; }

// These comments are only one line long each.
// They won't appear in the CSS output,
// since they use the single-line comment syntax.
a { color: green; }
```

被编译为：

```javascript
/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */
body {
  color: black; }

a {
  color: green; }
```

将`!`作为多行注释的第一个字符表示在压缩输出模式下保留这条注释并输出到 CSS 文件中，通常用于添加版权信息。



由于多行注释成为结果CSS的一部分，因此它们中的插值已解决。例如：

```javascript
$version: "1.2.3";
/* This CSS is generated by My Snazzy Framework version #{$version}. */
```

被编译为：

```javascript
/* This CSS is generated by My Snazzy Framework version 1.2.3. */
```

## SassScript #sassscript

除了纯CSS属性语法外，Sass还支持一组名为SassScript的扩展。SassScript允许属性使用变量，算术和额外的函数。SassScript可以用于任何属性值。

SassScript也可用于生成选择器和属性名称，这在编写mixins时很有用。这是通过插值完成的。

### **Interactive Shell**

您可以使用interactiveshell轻松测试SassScript。要启动shell，请在命令行中输入sass -i。在提示符下，输入任何合法的SassScript表达式来测试它并为您打印结果：

```javascript
$ sass -i
>> "Hello, Sassy World!"
"Hello, Sassy World!"
>> 1px + 1px + 1px
3px
>> #777 + #777
#eeeeee
>> #777 + #888
white
```

### 变量：`$`#variables_

使用SassScript最普遍的方法是使用变量。变量以美元符号开头，复制方法与CSS属性属性的写法一样：

```javascript
$width: 5em;
```

然后你可以在属性中引用它们：

```javascript
#main {
  width: $width;
}
```

变量仅在定义它们的嵌套选择器级别内可用。如果它们在任何嵌套选择器之外定义，则它们在任何地方都可用。它们也可以用`!global`标志来定义，在这种情况下，它们也可以在任何地方使用。例如：

```javascript
#main {
  $width: 5em !global;
  width: $width;
}

#sidebar {
  width: $width;
}
```

被编译为：

```javascript
#main {
  width: 5em;
}

#sidebar {
  width: 5em;
}
```

由于历史原因，变量名称（以及所有其他Sass标识符）可以互换使用连字符和下划线。例如，如果您定义了一个名为`$main-width`的变量，则可以将`$main_width`作为访问权限，反之亦然。

### 数据类型

SassScript支持八种数据类型：

- 数字（例如`1.2`，`13`，`10px`）
- 文本字符串，使用和不使用引号（例如`"foo"`，`'bar'`，`baz`）
- 颜色（例如`blue`，`#04a3f9`，`rgba(255, 0, 0, 0.5)`）
- 布尔值（例如`true`，`false`）
- 空值（例如`null`）
- 值列表，由空格或逗号隔开（例如`1.5em 1em 0 2em`，`Helvetica, Arial, sans-serif`）
- 从一个值映射到另一个值（例如`(key1: value1, key2: value2)`）
- 函数引用

SassScript还支持所有其他类型的CSS属性值，例如Unicode字符集和`!important`声明。但是，这些类型没有特殊处理。它们就像未加引号的字符串一样对待。

#### 字符串＃sass-script-strings

CSS指定了两种字符串：带引号的字符串，例如`"Lucida Grande"，'http://sass-lang.com'`，以及不带引号的字符串，例如`sans-serif，bold`。SassScript可以识别这两种类型，并且一般情况下，如果在Sass文档中使用了一种字符串，那么将在结果CSS中使用这种字符串。

但有一个例外：使用`#{}`插值时，带引号的字符串不加引号。这使得mixin中引用选择器名称更容易。例如：

```javascript
@mixin firefox-message($selector) {
  body.firefox #{$selector}:before {
    content: "Hi, Firefox users!";
  }
}

@include firefox-message(".header");
```

被编译为：

```javascript
body.firefox .header:before {
  content: "Hi, Firefox users!"; }
```

数组

数组是Sass如何处理CSS声明的值，例如`margin: 10px 15px 0 0，font-face: Helvetica, Arial, sans-serif`。这样通过空格或者逗号分隔的一系列的值，事实上，独立的值也被视数组：只包含一个值得数组。

就其本身而言，数组并没有多大作用，但SassScript列表函数使它们很有用。该`nth`功能可以访问数组中的项目，该`join`功能可以将多个数组连接在一起，并且该`append`功能可以将项目添加到数组中。该`@each`指令还可以为数组中的每个项目添加样式。

数组中可以包含子数组，比如`1px 2px, 5px 6px`是包含`1px 2px`与`5px 6px`两个数组的数组。如果内外两层数组使用相同的分隔方式，需要用圆括号包裹内层，所以也可以写成`(1px 2px) (5px 6px)`。变化是，之前的`1px 2px, 5px 6px`使用逗号分割了两个子数组 (comma-separated)，而`(1px 2px) (5px 6px)`则使用空格分割(space-separated)。



当数组被编译为 CSS 时，Sass 不会添加任何圆括号（CSS 中没有这种写法），所以`(1px 2px) (5px 6px)`与`1px 2px, 5px 6px`在编译后的 CSS 文件中是完全一样的，但是它们在 Sass 文件中却有不同的意义，前者是包含两个数组的数组，而后者是包含四个值的数组。

用`()`表示不包含任何值的空数组（在 Sass 3.3 版之后也视为空的 map）。空数组不可以直接编译成 CSS，比如编译`font-family: ()`Sass 将会报错。如果数组中包含空数组或空值，编译时将被清除，比如`1px 2px () 3px`或`1px 2px null 3px`。



基于逗号分隔的数组允许保留结尾的逗号，这样做的意义是强调数组的结构关系，尤其是需要声明只包含单个值的数组时。例如`(1,)`表示只包含`1`的数组，而`(1 2 3,)`表示包含`1 2 3`这个以空格分隔的数组的数组。



##### 括号数组

数组也可以用方括号写 - 我们称这些括号数组。包含的括号数组在[CSS Grid Layout](https://www.w3.org/TR/css-grid-1/)中用作线名称，但它们也可以用于纯Sass代码，就像任何其他列表一样。括号列表可以用逗号或空格分隔。

#### Maps

Maps表示键和值之间的关联，其中键用于查找值。他们可以轻松地将值收集到命名组中并动态访问这些组。尽管它们在语法上类似于媒体查询表达式，但它们在CSS中没有直接的平行关系：

```javascript
$map: (key1: value1, key2: value2, key3: value3);
```

与Lists不同，Maps必须始终用括号括起来，并且必须始终用逗号分隔。Maps中的键和值都可以是任何SassScript对象。Maps可能只有一个与给定键相关的值（尽管该值可能是一个列表）。但是，给定的值可能与许多键相关联。

像Lists一样，Maps大多使用SassScript函数进行操作。该`map-get`函数在Maps中查找值，该`map-merge`函数将值添加到Maps。该`@each`指令可用于为Maps中的每个键/值对添加样式。Maps中对的顺序始终与创建Maps时的顺序相同。

Maps也可以在任何地方使用lists。当由列表函数使用时，Maps将被视为成对lists。例如，`(key1: value1, key2: value2)`将被`key1 value1, key2 value2 lists`函数视为嵌套lists。虽然lists不能被视为maps，但空lists除外。`()`代表没有键/值对的maps和没有元素的lists。

请注意，映射键可以是任何Sass数据类型（甚至是另一个映射），并且用于声明映射的语法允许任意SassScript表达式进行评估以确定键。

Maps无法转换为纯CSS。使用一个作为变量的值或CSS函数的参数会导致错误。使用该`inspect($value)`函数可生成对调试映射有用的输出字符串。

#### Colors

任何CSS颜色表达式都会返回一个SassScript颜色值。这包括[大量](https://github.com/nex3/sass/blob/stable/lib/sass/script/value/color.rb#L28-L180)与无引号字符串无法区分[的指定颜色](https://github.com/nex3/sass/blob/stable/lib/sass/script/value/color.rb#L28-L180)。

在压缩输出模式下，Sass将输出颜色的最小CSS表示。例如，`#FF0000`将以`red`压缩模式`blanchedalmond`输出，但会输出为`#FFEBCD`。

命名颜色是用户遇到的一个问题常见，由于Sass喜欢与在其他输出模式下输入相同的输出格式，因此插入到选择器中的颜色在压缩时变为无效语法。为了避免这种情况，如果它们是用来构造选择器的，那么总是引用命名的颜色。

#### **First Class Functions**

函数引用由返回`get-function($function-name)`。该函数可以传递给`call($function, $args...)`它所引用的函数。第一类函数不能直接用作CSS输出，任何尝试都会导致错误。

### Operations

所有类型都支持平等操作（`==`和`!=`）。另外，每种类型都有自己的操作，它有特别的支持。

#### Number Operations

SassScript支持对数字进行标准算术运算（加法`+`，减法`-`，乘法`*`，除法`/`和模`%`）。Sass数学函数在算术运算期间保留单位。这意味着，就像在现实生活中一样，你不能使用不相容单位的数字（例如用`px`和添加数字`em`）和乘以相同单位的两个数字将产生平方单位（`10px * 10px == 100px * px`）。**注意**这`px * px`是一个无效的CSS单元，你会从Sass得到一个错误，试图在CSS中使用无效单元。

关系运算符（`<`，`>`，`<=`，`>=`）也支持数字和相等运算符（`==`，`!=`），支持所有类型。

##### Division and `/`

\#division-and-slash

CSS允许`/`出现在属性值中作为分隔数字的一种方式。由于SassScript是CSS属性语法的扩展，它必须支持这一点，同时也允许`/`用于划分。这意味着默认情况下，如果两个数字`/`在SassScript 中分开，那么它们将以这种方式出现在结果CSS中。

但是，有三种情况`/`将会被解释为分割。这些涵盖绝大多数实际使用部门的情况。他们是：

1. 如果该值或其任何部分存储在变量中或由函数返回。
2. 如果值被括号括起来，除非这些括号在列表之外并且值在里面。
3. 如果该值用作另一个算术表达式的一部分。

例如：

```javascript
p {
  font: 10px/8px;             // Plain CSS, no division
  $width: 1000px;
  width: $width/2;            // Uses a variable, does division
  width: round(1.5)/2;        // Uses a function, does division
  height: (500px/2);          // Uses parentheses, does division
  margin-left: 5px + 8px/2px; // Uses +, does division
  font: (italic bold 10px/8px); // In a list, parentheses don't count
}
```

被编译为：

```javascript
p {
  font: 10px/8px;
  width: 500px;
  height: 250px;
  margin-left: 9px; }
```

如果你想和一个普通的CSS一起使用变量`/`，你可以使用它`#{}`来插入它们。例如：

```javascript
p {
  $font-size: 12px;
  $line-height: 30px;
  font: #{$font-size}/#{$line-height};
}
```

被编译为：

```javascript
p {
  font: 12px/30px; }
```

##### Subtraction, Negative Numbers, and `-`

\#subtraction

`-`CSS和Sass 中有许多不同的含义。它可以是减法运算符（如in `5px - 3px`），负数的开始（如in `-3px`），单目否定运算符（如in `-$var`）或标识符的一部分（如in `font-weight`）。大多数时候，很清楚哪个是哪个，但有一些棘手的情况。作为一般规则，在下列情况下你是最安全的：

- `-`减法时，您总是在两侧包含空格。
- `-`对于负数或单数否定，您之前包含空格但不包含空格。
- 如果它在空格分隔的列表中，那么在圆括号中包含一个一元否定，如in `10px (-$var)`。

`-`按以下顺序排列不同的含义：

1. 一个 `-`作为标识符的一部分。这意味着这`a-1`是一个带有值的未加引号的字符串`"a-1"`。唯一的例外是单位; Sass通常允许使用任何有效的标识符作为标识符，但标识符可能不包含连字符后跟数字。这意味着这`5px-3px`是一样的`5px - 3px`。
2. 一个`-`不带空格两个数字之间。这表示减法，所以与之`1-2`相同`1 - 2`。
3. 一个 `-`在文字数字的开头。这表示一个负数，所以`1 -2`是一个包含`1`和的列表`-2`。
4. 一个`-`不管空格的两个数字之间。这表示减法，所以与之`1 -$var`相同`1 - $var`。
5. 一个 `-`值前。这表明一元否定算子; 也就是说，接受一个数字并返回其负数的运算符。

#### Color Operations

所有的算术运算都支持颜色值，他们在那里分段工作。这意味着操作依次在红色，绿色和蓝色组件上执行。例如：

```javascript
p {
  color: #010203 + #040506;
}
```

计算`01 + 04 = 05`，`02 + 05 = 07`和`03 + 06 = 09`，并编译为：

```javascript
p {
  color: #050709; }
```

通常使用颜色函数比尝试使用颜色算法达到相同效果更有用。

算术运算也适用于数字和颜色之间，也是分段的。例如：

```javascript
p {
  color: #010203 * 2;
}
```

计算`01 * 2 = 02`，`02 * 2 = 04`和`03 * 2 = 06`，并编译为：

```javascript
p {
  color: #020406; }
```

请注意，具有alpha通道的颜色（使用rgba或hsla函数创建的颜色）必须具有相同的alpha值，以便使用它们进行颜色算术。算术不会影响阿尔法值。例如：

```javascript
p {
  color: rgba(255, 0, 0, 0.75) + rgba(0, 255, 0, 0.75);
}
```

被编译为：

```javascript
p {
  color: rgba(255, 255, 0, 0.75); }
```

可以使用opacify 和transparentize调整alpha channel的颜色，例如：

```javascript
$translucent-red: rgba(255, 0, 0, 0.5);
p {
  color: opacify($translucent-red, 0.3);
  background-color: transparentize($translucent-red, 0.25);
}
```

被编译为：

```javascript
p {
  color: rgba(255, 0, 0, 0.8);
  background-color: rgba(255, 0, 0, 0.25); }
```

IE过滤器要求所有颜色都包含alpha层，并且采用#AABBCCDD的严格格式。您可以使用ie_hex_str函数更轻松地转换颜色。例如：

```javascript
$translucent-red: rgba(255, 0, 0, 0.5);
$green: #00ff00;
div {
  filter: progid:DXImageTransform.Microsoft.gradient(enabled='false', startColorstr='#{ie-hex-str($green)}', endColorstr='#{ie-hex-str($translucent-red)}');
}
```

被编译为：

```javascript
div {
  filter: progid:DXImageTransform.Microsoft.gradient(enabled='false', startColorstr=#FF00FF00, endColorstr=#80FF0000);
}
```

#### 字符串运算

`+`操作可用于连接字符串：

```javascript
p {
  cursor: e + -resize;
}
```

被编译为：

```javascript
p {
  cursor: e-resize; }
```

请注意，如果将带引号的字符串添加到未加引号的字符串（即引号字符串位于该字符串的左侧`+`），则结果为带引号的字符串。同样，如果将未加引号的字符串添加到带引号的字符串（未加引号的字符串位于该字符串的左侧`+`），则结果为未加引号的字符串。例如：

```javascript
p:before {
  content: "Foo " + Bar;
  font-family: sans- + "serif";
}
```

被编译为：

```javascript
p:before {
  content: "Foo Bar";
  font-family: sans-serif; }
```

默认情况下，如果两个值彼此相邻放置，则它们与空格连接在一起：

```javascript
p {
  margin: 3px + 4px auto;
}
```

被编译为：

```javascript
p {
  margin: 7px auto; }
```

在一串文本中，可以使用＃{}样式插值在字符串中放置动态值：

```javascript
p:before {
  content: "I ate #{5 + 10} pies!";
}
```

被编译为：

```javascript
p:before {
  content: "I ate 15 pies!"; }
```

空值被视为字符串插值的空字符串：

```javascript
$value: null;
p:before {
  content: "I ate #{$value} pies!";
}
```

被编译为：

```javascript
p:before {
  content: "I ate  pies!"; }
```

#### 布尔运算

SassScript支持`and`，`or`以及`not`为布尔值运算符。

#### 数组运算

数组不支持任何特殊操作。相反，他们使用数组函数进行操作。

### 圆括号

圆括号可以用来影响运算的顺序：

```javascript
p {
  width: 1em + (2em * 3);
}
```

被编译为：

```javascript
p {
  width: 7em; }
```

### 函数

SassScript定义了一些使用普通CSS函数语法调用的有用函数：

```javascript
p {
  color: hsl(0, 100%, 50%);
}
```

被编译为：

```javascript
p {
  color: #ff0000; }
```

请参阅此页面以获取可用功能的完整列表。

#### 关键词参数

Sass函数也可以使用明确的关键词参数来调用。上面的例子也可以写成：

```javascript
p {
  color: hsl($hue: 0, $saturation: 100%, $lightness: 50%);
}
```

虽然这不太简洁，但它可以使样式表更易于阅读。它还允许函数呈现更灵活的接口，提供许多参数而不会变得很难调用。

命名参数可以以任何顺序传递，并且可以省略具有默认值的参数。由于命名参数是变量名称，下划线和破折号可以互换使用。

有关Sass函数及其参数名称的完整列表，请参阅Sass :: Script :: Functions，以及有关在Ruby中定义自己的说明。

### 插值： `#{}`

\#interpolation_

您还可以使用`#{}`插值语法在选择器和属性名称中使用SassScript变量：

```javascript
$name: foo;
$attr: border;
p.#{$name} {
  #{$attr}-color: blue;
}
```

被编译为：

```javascript
p.foo {
  border-color: blue; }
```

也可以使用`#{}`将SassScript放入属性值中。在大多数情况下，这并不比使用变量更好，但使用`#{}`意味着它附近的任何操作都将被视为纯CSS。例如：

```javascript
p {
  $font-size: 12px;
  $line-height: 30px;
  font: #{$font-size}/#{$line-height};
}
```

被编译为：

```javascript
p {
  font: 12px/30px; }
```

### `&` in SassScript #parent-script

就像它在选择器`&`中使用时一样，SassScript引用当前的父选择器。它是以空格分隔的列表的逗号分隔列表。例如：

```javascript
.foo.bar .baz.bang, .bip.qux {
  $selector: &;
}
```

`$selector`的价值是`((".foo.bar" ".baz.bang"), ".bip.qux")`。复合选择器在这里被引用来表示它们是字符串，但实际上它们不会被引用。即使父选择器不包含逗号或空格，`&`也总是会有两层嵌套，因此可以一致地访问它。

如果没有父选择器，则值`&`将为空。这意味着你可以在mixin中使用它来检测父选择器是否存在：

```javascript
@mixin does-parent-exist {
  @if & {
    &:hover {
      color: red;
    }
  } @else {
    a {
      color: red;
    }
  }
}
```

### 变量定义： `!default`

如果尚未通过将`!default`标志添加到值的末尾来分配变量，则可以分配给变量。这意味着如果变量已经被赋值，它将不会被重新赋值，但是如果它还没有赋值，它将会被赋值。

例如：

```javascript
$content: "First content";
$content: "Second content?" !default;
$new_content: "First time reference" !default;

#main {
  content: $content;
  new-content: $new_content;
}
```

被编译为：

```javascript
#main {
  content: "First content";
  new-content: "First time reference"; }
```

具有`null`值的变量被视为未分配！default：

```javascript
$content: null;
$content: "Non-null content" !default;

#main {
  content: $content;
}
```

被编译为：

```javascript
#main {
  content: "Non-null content"; }
```

## `@`-Rules与指令#directives

Sass支持所有的CSS3 `@`规则，以及一些额外的Sass特有的“指令”。这些在Sass中有不同的效果，详见下文。另请参阅控制指令和mixin指令。

### `@import` #import

Sass扩展了CSS `@import`规则，允许它导入SCSS和Sass文件。所有导入的SCSS和Sass文件将合并成一个CSS输出文件。另外，在主文件中可以使用在导入文件中定义的任何变量或mixin。

Sass在当前目录中查找其他Sass文件，在Rack，Rails或Merb下查找Sass文件目录。可以使用该`:load_paths`选项或`--load-path`命令行上的选项指定其他搜索目录。

`@import`需要一个文件名来导入。默认情况下，它会直接查找Sass文件以进行导入，但在某些情况下，它将编译为CSS `@import`规则：

- 如果文件的扩展名是`.css`。
- 如果文件名以`http://`。开头。
- 如果文件名是`url()`。
- 如果`@import`有任何媒体查询。

如果上述条件都不符合且扩展名为`.scss`或`.sass`，则将导入命名的Sass或SCSS文件。如果没有扩展名，萨斯将尝试找到具有该名称的文件和`.scss`或`.sass`延伸和导入。

例如，

```javascript
@import "foo.scss";
```

或者

```javascript
@import "foo";
```

会同时导入文件`foo.scss`，而

```javascript
@import "foo.css";
@import "foo" screen;
@import "http://foo.com/bar";
@import url(foo);
```

都会编译成

```javascript
@import "foo.css";
@import "foo" screen;
@import "http://foo.com/bar";
@import url(foo);
```

也可以一次导入多个文件`@import`。例如：

```javascript
@import "rounded-corners", "text-shadow";
```

会导入文件`rounded-corners`和`text-shadow`文件。

导入可能包含`#{}`插值，但仅限于某些限制。无法根据变量动态导入Sass文件; 插值仅适用于CSS导入。因此，它只适用于`url()`进口。例如：

```javascript
$family: unquote("Droid+Sans");
@import url("http://fonts.googleapis.com/css?family=#{$family}");
```

将编译为

```javascript
@import url("http://fonts.googleapis.com/css?family=Droid+Sans");
```

#### Partials #partials

如果您想要导入但不想编译为CSS文件的SCSS或Sass文件，则可以在文件名的开头添加下划线。这将告诉Sass不要将其编译为普通的CSS文件。然后您可以导入这些文件而不使用下划线。

例如，你可能有`_colors.scss`。然后不想生成编译文件`_colors.css`，并且您可以执行此操作

```javascript
@import "colors";
```

并且`_colors.scss`将被导入。

请注意，您可能不会在同一个目录中包含具有相同名称的部分和非部分。例如，`_colors.scss`可能并不存在`colors.scss`。

#### 嵌套`@import`＃嵌套导入

虽然大多数情况下只`@import`在文档的顶层有s，但可以将它们包含在CSS规则和`@media`规则中。像基本级别一样`@import`，这包括`@import`ed文件的内容。但是，导入的规则将与原始文件嵌套在相同的位置`@import`。

例如，如果`example.scss`包含

```javascript
.example {
  color: red;
}
```

然后

```javascript
#main {
  @import "example";
}
```

将编译为

```javascript
#main .example {
  color: red;
}
```

仅在文档的基本级别允许的指令（例如`@mixin`或`@charset`）不能`@import`在嵌套上下文中编辑的文件中使用。

在mixin或控制指令中嵌套`@import`是不可能的。

### `@media` #media

`@media`Sass中的指令就像它们在纯CSS中一样，具有一个额外的功能：它们可以嵌套在CSS规则中。如果`@media`指令出现在CSS规则中，它将冒泡到样式表的顶层，将所有选择器放在规则内。这样可以轻松添加媒体特定的样式，而无需重复选择器或中断样式表的流程。例如：

```javascript
.sidebar {
  width: 300px;
  @media screen and (orientation: landscape) {
    width: 500px;
  }
}
```

被编译为：

```javascript
.sidebar {
  width: 300px; }
  @media screen and (orientation: landscape) {
    .sidebar {
      width: 500px; } }
```

`@media`查询也可以嵌套在另一个中。然后查询将使用`and`操作员进行组合。例如：

```javascript
@media screen {
  .sidebar {
    @media (orientation: landscape) {
      width: 500px;
    }
  }
}
```

被编译为：

```javascript
@media screen and (orientation: landscape) {
  .sidebar {
    width: 500px; } }
```

最后，`@media`查询可以包含SassScript表达式（包括变量，函数和运算符）来替代功能名称和特征值。例如：

```javascript
$media: screen;
$feature: -webkit-min-device-pixel-ratio;
$value: 1.5;

@media #{$media} and ($feature: $value) {
  .sidebar {
    width: 500px;
  }
}
```

被编译为：

```javascript
@media screen and (-webkit-min-device-pixel-ratio: 1.5) {
  .sidebar {
    width: 500px; } }
```

### `@extend` #extend

当一个类应该具有另一个类的所有样式以及它自己的特定样式时，经常会有设计页面的情况。处理这种情况的最常见方式是在HTML中使用更一般的类和更具体的类。例如，假设我们设计了一个正常的错误，并且还有一个严重的错误。我们可能会这样写我们的标记：

```javascript
<div class="error seriousError">
  Oh no! You've been hacked!
</div>
```

我们的风格如下：

```javascript
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError {
  border-width: 3px;
}
```

不幸的是，这意味着我们必须永远记住使用`.seriousErro`时需要参考.`error`的样式。这是一个维护负担，会导致棘手的错误，并且可能会将非语义风格问题带入标记。

`@extend`通过告诉Sass一个选择器应该继承另一个选择器的样式，该指令避免了这些问题。例如：

```javascript
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```

被编译为：

```javascript
.error, .seriousError {
  border: 1px #f00;
  background-color: #fdd;
}

.seriousError {
  border-width: 3px;
}
```

这意味着除了特定的样式之外，所有为其定义的样式`.error`也适用`.seriousError`于该样式`.seriousError`。实际上，每个有类的元素`.seriousError`也都有类`.error`。

使用其他规则`.error`会为工作`.seriousError`。例如，如果我们对黑客造成的错误有特殊的风格：

```javascript
.error.intrusion {
  background-image: url("/image/hacked.png");
}
```

然后`<div class="seriousError intrusion">`将有`hacked.png`背景图像。

#### How it Works

`@extend`的作用是将重复使用的样式 (`.error`) 延伸 (extend) 给需要包含这个样式的特殊样式（`.seriousError`），刚刚的例子

```javascript
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.error.intrusion {
  background-image: url("/image/hacked.png");
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```

被编译为：

```javascript
.error, .seriousError {
  border: 1px #f00;
  background-color: #fdd; }

.error.intrusion, .seriousError.intrusion {
  background-image: url("/image/hacked.png"); }

.seriousError {
  border-width: 3px; }
```

合并选择器时，`@extend`足够聪明以避免不必要的重复，因此`.seriousError.seriousError`可以将其转换为`.seriousError`。另外，它不会产生无法匹配任何内容的选择器，比如`#main#footer`。

#### 扩展复杂的选择器

类选择器不是唯一可以扩展的东西。这是可能的延伸仅涉及单个元件，例如任何选择`.special.cool`，`a:hover`或`a.user[href^="http://"]`。例如：

```javascript
.hoverlink {
  @extend a:hover;
}
```

就像类一样，这意味着定义的所有样式`a:hover`也适用于`.hoverlink`。例如：

```javascript
.hoverlink {
  @extend a:hover;
}
a:hover {
  text-decoration: underline;
}
```

被编译为：

```javascript
a:hover, .hoverlink {
  text-decoration: underline; }
```

就像`.error.intrusion`上面一样，即使他们也有其他选择器，任何使用的规则`a:hover`也适用`.hoverlink`。例如：

```javascript
.hoverlink {
  @extend a:hover;
}
.comment a.user:hover {
  font-weight: bold;
}
```

被编译为：

```javascript
.comment a.user:hover, .comment .user.hoverlink {
  font-weight: bold; }
```

#### 多重扩展

一个选择器可以扩展多个选择器。这意味着它继承了所有扩展选择器的样式。例如：

```javascript
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.attention {
  font-size: 3em;
  background-color: #ff0;
}
.seriousError {
  @extend .error;
  @extend .attention;
  border-width: 3px;
}
```

被编译为：

```javascript
.error, .seriousError {
  border: 1px #f00;
  background-color: #fdd; }

.attention, .seriousError {
  font-size: 3em;
  background-color: #ff0; }

.seriousError {
  border-width: 3px; }
```

实际上，每个具有类的元素`.seriousError`也具有类`.error` *和*类`.attention`。因此，稍后在文档中定义的样式优先：`.seriousError`具有背景颜色`#ff0`而不是`#fdd`，因为`.attention`定义在`.error`后面。

多重扩展也可以使用逗号分隔的选择器列表编写。例如，`@extend .error, .attention`和`@extend .error; @extend .attention`。一样。

#### 链接延伸

一个选择器可以扩展另一个选择器，而后者又扩展了三分之一。例如：

```javascript
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
.criticalError {
  @extend .seriousError;
  position: fixed;
  top: 10%;
  bottom: 10%;
  left: 10%;
  right: 10%;
}
```

现在一切都与类`.seriousError`也有阶级`.error`，一切都与类`.criticalError`具有类`.seriousError` *和*类`.error`。它被编译为：

```javascript
.error, .seriousError, .criticalError {
  border: 1px #f00;
  background-color: #fdd; }

.seriousError, .criticalError {
  border-width: 3px; }

.criticalError {
  position: fixed;
  top: 10%;
  bottom: 10%;
  left: 10%;
  right: 10%; }
```

#### 选择器序列

选择器序列，例如`.foo .bar`或`.foo + .bar`当前不能被扩展。但是，嵌套选择器本身也可以使用`@extend`。例如：

```javascript
#fake-links .link {
  @extend a;
}

a {
  color: blue;
  &:hover {
    text-decoration: underline;
  }
}
```

被编译为

```javascript
a, #fake-links .link {
  color: blue; }
  a:hover, #fake-links .link:hover {
    text-decoration: underline; }
```

##### 合并选择器序列

有时选择器序列会扩展以另一个序列出现的另一个选择器。在这种情况下，这两个序列需要合并。例如：

```javascript
#admin .tabbar a {
  font-weight: bold;
}
#demo .overview .fakelink {
  @extend a;
}
```

虽然在技术上可能会生成所有可能匹配任何序列的选择器，但这会使样式表过大。例如，上面的简单例子就需要十个选择器。相反，Sass只生成可能有用的选择器。

当两个被合并的序列没有共同的选择器时，就会生成两个新的选择器：一个在第二个之前，第一个在第一个之前，第二个在第一个之前。例如：

```javascript
#admin .tabbar a {
  font-weight: bold;
}
#demo .overview .fakelink {
  @extend a;
}
```

被编译为：

```javascript
#admin .tabbar a,
#admin .tabbar #demo .overview .fakelink,
#demo .overview #admin .tabbar .fakelink {
  font-weight: bold; }
```

如果两个序列确实共享某些选择器，那么这些选择器将合并在一起，只有差异（如果存在的话）会交替出现。在这个例子中，两个序列都包含id `#admin`，所以生成的选择器将合并这两个id：

```javascript
#admin .tabbar a {
  font-weight: bold;
}
#admin .overview .fakelink {
  @extend a;
}
```

这被编译为：

```javascript
#admin .tabbar a,
#admin .tabbar .overview .fakelink,
#admin .overview .tabbar .fakelink {
  font-weight: bold; }
```

#### `@extend`-Only Selectors #placeholders

有时，需要定义一套样式并不是给某个元素用，而是只通过`@extend`指令使用，尤其是在制作 Sass 样式库的时候，希望 Sass 能够忽略用不到的样式。



如果为此使用普通的类，则最终会在生成样式表时创建大量额外的CSS，并且有可能与HTML中正在使用的其他类发生冲突。这就是为什么Sass支持“占位符选择器”（例如`%foo`）。

占位符选择器看起来像class和id选择器，除了`#`or `.`被替换`%`。它们可以用于任何类或id的任何地方，并且它们可以防止规则集呈现给CSS。例如：

```javascript
// This ruleset won't be rendered on its own.
#context a%extreme {
  color: blue;
  font-weight: bold;
  font-size: 2em;
}
```

但是，可以扩展占位符选择器，就像类和ID一样。扩展选择器将生成，但基本占位符选择器不会被编译。例如：

```javascript
.notice {
  @extend %extreme;
}
```

编译为：

```javascript
#context a.notice {
  color: blue;
  font-weight: bold;
  font-size: 2em; }
```

#### `!optiona声明`

如果`@extend`失败会收到错误提示，比如，这样写`a.important {@extend .notice}`，当没有`.notice`选择器时，将会报错，只有`h1.notice`包含`.notice`时也会报错，因为`h1`与`a`冲突，会生成新的选择器。



但有时候，你想允许`@extend`不生成任何新的选择器。为此，只需`!optional`在选择器后面添加标志即可。例如：

```javascript
a.important {
  @extend .notice !optional;
}
```

#### `@extend` 在指令中延伸

使用`@extend`指令之类的内容有一些限制，例如`@media`。Sass无法将CSS以外的规则`@media`应用于其内部的选择器，而无需通过在整个位置复制样式来创建大量样式表膨胀。这意味着如果您`@extend`在`@media`（或其他CSS指令）中使用，则只能扩展出现在同一个指令块中的选择器。

例如，以下工作正常：

```javascript
@media print {
  .error {
    border: 1px #f00;
    background-color: #fdd;
  }
  .seriousError {
    @extend .error;
    border-width: 3px;
  }
}
```

但是这是一个错误：

```javascript
.error {
  border: 1px #f00;
  background-color: #fdd;
}

@media print {
  .seriousError {
    // INVALID EXTEND: .error is used outside of the "@media print" directive
    @extend .error;
    border-width: 3px;
  }
}
```

有一天，我们希望`@extend`在浏览器中本地支持，这将允许它在`@media`其他指令中使用。

### `@at-root` #at-root

该`@at-root`指令导致一个或多个规则发送到文档的根目录，而不是嵌套在它们的父选择器之下。它可以与单个内联选择器一起使用：

```javascript
.parent {
  ...
  @at-root .child { ... }
}
```

这会产生：

```javascript
.parent { ... }
.child { ... }
```

或者它可以与包含多个选择器的块一起使用：

```javascript
.parent {
  ...
  @at-root {
    .child1 { ... }
    .child2 { ... }
  }
  .step-child { ... }
}
```

这将输出以下内容：

```javascript
.parent { ... }
.child1 { ... }
.child2 { ... }
.parent .step-child { ... }
```

#### `@at-root (without: ...)` and `@at-root (with: ...)`

默认情况下，`@at-root`只是排除选择器。但是，也可以使用`@at-root`移动到嵌套指令之外的方式`@media`。例如：

```javascript
@media print {
  .page {
    width: 8in;
    @at-root (without: media) {
      color: red;
    }
  }
}
```

生成：

```javascript
@media print {
  .page {
    width: 8in;
  }
}
.page {
  color: red;
}
```

您可以使用`@at-root (without: ...)`移动到任何指令之外。您也可以使用多个由空格分隔的指令来执行此操作：`@at-root (without: media supports)`在两者之外移动`@media`和`@supports`查询。

有两个特殊值可以传递给`@at-root`。“规则”是指正常的CSS规则; 与没有查询`@at-root (without: rule)`相同`@at-root`。`@at-root (without: all)`意味着样式应该移到*所有*指令和CSS规则之外。

如果您想指定要包含哪些指令或规则，而不是列出应排除的指令或规则，则可以使用`with`而不是`without`。例如，`@at-root (with: rule)`将移到所有指令之外，但会保留任何CSS规则。

### `@debug`

该`@debug`指令将SassScript表达式的值打印到标准错误输出流。这对于调试有复杂SassScript的Sass文件很有用。例如：

```javascript
@debug 10em + 12em;
```

输出：

```javascript
Line 1 DEBUG: 22em
```

### `@warn`

该`@warn`指令将SassScript表达式的值打印到标准错误输出流。对于需要警告用户弃用或从小混音使用错误中恢复的库，这非常有用。`@warn`和`@debug`之间有两大区别：

1. 您可以使用`--quiet`命令行选项或`:quiet`Sass选项关闭警告。
2. 样式表跟踪将与消息一起打印出来，以便被警告的用户可以看到他们的样式引起警告的位置。

用法示例：

```javascript
@mixin adjust-location($x, $y) {
  @if unitless($x) {
    @warn "Assuming #{$x} to be in pixels";
    $x: 1px * $x;
  }
  @if unitless($y) {
    @warn "Assuming #{$y} to be in pixels";
    $y: 1px * $y;
  }
  position: relative; left: $x; top: $y;
}
```

### `@error`

该`@error`指令将引发一个SassScript表达式的值为致命错误，包括一个不错的堆栈跟踪。这对验证mixin和函数的参数很有用。例如：

```javascript
@mixin adjust-location($x, $y) {
  @if unitless($x) {
    @error "$x may not be unitless, was #{$x}.";
  }
  @if unitless($y) {
    @error "$y may not be unitless, was #{$y}.";
  }
  position: relative; left: $x; top: $y;
}
```

目前没有办法发现错误。

## 控制指令和表达

SassScript支持基本的控制指令和表达式，仅用于在某些情况下包含样式，或者在变体中多次包含相同样式。

**注意：**控制指令是一项高级功能，在日常样式中不常见。它们主要用于mixin，尤其是像[Compass](http://compass-style.org/)这样的图书馆的一部分，因此需要相当大的灵活性。

### `if()`

内置`if()`函数允许您在条件上进行分支并仅返回两种可能结果中的一种。它可以在任何脚本上下文中使用。该`if`函数仅评估与它将返回的参数相对应的参数 - 这允许您引用可能未定义的变量或进行计算，否则会导致错误（例如，除以零）。

```javascript
if(true, 1px, 2px) => 1px
if(false, 1px, 2px) => 2px
```

### `@if`

该`@if`指令采用SassScript表达式，并在表达式返回除`false`or 之外的任何内容时使用嵌套在其下的样式`null`：

```javascript
p {
  @if 1 + 1 == 2 { border: 1px solid;  }
  @if 5 < 3      { border: 2px dotted; }
  @if null       { border: 3px double; }
}
```

被编译为：

```javascript
p {
  border: 1px solid; }
```

您可以明确地测试`$var == false`或者`$var == null`想要区分这些。

该`@if`声明可以由几个`@else if`语句，一个`@else`声明。如果`@if`陈述失败，`@else if`则按顺序尝试陈述，直到成功或`@else`达到陈述。例如：

```javascript
$type: monster;
p {
  @if $type == ocean {
    color: blue;
  } @else if $type == matador {
    color: red;
  } @else if $type == monster {
    color: green;
  } @else {
    color: black;
  }
}
```

被编译为：

```javascript
p {
  color: green; }
```

### `@for`

该`@for`指令重复输出一组样式。对于每次重复，计数器变量用于调整输出。该指令有两种形式：`@for $var from <start> through <end>`和`@for $var from <start> to <end>`。请注意关键字`through`和区别`to`。`$var`可以是任何变量名称，比如`$i`; `<start>`并且`<end>`是应该返回整数的SassScript表达式。何时`<start>`大于`<end>`计数器将递减而不是递增。

该`@for`语句设置`$var`为指定范围内的每个连续数字，并且每次使用该值输出嵌套样式`$var`。对于具有以下形式`from ... through`，所述范围*包括*的值`<start>`和`<end>`，但形式`from ... to`运行至*但不包括*的值`<end>`。使用`through`语法，

```javascript
@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}
```

被编译为：

```javascript
.item-1 {
  width: 2em; }
.item-2 {
  width: 4em; }
.item-3 {
  width: 6em; }
```

### `@each` #each-directive

`@each`指令的格式是`$var in <list>`,`$var`可以是任何变量名，比如`$length`或者`$name`，而`<list>`是一连串的值，也就是值列表。



`@each`将变量`$var`作用于值列表中的每一个项目，然后输出结果，例如：



```javascript
@each $animal in puma, sea-slug, egret, salamander {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}
```

被编译为：

```javascript
.puma-icon {
  background-image: url('/images/puma.png'); }
.sea-slug-icon {
  background-image: url('/images/sea-slug.png'); }
.egret-icon {
  background-image: url('/images/egret.png'); }
.salamander-icon {
  background-image: url('/images/salamander.png'); }
```

#### Multiple Assignment #each-multi-assign

该`@each`指令也可以使用多个变量，如in `@each $var1, $var2, ... in <list>`。如果`<list>`是列表列表，则将子列表的每个元素分配给相应的变量。例如：

```javascript
@each $animal, $color, $cursor in (puma, black, default),
                                  (sea-slug, blue, pointer),
                                  (egret, white, move) {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
    border: 2px solid $color;
    cursor: $cursor;
  }
}
```

被编译为：

```javascript
.puma-icon {
  background-image: url('/images/puma.png');
  border: 2px solid black;
  cursor: default; }
.sea-slug-icon {
  background-image: url('/images/sea-slug.png');
  border: 2px solid blue;
  cursor: pointer; }
.egret-icon {
  background-image: url('/images/egret.png');
  border: 2px solid white;
  cursor: move; }
```

由于Maps被视为成对lists，因此多个作业也适用于它们。例如：

```javascript
@each $header, $size in (h1: 2em, h2: 1.5em, h3: 1.2em) {
  #{$header} {
    font-size: $size;
  }
}
```

被编译为：

```javascript
h1 {
  font-size: 2em; }
h2 {
  font-size: 1.5em; }
h3 {
  font-size: 1.2em; }
```

### `@while`

该`@while`指令采用SassScript表达式并重复输出嵌套样式，直到语句求值为止`false`。这可以用来实现比`@for`语句更复杂的循环，尽管这很少是必需的。例如：

```javascript
$i: 6;
@while $i > 0 {
  .item-#{$i} { width: 2em * $i; }
  $i: $i - 2;
}
```

被编译为：

```javascript
.item-6 {
  width: 12em; }

.item-4 {
  width: 8em; }

.item-2 {
  width: 4em; }
```

## 混合指令#mixins

Mixins允许您定义可以在整个样式表中重用的样式，而无需使用类似的非语义类`.float-left`。Mixins还可以包含完整的CSS规则，以及Sass文档中其他位置允许的其他内容。他们甚至可以拿出一些参数，让你用很少的mixin产生各种各样的风格。

### 定义混合指令: `@mixin` #defining_a_mixin

Mixins由`@mixin`指令定义。它后面跟着mixin的名称和可选的参数，以及一个包含mixin内容的块。例如，`large-text`mixin定义如下：

```javascript
@mixin large-text {
  font: {
    family: Arial;
    size: 20px;
    weight: bold;
  }
  color: #ff0000;
}
```

Mixins也可能包含选择器，可能与属性混合在一起。选择器甚至可以包含父引用。例如：

```javascript
@mixin clearfix {
  display: inline-block;
  &:after {
    content: ".";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
  }
  * html & { height: 1px }
}
```

由于历史原因，mixin名称（以及所有其他Sass标识符）可以互换使用连字符和下划线。例如，如果你定义了一个名为mixin的对象`add-column`，你可以将它包含为`add_column`，反之亦然。

### 引用混合样式: `@include` #including_a_mixin

Mixins包含在`@include`指令文档中。这需要一个mixin的名称和可选的参数传递给它，并将该mixin定义的样式包含到当前规则中。例如：

```javascript
.page-title {
  @include large-text;
  padding: 4px;
  margin-top: 10px;
}
```

被编译为：

```javascript
.page-title {
  font-family: Arial;
  font-size: 20px;
  font-weight: bold;
  color: #ff0000;
  padding: 4px;
  margin-top: 10px; }
```

只要不直接定义任何属性或使用任何父引用，Mixin也可以包含在任何规则之外（即，位于文档的根目录）。例如：

```javascript
@mixin silly-links {
  a {
    color: blue;
    background-color: red;
  }
}

@include silly-links;
```

被编译为：

```javascript
a {
  color: blue;
  background-color: red; }
```

Mixin定义还可以包含其他mixin。例如：

```javascript
@mixin compound {
  @include highlighted-background;
  @include header-text;
}

@mixin highlighted-background { background-color: #fc0; }
@mixin header-text { font-size: 20px; }
```

Mixins可能包含它们自己。这与3.3之前的Sass版本的行为不同，在该行为中，mixin递归被禁止。

只能定义后代选择器的Mixin可以安全地混合到文档的最顶层。

### 参数＃mixin-arguments

Mixin可以将SassScript值作为参数，当mixin被包含并在mixin中作为变量提供时，会给出这些参数。

在定义一个mixin时，参数被写为变量名称，用逗号分隔，所有变量名称后面的括号中。然后，在包含mixin时，可以以相同的方式传入值。例如：

```javascript
@mixin sexy-border($color, $width) {
  border: {
    color: $color;
    width: $width;
    style: dashed;
  }
}

p { @include sexy-border(blue, 1in); }
```

被编译为：

```javascript
p {
  border-color: blue;
  border-width: 1in;
  border-style: dashed; }
```

Mixins还可以使用普通的变量设置语法为参数指定默认值。然后，当包含mixin时，如果它没有传入该参数，则将使用默认值。例如：

```javascript
@mixin sexy-border($color, $width: 1in) {
  border: {
    color: $color;
    width: $width;
    style: dashed;
  }
}
p { @include sexy-border(blue); }
h1 { @include sexy-border(blue, 2in); }
```

被编译为：

```javascript
p {
  border-color: blue;
  border-width: 1in;
  border-style: dashed; }

h1 {
  border-color: blue;
  border-width: 2in;
  border-style: dashed; }
```

#### 关键字参数

Mixins也可以使用明确的关键字参数来包含。例如，上面的例子可以写成：

```javascript
p { @include sexy-border($color: blue); }
h1 { @include sexy-border($color: blue, $width: 2in); }
```

虽然这不太简洁，但它可以使样式表更易于阅读。它还允许函数呈现更灵活的接口，提供许多参数而不会变得很难调用。

命名参数可以以任何顺序传递，并且可以省略具有默认值的参数。由于命名参数是变量名称，下划线和破折号可以互换使用。

#### 尾随逗号

当mixin或函数的最后一个参数是位置或关键字式参数时，该参数后面可以跟随一个逗号。有些人更喜欢这种编码风格，因为它可以在重构时导致更简洁的差异和更少的语法错误。

#### 变量参数

有时候mixin或者函数需要一些未知数的参数是有意义的。例如，用于创建框阴影的混合可能会将任意数量的阴影作为参数。对于这些情况，Sass支持“可变参数”，这些参数是混合函数或函数声明结尾的参数，它们将所有剩余的参数作为列表打包。这些论点看起来就像正常的论点，但后面跟着`...`。例如：

```javascript
@mixin box-shadow($shadows...) {
  -moz-box-shadow: $shadows;
  -webkit-box-shadow: $shadows;
  box-shadow: $shadows;
}

.shadows {
  @include box-shadow(0px 4px 5px #666, 2px 6px 10px #999);
}
```

被编译为：

```javascript
.shadows {
  -moz-box-shadow: 0px 4px 5px #666, 2px 6px 10px #999;
  -webkit-box-shadow: 0px 4px 5px #666, 2px 6px 10px #999;
  box-shadow: 0px 4px 5px #666, 2px 6px 10px #999;
}
```

变量参数还包含传递给mixin或函数的任何关键字参数。可以使用`keywords($args)`函数来访问这些函数，该函数将它们作为从字符串（不含`$`）到值的映射返回。

调用mixin时也可以使用变量参数。使用相同的语法，可以展开值列表，以便将每个值作为单独的参数传递，或展开值的映射，以便将每个值视为关键字参数。例如：

```javascript
@mixin colors($text, $background, $border) {
  color: $text;
  background-color: $background;
  border-color: $border;
}

$values: #ff0000, #00ff00, #0000ff;
.primary {
  @include colors($values...);
}

$value-map: (text: #00ff00, background: #0000ff, border: #ff0000);
.secondary {
  @include colors($value-map...);
}
```

被编译为：

```javascript
.primary {
  color: #ff0000;
  background-color: #00ff00;
  border-color: #0000ff;
}

.secondary {
  color: #00ff00;
  background-color: #0000ff;
  border-color: #ff0000;
}
```

只要lists出现在Maps之前，您就可以传递参数lists和，maps，如同`@include colors($values..., $map...)`。

您可以使用可变参数来封装混音并添加其他样式，而无需更改mixin的参数签名。如果你这样做，关键字参数将直接传递给包装的mixin。例如：

```javascript
@mixin wrapped-stylish-mixin($args...) {
  font-weight: bold;
  @include stylish-mixin($args...);
}

.stylish {
  // The $width argument will get passed on to "stylish-mixin" as a keyword
  @include wrapped-stylish-mixin(#00ff00, $width: 100px);
}
```

### 将内容块传递给Mixin＃mixin-content

可以将一段样式传递给mixin，以便在mixin包含的样式中进行放置。样式将显示在`@content`mixin中找到的任何指令的位置。这使得可以定义与选择器和指令构造有关的抽象。

例如：

```javascript
@mixin apply-to-ie6-only {
  * html {
    @content;
  }
}
@include apply-to-ie6-only {
  #logo {
    background-image: url(/logo.gif);
  }
}
```

产生：

```javascript
* html #logo {
  background-image: url(/logo.gif);
}
```

`.sass`简写语法中可以使用相同的mixins ：

```javascript
=apply-to-ie6-only
  * html
    @content

+apply-to-ie6-only
  #logo
    background-image: url(/logo.gif)
```

**注意：**当`@content`指令被多次指定或循环指定时，每次调用时都会复制样式块。

一些mixin可能需要传递的内容块，或者可能会根据内容块是否通过而具有不同的行为。`content-exists()`当一个内容块被传递给当前的mixin并且可以用来实现这种行为时，该函数将返回true。

#### 变量范围和内容块

传递给mixin的内容块将在块定义的范围内进行评估，而不是在mixin的范围内。这意味着混入变量本地的变量**不能**在传入的样式块中使用，并且变量将解析为全局值：

```javascript
$color: white;
@mixin colors($color: blue) {
  background-color: $color;
  @content;
  border-color: $color;
}
.colors {
  @include colors { color: $color; }
}
```

编译为：

```javascript
.colors {
  background-color: blue;
  color: white;
  border-color: blue;
}
```

另外，这表明传递块中使用的变量和mixin与块定义处的其他样式相关。例如：

```javascript
#sidebar {
  $sidebar-width: 300px;
  width: $sidebar-width;
  @include smartphone {
    width: $sidebar-width / 3;
  }
}
```

## 函数指令#function_directives

可以在sass中定义自己的函数，并在任何值或脚本环境中使用它们。例如：

```javascript
$grid-width: 40px;
$gutter-width: 10px;

@function grid-width($n) {
  @return $n * $grid-width + ($n - 1) * $gutter-width;
}

#sidebar { width: grid-width(5); }
```

变为：

```javascript
#sidebar {
  width: 240px; }
```

正如你所看到的，函数可以访问任何全局定义的变量以及像mixin一样接受参数。一个函数可能包含多个语句，并且您必须调用`@return`以设置该函数的返回值。

与mixin一样，您可以使用关键字参数调用Sass定义的函数。在上面的例子中，我们可以像这样调用函数：

```javascript
#sidebar { width: grid-width($n: 5); }
```

建议您为函数加上前缀以避免命名冲突，并使样式表的读者知道它们不属于Sass或CSS。例如，如果您为ACME Corp工作，则可能已经命名了`-acme-grid-width`上的述功能。

用户定义的函数也像mixin一样支持可变参数。

由于历史原因，函数名称（以及所有其他Sass标识符）可以互换使用连字符和下划线。例如，如果您定义了一个名为的函数`grid-width`，则可以将其用作`grid_width`，反之亦然。

## 输出样式

虽然Sass输出的默认CSS样式非常好，并且反映了文档的结构，但品味和需求各不相同，所以Sass支持其他几种样式。

Sass允许您通过设置`:style`选项或使用`--style`命令行标志来选择四种不同的输出样式。

### `:nested`

嵌套样式是默认的Sass样式，因为它反映了CSS样式的结构和他们正在设计的HTML文档。每个属性都有自己的行，但缩进不是恒定的。每个规则都基于嵌套的深度缩进。例如：

```javascript
#main {
  color: #fff;
  background-color: #000; }
  #main p {
    width: 10em; }

.huge {
  font-size: 10em;
  font-weight: bold;
  text-decoration: underline; }
```

查看大型CSS文件时，嵌套样式非常有用：它可以让您轻松掌握文件的结构，而无需实际读取任何内容。

### `:expanded`

扩展是更典型的人造CSS样式，每个属性和规则占据一行。属性在规则中缩进，但规则不以任何特殊方式缩进。例如：

```javascript
#main {
  color: #fff;
  background-color: #000;
}
#main p {
  width: 10em;
}

.huge {
  font-size: 10em;
  font-weight: bold;
  text-decoration: underline;
}
```

### `:compact`

紧凑的风格占用比嵌套或扩展更少的空间。它还将重点更多地放在选择器而不是它们的属性上。每条CSS规则只占用一行，每条属性都在该行上定义。嵌套规则彼此相邻，没有换行符，而单独的规则组之间有换行符。例如：

```javascript
#main { color: #fff; background-color: #000; }
#main p { width: 10em; }

.huge { font-size: 10em; font-weight: bold; text-decoration: underline; }
```

### `:compressed`

压缩样式占用了可能的最小空间量，除了在文件末尾分隔选择符和换行符所需的空格之外，没有空格。它还包含一些其他次要压缩，例如为颜色选择最小的表示。这并不意味着人类可读。例如：

```javascript
#main{color:#fff;background-color:#000}#main p{width:10em}.huge{font-size:10em;font-weight:bold;text-decoration:underline}
```

## 扩展Sass

Sass为具有独特需求的用户提供了许多高级自定义功能。使用这些功能需要对Ruby有深入的了解。

### 自定义Sass函数

用户可以使用Ruby API定义自己的Sass函数。有关更多信息，请参阅源文档。

### 缓存存储

Sass缓存解析后的文档，以便可以重用它们，而不必再次解析它们，除非它们已经更改。默认情况下，Sass会将这些缓存文件写入文件系统上的位置`:cache_location`。如果您无法写入文件系统或需要跨Ruby进程或机器共享缓存，则可以定义自己的缓存存储并设置`:cache_store`选项。有关创建自己的缓存存储的详细信息，请参阅[源文档](http://sass-lang.com/documentation/Sass/CacheStores/Base.html)。

### 自定义导入

Sass进口商负责传递路径`@import`并为这些路径找到适当的Sass代码。默认情况下，此代码是从[文件系统](http://sass-lang.com/documentation/Sass/Importers/Filesystem.html)加载的，但可以添加导入程序以通过HTTP从数据库加载，或者使用与Sass预期不同的文件命名方案。

每个导入器负责一个单一的加载路径（或任何对应的后端概念）。进口商可以`:load_paths`和正常的文件系统路径一起放入阵列中。

解决问题时`@import`，Sass将通过加载路径查找成功导入路径的导入器。一旦找到一个，就使用导入的文件。

用户创建的导入器必须从[Sass :: Importers :: Base](http://sass-lang.com/documentation/Sass/Importers/Base.html)继承。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-06