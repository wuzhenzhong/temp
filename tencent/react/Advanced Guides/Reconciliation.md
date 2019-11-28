# Reconciliation

React提供了一个声明式API，因此您无需担心每次更新的确切更改。这使得编写应用程序变得更加简单，但在React中如何实现它可能并不明显。本文解释了我们在React的“差异化”算法中所做的选择，以便组件更新可预测，同时对于高性能应用程序足够快。

## 动机

当您使用React时，您可以在单个时间点将该`render()`功能视为创建React元素树。在下一个状态或道具更新时，该`render()`函数将返回不同的React元素树。然后React需要弄清楚如何有效地更新UI以匹配最近的树。

[纠错](javascript:;)

对于产生将一棵树转换为另一棵树的最小操作次数的算法问题，存在一些通用解决方案。然而，[现有技术的算法](http://grfia.dlsi.ua.es/ml/algorithms/references/editsurvey_bille.pdf)在O（n3）的顺序中具有复杂性，其中n是树中元素的数量。

如果我们在React中使用它，则显示1000个元素需要10亿次比较。这太贵了。相反，React基于两个假设实现了启发式O（n）算法：

1. 两种不同类型的元素会产生不同的树木。

\2. 开发人员可以通过`key`道具暗示哪些子元素可以在不同的渲染器中保持稳定。

实际上，这些假设对于几乎所有的实际使用情况都是有效的。

## Diffing算法

在分析两棵树时，React首先比较两个根元素。行为根据根元素的类型而不同。

### 不同类型的元素

每当根元素具有不同类型时，React就会拆除旧树并从头开始构建新树。从去`<a>`到`<img>`，或者`<Article>`到`<Comment>`，或`<Button>`到`<div>`-任何这些将导致完全重建。

拆除树时，旧的DOM节点被破坏。组件实例接收`componentWillUnmount()`。在构建新树时，将新的DOM节点插入到DOM中。组件实例接收`componentWillMount()`然后`componentDidMount()`。任何与旧树相关的状态都会丢失。

根以下的任何组件也将被卸载并且其状态被破坏。例如，差异时：

```javascript
<div>
  <Counter />
</div>

<span>
  <Counter />
</span>
```

这将摧毁旧的`Counter`并重新装上一个新的。

### 同一类型的DOM元素

比较相同类型的两个React DOM元素时，React查看两者的属性，保持相同的底层DOM节点，并仅更新已更改的属性。例如：

```javascript
<div className="before" title="stuff" />

<div className="after" title="stuff" />
```

通过比较这两个元素，React知道只修改`className`底层DOM节点。

更新时`style`，React也知道只更新已更改的属性。例如：

```javascript
<div style={{color: 'red', fontWeight: 'bold'}} />

<div style={{color: 'green', fontWeight: 'bold'}} />
```

在这两个元素之间转换时，React知道只修改`color`样式，而不是`fontWeight`。

在处理DOM节点之后，React会在子节点上递归。

### 同一类型的组件元素

当组件更新时，实例保持不变，以便在整个渲染过程中维护状态。反应更新底层组件实例的道具新元素匹配，并呼吁`componentWillReceiveProps()`和`componentWillUpdate()`对底层的实例。

接下来，该`render()`方法被调用，并且diff算法对先前结果和新结果进行递归。

### 递归于儿童

默认情况下，React在DOM节点的子节点上递归时，只是同时迭代两个子节点列表，并在出现差异时生成突变。

例如，在儿童的末尾添加元素时，在这两棵树之间转换效果很好：

```javascript
<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```

React将匹配两`<li>first</li>`棵树，匹配两`<li>second</li>`棵树，然后插入`<li>third</li>`树。

如果你天真地实现它，在开始插入一个元素会导致性能下降。例如，在这两棵树之间转换效果不佳：

```javascript
<ul>
  <li>Duke</li>
  <li>Villanova</li>
</ul>

<ul>
  <li>Connecticut</li>
  <li>Duke</li>
  <li>Villanova</li>
</ul>
```

反应会产生变异每一个孩子，而不是意识到它可以保留的`<li>Duke</li>`和`<li>Villanova</li>`子树完好无损。这种低效率可能是一个问题。

### Keys

为了解决这个问题，React支持一个`key`属性。当孩子拥有密钥时，React使用该密钥将原始树中的孩子与后续树中的孩子进行匹配。例如，在`key`上面的低效率示例中添加a 可以使树转换高效：

```javascript
<ul>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>

<ul>
  <li key="2014">Connecticut</li>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>
```

现在阵营都知道，与关键元素`'2014'`是新的，并用钥匙元素`'2015'`和`'2016'`刚刚搬进了。

在实践中，找到一个关键通常并不难。您要显示的元素可能已经有一个唯一的ID，所以密钥可以来自您的数据：

```javascript
<li key={item.id}>{item.name}</li>
```

如果情况并非如此，您可以将新的ID属性添加到您的模型中，或者对内容的某些部分进行散列以生成密钥。关键只在兄弟姐妹中是唯一的，而不是全球唯一的。

作为最后的手段，您可以将项目的索引作为关键字传递给数组。如果项目从未重新排序，这可以很好地工作，但重新排序会很慢。

## 权衡

请务必记住，协调算法是一个实现细节。React可以在每一个动作上重新渲染整个应用程序; 最终结果将是相同的。为了清楚起见，在这种情况下重申意味着要求`render`所有组件，但这并不意味着React将卸载并重新安装它们。它只会按照前面章节中所述的规则来应用差异。

我们经常改进启发式，以便更快地制作常见用例。在目前的实现中，你可以表达一个事实，即一棵子树已经在它的兄弟姐妹之间移动了，但是你不能说它已经移动到别的地方了。该算法将重新渲染完整的子树。

因为React依赖于启发式算法，如果它们背后的假设不符合，性能将受到影响。

1. 该算法不会尝试匹配不同组件类型的子树。如果您发现自己在输出类似的两种组件类型之间交替，您可能希望使它成为相同的类型。实际上，我们并未发现这是一个问题。

\2. 密钥应该是稳定的，可预测的和独特的。不稳定的键（如由那些产生的键`Math.random()`）将导致许多组件实例和DOM节点被不必要地重新创建，这可能导致性能下降并丢失子组件中的状态。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18