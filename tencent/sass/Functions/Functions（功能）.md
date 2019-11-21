# Functions（功能）

- 贡献者1人

  

### abs($number)

返回数字的绝对值。

例子：

```javascript
abs(10px) => 10px
abs(-10px) => 10px
```

参数：

- $number (Number)

返回：

- (Number)

增加：

- （ArgumentError） - 如果$ number不是数字

### adjust_color($color, $red, $green, $blue, $hue, $saturation, $lightness, $alpha)

增加或减少颜色的一个或多个属性。这可以改变红色、绿色、蓝色、色调、饱和度、值和alpha属性。属性被指定为关键字参数，并被添加到该属性的颜色当前值或从该属性的当前值中减去。

所有属性都是可选的。你不能同时在同一时间指定RGB属性（`$red`，`$green`，`$blue`）和HSL属性（`$hue`，`$saturation`，`$value`）。

例子：

```javascript
adjust-color(#102030, $blue: 5) => #102035
adjust-color(#102030, $red: -5, $blue: 5) => #0b2035
adjust-color(hsl(25, 100%, 80%), $lightness: -30%, $alpha: -0.4) => hsla(25, 100%, 50%, 0.6)
```

参数：

- $ color（Color）
- $ red（Number） - 对红色组件进行的调整，介于-255和255之间
- $ green（Number） - 在绿色组件上进行的调整，介于-255和255之间
- $ blue（Number） - 在蓝色组件上进行的调整，介于-255和255之间
- $ hue（Number） - 对色相组件进行的调整，以度为单位
- $ saturation（Number） - 在饱和度分量上进行的调整，范围在-100％和100％之间
- $ lightness（Number） - 亮度分量的调整值，在-100％和100％之间
- $ alpha（Number） - 对alpha组件进行的调整，介于-1和1之间

返回：

- (Color)

增加：

- （ArgumentError） - 如果有任何参数是错误的类型或者出界，或者RGB属性和HSL属性同时被调整。

### adjust_hue($color, $degrees)

改变颜色的色调。采用颜色和度数（通常介于`-360deg`和`360deg`之间），并返回一个颜色，并沿着色轮以该数量旋转色调。

例子：

```javascript
adjust-hue(hsl(120, 30%, 90%), 60deg) => hsl(180, 30%, 90%)
adjust-hue(hsl(120, 30%, 90%), -60deg) => hsl(60, 30%, 90%)
adjust-hue(#811, 45deg) => #886a11
```

参数：

- $ color（Color）
- $ degrees（Number） - 旋转色相的度数

返回：

- (Color)

增加：

- （ArgumentError） - 如果任一参数是错误的类型

### alpha($color)

返回颜色的alpha分量（不透明度）。除非另有说明，这是1。

此功能还支持专有的Microsoft `alpha(opacity=20)`语法作为特例。

参数：

- $color (Color)

返回：

- （Number） - α分量，介于0和1之间

增加:

- （ArgumentError） - 如果$ color不是一种颜色

### append($list, $val, $separator:auto)

将单个值附加到列表的末尾。

除非通过`$separator`参数，否则如果列表只有一个项目，则结果列表将以空格分隔。

像所有列表函数一样，`append()`返回一个新列表而不是修改它的参数。

例子：

```javascript
append(10px 20px, 30px) => 10px 20px 30px
append((blue, red), green) => blue, red, green
append(10px 20px, 30px 40px) => 10px 20px (30px 40px)
append(10px, 20px, comma) => 10px, 20px
append((blue, red), green, space) => blue red green
```

参数：

- $ list（Base）
- $ val（Base）
- $ separator（String） - 要使用的列表分隔符。如果这是逗号或空格，将使用该分隔符。如果这是自动的（默认），则按上面所述确定分隔符。

返回：

- (List)

### blue($color)

获取颜色的蓝色部分。必要时通过[该算法](http://www.w3.org/TR/css3-color/#hsl-color)从HSL中计算。

参数：

- $color (Color)

返回：

- （Number） - 蓝色分量，介于0和255之间（包括0和255）

Raises:

- (ArgumentError) — if $color isn't a color

### call($function, $args...)

动态调用一个函数。这可以调用用户定义的函数，内置函数或纯CSS函数。它会将所有参数（包括关键字参数）传递给被调用的函数。

例子：

```javascript
call(rgb, 10, 100, 255) => #0a64ff
call(scale-color, #0a64ff, $lightness: -10%) => #0058ef

$fn: nth;
call($fn, (a b c), 2) => b
```

参数：

- $ function（Function） - 要调用的函数。

### ceil($number)

将数字四舍五入到下一个整数。

例子：

```javascript
ceil(10.4px) => 11px
ceil(10.6px) => 11px
```

参数：

- $number (Number)

返回：

- (Number)

增加：

- （ArgumentError） - 如果$ number不是数字

### change_color($color, $red, $green, $blue, $hue, $saturation, $lightness, $alpha)

更改颜色的一个或多个属性。这可以改变红色，绿色，蓝色，色调，饱和度，值以及alpha属性。这些属性被指定为关键字参数，并替换该属性的颜色当前值。

所有属性都是可选的。你不能同时指定RGB属性（`$red`，`$green`，`$blue`）和HSL属性（`$hue`，`$saturation`，`$value`）在同一时间。

例子：

```javascript
change-color(#102030, $blue: 5) => #102005
change-color(#102030, $red: 120, $blue: 5) => #782005
change-color(hsl(25, 100%, 80%), $lightness: 40%, $alpha: 0.8) => hsla(25, 100%, 40%, 0.8)
```

参数：

- $ color（Color）
- $ red（Number） - 颜色的新红色分量，在0到255之间
- $ green（Number） - 颜色的新绿色组件，在0到255之间
- $ blue（Number） - 颜色的新蓝色分量，在0到255之间
- $ hue（Number） - 颜色的新色相组件，以度为单位
- $ saturation（Number） - 颜色的新饱和度分量，介于0％和100％之间
- $ lightness（Number） - 颜色的新亮度分量，在0％和100％范围内
- $ alpha（Number） - 颜色的新alpha组件，在0和1范围内

返回：

- (Color)

增加：

- （ArgumentError） - 如果有任何参数是错误的类型或出界，或者RGB属性和HSL属性同时被调整

### comparable($number1, $number2)

返回是否可以添加，减去或比较两个数字。

例子：

```javascript
comparable(2px, 1px) => true
comparable(100px, 3em) => false
comparable(10cm, 3mm) => true
```

参数：

- $number1 (Number)
- $number2 (Number)

返回：

- (Bool)

增加：

- （ArgumentError） - 如果任一参数是错误的类型

### complement($color)

返回颜色的补码。这与`adjust-hue(color, 180deg)`相同。

参数：

- $color (Color)

返回：

- (Color)

增加：

- （ArgumentError） - 如果$ color不是一种颜色

### content_exists

检查一个mixin是否通过了一个内容块。

除非`content-exists()`直接从mixin调用，否则会引发错误。

例子：

```javascript
@mixin needs-content {
  @if not content-exists() {
    @error "You must pass a content block!"
  }
  @content;
}
```

返回：

- （Bool） - 内容块是否传递给了mixin。

### counter($args...)

此功能仅作为IE7 [`content: counter`错误](http://jes.st/2013/ie7s-css-breaking-content-counter-bug/)的解决方法而存在。它与任何其他纯CSS功能的工作方式相同，只是它避免了在参数逗号之间添加空格。

例子：

```javascript
counter(item, ".") => counter(item,".")
```

返回：

- (String)

### counters($args...)

此功能仅作为IE7 [`content: counter`错误](http://jes.st/2013/ie7s-css-breaking-content-counter-bug/)的解决方法而存在。它与任何其他纯CSS功能的工作方式相同，只是它避免了在参数逗号之间添加空格。

例子：

```javascript
counters(item, ".") => counters(item,".")
```

返回：

- (String)

### darken($color, $amount)

使颜色变深。使用0％到100％之间的颜色和数字，并返回一个颜色，亮度降低该数量。

例子：

```javascript
darken(hsl(25, 100%, 80%), 30%) => hsl(25, 100%, 50%)
darken(#800, 20%) => #200
```

参数：

- $ color（Color）
- $ amount（数量） - 减少0％到100％之间亮度的数量

返回：

- (Color)

增加：

- （ArgumentError） - 如果$ amount超出范围，或者两个参数都是错误的类型

### desaturate($color, $amount)

使颜色饱和度降低。取一个颜色和一个介于0％和100％之间的数字，并返回一个颜色，饱和度降低该值。

例子：

```javascript
desaturate(hsl(120, 30%, 90%), 20%) => hsl(120, 10%, 90%)
desaturate(#855, 20%) => #726b6b
```

参数：

- $ color（Color）
- $ amount（数量） - 减少饱和度的数量，介于0％和100％之间

返回：

- (Color)

增加：

- （ArgumentError） - 如果$ amount超出范围，或者两个参数都是错误的类型

### feature_exists($feature)

返回当前Sass运行时是否存在功能。

支持以下功能：

- `global-variable-shadowing`表示一个局部变量将会影响一个全局变量，除非`!global`被使用。
- `extend-selector-pseudoclass`表示`@extend`将进入选择器伪类似`:not`。
- `units-level-3`表示完全支持使用“ [值和单位3级”](http://www.w3.org/TR/css3-values/)规范中定义的单位进行单位算术运算。
- `at-error`表示支持Sass `@error`指令。
- `custom-property`表示支持[自定义属性级别1](https://www.w3.org/TR/css-variables-1/)规范。这意味着自定义属性是静态分析的，只有插值被视为SassScript。

例子：

```javascript
feature-exists(some-feature-that-exists) => true
feature-exists(what-is-this-i-dont-know) => false
```

参数：

- $ feature（String） - 要素的名称

返回：

- （Bool） - 该版本的Sass是否支持该功能

增加：

- （ArgumentError） - 如果$ feature不是一个字符串

### floor($number)

将数字向下舍入到前一个整数。

例子：

```javascript
floor(10.4px) => 10px
floor(10.6px) => 10px
```

参数：

- $number (Number)

返回：

- (Number)

Raises:

- （ArgumentError） - 如果$ number不是数字

### function_exists($name)

Check whether a function with the given name exists.

Examples:

```javascript
function-exists(lighten) => true

@function myfunc { @return "something"; }
function-exists(myfunc) => true
```

参数：

- name（String） - 要检查的函数的名称或函数引用。

返回：

- （Bool） - 功能是否被定义。

### get_function($name, $css:false)

返回对函数的引用，以便稍后使用该`call()`函数调用。

如果`$css`是`false`，函数引用可能引用样式表中定义的函数或内置于主机环境的函数。如果它是`true`指一个纯CSS的函数。

例子：

```javascript
get-function("rgb")

@function myfunc { @return "something"; }
get-function("myfunc")
```

参数：

- name（String） - 被引用函数的名称。
- css（Bool） - 是否获得普通的CSS函数。

返回：

- （Function） - 功能参考。

### global_variable_exists($name)

检查具有给定名称的变量是否存在于全局范围内（在文件的顶层）。

例子：

```javascript
$a-false-value: false;
global-variable-exists(a-false-value) => true
global-variable-exists(a-null-value) => true

.foo {
  $some-var: false;
  @if global-variable-exists(some-var) { /* false, doesn't run */ }
}
```

参数：

- $ name（String） - 要检查的变量的名称。该名称不应包含$。

返回：

- （Bool） - 变量是否在全局范围内定义。

### grayscale($color)

将颜色转换为灰度。这与之相同`desaturate(color, 100%)`。

参数：

- $color (Color)

返回：

- (Color)

增加：

- （ArgumentError） - 如果$ color不是一种颜色

### green($color)

获取颜色的绿色部分。必要时通过[该算法](http://www.w3.org/TR/css3-color/#hsl-color)从HSL计算。

参数：

- $color (Color)

返回：

- （Number） - 绿色部分，介于0和255之间

增加：

- （ArgumentError） - 如果$ color不是一种颜色

### hsl($hue, $saturation, $lightness)

根据色调，饱和度和亮度值创建[颜色](http://sass-lang.com/documentation/Sass/Script/Value/Color.html)。使用[CSS3规范中](http://www.w3.org/TR/css3-color/#hsl-color)的算法。

参数：

- $ hue（Number） - 颜色的色调。应该在0到360度之间，包括在内
- $ saturation（Number） - 颜色的饱和度。必须介于0％和100％之间，包括0和100％
- $ lightness（Number） - 颜色的亮度。必须介于0％和100％之间，包括0和100％

返回：

- (Color)

增加：

- （ArgumentError） - 如果$ saturation或$ lightness超出范围，或者任何参数都是错误的类型

### hsla($hue, $saturation, $lightness, $alpha)

根据色调，饱和度，亮度和Alpha值创建[颜色](http://sass-lang.com/documentation/Sass/Script/Value/Color.html)。使用[CSS3规范中](http://www.w3.org/TR/css3-color/#hsl-color)的算法。

参数：

- $ hue（Number） - 颜色的色调。应该在0到360度之间，包括在内
- $ saturation（Number） - 颜色的饱和度。必须介于0％和100％之间，包括0和100％
- $ lightness（Number） - 颜色的亮度。必须介于0％和100％之间，包括0和100％
- $ alpha（Number） - 颜色的不透明度。必须介于0和1之间，包括0和1

返回：

- (Color)

增加：

- （ArgumentError） - 如果$ saturation，$ lightness或$ alpha超出范围或者任何参数是错误的类型

### hue($color)

返回颜色的色相分量。请参阅[CSS3 HSL规范](http://en.wikipedia.org/wiki/HSL_and_HSV#Conversion_from_RGB_to_HSL_or_HSV)。在必要时通过[该算法](http://en.wikipedia.org/wiki/HSL_and_HSV#Conversion_from_RGB_to_HSL_or_HSV)从RGB计算。

参数：

- $color (Color)

返回：

- （Number） - 色调成分，介于0deg和360deg之间

增加：

- （ArgumentError） - 如果$ color不是一种颜色

### ie_hex_str($color)

将颜色转换为IE筛选器可理解的格式。

例子：

```javascript
ie-hex-str(#abc) => #FFAABBCC
ie-hex-str(#3322BB) => #FF3322BB
ie-hex-str(rgba(0, 255, 0, 0.5)) => #8000FF00
```

参数：

- $color (Color)

返回：

- （String） - 颜色的IE格式的字符串表示形式

增加：

- （ArgumentError） - 如果$ color不是一种颜色

### if($condition, $if-true, $if-false)

根据是否`$condition`为真返回两个值中的一个。就像在`@if`，比其他所有的值`false`和`null`被认为是真的。

例子：

```javascript
if(true, 1px, 2px) => 1px
if(false, 1px, 2px) => 2px
```

参数：

- $ condition（Base） - 是否返回$ if-true或$ if-false
- $ if-true（Tree :: Node）
- $ if-false（Tree :: Node）

返回：

- (Base) — $if-true or $if-false

### index($list, $value)

返回列表中某个值的位置。如果找不到该值，则返回`null`。

请注意，与某些语言不同，Sass列表中的第一项是数字1，第二个数字是2，依此类推。

这可以返回地图中一对的位置。

例子：

```javascript
index(1px solid red, solid) => 2
index(1px solid red, dashed) => null
index((width: 10px, height: 20px), (height 20px)) => 2
```

参数：

- $list (Base)
- $value (Base)

返回：

- （Number，Null） - $ list中从$ 1开始的索引，或者为null

### inspect($value)

返回一个包含该值的字符串作为其Sass表示。

参数：

- $ value（Base） - 要检查的值。

返回：

- （String） - 表示将在Sass中写入的值。

### invert($color) invert($color, $weight:100%)

返回颜色的反（负）。红色，绿色和蓝色的值是倒置的，而不透明度是独立的。

重载：

- **反转**（$ color）参数：

```js
-  $color (Color) 
```

- **反转**（$ color，$ weight：100％）参数：

```js
-  $color (Color) 
-  $weight (Number) — The relative weight of the color color's inverse  
```

返回：

- (Color)

增加：

- （ArgumentError） - 如果$ color不是颜色或$ weight不是0％到100％之间的百分比

### is_bracketed($list)

返回列表是否使用方括号。

例子：

```javascript
is-bracketed(1px 2px 3px) => false
is-bracketed([1px, 2px, 3px]) => true
```

参数：

- $list (Base)

返回：

- (Bool)

### is_superselector($super, $sub)

返回是否`$super`是超级选择器`$sub`。这意味着`$super`匹配所有匹配的元素，`$sub`以及可能的附加元素。一般来说，简单的选择器往往是更复杂的超级选择器。

例子：

```javascript
is-superselector(".foo", ".foo.bar") => true
is-superselector(".foo.bar", ".foo") => false
is-superselector(".bar", ".foo .bar") => true
is-superselector(".foo .bar", ".bar") => false
```

返回是否`$selector1`是一个超级选择器`$selector2`。

参数：

- $ super（String，List） - 潜在的超级选择器。这可以是字符串，字符串列表或由＆返回的字符串列表列表。
- $ sub（String，List） - 潜在的子选择器。这可以是字符串，字符串列表或由＆返回的字符串列表列表。

返回：

- （Bool） - $ selector1是否是$ selector2的超级选择器。

### join($list1, $list2, $separator:auto, $bracketed:auto)

将两个列表合并成一个。

除非`$separator`通过，否则，如果一个列表以逗号分隔且一个以空格分隔，则第一个参数的分隔符将用于结果列表。如果两个列表都少于两个项目，则空格将用于结果列表。

除非`$bracketed`通过，否则如果第一个参数是，则结果列表被括起来。

像所有列表函数一样，`join()`返回一个新列表而不是修改其参数。

例子：

```javascript
join(10px 20px, 30px 40px) => 10px 20px 30px 40px
join((blue, red), (#abc, #def)) => blue, red, #abc, #def
join(10px, 20px) => 10px 20px
join(10px, 20px, comma) => 10px, 20px
join((blue, red), (#abc, #def), space) => blue red #abc #def
join([10px], 20px) => [10px 20px]
```

参数：

- $ list1（Base）
- $ list2（Base）
- $ separator（String） - 要使用的列表分隔符。如果这是逗号或空格，将使用该分隔符。如果这是自动的（默认），则按上面所述确定分隔符。
- $括号（Base） - 结果列表是否被括起来。如果这是自动的（默认），则按上面所述确定分隔符。

返回：

- (List)

### keywords($args)

返回传递给具有可变参数列表的函数或mixin的命名参数的映射。参数名称是字符串，并且不包含前导`$`。

例子：

```javascript
@mixin foo($args...) {
  @debug keywords($args); //=> (arg1: val, arg2: val)
}

@include foo($arg1: val, $arg2: val);
```

参数：

- $args (ArgList)

返回：

- (Map)

增加：

- （ArgumentError） - 如果$ args不是可变参数列表

### length($list)

返回列表的长度。

这也可以返回地图中对的数量。

例子：

```javascript
length(10px) => 1
length(10px 20px 30px) => 3
length((width: 10px, height: 20px)) => 2
```

参数：

- $list (Base)

返回：

- (Number)

### lighten($color, $amount)

使颜色变浅。在`0%`和`100%`之间取一个颜色和一个数字，并返回一个颜色，亮度增加了该数量。

例子：

```javascript
lighten(hsl(0, 0%, 0%), 30%) => hsl(0, 0, 30)
lighten(#800, 20%) => #e00
```

参数：

- $ color（Color）
- $ amount（数量） - 增加亮度的量，介于0％和100％之间

返回：

- (Color)

增加：

- （ArgumentError） - 如果$ amount超出范围，或者两个参数都是错误的类型

### lightness($color)

返回颜色的亮度分量。请参阅[CSS3 HSL规范](http://en.wikipedia.org/wiki/HSL_and_HSV#Conversion_from_RGB_to_HSL_or_HSV)。在必要时通过[该算法](http://en.wikipedia.org/wiki/HSL_and_HSV#Conversion_from_RGB_to_HSL_or_HSV)从RGB计算。

参数：

- $color (Color)

返回：

- （Number） - 亮度分量，介于0％和100％之间

增加：

- （ArgumentError） - 如果$ color不是一种颜色

### list_separator($list)

返回列表的分隔符。如果由于少于两个元素而导致列表中没有分隔符，则返回`space`。

例子：

```javascript
list-separator(1px 2px 3px) => space
list-separator(1px, 2px, 3px) => comma
list-separator('foo') => space
```

参数：

- $list (Base)

返回：

- （String） - 逗号或空格

### map_get($map, $key)

返回与给定键相关联的映射中的值。如果地图没有这样的密钥，则返回`null`。

例子：

```javascript
map-get(("foo": 1, "bar": 2), "foo") => 1
map-get(("foo": 1, "bar": 2), "bar") => 2
map-get(("foo": 1, "bar": 2), "baz") => null
```

参数：

- $map (Map)
- $key (Base)

返回：

- （Base） - 由$ key索引的值，如果映射不包含给定的键，则为null

增加：

- （ArgumentError） - 如果$ map不是地图

### map_has_key($map, $key)

返回映射是否具有与给定键相关联的值。

例子：

```javascript
map-has-key(("foo": 1, "bar": 2), "foo") => true
map-has-key(("foo": 1, "bar": 2), "baz") => false
```

参数：

- $map (Map)
- $key (Base)

返回：

- (Bool)

增加：

- （ArgumentError） - 如果$ map不是地图

### map_keys($map)

返回地图中所有键的列表。

例子：

```javascript
map-keys(("foo": 1, "bar": 2)) => "foo", "bar"
```

参数：

- $map (Map)

返回：

- （List） - 用逗号分隔的键列表

增加：

- （ArgumentError） - 如果$ map不是地图

### map_merge($map1, $map2)

将两张地图合并成一张新地图。在键`$map2`将优先于键`$map1`。

这是向地图添加新值的最佳方式。

返回的地图中的所有键也会出现在`$map1`并与`$map1`中的顺序相同。从`$map2`出的新的键将放置在地图的末尾。

像所有的地图函数一样，`map-merge()`返回一个新的地图而不是修改它的参数。

例子：

```javascript
map-merge(("foo": 1), ("bar": 2)) => ("foo": 1, "bar": 2)
map-merge(("foo": 1, "bar": 2), ("bar": 3)) => ("foo": 1, "bar": 3)
```

参数：

- $map1 (Map)
- $map2 (Map)

返回：

- (Map)

增加：

- （ArgumentError） - 如果任一参数不是地图

### map_remove($map, $keys...)

返回删除了键的新地图。

像所有的地图函数一样，`map-merge()`返回一个新的地图而不是修改它的参数。

例子：

```javascript
map-remove(("foo": 1, "bar": 2), "bar") => ("foo": 1)
map-remove(("foo": 1, "bar": 2, "baz": 3), "bar", "baz") => ("foo": 1)
map-remove(("foo": 1, "bar": 2), "baz") => ("foo": 1, "bar": 2)
```

参数：

- $map (Map)
- $keys (Base)

返回：

- (Map)

增加：

- （ArgumentError） - 如果$ map不是地图

### map_values($map)

返回地图中所有值的列表。如果多个键具有相同的值，则此列表可能包含重复的值。

例子：

```javascript
map-values(("foo": 1, "bar": 2)) => 1, 2
map-values(("foo": 1, "bar": 2, "baz": 1)) => 1, 2, 1
```

参数：

- $map (Map)

返回：

- （List） - 以逗号分隔的值列表

增加：

- (ArgumentError) — if $map is not a map

### max($numbers...)

查找最多的几个数字。该函数可以使用任意数量的参数。

例子：

```javascript
max(1px, 4px) => 4px
max(5em, 3em, 4em) => 5em
```

参数：

- $numbers (Number)

返回：

- (Number)

增加：

- （ArgumentError） - 如果任何参数不是数字，或者如果不是所有参数都有可比较的单位

### min($numbers...)

查找几个数字中的最小值。该函数可以使用任意数量的参数。

例子：

```javascript
min(1px, 4px) => 1px
min(5em, 3em, 4em) => 3em
```

参数：

- $numbers (Number)

返回：

- (Number)

增加：

- （ArgumentError） - 如果任何参数不是数字，或者如果不是所有参数都有可比较的单位

### mix($color1, $color2, $weight:50%)

将两种颜色混合在一起。具体来说，取每个RGB分量的平均值，可选地按给定百分比加权。加权组件时也会考虑颜色的不透明度。

重量指定应该包含在返回的颜色中的第一种颜色的数量。默认值，`50%`意味着应该使用第一种颜色的一半和第二种颜色的一半。`25%`意味着应该使用第一种颜色的四分之一和第二种颜色的四分之三。

例子：

```javascript
mix(#f00, #00f) => #7f007f
mix(#f00, #00f, 25%) => #3f00bf
mix(rgba(255, 0, 0, 0.5), #00f) => rgba(63, 0, 191, 0.75)
```

参数：

- $ color1（Color）
- $ color2（Color）
- $ weight（Number） - 每种颜色的相对重量。接近100％给予$ color1更多的权重，接近0％给予$ color2更多的权重

返回：

- (Color)

增加：

- （ArgumentError） - 如果$ weight超出范围或者任何参数是错误的类型

### mixin_exists($name)

检查是否存在具有给定名称的混音。

例子：

```javascript
mixin-exists(nonexistent) => false

@mixin red-text { color: red; }
mixin-exists(red-text) => true
```

参数：

- name（String） - 要检查的mixin的名称。

返回：

- （Bool） - mixin是否被定义。

### nth($list, $n)

获取列表中的第n个项目。

请注意，与某些语言不同，Sass列表中的第一项是数字1，第二个数字是2，依此类推。

这也可以返回地图中的第n对。

负索引值以相反顺序处理元素，从列表中的最后一个元素开始。

例子：

```javascript
nth(10px 20px 30px, 1) => 10px
nth((Helvetica, Arial, sans-serif), 3) => sans-serif
nth((width: 10px, length: 20px), 2) => length, 20px
```

参数：

- $ list（Base）
- $ n（Number） - 要获取的项目的索引。负数来自列表末尾。

返回：

- (Base)

增加：

- （ArgumentError） - 如果$ n不是1和$ list长度之间的整数

### opacify($color, $amount) Also known as: fade_in

使颜色更加不透明。采用0到1之间的颜色和数字，并返回一个颜色，并将不透明度增加该数量。

例子：

```javascript
opacify(rgba(0, 0, 0, 0.5), 0.1) => rgba(0, 0, 0, 0.6)
opacify(rgba(0, 0, 17, 0.8), 0.2) => #001
```

参数：

- $ color（Color）
- $ amount（Number） - 在0和1之间增加不透明度的数量

返回：

- (Color)

增加：

- （ArgumentError） - 如果$ amount超出范围，或者两个参数都是错误的类型

### opacity($color)

返回颜色的alpha分量（不透明度）。除非另有说明，这就是1。

参数：

- $color (Color)

返回：

- （Number） - α分量，介于0和1之间

增加：

- （ArgumentError） - 如果$ color不是一种颜色

### percentage($number)

将无单位数转换为百分比。

例子：

```javascript
percentage(0.2) => 20%
percentage(100px / 50px) => 200%
```

参数：

- $number (Number)

返回：

- (Number)

增加：

- （ArgumentError） - 如果$ number不是一个无单位的数字

### quote($string)

如果字符串未被引用，则将引号添加到字符串；如果是，则返回相同的字符串。

例子：

```javascript
quote("foo") => "foo"
quote(foo) => "foo"
```

参数：

- $string (String)

返回：

- (String)

增加：

- （ArgumentError） - 如果$ string不是字符串

### random random($limit)

重载：

- **随机** 在0和1之间返回一个小数，包括0但不是1.返回：

```js
-  (Number) — A decimal value.  
```

- **random**（$ limit）返回1和`$limit`之间的整数，包括1和`$limit`。参数：

```js
-  $limit (Number) — The maximum of the random integer to be returned, a positive integer.  
```

返回：

```js
-  (Number) — An integer.  
```

增加：

```js
-  (ArgumentError) — if the $limit is not 1 or greater  
```

### red($color)

获取一种颜色的红色部分。必要时通过[该算法](http://www.w3.org/TR/css3-color/#hsl-color)从HSL 进行计算。

参数：

- $color (Color)

返回：

- （Number） - 红色分量，介于0和255之间

增加：

- （ArgumentError） - 如果$ color不是一种颜色

### rgb($red, $green, $blue)

从红色，绿色和蓝色值创建一个[Color](http://sass-lang.com/documentation/Sass/Script/Value/Color.html)对象。

参数：

- $ red（Number） - 颜色中的红色数量。必须介于0和255之间（含），或0％和100％（含）之间
- $ green（Number） - 颜色中的绿色数量。必须介于0和255之间（含），或0％和100％（含）之间
- $ blue（Number） - 颜色中的蓝色数量。必须介于0和255之间（含），或0％和100％（含）之间

返回：

- (Color)

增加：

- （ArgumentError） - 如果任何参数是错误的类型或超出范围

### rgba($red, $green, $blue, $alpha) rgba($color, $alpha)

从红色，绿色，蓝色和Alpha值创建一个[颜色](http://sass-lang.com/documentation/Sass/Script/Value/Color.html)。

重载：

- **rgba**($red, $green, $blue, $alpha) Parameters:

```js
-  $red (Number) — The amount of red in the color. Must be between 0 and 255 inclusive or 0% and 100% inclusive  
-  $green (Number) — The amount of green in the color. Must be between 0 and 255 inclusive or 0% and 100% inclusive  
-  $blue (Number) — The amount of blue in the color. Must be between 0 and 255 inclusive or 0% and 100% inclusive  
-  $alpha (Number) — The opacity of the color. Must be between 0 and 1 inclusive  
```

返回：

```js
-  (Color) 
```

增加：

```js
-  (ArgumentError) — if any parameter is the wrong type or out of bounds  
```

- **rgba**（$ color，$ alpha）设置现有颜色的不透明度。例子：rgba(#102030, 0.5) => rgba(16, 32, 48, 0.5) rgba(blue, 0.2) => rgba(0, 0, 255, 0.2)

参数：

```js
-  $color (Color) — The color whose opacity will be changed.  
-  $alpha (Number) — The new opacity of the color. Must be between 0 and 1 inclusive  
```

返回：

```js
-  (Color) 
```

增加：

```js
-  (ArgumentError) — if $alpha is out of bounds or either parameter is the wrong type  
```

### round($number)

将数字四舍五入到最接近的整数。

例子：

```javascript
round(10.4px) => 10px
round(10.6px) => 11px
```

参数：

- $number (Number)

返回：

- (Number)

增加：

- （ArgumentError） - 如果$ number不是数字

### saturate($color, $amount)

使颜色更饱和。采用0％到100％之间的颜色和数字，并返回饱和度增加该颜色的颜色。

例子：

```javascript
saturate(hsl(120, 30%, 90%), 20%) => hsl(120, 50%, 90%)
saturate(#855, 20%) => #9e3f3f
```

参数：

- $color (Color)
- $amount (Number) — The amount to increase the saturation by, between 0% and 100%

返回：

- (Color)

增加：

- （ArgumentError） - 如果$ amount超出范围，或者两个参数都是错误的类型

### saturation($color)

返回颜色的饱和度分量。请参阅[CSS3 HSL规范](http://en.wikipedia.org/wiki/HSL_and_HSV#Conversion_from_RGB_to_HSL_or_HSV)。在必要时通过[该算法](http://en.wikipedia.org/wiki/HSL_and_HSV#Conversion_from_RGB_to_HSL_or_HSV)从RGB进行计算。

参数：

- $color (Color)

返回：

- （Number） - 饱和度分量，介于0％和100％之间

增加：

- （ArgumentError） - 如果$ color不是一种颜色

### scale_color($color, $red, $green, $blue, $saturation, $lightness, $alpha)

流畅地缩放一种或多种颜色的属性。与调整颜色不同，颜色的属性会根据固定的数量改变颜色，缩放颜色会根据它们已经有多高或低多少来流畅地改变它们。这意味着使用缩放颜色减轻已经很浅的颜色不会使亮度变化太大，但是将暗色减少相同的量会使其变得更加显着。`scale-color($color, ...)`无论是什么`$color`，这都具有类似的效果。

例如，颜色的亮度可以在`0%`和之间的任何地方`100%`。如果`scale-color($color, $lightness: 40%)`被调用，结果颜色的亮度将是其原始亮度和100之间的40％。如果`scale-color($color, $lightness: -40%)`被调用，亮度将是原始值和0之间的40％。

这可以改变红色，绿色，蓝色，饱和度，数值和alpha属性。这些属性被指定为关键字参数。所有参数都应该是`0%`和`100%`之间的百分比。

所有属性都是可选的。你不能在同一时间指定RGB属性（`$red`，`$green`，`$blue`）同时和HSL属性（`$saturation`，`$value`）。

例子：

```javascript
scale-color(hsl(120, 70%, 80%), $lightness: 50%) => hsl(120, 70%, 90%)
scale-color(rgb(200, 150%, 170%), $green: -40%, $blue: 70%) => rgb(200, 90, 229)
scale-color(hsl(200, 70%, 80%), $saturation: -90%, $alpha: -30%) => hsla(200, 7%, 80%, 0.7)
```

参数：

- $color (Color)
- $red (Number)
- $green (Number)
- $blue (Number)
- $saturation (Number)
- $lightness (Number)
- $alpha (Number)

返回：

- (Color)

增加：

- （ArgumentError） - 如果有任何参数是错误的类型或出界，或者RGB属性和HSL属性同时被调整

### selector_append($selectors...)

返回一个新的选择器，其`$selectors`中包含所有选择器，就像它们已被嵌套在样式表中`$selector1 { &$selector2 { ... } }`一样。

例子：

```javascript
selector-append(".foo", ".bar", ".baz") => .foo.bar.baz
selector-append(".a .foo", ".b .bar") => "a .foo.b .bar"
selector-append(".foo", "-suffix") => ".foo-suffix"
```

返回表示附加`$selectors`结果的字符串列表列表。这与返回`&`的选择器格式相同。

参数：

- $ selectors（String，List） - 要追加的选择器。至少必须通过一个选择器。它们中的每一个都可以是字符串，字符串列表或由＆返回的字符串列表列表。

返回：

- （List） - 表示追加$选择符结果的字符串列表。这与由＆返回的选择器格式相同。

增加：

- （ArgumentError） - 如果选择器不能被追加。

### selector_extend($selector, $extendee, $extender)

使用`$extendee`扩展名返回`$selector`新版本`$extender`。这工作就像结果一样

```javascript
$selector { ... }
$extender { @extend $extendee }
```

例子：

```javascript
selector-extend(".a .b", ".b", ".foo .bar") => .a .b, .a .foo .bar, .foo .a .bar
```

返回表示扩展结果的字符串列表列表。这与返回的选择器格式相同`&`。

参数：

- $ selector（String，List） - 其中$ extendee用$ extender扩展的选择器。这可以是字符串、字符串列表或由＆返回的字符串列表列表。
- $ extendee（String，List） - 选择器被扩展。这可以是字符串，字符串列表或由＆返回的字符串列表列表。
- $ extender（String，List） - 选择器被注入$选择器。这可以是字符串，字符串列表或由＆返回的字符串列表列表。

返回：

- （List） - 表示扩展结果的字符串列表。这与由＆返回的选择器格式相同。

增加：

- （ArgumentError） - 如果扩展失败

### selector_nest($selectors...)

返回一个新的选择器，其中所有选择器`$selectors`嵌套在另一个下面，就好像它们已经嵌套在样式表中`$selector1 { $selector2 { ... } }`一样。

与大多数选择器函数不同，`selector-nest`允许父选择器`&`用于任何选择器，但第一个选择器。

例子：

```javascript
selector-nest(".foo", ".bar", ".baz") => .foo .bar .baz
selector-nest(".a .foo", ".b .bar") => .a .foo .b .bar
selector-nest(".foo", "&.bar") => .foo.bar
```

返回表示嵌套`$selectors`结果的字符串列表。这与返回`&`的选择器格式相同。

参数：

- $ selectors（String，List） - 嵌套的选择器。至少必须通过一个选择器。它们中的每一个都可以是字符串，字符串列表或由＆返回的字符串列表列表。

返回：

- （List） - 表示嵌套$选择符结果的字符串列表。这与由＆返回的选择器格式相同。

### selector_parse($selector)

将用户提供的选择器解析为返回`&`的字符串列表列表。

例子：

```javascript
selector-parse(".foo .bar, .baz .bang") => ('.foo' '.bar', '.baz' '.bang')
```

返回表示`$selector`字符串列表的列表。这与返回`&`的选择器格式相同。

参数：

- $ selector（String，List） - 要解析的选择器。这可以是字符串，字符串列表或由＆返回的字符串列表列表。

返回：

- （List） - 代表$ selector的字符串列表列表。这与由＆返回的选择器格式相同。

### selector_replace($selector, $original, $replacement)

在`$selector`中替换的所有`$original`与`$replacement`实例

这通过使用`@extend`和扔掉原始选择器来工作。这意味着它可以用来做非常先进的替换；看下面的例子。

例子：

```javascript
selector-replace(".foo .bar", ".bar", ".baz") => ".foo .baz"
selector-replace(".foo.bar.baz", ".foo.baz", ".qux") => ".bar.qux"
```

返回表示扩展结果的字符串列表列表。这与返回`&`的选择器格式相同。

参数：

- $ selector（String，List） - 将$ original替换为$ replacement的选择器。这可以是字符串，字符串列表或由＆返回的字符串列表列表。
- $ original（String，List） - 被替换的选择器。这可以是字符串，字符串列表或由＆返回的字符串列表列表。
- $ replacement（String，List） - $ original被替换的选择器。这可以是字符串，字符串列表或由＆返回的字符串列表列表。

返回：

- （List） - 表示扩展结果的字符串列表。这与由＆返回的选择器格式相同。

增加：

- （ArgumentError） - 如果替换失败

### selector_unify($selector1, $selector2)

将两个选择器统一为一个只匹配两个输入选择器匹配的元素的选择器。如果没有这样的选择器，则返回`null`。

与选择器统一完成`@extend`一样，这不保证输出选择器将匹配由两个输入选择器匹配的*所有*元素。例如，如果`.a .b`与`.x .y`，`.a .x .b.y, .x .a .b.y`统一将被退回，但`.a.x .b.y`不会。这避免了指数输出大小，同时匹配实践中可能存在的所有元素。

例子：

```javascript
selector-unify(".a", ".b") => .a.b
selector-unify(".a .b", ".x .y") => .a .x .b.y, .x .a .b.y
selector-unify(".a.b", ".b.c") => .a.b.c
selector-unify("#a", "#b") => null
```

返回表示统一结果的字符串列表列表，如果不存在统一结果，则返回null。这与返回`&`的选择器格式相同。

参数：

- $ selector1（String，List） - 要统一的第一个选择器。这可以是字符串，字符串列表或由＆返回的字符串列表列表。
- $ selector2（String，List） - 要统一的第二个选择器。这可以是字符串，字符串列表或由＆返回的字符串列表列表。

返回：

- （List，Null） - 表示统一结果的字符串列表列表，如果不存在统一结果，则为null。这与由＆返回的选择器格式相同。

### 设置

根据提供的列表返回一个新列表，但第n个元素更改为给定的值。

请注意，与某些语言不同，Sass列表中的第一项是数字1，第二个数字是2，依此类推。

负索引值以相反顺序处理元素，从列表中的最后一个元素开始。

例子：

```javascript
set-nth($list: 10px 20px 30px, $n: 2, $value: -20px) => 10px -20px 30px
```

参数：

- $ list（Base） - 将被复制的列表，其索引$ n处的元素已更改。
- $ n（Number） - 要设置的项目的索引。负数来自列表末尾。
- $ value（Base） - 索引$ n处的新值。

返回：

- (List)

增加：

- （ArgumentError） - 如果$ n不是1和$ list长度之间的整数

### simple_selectors($selector)

返回[简单选择](http://dev.w3.org/csswg/selectors4/#simple)组成选择器的`$selector`器。

注意，`$selector` **必须是**一个[复合选择](http://dev.w3.org/csswg/selectors4/#compound)。这意味着它不能包含逗号或空格。这也意味着与其他选择器函数不同，这只需要字符串，而不是列表。

例子：

```javascript
simple-selectors(".foo.bar") => ".foo", ".bar"
simple-selectors(".foo.bar.baz") => ".foo", ".bar", ".baz"
```

返回一个组合选择器中的简单选择器列表。

参数：

- $ selector（String） - 简单选择器将被提取的复合选择器。

返回：

- （List） - 复合选择器中的简单选择器列表。

### str_index($string, $substring)

在`$string`中返回的第一个`$substring` 匹配项的索引。如果不存在这种情况，则返回`null`。

请注意，与某些语言不同，Sass字符串中的第一个字符是数字1，第二个数字2，等等。

例子：

```javascript
str-index(abcd, a)  => 1
str-index(abcd, ab) => 1
str-index(abcd, X)  => null
str-index(abcd, c)  => 3
```

参数：

- $string (String)
- $substring (String)

返回：

- (Number, Null)

增加：

- （ArgumentError） - 如果任何参数是错误的类型

### str_insert($string, $insert, $index)

Inserts `$insert` into `$string` at `$index`.

请注意，与某些语言不同，Sass字符串中的第一个字符是数字1，第二个数字2，等等。

例子：

```javascript
str-insert("abcd", "X", 1) => "Xabcd"
str-insert("abcd", "X", 4) => "abcXd"
str-insert("abcd", "X", 5) => "abcdX"
```

参数：

- $ string（String）
- $ insert（String）
- $ index（Number） - $ insert将被插入的位置。负数指数从$ string的末尾算起。超出字符串范围的索引将在字符串的前面或后面插入$ insert

返回：

- （String） - 结果字符串。当且仅当$ string被引用时才会引用它

增加：

- （ArgumentError） - 如果任何参数是错误的类型

### str_length($string)

返回字符串中的字符数。

例子：

```javascript
str-length("foo") => 3
```

参数：

- $string (String)

返回：

- (Number)

增加：

- （ArgumentError） - 如果$ string不是字符串

### str_slice($string, $start-at, $end-at:-1)

从`$string`中提取一个子字符串。子字符串将从索引`$start-at`开始并在索引`$end-at`处结束。

请注意，与某些语言不同，Sass字符串中的第一个字符是数字1，第二个数字2，等等。

例子：

```javascript
str-slice("abcd", 2, 3)   => "bc"
str-slice("abcd", 2)      => "bcd"
str-slice("abcd", -3, -2) => "bc"
str-slice("abcd", 2, -2)  => "bc"
```

返回子字符串。这将被引用，当且仅当`$string`被引用

参数：

- $ start-at（Number） - 子字符串的第一个字符的索引。如果这是负数，则从$ string的末尾开始计数
- $ end-at（Number） - 子串最后一个字符的索引。如果这是负数，则从$ string的末尾开始计数。默认为 -1

返回：

- （String） - 子字符串。当且仅当$ string被引用时才会引用它

增加：

- （ArgumentError） - 如果任何参数是错误的类型

### to_lower_case($string)

将字符串转换为小写，

例子：

```javascript
to-lower-case(ABCD) => abcd
```

参数：

- $string (String)

返回：

- (String)

增加：

- （ArgumentError） - 如果$ string不是字符串

### to_upper_case($string)

将字符串转换为大写。

例子：

```javascript
to-upper-case(abcd) => ABCD
```

参数：

- $string (String)

返回：

- (String)

增加：

- （ArgumentError） - 如果$ string不是字符串

### transparentize($color, $amount) Also known as: fade_out

使颜色更透明。采用0到1之间的颜色和数字，并返回一个颜色，并将不透明度降低该数量。

例子：

```javascript
transparentize(rgba(0, 0, 0, 0.5), 0.1) => rgba(0, 0, 0, 0.4)
transparentize(rgba(0, 0, 0, 0.8), 0.2) => rgba(0, 0, 0, 0.6)
```

参数：

- $ color（Color）
- $ amount（Number） - 减少不透明度的数量，介于0和1之间

返回：

- (Color)

增加：

- （ArgumentError） - 如果$ amount超出范围，或者两个参数都是错误的类型

### type_of($value)

返回值的类型。

例子：

```javascript
type-of(100px)  => number
type-of(asdf)   => string
type-of("asdf") => string
type-of(true)   => bool
type-of(#fff)   => color
type-of(blue)   => color
type-of(null)   => null
type-of(a b c)  => list
type-of((a: 1, b: 2)) => map
type-of(get-function("foo")) => function
```

参数：

- $ value（Base） - 要检查的值

返回：

- （String） - 值类型的未加引号的字符串名称

### unique_id

返回唯一的CSS标识符。该标识符作为未加引号的字符串返回。返回的标识符只保证在单个Sass运行范围内是唯一的。

返回：

- (String)

### unit($number)

返回与数字关联的单位。相关联的单位按分子和分母的字母顺序排序。

例子：

```javascript
unit(100) => ""
unit(100px) => "px"
unit(3em) => "em"
unit(10px * 5em) => "em*px"
unit(10px * 5em / 30cm / 1rem) => "em*px/cm*rem"
```

参数：

- $number (Number)

返回：

- （String） - 数字的单位，作为引用的字符串

增加：

- （ArgumentError） - 如果$ number不是数字

### unitless($number)

返回一个是否有单位的数字。

例子：

```javascript
unitless(100) => true
unitless(100px) => false
```

参数：

- $number (Number)

返回：

- (Bool)

增加：

- （ArgumentError） - 如果$ number不是数字

### unquote($string)

从字符串中删除引号。如果该字符串已经没有引用，这将返回未修改。

例子：

```javascript
unquote("foo") => foo
unquote(foo) => foo
```

参数：

- $string (String)

返回：

- (String)

增加：

- （ArgumentError） - 如果$ string不是字符串

### variable_exists($name)

检查具有给定名称的变量是否存在于当前作用域或全局作用域中。

例子：

```javascript
$a-false-value: false;
variable-exists(a-false-value) => true
variable-exists(a-null-value) => true

variable-exists(nonexistent) => false
```

参数：

- $ name（String） - 要检查的变量的名称。该名称不应包含$。

返回：

- （Bool） - 变量是否在当前范围内定义。

### zip($lists...)

将多个列表组合到一个多维列表中。结果列表的第n个值是源列表的第n个值的空格分隔列表。

结果列表的长度是最短列表的长度。

例子：

```javascript
zip(1px 1px 3px, solid dashed solid, red green blue)
=> 1px solid red, 1px dashed green, 3px solid blue
```

参数：

- $lists (Base)

返回：

- (List)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-06