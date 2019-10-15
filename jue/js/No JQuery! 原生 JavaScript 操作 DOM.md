# No JQuery! 原生 JavaScript 操作 DOM

阅读 2148

收藏 93

2017-04-10

原文链接：[www.jianshu.com](https://link.juejin.im/?target=http%3A%2F%2Fwww.jianshu.com%2Fp%2F9681467d58e7)

[在实时音视频中应用深度学习，你需要了解这些juejin.im](https://juejin.im/post/5d9da1446fb9a04df26c2145)

*翻译自sitepoint的一篇文章，作者是Sebastian Seitz。虽然日常工作中很少再写原生js来操作DOM了，大家可能都在用主流的前端框架，我也是，但是看到这篇很浅显易懂的文章，还是忍不住想细读一下，复习的同时也会有新的发现。*

无论何时我们需要操作DOM的时候，我们都会很快去用jQuery。然而，原生的JavaScript DOM API其实以它自己的方式已经可以解决非常多的需求。因为11以下的IE版本已经被官方丢弃，我们可以没有任何担忧地使用它。

在这篇文章，我将展示如何用原生JavaScript来完成一些最普遍的DOM操作任务，即：

- 查找并修改DOM
- 修改class和属性
- 事件监听
- 动画

我将在最后展示给各位，如何来创建一个可以用在任何项目里的你自己的超精简DOM库。与此同时，各位可以学到用原生JS操作DOM其实并不难，很多jQuery的方法事实上都有对等的native API。

那么我们开始吧...

#### DOM操作：查找DOM

*请注意：我不会详细地讲解原生DOM API的细节，只是停留在表面。在用例里，你可能会遇到我并没有清楚介绍的方法。这时你可以参考Mozilla Developer Network。*

可以用`.querySelector()`方法来查询DOM。需要传入任意的CSS选择器作为参数：

```
const myElement = document.querySelector('#foo > div.bar')
```

这行代码返回第一个匹配的元素（深度优先）。相反的，我们可以检查一个元素是否匹配一个选择器：

```
myElement.matches('div.bar') === true
```

如果我们想得到所有匹配元素，我们可以用：

```
const myElements = document.querySelectorAll('.bar')
```

如果我们已经得到一个父元素的引用，我们可以只查找它的子元素，而不是整个document。像这样缩小查找范围，我们可以简化选择器提高查找性能。

```
const myChildElemet = myElement.querySelector('input[type="submit"]')

// Instead of
// document.querySelector('#foo > div.bar input[type="submit"]')
```

那么我们为什么还要用其他的不那么方便的方法呢？比如`.getElementsByTagName()`？一个重要的区别是`.querySelector()`的结果不是实时的，所以当我们动态地添加一个匹配该选择器的元素(参考[第三部分](https://link.juejin.im/?target=https%3A%2F%2Fwww.sitepoint.com%2Fdom-manipulation-vanilla-javascript-no-jquery%2F%23modifyingthedom))的时候，元素集合不会更新。

```
const elements1 = document.querySelectorAll('div')
const elements2 = document.getElementsByTagName('div')
const newElement = document.createElement('div')

document.body.appendChild(newElement)
elements1.length === elements2.length // false
```

另一个原因是这样的实时的元素集合不需要预先获得所有的元素信息，而`.querySelectorAll()`会立刻收集所有的信息到一个静态的列表里，因而会[降低性能](https://link.juejin.im/?target=https%3A%2F%2Fwww.nczonline.net%2Fblog%2F2010%2F09%2F28%2Fwhy-is-getelementsbytagname-faster-that-queryselectorall%2F)。

##### 元素列表

关于`.querySelectorAll()`有两个坑。一个是我们不能在结果集上调用Node方法从而获得它的元素（像jQuery对象那样用）。我们不得不明确地遍历这些元素。另一个是返回的结果是一个NodeList，不是数组。也就是说只能直接调用数组的方法。NodeList自己有一些数组方法的实现，比如`.forEach`，但是任何版本的IE浏览器都不支持。所以我们必须先把它转换成数组，或者从Array原型上“借用”那些方法。

```
// Using Array.from()
Array.from(myElements).forEach(doSomethingWithEachElement)

// Or prior to ES6
Array.prototype.forEach.call(myElements, doSomethingWithEachElement)

// Shorthand:
[].forEach.call(myElements, doSomethingWithEachElement)
```

每个元素都有一些非常语义化的只读的属性，都是实时更新的：

```
myElement.children
myElement.firstElementChild
myElement.lastElementChild
myElement.previousElementSibling
myElement.nextElementSibling
```

因为[Element](https://link.juejin.im/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2Felement)接口继承自[Node](https://link.juejin.im/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FNode)接口，它也有以下的属性：

```
myElement.childNodes
myElement.firstChild
myElement.lastChild
myElement.previousSibling
myElement.nextSibling
myElement.parentNode
myElement.parentElement
```

前一组属性的值只可以是元素节点，而后一组属性（除了`.parentElement`）的值可以是任何节点，比如文本节点。我们可以像这样检查节点的类型：

```
myElement.firstChild.nodeType === 3 // this would be a text node
```

像任何对象那样，我们可以用`instanceof`操作符检查节点的原型链：

```
myElement.firstChild.nodeType instanceof Text
```

#### 修改class和属性

修改元素的class像下面的代码这样简单：

```
myElement.classList.add('foo')
myElement.classList.remove('bar')
myElement.classList.toggle('baz')
```

你可以在[quick tip by Yaphi Berhanu](https://link.juejin.im/?target=https%3A%2F%2Fwww.sitepoint.com%2Fadd-remove-css-class-vanilla-js%2F)读到关于如何修改class的更深度的讨论。元素属性值可以像其他任何对象属性一样得到。

```
// Get an attribute value
const value = myElement.value

// Set an attribute as an element property
myElement.value = 'foo'

// Set multiple properties using Object.assign()
Object.assign(myElement, {
  value: 'foo',
  id: 'bar'
})

// Remove an attribute
myElement.value = null
```

注意还有`.getAttibute()`, `.setAttribute()`和`.removeAttribute()`这三个方法。这些方法直接修改的是元素的HTML属性（与DOM属性相对），因此会使浏览器重新渲染（你可以用你的浏览器自带的开发调试工具来检查元素观察它的变化）。浏览器重新渲染不仅比只是设置DOM属性代价更高，而且还会产生[不期望的后果](https://link.juejin.im/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FElement%2FsetAttribute%23Notes)。

作为一个小原则，除非你真的想对HTML“持久化”那些改变，你就只用上面的方法修改与DOM属性不相关的HTML属性（比如`colspan`）。(比如当克隆一个元素或者修改它的父元素的`.innerHTML`的时候想保持这些改变,参考[第三部分](https://link.juejin.im/?target=https%3A%2F%2Fwww.sitepoint.com%2Fdom-manipulation-vanilla-javascript-no-jquery%2F%23modifyingthedom))

##### 添加CSS样式

CSS规则可以像其他属性那样设置。需要注意的是在JavaScript里要写成驼峰形式：

```
myElement.style.marginLeft = '2em'
```

如果我们想获得CSS规则的值，我们可以通过`.style`属性。然而，通过它只能拿到我们明确设置过的样式。想拿到计算后的样式值，我们可以用`.window.getComputedStyle()`。它可以拿到这个元素并返回一个[CSSStyleDeclaration](https://link.juejin.im/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FCSSStyleDeclaration)。这个返回值包括了这个元素自己的和继承自父元素的全部样式。

```
window.getComputedStyle(myElement).getPropertyValue('margin-left')
```

#### 修改DOM

我们可以像下面这样移动元素：

```
// Append element1 as the last child of element2
element1.appendChild(element2)

// Insert element2 as child of element 1, right before element3
element1.insertBefore(element2, element3)
```

如果我们不想移动元素，而是插入一个拷贝，我们可以这样克隆它：

```
// Create a clone
const myElementClone = myElement.cloneNode()
myParentElement.appendChild(myElementClone)
```

`.cloneNode()`方法可选地接受一个boolean类型的参数；如果传入的是true, 将会创建一个深拷贝，也就是它的所有子元素也会被克隆。

当然我们可以创建一个全新的元素或文本节点：

```
const myNewElement = document.createElement('div')
const myNewTextNode = document.createTextNode('some text')
```

然后我们可以像上面展示的代码那样插入创建的元素。如果我们想删除一个元素，我们不能直接删除，而要采用从它的父元素删除子元素的办法来实现，像这样：

```
myParentElement.removeChild(myElement)
```

这给了我们一个优雅的解决办法，也就是可以通过它的父元素间接的删除一个元素：

```
myElement.parentNode.removeChild(myElement)
```

##### 元素属性

每个元素都有`.innerHTML`和`.textContent`（还有`.innerText`，跟`.textContent`类似，但是有一些[重要的区别](https://link.juejin.im/?target=http%3A%2F%2Fperfectionkills.com%2Fthe-poor-misunderstood-innerText%2F)。它们分别表示HTML内容和纯文本内容。它们是可写的属性，也就是说我们可以直接修改元素和它们的内容：

```
// Replace the inner HTML
myElement.innerHTML = `
  <div>
    <h2>New content</h2>
    <p>beep boop beep boop</p>
  </div>
`

// Remove all child nodes
myElement.innerHTML = null

// Append to the inner HTML
myElement.innerHTML += `
  <a href="foo.html">continue reading...</a>
  <hr/>
`
```

像上面的代码那样向HTML添加标记是通常是一个不好的注意，因为这样是丢失之前对影响元素的属性做的修改（除非我们把那些修改作为HTML属性而保留下来，参考[第二部分](https://link.juejin.im/?target=https%3A%2F%2Fwww.sitepoint.com%2Fdom-manipulation-vanilla-javascript-no-jquery%2F%23modifyingclassesandattributes)）和已经绑定的事件监听。设置`.innerHTML`可以适合用在需要完全丢弃原来的而替换成新的标记的场景，比如服务端渲染。所以添加元素这样做比较好：

```
const link = document.createElement('a')
const text = document.createTextNode('continue reading...')
const hr = document.createElement('hr')

link.href = 'foo.html'
link.appendChild(text)
myElement.appendChild(link)
myElement.appendChild(hr)
```

但是这个办法会引起两次浏览器的重新渲染-每次添加元素都会渲染一次-而用设置`.innerHTML`的办法的话只会重新渲染一次。我们可以先把所有的节点组合在一个[DocumentFragment](https://link.juejin.im/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FDocumentFragment)里，然后把这一个片段添加到DOM里，这样可以解决这个性能问题。

```
const fragment = document.createDocumentFragment()

fragment.appendChild(link)
fragment.appendChild(hr)
myElement.appendChild(fragment)
```

#### 事件监听

这可能是最知名的绑定事件监听的方法：

```
myElement.onclick = function onclick (event) {
  console.log(event.type + ' got fired')
}
```

但是这是通常应该避免采用的方法。这里，`.onclick`是一个元素的属性，也就是说你可以修改它，但是你不能用它再绑定其他的监听函数-你只能把新的函数赋给它，覆盖掉旧函数的引用。

我们可以用更加强大的`.addEventListener()`方法来尽情地添加各种类型的各种事件的监听器。它接受三个参数：事件类型（比如`click`），一个无论何时在这个绑定元素上该事件发生都会触发的函数（这个函数会得到一个事件对象传进去作为参数）和一个可选的配置参数，下面会更详细的解释。

```
myElement.addEventListener('click', function (event) {
  console.log(event.type + ' got fired')
})

myElement.addEventListener('click', function (event) {
  console.log(event.type + ' got fired again')
})
```

在监听函数内部，`event.target`指向这个事件触发的元素（`this`也是，当然除非你用的是箭头函数。*译者注：如果监听函数是箭头函数，里面的this指向的是window对象，如果是普通的function函数，里面的this指向的跟event.target相同，都是该元素本身*）。因此你可以轻松的拿到它的属性：

```
// The `forms` property of the document is an array holding
// references to all forms
const myForm = document.forms[0]
const myInputElements = myForm.querySelectorAll('input')

Array.from(myInputElements).forEach(el => {
  el.addEventListener('change', function (event) {
    console.log(event.target.value)
  })
})
```

##### 阻止默认行为

注意在监听函数内部总是可以拿到`event`，但是当需要的时候明确地传入这个参数是一个好的实践（当然参数名称可以随意设置）（译者注：即使没有明确地给监听函数传入任何参数，在内部仍然可以拿到原生`event`对象，变量名就是`event`）。先不详细解释[Event](https://link.juejin.im/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FEvent)接口，一个特别需要注意的方法是`.preventDefault()`。它可以用来阻止浏览器的默认行为，比如跳转链接。另一个常见的应用场景是当前端的表单校验失败的时候，可以根据判断条件阻止表单提交。

```
myForm.addEventListener('submit', function (event) {
  const name = this.querySelector('#name')

  if (name.value === 'Donald Duck') {
    alert('You gotta be kidding!')
    event.preventDefault()
  }
})
```

另一个重要的事件方法是`.stopPropagation()`，它可以阻止事件冒泡。也就是说在一个子元素上绑定了阻止事件冒泡的点击事件监听函数，而在它的某一个父元素上也监听了点击事件，在子元素上触发的点击事件，不会触发它的这个父元素的点击事件监听函数-否则，父子元素都会触发。

现在我们看一下`.addEventListener()`的可选的配置对象这个第三个参数，它可以有以下的布尔属性（它们的默认值都是`false`）：

- `capture`: 这个事件会先在父元素触发，然后再向下传递给它的子元素（关于事件捕获和事件冒泡更详细地解释可以参考[这里](https://link.juejin.im/?target=http%3A%2F%2Fjavascript.info%2Ftutorial%2Fbubbling-and-capturing)）
- `once`: 你已经猜到，这个属性表示这个事件只会被触发一次
- `passive`: 它的意思是`event.preventDefault()`会被忽略（通常在控制台都会打印一句警告）

最常用的选项是`.capture`；事实上，因为它非常常用，所以可以只传入它的一个布尔值，而不必传入整个配置对象：

```
myElement.addEventListener(type, listener, true)
```

事件监听可以用`.removeEventListener()`方法删除。它接受事件类型和回调函数的引用两个参数；例如，`once`选项也可以像这样实现：

```
myElement.addEventListener('change', function listener (event) {
  console.log(event.type + ' got triggered on ' + this)
  this.removeEventListener('change', listener)
})
```

##### 事件委托

另一个有用的模式是事件委托：假如我们有一个表单，并且想给它的每一个`input`元素绑定一个`change`事件的监听函数。一种方法是上面已经介绍过的那样用`myForm.querySelectorAll('input')`取到所有的`input`元素，然后再通过遍历绑定事件。然而，我们其实只需要给表单本身绑定这个事件监听函数，然后检查`event.target`是否是`input`元素就可以了。

```
myForm.addEventListener('change', function (event) {
  const target = event.target
  if (target.matches('input')) {
    console.log(target.value)
  }
})
```

用这种模式的另一个优势就是它对动态插入的子元素同样有效，而不需要给每一个绑定新的监听函数。

#### 动画

通常，最优雅的生成动画的方式是结合`transition`属性用CSS的类，或者用CSS的`@keyframes`。但是如果你需要更加灵活的方式（比如做游戏），也可以用JavaScript。

简单的方法就是有一个`window.setTimeout()`函数，不断地调用自己直到期望的动画完成。然而，这会低效地强迫文档进行迅速的重排；并且结构的抖动会很快使页面卡顿，特别是在移动设备上。替代方案是，我们可以用`window.requestAnimationFrame()`同步页面的更新，把当前的所有改变安排到下一次浏览器重绘。它接受一个回调函数作为参数。这个回调函数会接收到当前的时间戳作为参数：

```
const start = window.performance.now()
const duration = 2000

window.requestAnimationFrame(function fadeIn (now)) {
  const progress = now - start
  myElement.style.opacity = progress / duration

  if (progress < duration) {
    window.requestAnimationFrame(fadeIn)
  }
}
```

用这个方法我们可以得到非常流畅的动画。想了解更加详细的讨论，可以参考Mark Brown写的[这篇](https://link.juejin.im/?target=https%3A%2F%2Fwww.sitepoint.com%2Fquick-tip-game-loop-in-javascript%2F)文章。

#### 写你自己的帮助函数

确实，与jQuery简洁的链式的`$('.foo').css({color: 'red'})`表达式相比，总是要遍历元素去做什么可能是非常的繁琐。所以为什么我们不像下面这样写我们自己的快捷的方法呢？

```
const $ = function $ (selector, context = document) {
const elements = Array.from(context.querySelectorAll(selector))

  return {
    elements,

    html (newHtml) {
      this.elements.forEach(element => {
        element.innerHTML = newHtml
      })

      return this
    },

    css (newCss) {
      this.elements.forEach(element => {
        Object.assign(element.style, newCss)
      })

      return this
    },

    on (event, handler, options) {
      this.elements.forEach(element => {
        element.addEventListener(event, handler, options)
      })

      return this
    }

    // etc.
  }
}
```

因此我们有了一个没有向下兼容负担的只有我们需要的方法的超简洁的DOM库。尽管通常在元素的原型链上已经有了那些方法。这里有一个gist（更加详细深入一些），它展示了一些实现这些帮助函数的办法。我们还可以这样保持简单：

```
const $ = (selector, context = document) => context.querySelector(selector)
const $$ = (selector, context = document) => context.querySelectorAll(selector)

const html = (nodeList, newHtml) => {
  Array.from(nodeList).forEach(element => {
    element.innerHTML = newHtml
  })
}

// And so on...
```

#### Demo

为使文章圆满结束，下面的CodePen通过实现一个简单的灯箱效果展示了上面提到的很多概念。我鼓励你们花点时间去看一下源码，如果你们有任何想法或者疑问请在下面评论来让我知道。
[CodePen上的Demo代码](https://link.juejin.im/?target=http%3A%2F%2Fcodepen.io%2FSitePoint%2Fpen%2FaJaggB)

#### 结论

我希望我已经证明了用原生JavaScript来操作DOM并不是什么高科技，而且事实上，很多jQuery里的方法在原生DOM的API里有直接对应的实现。这意味着在一些日常的应用场景里（比如导航菜单或者是跳出的模态框），额外的加载过重的DOM库是不合适的。

虽然一部分原生API确实繁琐或是不方便（比如必须总是要手动遍历节点列表），但是我们能够非常轻松的把这些重复工作抽象出来写成我们自己的短小的帮助函数。

但是现在轮到你了。你怎么看？你更愿意在你可以的地方避免使用第三方库，还是使自己卷入根本不值得的认知开销里面？请在下面的评论中让我知道。