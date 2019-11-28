# ReactDOMServer

`ReactDOMServer`对象使您可以将组件呈现为静态标记。通常，它在节点服务器上使用：

```javascript
// ES modules
import ReactDOMServer from 'react-dom/server';
// CommonJS
var ReactDOMServer = require('react-dom/server');
```

## 概观

以下方法可以在服务器和浏览器环境中使用：

- `renderToString()`

- `renderToStaticMarkup()`

这些附加方法取决于**仅在服务器上可用**的package（`stream`），并且在浏览器中不起作用。

- `renderToNodeStream()`

- `renderToStaticNodeStream()`

## 参考

### `renderToString()`

```javascript
ReactDOMServer.renderToString(element)
```

将React元素渲染为其初始HTML。React将返回一个HTML字符串。您可以使用此方法在服务器上生成HTML，并在初始请求时发送标记以加快页面加载速度，并允许搜索引擎抓取您的页面以实现SEO目的。

如果你调用`ReactDOM.hydrate()`一个已经有了这个服务器渲染标记的节点，React将会保留它并且只附加事件处理程序，这样你就可以拥有非常高效的第一次加载体验。

### `renderToStaticMarkup()`

```javascript
ReactDOMServer.renderToStaticMarkup(element)
```

与`renderToString`此类似，除了这不会创建React在内部使用的额外DOM属性，例如`data-reactroot`。如果你想使用React作为一个简单的静态页面生成器，这很有用，因为删除额外的属性可以节省一些字节。

如果您打算在客户端上使用React来交互标记，请不要使用此方法。相反，`renderToString`在服务器和`ReactDOM.hydrate()`客户端上使用。

### `renderToNodeStream()`

```javascript
ReactDOMServer.renderToNodeStream(element)
```

将React元素渲染为其初始HTML。返回一个输出HTML字符串的[可读流](https://nodejs.org/api/stream.html#stream_readable_streams)。该流输出的HTML完全等于`ReactDOMServer.renderToString`返回的内容。您可以使用此方法在服务器上生成HTML，并在初始请求时发送标记以加快页面加载速度，并允许搜索引擎抓取您的页面以实现SEO目的。

如果你调用`ReactDOM.hydrate()`一个已经有了这个服务器渲染标记的节点，React将会保留它并且只附加事件处理程序，这样你就可以拥有非常高效的第一次加载体验。

> 注意：仅限服务器。该API在浏览器中不可用。从此方法返回的流将返回以utf-8编码的字节流。如果您需要另一种编码的流，请查看像[iconv-lite](https://www.npmjs.com/package/iconv-lite)这样的项目，该项目为转码文本提供转换流。

### `renderToStaticNodeStream()`

```javascript
ReactDOMServer.renderToStaticNodeStream(element)
```

与`renderToNodeStream`此类似，除了这不会创建React在内部使用的额外DOM属性，例如`data-reactroot`。如果你想使用React作为一个简单的静态页面生成器，这很有用，因为删除额外的属性可以节省一些字节。

该流输出的HTML完全等于`ReactDOMServer.renderToStaticMarkup`返回的内容。

如果您打算在客户端上使用React来交互标记，请不要使用此方法。相反，`renderToNodeStream`在服务器和`ReactDOM.hydrate()`客户端上使用。

> 注意：仅限服务器。该API在浏览器中不可用。从此方法返回的流将返回以utf-8编码的字节流。如果您需要另一种编码的流，请查看像[iconv-lite](https://www.npmjs.com/package/iconv-lite)这样的项目，该项目为转码文本提供转换流。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com