# 接口 | API

- 贡献者1人

  

## 全局配置

`Vue.config`是一个包含Vue全局配置的对象。在引导应用程序之前，您可以修改下面列出的属性：

### 调试

- **类型：** `Boolean`
- **默认：** `false`
- **用法：** Vue.config.debug = true在调试模式下，Vue将：

```js
1.  Print stack traces for all warnings.
2.  Make all anchor nodes visible in the DOM as Comment nodes. This makes it easier to inspect the structure of the rendered result.
```

调试模式仅在开发版本中可用。

### 分隔符

- **类型：** `Array<String>`
- **默认：** `["{{", "}}"]`
- **用法：** // ES6 template string style Vue.config.delimiters ='$ {'，'}'更改纯文本插值分隔符。

### unsafeDelimiters

- **类型：** `Array<String>`
- **默认：** `["{{{", "}}}"]`
- **用法：** //让它看起来更危险Vue.config.unsafeDelimiters ='{!!'，'!!}'更改原始HTML插值分隔符。

### **silent**

- **类型：** `Boolean`
- **默认：** `false`
- **用法：** Vue.config.silent = true禁止所有Vue.js日志和警告。

### async

- **类型：** `Boolean`
- **默认：** `true`
- **用法：** Vue.config.async = false当异步模式关闭时，Vue将在检测到数据更改时同步执行所有DOM更新。这可能有助于在某些情况下进行调试，但也可能导致性能下降并影响调用观察程序回调的顺序。**不建议在生产中使用。async: false**

### devtools

- **类型：** `Boolean`
- **默认值：** `true`（`false`生产版本）
- **用法：** //确保在加载Vue Vue.config.devtools = true后立即同步设置此配置是否允许[vue-devtools](https://github.com/vuejs/vue-devtools)检查。此选项的默认值`true`位于开发版本和`false`生产版本中。您可以将其设置`true`为启用生产构建的检查。

## 全局 API

### **Vue.extend( options )**

- **参数：**

```js
- `{Object} options`
```

- **用法：** 创建基础Vue构造函数的“子类”。参数应该是包含组件选项的对象。这里需要注意的特殊情况是`el`和`data`选项 - 它们在使用时必须是函数`Vue.extend()`。<div id =“mount-point”> </ div> //创建可重用构造函数var Profile = Vue.extend（{template：'<p > {{firstName}} {{lastName}}又名{{别名}} </ p>'}）//创建一个配置文件的实例var profile = new Profile（{data：{firstName：'Walter'，lastName：' White'，别名：'Heisenberg'}}）//将它装载到元素配置文件中$ mount（'＃mount-point'）将导致：<p> Walter White又名Heisenberg </ p>
- **另请参阅：**组件

### **Vue.nextTick( callback )**

- **参数：**

```js
- `{Function} callback`
```

- **用法：** 推迟在下一个DOM更新周期后执行的回调。在您更改某些数据以等待DOM更新后立即使用它。//修改数据vm.msg ='Hello'// DOM未更新Vue.nextTick（function（）{// DOM updated}）
- **另请参阅：**异步更新队列

### **Vue.set( object, key, value )**

- **参数：**

```js
- `{Object} object`
- `{String} key`
- `{*} value`
```

- **返回：**设定值。
- **用法：** 在对象上设置属性。如果对象处于被动状态，请确保将该属性创建为反应属性并触发视图更新。这主要用于解决Vue无法检测属性添加的限制。
- **另见：**深度的反应性

### **Vue.delete( object, key )**

- **参数：**

```js
- `{Object} object`
- `{String} key`
```

- **用法：** 删除对象的属性。如果对象处于被动状态，请确保删除触发器查看更新。这主要用于解决Vue无法检测属性删除的限制，但您应该很少需要使用它。
- **另见：**深度的反应性

### Vue.directive（id，**definition）**

- **参数：**

```js
- `{String} id`
- `{Function | Object} [definition]`
```

- **用法：** 注册或检索全局指令。// register Vue.directive（'my-directive'，{bind：function（）{}，update：function（）{}，unbind：function（）{}}）// register（简单函数指令）Vue.directive （'my-directive'，function（）{//这将被调用为`update`}）// getter，如果注册返回指令定义var myDirective = Vue.directive（'my-directive'）
- **另请参阅：**自定义指令

### Vue.elementDirective( id, definition )

- **参数：**

```js
- `{String} id`
- `{Object} [definition]`
```

- **用法：** 注册或检索全局元素指令。//注册Vue.elementDirective（'my-element'，{bind：function（）{}，//元素指令不使用`update` unbind：function（）{}}）// getter，返回指令定义（如果已注册）var myDirective = Vue.elementDirective（'my-element'）
- **另见：**元素指令

### Vue.filter( id, definition )

- **参数：**

```js
- `{String} id`
- `{Function | Object} [definition]`
```

- **用法：** 注册或检索全局过滤器。// register Vue.filter（'my-filter'，function（value）{// return processed value}）//双向过滤器Vue.filter（'my-filter'，{read：function（）{}，write ：function（）{}}）// getter，如果注册返回过滤器var myFilter = Vue.filter（'my-filter'）
- **另请参阅：**自定义过滤器

### Vue.component( id, definition )

- **参数：**

```js
- `{String} id`
- `{Function | Object} [definition]`
```

- **用法：** 注册或检索全局组件。//注册一个扩展构造函数Vue.component（'my-component'，Vue.extend（{/ * ... * /}））//注册一个选项对象（自动调用Vue.extend）Vue.component（'my -component'，{/ * ... * /}）//获取一个已注册的组件（总是返回构造函数）var MyComponent = Vue.component（'my-component'）
- **另请参阅：**组件。

### Vue.transition( id, hooks )

- **参数：**

```js
- `{String} id`
- `{Object} [hooks]`
```

- **用法：** 注册或检索全局转换挂钩对象。//注册Vue.transition（'fade'，{enter：function（）{}，leave：function（）{}}）//检索已注册的钩子var fadeTransition = Vue.transition（'fade'）
- **另请参阅：**转场。

### Vue.partial( id, partial )

- **参数：**

```js
- `{String} id`
- `{String} [partial]`
```

- **用法：** 注册或检索全局模板部分字符串。//注册Vue.partial（'my-partial'，'<div> Hi </ div>'）//检索已注册的部分var myPartial = Vue.partial（'my-partial'）
- **另见：**特殊元素 - <partial>。

### Vue.use( plugin, options )

- **参数：**

```js
- `{Object | Function} plugin`
- `{Object} [options]`
```

- **用法：** 安装一个Vue.js插件。如果插件是一个对象，它必须公开一个`install`方法。如果它本身是一个函数，它将被视为安装方法。安装方法将以Vue作为参数来调用。
- **另请参阅：**插件。

### Vue.mixin( mixin )

- **参数：**

```js
- `{Object} mixin`
```

- **用法：** 全局应用mixin，这会影响后面创建的每个Vue实例。这可以被插件作者用来将自定义行为注入组件。**不在应用程序代码中推荐**。
- **另请参阅：**全局混合

## Options / Data

### data

- **类型：** `Object | Function`
- **限制：**仅`Function`在组件定义中使用时才被接受。
- **详细信息：** Vue实例的数据对象。Vue.js将递归地将它的属性转换为getter / setters以使其成为“被动”。**该对象必须是普通的**：本地对象，现有的getter / setter和原型属性将被忽略。不建议观察复杂的物体。一旦创建实例，原始数据对象可以被访问为`vm.$data`。Vue实例还代理了在数据对象上找到的所有属性。与启动属性`_`或`$`将**不**被在Vue公司的实例代理，因为它们可能与Vue公司的内部属性和API方法相冲突。你将不得不作为访问它们`vm.$data._property`。定义**组件时**，`data`必须声明为返回初始数据对象的函数，因为将会有许多使用相同定义创建的实例。如果我们仍然使用普通对象`data`，那么将**通过**创建的所有实例**的引用**来**共享**同一对象！通过提供一个`data`函数，每次创建新实例时，我们都可以简单地调用它来返回初始数据的全新副本。如果需要，可以通过`vm.$data`穿透获得原始对象的深层克隆`JSON.parse(JSON.stringify(...))`。
- **示例：** var data = {a：1} //直接创建实例var vm = new Vue（{data：data}）vm.a // - > 1 vm。$ data === data // - > true //必须在Vue.extend（）中使用函数var Component = Vue.extend（{data：function（）{return {a：1}}}）
- **另见：**深度的反应性。

### props

- **类型：** `Array | Object`
- **详细信息：** 用于接受来自父组件的数据的属性的列表/散列。它具有简单的基于数组的语法和基于对象的可选语法，允许进行高级配置，例如类型检查，自定义验证和默认值。
- **例子：** //简单的语法Vue.component（'props-demo-simple'，{props：'size'，'myMessage'}）//带有验证的对象语法Vue.component（'props-demo-advanced'，{props ：{//只是输入check size：Number，//输入check和其他验证名称：{type：String，required：true，//警告如果没有双向绑定twoWay：true}}}）
- **另请参阅：**Props

### propsData

> 1.0.22+

- **类型：** `Object`
- **限制：**仅在通过实例创建时受到尊重`new`。
- **详细信息：** 在创建过程中将道具传递给实例。这主要是为了使单元测试更容易。
- **示例：** var Comp = Vue.extend（{props：'msg'，template：'<div> {{msg}} </ div>'}）var vm = new Comp（{propsData：{msg：'hello'} }）

### **computed**

- **类型：** `Object`
- **详细信息：将** 计算属性混合到Vue实例中。所有getter和setter都将自己的`this`上下文自动绑定到Vue实例。
- **例如：** var vm = new Vue（{data：{a：1}，calculated：{//只获取，只需要一个函数aDouble：function（）{return this.a * 2}，//获取并设置aPlus ：{get：function（）{return this.a + 1}，set：function（v）{this.a = v - 1}}}}）vm.aPlus // - > 2 vm.aPlus = 3 vm。 a // - > 2 vm.aDouble // - > 4
- **也可以看看：**

```js
- [Computed Properties](../guide/computed)
- [Reactivity in Depth: Inside Computed Properties](../guide/reactivity#Inside-Computed-Properties)
```

### 方法

- **类型：** `Object`
- **详细信息：** 混合到Vue实例中的方法。您可以直接在VM实例上访问这些方法，或者在指令表达式中使用它们。所有方法都会将其`this`上下文自动绑定到Vue实例。
- **示例：** var vm = new Vue（{data：{a：1}，methods：{plus：function（）{this.a ++}}}）vm.plus（）vm.a // 2
- **另请参阅：**方法和事件处理

### watch

- **类型：** `Object`
- **详细信息：** 一个对象，其中键是表达式，值是相应的回调。该值也可以是方法名称的字符串，也可以是包含其他选项的对象。Vue实例将`$watch()`在实例化时调用对象中的每个条目。
- **示例：** var vm = new Vue（{data：{a：1}，watch：{'a'：function（val，oldVal）{console.log（'new：％s，old：％s'，val，oldVal ）}，// string method name'b'：'someMethod'，// deep watcher'c'：{handler：function（val，oldVal）{/ * ... * /}，deep：true}}}） vm.a = 2 // - > new：2，old：1
- **另请参阅：**实例方法 - vm。$ watch

## Options / DOM

### el

- **类型：** `String | HTMLElement | Function`
- **限制：**仅`Function`在组件定义中使用时才接受类型。
- **详细信息：** 为Vue实例提供一个现有的DOM元素进行安装。它可以是CSS选择器字符串，实际的HTMLElement或返回HTMLElement的函数。请注意，提供的元素仅用作安装点; 如果还提供了模板，它将被替换，除非`replace`设置为false。已解析的元素将可以被访问为`vm.$el`。在使用时`Vue.extend`，必须提供一个函数，以便每个实例获取单独创建的元素。如果此选项在实例化时可用，实例将立即输入编译; 否则，用户将不得不显式调用`vm.$mount()`手动启动编译。
- **另见：**生命周期图

### template

- **类型：** `String`
- **详细信息：** 用作Vue实例标记的字符串模板。默认情况下，该模板将**替换**已安装的元素。当`replace`选项设置为`false`，该模板将被插入到安装的元件来代替。在这两种情况下，挂起元素内的任何现有标记都将被忽略，除非模板中存在内容分发槽。如果字符串以`#`它作为开始，它将用作querySelector，并使用选定元素的innerHTML作为模板字符串。这允许使用通用的`<script type="x-template">`欺骗包括模板。请注意，在某些情况下，例如，当模板包含多个顶级元素或仅包含纯文本时，该实例将成为片段实例 - 即管理节点列表而不是单个节点的实例。片段实例的装入点上的非流量控制指令将被忽略。
- **也可以看看：**

```js
- [Lifecycle Diagram](../guide/instance#Lifecycle-Diagram)
- [Content Distribution](../guide/components#Content-Distribution-with-Slots)
- [Fragment Instance](../guide/components#Fragment-Instance)
```

### 替代

- **类型：** `Boolean`
- **默认：** `true`
- **限制：**只有在**模板**选项也存在的情况下才能使用。
- **详细信息：** 确定是否用模板替换正在安装的元素。如果设置为`false`，模板将覆盖元素的内部内容而不替换元素本身。如果设置为`true`，模板将覆盖该元素并将该元素的属性与该组件的根节点的属性合并。
- **例如**：<div id =“replace”class =“foo”> </ div> new Vue（{el：'#replace'，template：'<p class =“bar”> replaced </ p>'}）结果是：<p class =“foo bar”id =“replace”> replacement </ p>相比之下，当`replace`设置为false时：<div id =“insert”class =“foo”> < / div> new Vue（{el：'#insert'，replace：false，template：'<p class =“bar”> inserted </ p>'}）会导致：<div id =“insert”class = “foo”> <p class =“bar”>插入</ p> </ div>

## Options / Lifecycle Hooks

### init

- **类型：** `Function`
- **详细信息：** 在实例刚刚初始化后，在数据观察和事件/观察器设置之前同步调用。
- **另见：**生命周期图

### 创建

- **类型：** `Function`
- **详细信息：** 在实例创建后同步调用。在这个阶段，实例已经完成了处理选项，这意味着已经设置了以下内容：数据观察，计算属性，方法，监视/事件回调。但是，DOM编译尚未启动，该`$el`属性将不可用。
- **另见：**生命周期图

### beforeCompile

- **类型：** `Function`
- **详细信息：** 在编译开始之前调用。
- **另见：**生命周期图

### 编译

- **类型：** `Function`
- **详细信息：** 编译完成后调用。在这个阶段，所有的指令都已经被链接，所以数据的改变会触发DOM更新。但是，`$el`不能保证已经插入到文档中。
- **另见：**生命周期图

### ready

- **类型：** `Function`
- **详细说明：** 编译后调用**，并**在`$el`被**插入到首次的文件**，即第一后立即`attached`挂机。注意，这个插入必须通过Vue执行（使用方法`vm.$appendTo()`或者作为指令更新的结果）来触发`ready`钩子。
- **另见：**生命周期图

### 附

- **类型：** `Function`
- **详细信息：**`vm.$el`通过指令或VM等实例方法 调用何时附加到DOM `$appendTo()`。的直接操作`vm.$el`将**不会**触发这个钩子。

### **detached**

- **类型：** `Function`
- **详细信息：**`vm.$el`通过指令或VM实例方法 调用何时从DOM中删除。的直接操作`vm.$el`将**不会**触发这个hook。

### beforeDestroy

- **类型：** `Function`
- **详细信息：** 在Vue实例被销毁之前调用。在这个阶段，实例仍然功能齐全。
- **另见：**生命周期图

### **destroyed**

- **类型：** `Function`
- **详细信息：** 在Vue实例被销毁后调用。当这个钩子被调用时，Vue实例的所有绑定和指令都已被解除绑定，并且所有子Vue实例也被销毁。请注意，如果有离开转换，则在转换完成**后**`destroyed`调用挂接。
- **另见：**生命周期图

## Options / Assets

### 指令

- **类型：** `Object`
- **详细信息：** Vue实例可用的指令散列。
- **也可以参看：**

```js
- [Custom Directives](../guide/custom-directive)
- [Assets Naming Convention](../guide/components#Assets-Naming-Convention)
```

### elementDirectives

- **类型：** `Object`
- **详细信息：** 要使Vue实例可用的元素指令的哈希值。
- **也可以参看：**

```js
- [Element Directives](../guide/custom-directive#Element-Directives)
- [Assets Naming Convention](../guide/components#Assets-Naming-Convention)
```

### 过滤器

- **类型：** `Object`
- **详细信息：** Vue实例可用的过滤器哈希。
- **也可参看：**

```js
- [Custom Filters](../guide/custom-filter)
- [Assets Naming Convention](../guide/components#Assets-Naming-Convention)
```

### 组件

- **类型：** `Object`
- **详细信息：** 要提供给Vue实例的组件散列。
- **也可参看：**

```js
- [Components](../guide/components)
```

### 过渡

- **类型：** `Object`
- **Details：** 对Vue实例可用的转换的散列。
- **也可参看：**

```js
- [Transitions](../guide/transitions)
```

### partials

- **类型：** `Object`
- **详细信息：** Vue实例可用的部分字符串散列。
- **也可参看：**

```js
- [Special Elements - partial](about:blank#partial)
```

## Options / Misc

### 父类

- **类型：** `Vue instance`
- **详细信息：** 指定要创建的实例的父实例。建立两者之间的亲子关系。`this.$parent`对于孩子来说，父母可以被访问，并且孩子将被推入父母的`$children`数组中。
- **另见：**父子节点关系

### events

- **类型：** `Object`
- **详细信息：** 键是要监听的事件的对象，值是相应的回调。注意这些是Vue事件而不是DOM事件。该值也可以是方法名称的字符串。Vue实例将`$on()`在实例化时调用对象中的每个条目。
- **示例：** var vm = new Vue（{events：{'hook：created'：function（）{console.log（'created！'）}，greeting：function（msg）{console.log（msg）}，//也可以使用字符串方法bye：'sayGoodbye'}，方法：{sayGoodbye：function（）{console.log（'goodbye！'）}}}）// - > created！vm。$ emit（'greeting'，'hi！'）// - >嗨！vm。$ emit（'bye'）// - >再见！
- **也可参看：**

```js
- [Instance Methods - Events](about:blank#Instance-Methods-Events)
- [Parent-Child Communication](../guide/components#Parent-Child-Communication)
```

### mixins

- **类型：** `Array`
- **详细信息：** 该`mixins`选项接受一组mixin对象。这些mixin对象可以像普通实例对象一样包含实例选项，并且它们将与使用相同选项合并逻辑的最终选项合并`Vue.extend()`。例如，如果你的mixin包含一个创建的钩子，并且组件本身也有一个，那么这两个函数都会被调用。Mixin钩子按照它们提供的顺序调用，并在组件自己的钩子之前调用。
- **例如：** var mixin = {created：function（）{console.log（1）}} var vm = new Vue（{created：function（）{console.log（2）}，mixins：mixin}）// - > 1 // - > 2
- **另见：** Mixins

### 命名

- **类型：** `String`
- **限制：**只有在使用时才受到尊重`Vue.extend()`。
- **详细信息：** 允许组件在其模板中递归调用其自身。请注意，当组件全局注册时`Vue.component()`，全局ID会自动设置为其名称。指定`name`选项的另一个好处是控制台检查。在控制台中检查扩展的Vue组件时，默认的构造函数名称是`VueComponent`，这是不是很丰富。通过传递可选`name`选项`Vue.extend()`，您将获得更好的检查输出，以便您知道您正在查看哪个组件。该字符串将被骆驼化并用作组件的构造函数名称。
- **例子：** var Ctor = Vue.extend（{name：'stack-overflow'，template：'<div>'// // recursively invoke self'<stack-overflow> </ stack-overflow>'+'</ div> '}）//这实际上会导致超过最大堆栈大小//错误，但让我们假设它可以工作... var vm = new Ctor（）console.log（vm）// - > StackOverflow {$ el：null ，...}

### 扩展

> 1.0.22+

- **类型：** `Object | Function`
- **细节：** 允许声明性地扩展另一个组件（可以是简单的选项对象或构造函数）而无需使用`Vue.extend`。这主要是为了更容易在单个文件组件之间进行扩展。这与之类似`mixins`，不同之处在于组件自身的选项比源组件的扩展优先级更高。
- **示例：** var CompA = {...} //扩展CompA，而不必在任何变种上调用Vue.extend CompB = {extends：CompA，...}

## 实例属性

### vm.$data

- **类型：** `Object`
- **详细信息：** Vue实例正在观察的数据对象。你可以把它换成一个新的对象。Vue实例代理访问其数据对象上的属性。

### vm.$el

- **类型：** `HTMLElement`
- **只读**
- **详细信息：** Vue实例正在管理的DOM元素。请注意，对于片段实例，`vm.$el`将返回一个指示片段起始位置的锚节点。

### vm.$options

- **类型：** `Object`
- **只读**
- **详细信息：** 用于当前Vue实例的实例化选项。当你想在选项中包含自定义属性时这很有用：new Vue（{customOption：'foo'，created：function（）{console.log（this。$ options.customOption）// - >'foo'}} ）

### vm.$parent

- **类型：** `Vue instance`
- **只读**
- **详细信息：** 父实例，如果当前实例有一个实例。

### vm.$root

- **类型：** `Vue instance`
- **只读**
- **详细信息：** 当前组件树的根Vue实例。如果当前实例没有父母，那么这个值就是它自己。

### vm.$children

- **类型：** `Array<Vue instance>`
- **只读**
- **详细信息：** 当前实例的直接子组件。

### vm.$refs

- **类型：** `Object`
- **只读**
- **详细信息：** 保存已`v-ref`注册的子组件的对象。
- **也可参看：**

```js
- [Child Component Refs](../guide/components#Child-Component-Refs)
-  [v-ref](about:blank#v-ref).
```

### vm.$els

- **类型：** `Object`
- **只读**
- **详细信息：** 保存已`v-el`注册DOM元素的对象。
- **另见：** v-el。

## Instance Methods / Data

### vm.$watch( expOrFn, callback, options )

- **参数：**

```js
- `{String | Function} expOrFn`
- `{Function} callback`
-  `{Object} [options]`
    - `{Boolean} deep`
    - `{Boolean} immediate`
```

- **返回：** `{Function} unwatch`
- **用法：** 观察Vue实例上的表达式或计算的函数以进行更改。回调被调用新值和旧值。该表达式可以是单个键路径或任何有效的绑定表达式。

注意：在对对象或数组进行变异（而不是替换）时，旧值将与新值相同，因为它们引用相同的对象/数组。Vue不保留pre-mutate值的副本。

- **例如：** // keypath vm。$ watch（'abc'，function（newVal，oldVal）{// do something}）// expression vm。$ watch（'a + b'，function（newVal，oldVal）{// （）函数vm。$ watch（function（）{return this.a + this.b}，function（newVal，oldVal）{// do something}） `vm.$watch`返回一个unwatch函数，停止触发回调函数：var unwatch = vm。$ watch（'a'，cb）//稍后，拆散观察者unwatch（）
- **选项：deep** 为了也检测对象内嵌套的值更改，您需要传入`deep: true`选项参数。请注意，您不需要这样做来侦听Array突变。vm。$ watch（'someObject'，callback，{deep：true}）vm.someObject.nestedValue = 123 //回调被触发
- **选项：立即** 传入`immediate: true`选项将立即用表达式的当前值触发回调：vm。$ watch（'a'，callback，{immediate：true}）//立即触发回调，当前值为`a`

### vm.$get( expression )

- **参数：**

```js
- `{String} expression`
```

- **用法：** 从给定表达式的Vue实例中检索一个值。抛出错误的表达式将被抑制并返回`undefined`。
- **例如：** var vm = new Vue（{data：{a：{b：1}}}）vm。$ get（'a.b'）// - > 1 vm。$ get（'ab + 1'）/ / - > 2

### vm.$set( keypath, value )

- **参数：**

```js
- `{String} keypath`
- `{*} value`
```

- **用法：** 在给定有效的keypath的Vue实例上设置一个数据值。在大多数情况下，您应该更喜欢使用简单对象语法设置属性，例如`vm.a.b = 123`。该方法仅在两种情况下需要：

```js
1.  When you have a keypath string and want to dynamically set the value using that keypath.
2.  When you want to set a property that doesn’t exist.
```

如果路径不存在，它将被递归创建并被动。如果由于`$set`调用而创建新的根级别反应属性，则Vue实例将被强制进入“摘要循环”，在此期间所有观察者都将被重新评估。

- **例如：** var vm = new Vue（{data：{a：{b：1}}}）//设置现有路径vm。$ set（'a.b'，2）vm.ab // - > 2 / /设置一个不存在的路径，将强制digest vm。$ set（'c'，3）vm.c // - >
- **另见：**深度的反应性

### vm.$delete( key )

- **参数：**

```js
- `{String} key`
```

- **用法：** 删除Vue实例（及其`$data`）的根级别属性。强制消化循环。不建议。

### vm.$eval( expression )

- **参数：**

```js
- `{String} expression`
```

- **用法：** 评估当前实例上的有效绑定表达式。该表达式还可以包含过滤器。
- **例如：** //假设vm.msg ='hello'vm。$ eval（'msg | uppercase'）// - >'HELLO'

### vm.$interpolate( templateString )

- **参数：**

```js
- `{String} templateString`
```

- **用法：** 评估一段包含胡须插值的模板字符串。请注意，此方法仅执行字符串插值; 属性指令被忽略。
- **例如：** //假设vm.msg ='hello'vm。$ interpolate（'{{msg}} world！'）// - >'hello world！'

### vm.$log( keypath )

- **参数：**

```js
- `{String} [keypath]`
```

- **用法：** 将当前实例数据记录为一个普通对象，该对象比一组getter / setter更易于检查。也接受一个可选的密钥。vm。$ log（）//记录整个ViewModel数据vm。$ log（'item'）// logs vm.item

## Instance Methods / Events

### vm.$on( event, callback )

- **参数：**

```js
- `{String} event`
- `{Function} callback`
```

- **用法：** 监听当前虚拟机上的自定义事件。事件可以由被触发`vm.$emit`，`vm.$dispatch`或`vm.$broadcast`。该回调将接收传入这些事件触发方法的所有附加参数。
- **例如：** vm。$ on（'test'，function（msg）{console.log（msg）}）vm。$ emit（'test'，'hi'）// - >“hi”

### vm.$once( event, callback )

- **参数：**

```js
- `{String} event`
- `{Function} callback`
```

- **用法：** 听取自定义事件，但只能听一次。侦听器第一次触发后会被删除。

### vm.$off( event, callback )

- **参数：**

```js
- `{String} [event]`
- `{Function} [callback]`
```

- **用法：** 删除事件侦听器。

```js
-  If no arguments are provided, remove all event listeners;
-  If only the event is provided, remove all listeners for that event;
-  If both event and callback are given, remove the listener for that specific callback only.
```

### vm.$emit( event, …args )

- **参数：**

```js
- `{String} event`
- `[...args]`
```

触发当前实例上的事件。任何其他参数都将传递到侦听器的回调函数中。

### vm.$dispatch( event, …args )

- **参数：**

```js
- `{String} event`
- `[...args]`
```

- **用法：** 派发一个事件，首先在实例本身触发它，然后沿着父链向上传播。传播在触发父事件侦听器时停止，除非该侦听器返回`true`。任何其他参数都将传递给侦听器的回调函数。
- **例如：** //创建父链var parent = new Vue（）var child1 = new Vue（{parent：parent}）var child2 = new Vue（{parent：child1}）parent。$ on（'test'，function（ ）{console.log（'parent notifications'）}）child1。$ on（'test'，function（）{console.log（'child1 notifications'）}）child2。我们不会通知你，因为child1没有返回/ /在其回调中为true
- **另见：**父子节点关系

### vm.$broadcast( event, …args )

- **参数：**

```js
- `{String} event`
- `[...args]`
```

- **用法：** 广播一个向下传播到当前实例的所有后代的事件。由于后代扩展为多个子树，事件传播将遵循许多不同的“路径”。除非回调函数返回，否则沿路径发出侦听器回调时，每条路径的传播都将停止`true`。
- **例如：** var parent = new Vue() // child1 and child2 are siblings var child1 = new Vue({ parent: parent }) var child2 = new Vue({ parent: parent }) // child3 is nested under child2 var child3 = new Vue({ parent: child2 }) child1.$on('test', function () { console.log('child1 notified') }) child2.$on('test', function () { console.log('child2 notified') }) child3.$on('test', function () { console.log('child3 notified') }) parent.$broadcast('test') // -> "child1 notified" // -> "child2 notified" // child3不会被通知，因为child2在回调中没有返回true

## Instance Methods / DOM

### vm.$appendTo( elementOrSelector, callback )

- **参数：**

```js
- `{Element | String} elementOrSelector`
- `{Function} [callback]`
```

- **返回：** `vm` - 实例本身
- **用法：** 将Vue实例的DOM元素或片段追加到目标元素。目标可以是元素或querySelector字符串。如果存在，此方法将触发转换。转换完成后触发回调（或者如果没有触发转换，立即触发回调）。

### vm.$before( elementOrSelector, callback )

- **参数：**

```js
- `{Element | String} elementOrSelector`
- `{Function} [callback]`
```

- **返回：** `vm` - 实例本身
- **用法：** 在目标元素之前插入Vue实例的DOM元素或片段。目标可以是元素或querySelector字符串。如果存在，此方法将触发转换。转换完成后触发回调（或者如果没有触发转换，立即触发回调）。

### vm.$after( elementOrSelector, callback )

- **参数：**

```js
- `{Element | String} elementOrSelector`
- `{Function} [callback]`
```

- **返回：** `vm` - 实例本身
- **用法：** 在目标元素之后插入Vue实例的DOM元素或片段。目标可以是元素或querySelector字符串。如果存在，此方法将触发转换。转换完成后触发回调（或者如果没有触发转换，立即触发回调）。

### vm.$remove( callback )

- **参数：**

```js
- `{Function} [callback]`
```

- **返回：** `vm` - 实例本身
- **用法：** 从DOM中移除Vue实例的DOM元素或片段。如果存在，此方法将触发转换。转换完成后触发回调（或者如果没有触发转换，立即触发回调）。

### vm.$nextTick( callback )

- **参数：**

```js
- `{Function} [callback]`
```

- **用法：** 推迟在下一个DOM更新周期后执行的回调。在您更改某些数据以等待DOM更新后立即使用它。这与全局相同`Vue.nextTick`，只是回调的`this`上下文自动绑定到调用此方法的实例。
- **例子：** new Vue({ // ... methods: { // ... example: function () { // modify data this.message = 'changed' // DOM is not updated yet this.$nextTick(function () { // DOM is now updated // `this`绑定到当前实例this.doSomethingElse（）}）}}}）
- **也可参看：**

```js
- [Vue.nextTick](about:blank#Vue-nextTick)
- [Async Update Queue](../guide/reactivity#Async-Update-Queue)
```

## 实例方法/生命周期

### vm.$mount( elementOrSelector )

- **参数：**

```js
- `{Element | String} [elementOrSelector]`
```

- **返回：** `vm` - 实例本身
- **用法：** 如果Vue实例`el`在实例化时没有收到选项，它将处于“未安装”状态，没有关联的DOM元素或片段。`vm.$mount()`可用于手动启动挂载/编译未挂载的Vue实例。如果没有提供参数，模板将被创建为文档外的片段，您将不得不使用其他DOM实例方法将其插入到文档中。如果`replace`选项设置为`false`，则空白`<div>`将自动创建为包装元素。调用`$mount()`已挂载的实例将不起作用。该方法返回实例本身，因此您可以在其之后链接其他实例方法。
- **示例：** var MyComponent = Vue.extend（{template：'<div> Hello！</ div>'}）//创建并挂载到#app（将替换#app）new MyComponent（）。$ mount（'＃app '）//以上内容与新的MyComponent（{el：'#app'}）相同//或者编译脱离文件并追加：new MyComponent（）。$ mount（）。$ appendTo（'容器'）
- **另见：**生命周期图

### vm.$destroy( remove )

- **参数：**

```js
- `{Boolean} [remove] - default: false`
```

- **用法：** 彻底摧毁虚拟机。清理与其他现有vms的连接，解除其所有指令的绑定，关闭所有事件侦听器，并且如果`remove`参数为true，则从DOM中删除其关联的DOM元素或片段。触发`beforeDestroy`和`destroyed`钩。
- **另见：**生命周期图

## Directives

### v-text

- **预计：** `String`
- **详细信息：** 更新元素的`textContent`。在内部，`{{ Mustache }}`内插也被编译为`v-text`textNode上的指令。指令形式需要包装器元素，但性能稍好，并避免FOUC（未编译内容的Flash）。
- **例如：** <span v-text =“msg”> </ span> <！ - 与 - > <span> {{msg}} </ span>相同

### v-html

- **预计：** `String`
- **详细信息：** 更新元素的`innerHTML`。内容以纯HTML格式插入 - 数据绑定被忽略。如果您需要重新使用模板片段，则应该使用partials。在内部，`{{{ Mustache }}}`插值也被编译为`v-html`使用锚节点的指令。指令形式需要包装器元素，但性能稍好，并避免FOUC（未编译内容的Flash）。在您的网站上动态呈现任意HTML可能非常危险，因为它很容易导致[XSS攻击](https://en.wikipedia.org/wiki/Cross-site_scripting)。只能使用`v-html`可信内容，**绝对不能**使用用户提供的内容。
- **例如：** <div v-html =“html”> </ div> <！ - 与 - > <div> {{{}}} </ div>

### v-if

- **预计：** `*`
- **用法：** 根据表达式值的真实性有条件地渲染元素。元素及其包含的数据绑定/组件在切换期间被销毁并重新构建。如果该元素是一个`<template>`元素，则其内容将被提取为条件块。
- **另请参阅：**条件呈现

### v-show

- **预计：** `*`
- **用法：**`display`根据表达式值的真实性 切换元素的CSS属性。触发器如果存在则转换。
- **另请参阅：**条件呈现 - v展示

### v-else

- **不期望表达**
- **限制：**以前的兄弟元素必须有`v-if`或`v-show`。
- **用法：** 表示“else块”为`v-if`和`v-show`<div v-if="Math.random() > 0.5”>对不起</ DIV> <DIV V-else>不好意思</ DIV>。
- **另请参阅：**条件呈现 - v-else
- **另请参阅：**条件呈现 - 组件警告

### v-for

- **预计：** `Array | Object | Number | String`
- **参数属性：**

```js
- [`track-by`](../guide/list#track-by)
- [`stagger`](../guide/transitions#Staggering-Transitions)
- [`enter-stagger`](../guide/transitions#Staggering-Transitions)
- [`leave-stagger`](../guide/transitions#Staggering-Transitions)
```

- **用法：** 根据源数据多次渲染元素或模板块。该指令的值必须使用特殊语法`alias (in|of) expression`为当前正在迭代的元素提供别名：<div v-for =“item in items”> {{item.text}} </ div>注意使用`of`作为分隔符只支持1.0.17+。或者，您还可以为索引指定别名（或用于对象的键）：项目中的<div v-for =“（index，item）> </ div> <div v-for =”（ key），val）in object“> </ div>`v-for`的详细用法在下面链接的指南部分进行了说明。
- **另请参阅：**列表渲染。

### v-on

- **速记：** `@`
- **预计：** `Function | Inline Statement`
- **论据：** `event (required)`
- **修饰符：**

```js
-  `.stop` - call `event.stopPropagation()`.
-  `.prevent` - call `event.preventDefault()`.
-  `.capture` - add event listener in capture mode.
-  `.self` - only trigger handler if event was dispatched from this element.
-  `.{keyCode | keyAlias}` - only trigger handler on certain keys.
```

- **用法：** 将一个事件监听器附加到该元素。事件类型由参数表示。该表达式既可以是方法名称也可以是内联语句，或者在存在修饰符时简单地省略。在普通元素上使用时，它只侦听**本地DOM事件**。在自定义元素组件上使用时，它还会侦听在该子组件上发出的**自定义事件**。在监听本地DOM事件时，该方法接收本地事件作为唯一参数。如果使用内联语句，则语句可以访问特殊`$event`属性：`v-on:click="handle('ok', $event)"`。 **1.0.11+**在收听自定义事件时，内联语句可以访问特殊事件`$arguments`属性，它是传递给子组件`$emit`调用的附加参数的数组。
- **例：**<！ - 方法处理程序 - > <button v-on：click =“doThis”> </ button>
- <！ - 内联语句 - > <button v-on：click =“doThat（'hello'，$事件）“> </ button>
- <！ - shorthand - > <button @click =”doThis“> </ button>
- <！ - 停止传播 - > <button @ click.stop =”doThis“> < / button>
- <！ - 防止默认 - > <button @ click.prevent =“doThis”> </ button>
- <！ - 防止默认无表达式 - > <form @ submit.prevent> </ form>
- < ！ - 链修饰符 - > <button @ click.stop.prevent =“doThis”> </ button>
- <！ - 使用keyAlias的键修饰符 - > <input @ keyup.enter =“onEnter”>
- <！ - - 使用keyCode的键修饰符 - >
- <input @ keyup.13 =“onEnter”>监听子组件上的自定义事件（当孩子发出“my-event”时会调用该处理程序）：<my-component @ my-event =“handleThis”> </ my-component>
- <！ - inline语句 - > <my-component @ my-event =“handleThis（123，$ arguments）”> </ my-component>
- **另请参阅：**方法和事件处理

### v-bind

- **速记：** `:`
- **预计：** `* (with argument) | Object (without argument)`
- **论据：** `attrOrProp (optional)`
- **修饰符：**

```js
-  `.sync` - make the binding two-way. Only respected for prop bindings.
-  `.once` - make the binding one-time. Only respected for prop bindings.
-  `.camel` - convert the attribute name to camelCase when setting it. Only respected for normal attributes. Used for binding camelCase SVG attributes.
```

- **用法：** 将一个或多个属性或组件prop动态绑定到表达式。用于绑定`class`or `style`属性时，它支持其他值类型，如Array或Objects。有关更多详细信息，请参阅下面的链接指南部 当用于道具绑定时，道具必须在子组件中正确声明。支柱绑定可以使用其中一个修饰符指定不同的绑定类型。在没有参数的情况下使用时，可用于绑定包含属性名称 - 值对的对象。注意：在此模式下`class`和`style`不支持数组或对象。
- **例子：** <！ - 绑定一个属性 - > <img v-bind：src =“imageSrc”> <！ - shorthand - > <img：src =“imageSrc”> <！ - class binding - > <div class =“{red：isRed}”> </ div> <div class =“[classA，classB]”> </ div> <div class =“[classA，{classB：isB，classC： isC}]>> <！ - style binding - > <div：style =“{fontSize：size +'px'}”> </ div> <div style =“[styleObjectA，styleObjectB]”> </ div> <！ - 绑定属性对象 - > <div v-bind =“{id：someProp，'other-attr'：otherProp}”> </ div> <！ - prop绑定。“prop”必须在my-component中声明。 - > <my-component：prop =“someThing”> </ my-component>
- **也可参看：**

```js
- [Class and Style Bindings](../guide/class-and-style)
- [Component Props](../guide/components#Props)
```

### v-model

- **预计：**根据输入类型而变化
- **仅限于：**

```js
- `<input>`
- `<select>`
- `<textarea>`
```

- **参数属性：**

```js
- [`lazy`](../guide/forms#lazy)
- [`number`](../guide/forms#number)
- [`debounce`](../guide/forms#debounce)
```

- **用法：** 在表单输入元素上创建双向绑定。有关详细用法，请参阅下面链接的指南部分。
- **另请参阅：**表单输入绑定

### v-ref

- **不期望表达**
- **限于：**子组件
- **论据：** `id (required)`
- **用法：** 在其父级上注册对子组件的引用以供直接访问。不期望表达。必须提供一个参数作为注册的ID。组件实例将可以在其父`$refs`对象上访问。当与组件一起使用时`v-for`，注册值将是一个数组，其中包含与它们绑定的数组对应的所有子组件实例。如果数据源为`v-for`Object，则注册的值将是包含镜像源Object的键 - 实例对的Object。
- **注意：** 由于HTML不区分大小写，因此camelCase的用法`v-ref:someRef`将被转换为全部小写。你可以使用`v-ref:some-ref`正确的设置`this.$refs.someRef`。
- **示例：** <comp v-ref：child> </ comp> <comp v-ref：some-child> </ comp> //从父级访问：this。$ refs.child this。$ refs.someChild With `v-for`：<comp v-ref：list v-for =“item in list”> </ comp> //这是父数组中的数组$ refs.list
- **另请参阅：**子组件参考

### v-el

- **不期望表达**
- **论据：** `id (required)`
- **用法：** 在其所有者Vue实例的`$els`对象上注册对DOM元素的引用，以便于访问。
- **注意：** 由于HTML不区分大小写，因此camelCase的用法`v-el:someEl`将被转换为全部小写。你可以使用`v-el:some-el`正确的设置`this.$els.someEl`。
- **例如：** <span v-el：msg> hello </ span> <span v-el：other-msg> world </ span>。$ els.msg.textContent // - >“hello”this。$ els。 otherMsg.textContent // - >“world”

### v-pre

- **不期望表达**
- **用法** 跳过此元素及其所有子元素的编译。您可以使用它来显示原始胡须标签。跳过大量没有指令的节点也可以加快编译速度。
- **例如：** <span v-pre> {{this will be be compiled}} </ span>

### v-cloak

- **不期望表达**
- **用法：** 该指令将保留在元素上，直到关联的Vue实例完成编译。结合诸如CSS规则`[v-cloak] { display: none }`，该指令可用于隐藏未编译的胡须绑定，直到Vue实例准备就绪。
- **例如：** v-cloak {display：none; } <div v-cloak> {{message}} </ div>在编译完成之前，<div>不可见。

## 特殊元素

### 组件

- **属性：**

```js
- `is`
```

- **参数属性：**

```js
- [`keep-alive`](../guide/components#keep-alive)
- [`transition-mode`](../guide/components#transition-mode)
```

- **用法：** 调用组件的替代语法。主要用于具有以下`is`属性的动态组件：<！ - 动态组件由 - > <！ - vm上的`componentId`属性 - > <component：is =“componentId”> </ component>
- **另请参阅：**动态组件

### slot

- **属性：**

```js
- `name`
```

- **用法：** `<slot>`元素用作组件模板中的内容分发网点。插槽元素本身将被替换。具有该`name`属性的插槽称为命名插槽。指定的插槽将分发具有`slot`与其名称匹配的属性的内容。有关详细用法，请参阅下面链接的指南部分。
- **另请参阅：**使用插槽的内容分发

### 局部

- **属性：**

```js
- `name`
```

- **用法：** `<partial>`元素作为注册模板部分的出口。部分内容在插入时也由Vue编译。该`<partial>`元素本身将被替换。它需要一个`name`将用于解析部分内容的属性。
- **示例：** //注册一个局部Vue.partial（'my-partial'，'<p>这是一个局部！{{msg}} </ p>'）<！ - 静态局部 - > <局部名称=“my-partial”> </ partial> <！ - 一个动态部分 - > <！ - 渲染带有id的部分=== vm.partialId - > <partial v-bind：name =“partialId”> </ partial> <！ - 使用v-bind速记的动态部分 - > <partial：name =“partialId”> </ partial>

## 过滤器

### **capitalize**

- **例如：** {{msg | 大写}} *'abc'=>'Abc'*

### 大写

- **例如：** {{msg | 大写}} *'abc'=>'ABC'*

### 小写

- **例如：** {{msg | 小写}} *'ABC'=>'abc'*

### **currency**

- **参数：**

```js
- `{String} [symbol] - default: '$'`
-  **1.0.22+** `{Number} [decimal places] - default: 2` 
```

- **例如：** {{amount | currency}} *12345 => $ 12,345.00* 使用不同的符号：{{amount | currency'£'}} *12345 =>£12,345.00* 部分货币有3位或4位小数位，而其他货币则没有，例如日元（¥）或越南盾（₫）：{{amount | currency'₫'0}} *12345 =>₫12,345*

### pluralize

- **参数：**

```js
- `{String} single, [double, triple, ...]`
```

- **用法：** 根据过滤的值多重化参数。当只有一个参数时，复数形式最后只加一个“s”。当有多个参数时，参数将被用作对应于要被复数化的单个单，双，三...形式的字符串数组。当要复数的数字超过参数的长度时，它将使用数组中的最后一个条目。
- **例如：** {{count}} {{count | pluralize'item'}} *1 =>'1 item'2* *=>'2 items'* {{date}} {{date | 复数'st''nd''rd''th'}}将导致： *1 =>'1st'2* *=>'2nd'3* *=>'3rd'4* *=>'4th'5* *=>'5th'*

### json

- **参数：**

```js
- `{Number} [indent] - default: 2`
```

- **用法：** 输出调用结果`JSON.stringify()`而不是输出`toString()`值（例如`[object Object]`）。
- **示例：** 使用4空格缩进打印对象：<pre> {{nestedObject | json 4}} </ pre>

### debounce

- **限于：**期望值的指令`Function`，例如`v-on`
- **参数：**

```js
- `{Number} [wait] - default: 300`
```

- **用法：** 包装处理程序以`x`毫秒为单位去除它，`x`参数在哪里。默认的等待时间是300ms。一个去抖动的处理程序将被延迟，直到`x`通话结束后经过至少ms。如果在延迟时间之前再次调用处理程序，则延迟时间将重置为`x`ms。
- **例如：** <input @ keyup =“onKeyup | debounce 500”>

### limitBy

- **限于：**期望值的指令`Array`，例如`v-for`
- **参数：**

```js
- `{Number} limit`
- `{Number} [offset]`
```

- **用法：** 按照参数的指定将数组限制为前N个项目。<！ - 只显示前10项 - > <div v-for =“ items| limitBy 10 items> 5至15 - > <div v-for =“item in items | limitBy 10 5”> </ div>

### filterBy

- **限于：**期望值的指令`Array`，例如`v-for`
- **参数：**

```js
- `{String | Function} targetStringOrFunction`
- `"in" (optional delimiter)`
- `{String} [...searchKeys]`
```

- **用法：** 返回源数组的过滤版本。第一个参数可以是一个字符串或一个函数。当第一个参数是一个字符串时，它将被用作在数组的每个元素中搜索的目标字符串：items | filterBy'hello'“>在上面的例子中，只有项目将会显示包含目标字符串`“hello”`的信息。如果项目是一个对象，过滤器将递归搜索目标字符串的对象的每个嵌套属性。要缩小搜索范围，可以指定其他搜索关键字：<div v-for =“user in user | filterBy'Jack'in'name'”>在上例中，过滤器将只搜索`“Jack” `在每个用户对象的`name`字段中。**总是限制搜索范围以获得更好的性能是一个好主意**上面的例子使用静态参数 - 当然，我们可以使用动态参数作为目标字符串或搜索关键字。结合`v-model`，我们可以轻松实现输入提前过滤：<div id =“filter-by-example”> <input v-model =“name”> <ul> <li v-for =“user in用户名| filterBy name in'name'“> {{user.name}} </ li> </ ul> </ div> new Vue（{el：'＃filter-by-example'，data：{name：' '，users：[{name：'Bruce'}，{name：'Chuck'}，{name：'Jackie'}]}}）
- **其他示例：** 多个搜索键：<li v-for =“用户中的用户| filterBy'name''phone'中的searchText>”>> </ li>多个搜索键与动态数组参数：<！ - fields = ['使用自定义过滤器函数：<div v-for =“用户中的用户| filterBy myCustomFilterFunction”>用户在过滤器中使用自定义过滤器函数<fieldA'，'fieldB'] - > <div v-for =

### orderBy

- **限于：**期望值的指令`Array`，例如`v-for`
- **参数：**

```js
- `{String | Array<String> | Function} ...sortKeys`
- `{String} [order] - default: 1`
```

- **用法：** 返回源数组的排序版本。您可以传递任意数量的字符串来对键进行排序。如果您想使用自己的排序策略，也可以传递包含排序键或函数的数组。可选`order`参数指定结果是以升序（`order >= 0`）还是降序（`order < 0`）顺序排列。对于原始值数组，只需省略`sortKeys`并提供顺序，例如`orderBy 1`。
- **例：** 按名称排序用户：<ul> <li v-for =“user in users | orderBy'name'”> {{user.name}} </ li> </ ul>按降序排列：<ul> <li v -for =“user in users | orderBy'name'-1”> {{user.name}} </ li> </ ul>对原始值进行排序：<ul> <li v-for =“n in number | orderBy动态排序顺序：<div id =“orderby-example”> <button @ click =“order = order \ * -1”>反向排序顺序</ li> </ li>按钮> <ul> <li v-for =“user in users | orderBy'name'order”> {{user.name}} </ li>新vue（{el：'＃ orderby-example'，data：{order：1，users：[{name：'Bruce'}，{name：'Chuck'}，{name：'Jackie'}]}}）使用两个键进行排序：<ul> <li v-for =“user in user | orderBy'lastName''firstName'”> {{user.lastName}} {{user.firstName}} </ li> </ ul>使用函数排序：<div id =“orderby-compare-example”class =“demo”> <button @ click =“order = order \ * -1 “>反向排序顺序</ button> <ul> <li v-for =”user in users | orderBy ageByTen order“> {{user.name}} - {{user.age}} </ li> </ ul > </ div> new Vue（{el：'＃orderby-compare-example'，data：{order：1，users：[{name：'Jackie'，age：62}，{name：'Chuck'，age ：76}， { name: 'Bruce', age: 61 } }，方法：{ageByTen：function（a，b）{return Math.floor（a.age / 10） - Math.floor（b.age / 10）}}}）

## 数组扩展方法

Vue.js扩展`Array.prototype`了两个额外的方法，使得更容易执行一些常见的数组操作，同时确保被动更新被正确触发。

### array.$set(index, value)

- **参数**

```js
- `{Number} index`
- `{*} value`
```

- **用法** 通过索引将数组中的元素设置为值，并触发视图更新。vm.animals。$ set（0，{name：'Aardvark'}）
- **另见：**阵列检测警告

### array.$remove(reference)

- **参数**

```js
- `{Reference} reference`
```

- **用法** 通过引用从数组中删除元素并触发视图更新。这是一个用于首先搜索数组中的元素的糖方法，然后如果找到，调用`array.splice(index, 1)`。var aardvark = vm.animals0 vm.animals。$ remove（aardvark）
- **另见：**突变方法

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-04-12