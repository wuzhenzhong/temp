# .NET Core实战项目之CMS 第六章 入门篇-Vue的快速入门及其使用

2018.12.05 22:17:48字数 2396阅读 136

## 写在前面

上面文章我给大家介绍了Dapper这个ORM框架的简单使用，大伙会用了嘛！本来今天这篇文章是要讲Vue的快速入门的，原因是想在后面的文章中使用Vue进行这个CMS系统的后台管理界面的实现。但是奈何Vue实现的SPA有一定的门槛，不太适合新手朋友，所以为了照顾大多数人，我准备还是采用asp.net core mvc+html+js+css+layui这个传统的技术栈来实现。但是，不管怎么说我还是会把Vue的基本使用给大伙介绍一下！
当然，如果这篇文章我也是抱着学习的态度跟大家一起来了解Vue的，如果你想通过这篇文章就能熟练的使用Vue那你就太天真了！目前，作为后端的我对Vue的掌握也仅仅停留在入门阶段。后期再带着大家一起把这个项目升级到Vue吧！

> 本篇文章已经收纳入《[.NET Core实战项目之CMS 第一章 入门篇-开篇及总体规划](https://www.cnblogs.com/yilezhu/p/9977862.html)》另附上.NET Core实战项目交流群：637326624 有兴趣的朋友可以共同交流技术经验。
> 作者：依乐祝
> 原文地址：https://www.cnblogs.com/yilezhu/p/10035275.html

## Vue是什么

Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。
说白了Vue.js就是当下很火的一个JavaScript MVVM库（前端库）。相比于Angular.js，Vue.js提供了更加简洁、更易于理解的API，使得我们能够快速地上手并使用Vue.js。
如果你之前已经习惯了用jQuery操作DOM，学习Vue.js时请先抛开手动操作DOM的思维，因为Vue.js是数据驱动的，你无需手动操作DOM。它通过一些特殊的HTML语法，将DOM和数据绑定起来。一旦你创建了绑定，DOM将和数据保持同步，每当变更了数据，DOM也会相应地更新。
当然了，在使用Vue.js时，你也可以结合其他库一起使用，比如jQuery。

## Vue.js的特点

- 简洁： HTML 模板 + JSON 数据，再创建一个 Vue 实例，就这么简单。
- 数据驱动： 自动追踪依赖的模板表达式和计算属性。
- 组件化： 用解耦、可复用的组件来构造界面。
- 轻量： 生产版本删除了警告后共30.90KB min+gzip，无依赖（2.5.17版本）。
- 快速： 精确有效的异步批量 DOM 更新。
- 模块友好： 通过 NPM 或 Bower 安装，无缝融入你的工作流。

## 如何学习Vue.js

这里介绍几个比较好的Vue学习的网站，都是免费的！大伙可以跟着系统的学习下。毕竟我最开始也是看了下面的官方教程以及菜鸟教程里面的Vue教程的。
Vue的官方中文教程：https://cn.vuejs.org/v2/guide/index.html
Vue.js菜鸟教程：http://www.runoob.com/vue2/vue-tutorial.html
GitHub地址：https://github.com/vuejs/vue
Releases地址：https://github.com/vuejs/vue/releases

## 快速开始运行Vue.js

### Vue的安装

这里需要注意的是Vue 不支持 IE8 及以下版本，因为 Vue 使用了 IE8 无法模拟的 ECMAScript 5 特性。但它支持所有兼容 ECMAScript 5 的浏览器。目前最新稳定版本是：2.5.17。目前就两种我认为常用的安装方式罗列如下：至于NPM以及CLI的方式我就不推荐了，专业的前端玩的东西我感觉高大上，懒得折腾。

1. 直接下载并用 <script> 标签引入（像平时引入js一样引入即可），Vue 会被注册为一个全局变量。

   > 在开发环境下不要使用压缩版本，不然你就失去了所有常见错误相关的警告!
   >
   > 开发版本：https://vuejs.org/js/vue.js 包含完整的警告和调试模式
   >
   > 生产版本：https://vuejs.org/js/vue.min.js 删除了警告，30.90KB min+gzip

2. CDN 方式直接引入csn地址即可

   官方推荐链接到一个你可以手动更新的指定版本号：

   ```html
   <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.js"></script>
   ```

   你可以在 [cdn.jsdelivr.net/npm/vue](https://cdn.jsdelivr.net/npm/vue/) 浏览 NPM 包的源代码。

   Vue 也可以在 [unpkg](https://unpkg.com/vue@2.5.17/dist/vue.js) 和 [cdnjs](https://cdnjs.cloudflare.com/ajax/libs/vue/2.5.17/vue.js) 上获取 (cdnjs 的版本更新可能略滞后)。

   请确认了解[不同构建版本](https://cn.vuejs.org/v2/guide/installation.html#对不同构建版本的解释)并在你发布的站点中使用**生产环境版本**，把 `vue.js` 换成 `vue.min.js`。这是一个更小的构建，可以带来比开发环境下更快的速度体验。

### Vue.js输出一个Hello World

程序员界万年不变的定理，开始一门语言就从Hello World开始。下面就让我们快速开始用Vue.js输出一个“Hello World!”吧

1. 新建一个html文件，不要跟我说你不知道怎么创建一个html文件，不然你会被“达丝蒂”。这里我们用Visual Studio Code来进行代码的编写。

   

   ![img](https://upload-images.jianshu.io/upload_images/2767091-28f57d9d53f59ef3.png?imageMogr2/auto-orient/strip|imageView2/2/w/478/format/webp)

   1543406875477

2. 打开上面新建的helloworld.html文件，并输入如下的内容：

   ```html
   <!DOCTYPE html>
   ```

<html>
<head>
<meta charset="utf-8">
<title>Hello World</title>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
<div id="app">
<p>{{ message }}</p>
</div>

```xml
<script>
  new Vue({
    el: '#app',
    data: {
      message: 'Hello World!'
    }
  })
</script>
```

</body>
</html>

~~~xml
然后在浏览器中打开这个html文件看到如下的结果：![1543407902072](http://upload-images.jianshu.io/upload_images/2767091-8e889e8731e6f6e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们已经成功创建了第一个 Vue 应用！看起来这跟渲染一个字符串模板非常类似，但是 Vue 在背后做了大量工作。现在数据和 DOM 已经被建立了关联，所有东西都是**响应式的**。我们要怎么确认呢？打开你的浏览器的 JavaScript 控制台 (就在这个页面打开)，并修改 `app.__vue__.message` 的值并按回车，你将看到上例相应地更新。

![1543408250085](http://upload-images.jianshu.io/upload_images/2767091-2facf5c983cc6160.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Vue.js常用的指令

Vue.js的指令是以v-开头的，它们作用于HTML元素，指令提供了一些特殊的特性，将指令绑定在元素上时，指令会为绑定的目标元素添加一些特殊的行为，我们可以将指令看作特殊的HTML特性（attribute）。
接下来我们就给大家分别来介绍一下Vue中比较常用的指令
#### v-mode
在Vue.js中可以使用v-model指令，它能轻松实现表单输入和应用状态之间的双向绑定。
为了演示这个效果，我们新建一个sample02.html的文件，然后输入如下的代码：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Hello World</title>
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
 <div id="app">
   <p>{{ message }}</p>
   <input v-model="message">
 </div>

 <script>
   new Vue({
     el: '#app',
     data: {
       message: 'Hello World!'
     }
   })
 </script>
</body>
</html>
~~~

可以看到文本框的内容发生变化后，对应的文本框上面的文本也发生了变化，这里没有截成动态图，大家可以自行测试。





![img](https://upload-images.jianshu.io/upload_images/2767091-e0b10cf0b0bc90a2.png?imageMogr2/auto-orient/strip|imageView2/2/w/573/format/webp)

1543408771232

#### v-if ，v-else,v-else-if

条件判断指令,大家看着是不是觉得很熟悉呢，简直就跟c#中的if, else if,else 一毛一样啊（当然又有些区别，不过用法一样）下面我们给出要一个实例代码来进行演示
，这里我们新建要给sample03.html的文件，然后输入如下的代码：

```html
<!DOCTYPE html>
<html>
  <head>
  <meta charset="utf-8">
  <title>Hello World</title>
  <!-- 开发环境版本，包含了有帮助的命令行警告 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <p>{{ score }}</p>
      <input v-model="score">
      <div v-if="score>=90">
        优秀
      </div>
      <div v-else-if="score>=80&&score<90">
        优
      </div>
      <div v-else-if="score>=70&&score<80">
        良
      </div>
      <div v-else-if="score>=60&&score<70">
        及格
      </div>
      <div v-else>
        不及格
      </div>
    </div>

    <script>
      new Vue({
        el: '#app',
        data: {
            score: 100
        }
      })
    </script>
  </body>
</html>
```

在浏览器中打开看到如下的界面，因为初始值我们设置的是100



![img](https://upload-images.jianshu.io/upload_images/2767091-e9f7b8466edc28e2.png?imageMogr2/auto-orient/strip|imageView2/2/w/573/format/webp)

1543410028342





当你在输入框中改变值的时候，对应的文本框上面及下面的值都会发生变化，大伙可以自己试一下。



![img](https://upload-images.jianshu.io/upload_images/2767091-c07ffa02996d877c.png?imageMogr2/auto-orient/strip|imageView2/2/w/302/format/webp)

1543410084702

#### v-show

v-show也是条件渲染指令，和v-if指令不同的是，使用v-show指令的元素始终会被渲染到HTML，它只是简单地为元素设置CSS的style属性。
下面我们创建一个sample04.html的文件，然后输入如下的代码进行测试：

```html
<!DOCTYPE html>
<html>
  <head>
  <meta charset="utf-8">
  <title>Hello World</title>
  <!-- 开发环境版本，包含了有帮助的命令行警告 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      <p v-show="score<60">成绩为：{{ score }}不及格了！</p>
      <p v-show="score>=60">成绩为：{{ score }}及格了！</p>
      <input v-model="score">
    </div>

    <script>
      new Vue({
        el: '#app',
        data: {
          score: 50
        }
      })
    </script>
  </body>
</html>
```

然后在浏览器中看到如下的结果



![img](https://upload-images.jianshu.io/upload_images/2767091-e4557123a0b31e3d.png?imageMogr2/auto-orient/strip|imageView2/2/w/616/format/webp)

1543410497835

这时候我们查看一下源文件，可以看到下面的那个多了一些style="display:none" 的样式。但是html代码还是被渲染出来了



![img](https://upload-images.jianshu.io/upload_images/2767091-4f45c59e8b2ea999.png?imageMogr2/auto-orient/strip|imageView2/2/w/504/format/webp)

1543410567542

#### v-for

循环使用 v-for 指令。
v-for 指令需要以 site in sites 形式的特殊语法， sites 是源数据数组并且 site 是数组元素迭代的别名。
v-for 可以绑定数据到数组来渲染一个列表
下面我们创建一个sample05.html的文件，然后输入如下的代码进行测试：

```html
<!DOCTYPE html>
<html>
  <head>
  <meta charset="utf-8">
  <title>Hello World</title>
  <!-- 开发环境版本，包含了有帮助的命令行警告 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
        <h2>解决问题途径</h2>
        <ol>
            <li v-for="method in methods">
                {{ method.name }}
            </li>
        </ol>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                methods: [
                    { name: '谷歌' },
                    { name: '必应' },
                    { name: '百度' },
                    { name: '群里问别人'}
                ]
            }
        })
    </script>
  </body>
</html>
```

然后再浏览器中打开，看到如下的结果：



![img](https://upload-images.jianshu.io/upload_images/2767091-c330b735e0d687de.png?imageMogr2/auto-orient/strip|imageView2/2/w/612/format/webp)

1543411227704

#### v-bind

v-bind指令可以在其名称后面带一个参数，中间放一个冒号隔开，这个参数通常是HTML元素的特性（attribute）,如：`<div v-bind:class="{ active: isActive }"></div>`
我们新建一个sample06.html的文件，然后输入如下的代码

```html
<!DOCTYPE html>
<html>
  <head>
  <meta charset="utf-8">
  <title>Hello World</title>
  <!-- 开发环境版本，包含了有帮助的命令行警告 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <style>
        .active {
            font-size: 20px;
            color: red;
        }
        </style>
  </head>
  <body>
    <div id="app">
      <p v-bind:class="{active:isActive}">{{ message }}</p>
      <input v-model="message">
    </div>

    <script>
      new Vue({
        el: '#app',
        data: {
          message: 'Hello World!',
          isActive: true
        }
      })
    </script>
  </body>
</html>
```

再浏览器中打开可以看到如下的结果



![img](https://upload-images.jianshu.io/upload_images/2767091-d5829fdfd354a3c8.png?imageMogr2/auto-orient/strip|imageView2/2/w/679/format/webp)

1543411831503

> v-bind指令可以缩写为一个冒号 ，`<div :class="{ active: isActive }"></div>`

#### v-on

v-on指令用于监听DOM事件，它的用语法和v-bind是类似的，例如监听button元素的点击事件：
`<button v-on:click="doSomething">`

下面我们简单的进行下代码的演示，老规矩，新建一个sample07.html文件，然后输入如下的代码：

```html
<!DOCTYPE html>
<html>
  <head>
  <meta charset="utf-8">
  <title>Hello World</title>
  <!-- 开发环境版本，包含了有帮助的命令行警告 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <style>
        .active {
            font-size: 20px;
            color: red;
        }
    </style>
  </head>
  <body>
    <div id="app">
      <p class="active">您点击了{{counter}}次！</p>
      <button v-on:click="addCounter">快点我看效果吧！</button>
    </div>

    <script>
      new Vue({
        el: '#app',
        data: {
            counter:0
        },
        methods:{
            addCounter:function(){
                this.counter++;
            }
        }
      })
    </script>
  </body>
</html>
```

然后在浏览器中打开，并点击按钮按下效果吧



![img](https://upload-images.jianshu.io/upload_images/2767091-be98bad575e75287.png?imageMogr2/auto-orient/strip|imageView2/2/w/659/format/webp)

1543412504766

> 注：v-on指令可以缩写为@符号，如：`<button @click="addCounter">快点我看效果吧！</button>`

## 总结

今天带着大家学习了一下Vue.js这个前端框架。首先通过一个Hello World的实例开始，然后给大家介绍了Vue.js中常用的一些指令，并且每个指令都给出了一个实例代码，大伙可以自己跑一下看下效果。当然这也仅仅是Vue的基础，像涉及比较深的组件，路由，动画等等内容没有过多的讲解。主要还是让大家快速的了解一下Vue。同时为了照顾更多的朋友。有兴趣的朋友可以加下我们的.NET Core实战项目交流群637326624，跟大伙进行交流。好了，到这里.NET Core实战项目之CMS的入门篇就结束了。接下来我们就将进入设计篇的内容了！大家准备好了嘛？