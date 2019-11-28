# JavaScript Environment Requirements

- 贡献者1人

  

React16需要集合类型[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)和[Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)的支持。如果您支持旧版浏览器和尚未提供这些内容的设备（例如IE <11），请考虑在您的捆绑应用程序中包含全局填充，例如[core-js](https://github.com/zloirock/core-js)或[babel-polyfill](https://babeljs.io/docs/usage/polyfill/)。

React 16使用core-js支持旧版浏览器的多填充环境可能如下所示：

```javascript
import 'core-js/es6/map';
import 'core-js/es6/set';

import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

React也依赖于`requestAnimationFrame`（即使在测试环境中）。

您可以使用[raf](https://www.npmjs.com/package/raf)软件包来填充`requestAnimationFrame`：

```javascript
import 'raf/polyfill';
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-04-18