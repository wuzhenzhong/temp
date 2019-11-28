# Shallow Renderer

**输入**

```javascript
import ShallowRenderer from 'react-test-renderer/shallow'; // ES6
var ShallowRenderer = require('react-test-renderer/shallow'); // ES5 with npm
```

## 概观

在为React编写单元测试时，浅层渲染可能会有所帮助。浅层渲染使您可以渲染“一层深度”的组件，并确定其渲染方法返回的事实，而不必担心未实例化或渲染的子组件的行为。这不需要DOM。

例如，如果您有以下组件：

```javascript
function MyComponent() {
  return (
    <div>
      <span className="heading">Title</span>
      <Subcomponent foo="bar" />
    </div>
  );
}
```

然后你可以断言：

```javascript
import ShallowRenderer from 'react-test-renderer/shallow';

// in your test:
const renderer = new ShallowRenderer();
renderer.render(<MyComponent />);
const result = renderer.getRenderOutput();

expect(result.type).toBe('div');
expect(result.props.children).toEqual([
  <span className="heading">Title</span>,
  <Subcomponent foo="bar" />
]);
```

浅测试目前有一些限制，即不支持参考。

> 注意：我们也推荐检查酶的[浅显示API](http://airbnb.io/enzyme/docs/api/shallow.html)。它通过相同的功能提供更好的更高级别的API。

## 参考

### `shallowRenderer.render()`

您可以将shallowRenderer视为渲染正在测试的组件的“地点”，并从中提取组件的输出。

`shallowRenderer.render()`是类似的，`ReactDOM.render()`但它不需要DOM，只能渲染一个深度。这意味着您可以测试与孩子实施方式相关的组件。

### `shallowRenderer.getRenderOutput()`

在`shallowRenderer.render()`调用之后，可以使用`shallowRenderer.getRenderOutput()`获取浅显示的输出。

然后你可以开始断言输出结果。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com