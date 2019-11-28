# Higher-Order Components

高阶组件（HOC）是React中用于重用组件逻辑的高级技术。HOC本身不是React API的一部分。它们是从React的构图本质中浮现出来的一种模式。

具体而言，**高阶组件是一个接收组件并返回新组件的函数。**

```javascript
const EnhancedComponent = higherOrderComponent(WrappedComponent);
```

尽管组件将道具转换为UI，但高阶组件会将组件转换为另一个组件。

HOC在第三方React库中很常见，例如Redux [`connect`](https://github.com/reactjs/react-redux/blob/master/docs/api.md#connectmapstatetoprops-mapdispatchtoprops-mergeprops-options)和Relay [`createContainer`](https://facebook.github.io/relay/docs/api-reference-relay.html#createcontainer-static-method)。

在本文中，我们将讨论为什么高阶组件有用，以及如何编写自己的。

## 针对交叉问题使用HOC

> **注意** 我们以前推荐mixin作为处理交叉问题的一种方式。之后我们意识到mixin会造成比他们的价值更大的麻烦。[了解更多](https://reactjs.org/blog/2016/07/13/mixins-considered-harmful.html)关于我们为什么离开mixin以及如何转换现有组件的更多信息。

组件是React中代码重用的主要单元。但是，您会发现某些模式不适合传统组件。

例如，假设您有一个`CommentList`组件订阅外部数据源来呈现评论列表：

```javascript
class CommentList extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      // "DataSource" is some global data source
      comments: DataSource.getComments()
    };
  }

  componentDidMount() {
    // Subscribe to changes
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    // Clean up listener
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    // Update component state whenever the data source changes
    this.setState({
      comments: DataSource.getComments()
    });
  }

  render() {
    return (
      <div>
        {this.state.comments.map((comment) => (
          <Comment comment={comment} key={comment.id} />
        ))}
      </div>
    );
  }
}
```

之后，您将编写一个组件订阅单个博客帖子，该帖子遵循类似的模式：

```javascript
class BlogPost extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      blogPost: DataSource.getBlogPost(props.id)
    };
  }

  componentDidMount() {
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    DataSource.removeChangeListener(this.handleChange);
  }

  handleChange() {
    this.setState({
      blogPost: DataSource.getBlogPost(this.props.id)
    });
  }

  render() {
    return <TextBlock text={this.state.blogPost} />;
  }
}
```

`CommentList`并且`BlogPost`不完全相同 - 它们调用不同的方法`DataSource`，并且它们呈现不同的输出。但是他们的大部分实现都是一样的：

- 在mount上，添加一个更改监听器`DataSource`。

- 在监听器内部，`setState`每当数据源发生变化时都会调用。

- 在卸载时，删除更改侦听器。

你可以想象，在一个大型应用程序中，订阅`DataSource`和调用的相同模式`setState`将会一遍又一遍地发生。我们需要一种抽象，使我们能够在单个地方定义这种逻辑，并在多个组件之间共享这些逻辑。这是高阶元件擅长的地方。

我们可以编写一个创建组件的函数，比如`CommentList`和`BlogPost`订阅`DataSource`。该函数将接受作为其参数之一的接收订阅数据作为道具的子组件。我们来调用这个函数`withSubscription`：

```javascript
const CommentListWithSubscription = withSubscription(
  CommentList,
  (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
);
```

第一个参数是包装组件。第二个参数检索我们感兴趣的数据，给出一个`DataSource`和当前的道具。

当`CommentListWithSubscription`与`BlogPostWithSubscription`被渲染，`CommentList`并且`BlogPost`将传递一个`data`与从检索到的最新的数据道具`DataSource`：

```javascript
// This function takes a component...
function withSubscription(WrappedComponent, selectData) {
  // ...and returns another component...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }

    componentDidMount() {
      // ... that takes care of the subscription...
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      DataSource.removeChangeListener(this.handleChange);
    }

    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... and renders the wrapped component with the fresh data!
      // Notice that we pass through any additional props
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```

请注意，HOC不会修改输入组件，也不会使用继承来复制其行为。相反，HOC 通过*将*其*包装*在容器组件中来*组成*原始组件。HOC是一种纯粹的功能，具有零副作用。

就是这样！被包装的组件接收容器的所有道具以及一个新的道具，`data`它用来渲染其输出。HOC不关心如何或为什么使用数据，并且封装的组件不关心数据来自何处。

因为`withSubscription`是一个正常的函数，所以你可以添加尽可能多或者很少的参数。例如，您可能希望使`data`prop 的名称可配置，以进一步将HOC与封装组件隔离。或者您可以接受配置的参数`shouldComponentUpdate`，或者配置数据源的参数。这些都是可能的，因为HOC完全控制组件的定义。

与组件一样，合约`withSubscription`与包装组件之间的合约完全基于道具。这可以很容易地将一个HOC换成另一个HOC，只要它们为包装组件提供相同的道具。例如，如果您更改数据提取库，这可能很有用。

## 不要改变原始组件。使用构图。

抵制HOC内部修改组件原型（或者改变它）的诱惑。

```javascript
function logProps(InputComponent) {
  InputComponent.prototype.componentWillReceiveProps = function(nextProps) {
    console.log('Current props: ', this.props);
    console.log('Next props: ', nextProps);
  };
  // The fact that we're returning the original input is a hint that it has
  // been mutated.
  return InputComponent;
}

// EnhancedComponent will log whenever props are received
const EnhancedComponent = logProps(InputComponent);
```

这有几个问题。一个是输入组件不能与增强组件分开重复使用。更关键的是，如果你申请的另一个HOC到`EnhancedComponent`那个*也*发生变异`componentWillReceiveProps`，第一HOC的功能将被改写！这个HOC也不能用于没有生命周期方法的函数组件。

突变HOC是一个漏洞抽象 - 消费者必须知道它们是如何实施的，以避免与其他HOC发生冲突。

通过将输入组件包装在容器组件中，HOC不应该使用变异，而应该使用组合：

```javascript
function logProps(WrappedComponent) {
  return class extends React.Component {
    componentWillReceiveProps(nextProps) {
      console.log('Current props: ', this.props);
      console.log('Next props: ', nextProps);
    }
    render() {
      // Wraps the input component in a container, without mutating it. Good!
      return <WrappedComponent {...this.props} />;
    }
  }
}
```

这个HOC具有与变种版本相同的功能，同时避免了冲突的可能性。它与类和功能组件一样有效。而且因为它是一个纯粹的功能，它可以与其他HOC组合，甚至可以与其自身组合。

您可能已经注意到HOC和称为**容器组件**的模式之间的相似之处。集装箱组件是在高层和低层关注点之间分离责任战略的一部分。容器管理诸如订阅和状态之类的东西，并将道具传递给处理诸如呈现UI之类的事物的组件。HOC使用容器作为其实施的一部分。您可以将HOC视为参数化容器组件定义。

## 约定：将不相关道具传递给包装组件

HOC向组件添加功能。他们不应该大幅改变合同。预计从HOC返回的组件具有与被包装组件类似的接口。

HOC应该通过与其特定关注无关的道具。大多数HOC包含一个类似于下面的渲染方法：

```javascript
render() {
  // Filter out extra props that are specific to this HOC and shouldn't be
  // passed through
  const { extraProp, ...passThroughProps } = this.props;

  // Inject props into the wrapped component. These are usually state values or
  // instance methods.
  const injectedProp = someStateOrInstanceMethod;

  // Pass props to wrapped component
  return (
    <WrappedComponent
      injectedProp={injectedProp}
      {...passThroughProps}
    />
  );
}
```

此惯例有助于确保HOC尽可能灵活且可重用。

## 约定：最大化组合性

并非所有HOC看起来都一样。有时他们只接受一个参数，包装组件：

```javascript
const NavbarWithRouter = withRouter(Navbar);
```

HOC通常会接受其他参数。在Relay的这个例子中，一个配置对象被用来指定一个组件的数据依赖关系：

```javascript
const CommentWithRelay = Relay.createContainer(Comment, config);
```

HOC最常见的签名如下所示：

```javascript
// React Redux's `connect`
const ConnectedComment = connect(commentSelector, commentActions)(CommentList);
```

*什么？！*如果你把它分开，很容易看到发生了什么。

```javascript
// connect is a function that returns another function
const enhance = connect(commentListSelector, commentListActions);
// The returned function is an HOC, which returns a component that is connected
// to the Redux store
const ConnectedComment = enhance(CommentList);
```

换句话说，`connect`是一个返回高阶组件的高阶函数！

这种形式可能看起来很混乱或不必要，但它有一个有用的特性。单参数HOC（如`connect`函数返回的HOC）具有签名`Component => Component`。输出类型与输入类型相同的函数非常容易组合在一起。

```javascript
// Instead of doing this...
const EnhancedComponent = withRouter(connect(commentSelector)(WrappedComponent))

// ... you can use a function composition utility
// compose(f, g, h) is the same as (...args) => f(g(h(...args)))
const enhance = compose(
  // These are both single-argument HOCs
  withRouter,
  connect(commentSelector)
)
const EnhancedComponent = enhance(WrappedComponent)
```

（这个属性也允许使用`connect`其他增强器样式的HOC作为装饰器，这是一个实验性JavaScript提案。）

所述`compose`效用函数是由许多第三方库包括lodash（如提供[`lodash.flowRight`](https://lodash.com/docs/#flowRight)），[终极版](http://redux.js.org/docs/api/compose.html)，和[Ramda](http://ramdajs.com/docs/#compose)。

## 约定：包装显示名称以便于调试

由HOC创建的容器组件像任何其他组件一样出现在[React Developer Tools中](https://github.com/facebook/react-devtools)。为了便于调试，选择一个显示名称来传达它是HOC的结果。

最常用的技术是封装包装组件的显示名称。因此，如果您的高阶组件被命名`withSubscription`，并且包装组件的显示名称是`CommentList`，则使用显示名称`WithSubscription(CommentList)`：

```javascript
function withSubscription(WrappedComponent) {
  class WithSubscription extends React.Component {/* ... */}
  WithSubscription.displayName = `WithSubscription(${getDisplayName(WrappedComponent)})`;
  return WithSubscription;
}

function getDisplayName(WrappedComponent) {
  return WrappedComponent.displayName || WrappedComponent.name || 'Component';
}
```

## 注意事项

如果您是React的新手，那么高阶组件会附带一些注意事项，这些注意事项不会立即显现出来。

### 不要在render方法中使用HOC

React的差异算法（称为reconciliation）使用组件标识来确定它是应该更新现有的子树还是将其丢弃并挂载新的子树。如果返回的组件与来自先前渲染的组件`render`相同（`===`），则React通过用新组件区分它来递归更新子树。如果不相等，则前一个子树完全卸载。

通常情况下，你不需要考虑这一点。但是它对于HOC很重要，因为它意味着你无法将HOC应用到组件的渲染方法中的组件：

```javascript
render() {
  // A new version of EnhancedComponent is created on every render
  // EnhancedComponent1 !== EnhancedComponent2
  const EnhancedComponent = enhance(MyComponent);
  // That causes the entire subtree to unmount/remount each time!
  return <EnhancedComponent />;
}
```

这里的问题不仅仅是性能 - 重新安装组件会导致组件及其所有子组件的状态丢失。

相反，在组件定义之外应用HOC，以便只生成一次结果组件。那么，它的身份将在整个渲染过程中保持一致。无论如何，这通常是你想要的。

在您需要动态应用HOC的罕见情况下，您也可以在组件的生命周期方法或其构造函数中执行此操作。

### 必须复制静态方法

有时在React组件上定义静态方法很有用。例如，中继容器公开了一个静态方法`getFragment`来促进GraphQL片段的组合。

但是，如果将HOC应用于组件，则原始组件将使用容器组件进行包装。这意味着新组件没有任何原始组件的静态方法。

```javascript
// Define a static method
WrappedComponent.staticMethod = function() {/*...*/}
// Now apply an HOC
const EnhancedComponent = enhance(WrappedComponent);

// The enhanced component has no static method
typeof EnhancedComponent.staticMethod === 'undefined' // true
```

为了解决这个问题，你可以在返回之前将这些方法复制到容器中：

```javascript
function enhance(WrappedComponent) {
  class Enhance extends React.Component {/*...*/}
  // Must know exactly which method(s) to copy :(
  Enhance.staticMethod = WrappedComponent.staticMethod;
  return Enhance;
}
```

但是，这需要您确切地知道需要复制哪些方法。您可以使用[hoist-non-react-statics](https://github.com/mridgway/hoist-non-react-statics)来自动复制所有非React静态方法：

```javascript
import hoistNonReactStatic from 'hoist-non-react-statics';
function enhance(WrappedComponent) {
  class Enhance extends React.Component {/*...*/}
  hoistNonReactStatic(Enhance, WrappedComponent);
  return Enhance;
}
```

另一种可能的解决方案是将静态方法与组件本身分开导出。

```javascript
// Instead of...
MyComponent.someFunction = someFunction;
export default MyComponent;

// ...export the method separately...
export { someFunction };

// ...and in the consuming module, import both
import MyComponent, { someFunction } from './MyComponent.js';
```

### Refs没有通过

虽然高阶组件的惯例是将所有道具传递给包装组件，但不可能通过参考。这是因为`ref`它不是一个真正的道具`key`，它是由React专门处理的。如果将ref添加到其组件是HOC结果的元素，则ref引用最外层容器组件的实例，而不是包装组件。

如果你发现自己面临这个问题，理想的解决方案是找出如何避免使用`ref`。偶尔，刚刚接触React范例的用户依赖于在支撑物更好地工作的情况下的参考。

也就是说，有些时候refs是必要的逃生舱口，否则React不会支持它们。聚焦输入字段是一个例子，您可能需要对组件进行必要的控制。在这种情况下，一种解决方案是通过给它一个不同的名称来传递一个ref回调作为普通道具：

```javascript
function Field({ inputRef, ...rest }) {
  return <input ref={inputRef} {...rest} />;
}

// Wrap Field in a higher-order component
const EnhancedField = enhance(Field);

// Inside a class component's render method...
<EnhancedField
  inputRef={(inputEl) => {
    // This callback gets passed through as a regular prop
    this.inputEl = inputEl
  }}
/>

// Now you can call imperative methods
this.inputEl.focus();
```

这不是一个完美的解决方案。我们更喜欢参考资料仍然是图书馆关注的问题，而不是要求您手动处理它们。我们正在探索解决这个问题的方法，以便使用HOC是不可观测的。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18