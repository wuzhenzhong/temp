# Typechecking With PropTypes

> 注意： `React.PropTypes`自从React v15.5以来，它已经进入了一个不同的包。请使用[该`prop-types`库，而不是](https://www.npmjs.com/package/prop-types)。我们提供[了一个codemod脚本](https://reactjs.org/blog/2017/04/07/react-v15.5.0.html#migrating-from-react.proptypes)来自动化转换。

随着您的应用程序的增长，您可以通过类型检查来捕捉大量错误。对于某些应用程序，您可以使用JavaScript扩展（如[Flow](https://flowtype.org/)或[TypeScript）](https://www.typescriptlang.org/)来检查整个应用程序。但即使你不使用这些，React也有一些内置的类型检测功能。要在组件的道具上运行类型检查，可以指定特殊`propTypes`属性：

```javascript
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};
```

`PropTypes`导出一系列可用于确保您收到的数据有效的验证程序。在这个例子中，我们正在使用`PropTypes.string`。当为prop提供无效值时，JavaScript控制台中将显示警告。出于性能原因，`propTypes`仅在开发模式下进行检查。

### PropTypes

这里是一个记录提供的不同验证器的例子：

```javascript
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // You can declare that a prop is a specific JS primitive. By default, these
  // are all optional.
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // Anything that can be rendered: numbers, strings, elements or an array
  // (or fragment) containing these types.
  optionalNode: PropTypes.node,

  // A React element.
  optionalElement: PropTypes.element,

  // You can also declare that a prop is an instance of a class. This uses
  // JS's instanceof operator.
  optionalMessage: PropTypes.instanceOf(Message),

  // You can ensure that your prop is limited to specific values by treating
  // it as an enum.
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // An object that could be one of many types
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // An array of a certain type
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // An object with property values of a certain type
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // An object taking on a particular shape
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // You can chain any of the above with `isRequired` to make sure a warning
  // is shown if the prop isn't provided.
  requiredFunc: PropTypes.func.isRequired,

  // A value of any data type
  requiredAny: PropTypes.any.isRequired,

  // You can also specify a custom validator. It should return an Error
  // object if the validation fails. Don't `console.warn` or throw, as this
  // won't work inside `oneOfType`.
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // You can also supply a custom validator to `arrayOf` and `objectOf`.
  // It should return an Error object if the validation fails. The validator
  // will be called for each key in the array or object. The first two
  // arguments of the validator are the array or object itself, and the
  // current item's key.
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```

### 需要单个孩子

随着`PropTypes.element`您可以指定只有一个孩子可以传递给儿童一个组成部分。

```javascript
import PropTypes from 'prop-types';

class MyComponent extends React.Component {
  render() {
    // This must be exactly one element or it will warn.
    const children = this.props.children;
    return (
      <div>
        {children}
      </div>
    );
  }
}

MyComponent.propTypes = {
  children: PropTypes.element.isRequired
};
```

### 默认支柱值

您可以`props`通过分配特殊`defaultProps`属性来为您定义默认值：

```javascript
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// Specifies the default values for props:
Greeting.defaultProps = {
  name: 'Stranger'
};

// Renders "Hello, Stranger":
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);
```

如果使用类似[transform-class-properties](https://babeljs.io/docs/plugins/transform-class-properties/)的Babel变换，则还可以`defaultProps`在React组件类中声明为静态属性。尽管这个语法尚未完成，并且需要编译步骤才能在浏览器中工作。有关更多信息，请参阅[班级字段提案](https://github.com/tc39/proposal-class-fields)。

```javascript
class Greeting extends React.Component {
  static defaultProps = {
    name: 'stranger'
  }

  render() {
    return (
      <div>Hello, {this.props.name}</div>
    )
  }
}
```

的`defaultProps`将被用于确保`this.props.name`，如果不是由父组件指定它将有一个值。类型检查`propTypes`发生在`defaultProps`解决后，因此类型检查也将适用于该类型`defaultProps`。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18