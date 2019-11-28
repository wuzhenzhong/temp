# Installation

- 贡献者2人

  

反应灵活，可用于各种项目。您可以使用它创建新的应用程序，但您也可以逐渐将其引入现有的代码库，而无需重写。

以下是几种开始的方法：

- 试试React

- 创建一个新应用程序

- 将反应添加到现有的应用程序

## 尝试 **React**



如果你只是想玩弄React，你可以使用CodePen。尝试从此[Hello World示例代码开始](http://codepen.io/gaearon/pen/rrpgNB?editors=0010)。你不需要安装任何东西; 您可以修改代码并查看它是否有效。

如果您更喜欢使用自己的文本编辑器，还可以下载此HTML文件，对其进行编辑，然后在浏览器中从本地文件系统打开它。它的运行时代码转换很慢，所以不要在生产环境中使用它。

如果您想将其用于完整应用程序，可以采用两种流行的方式开始使用React：使用Create React App或将其添加到现有应用程序。

## 创建新的应用程序

[创建React App](http://github.com/facebookincubator/create-react-app)是开始构建新的React单页应用程序的最佳方式。它设置了您的开发环境，以便您可以使用最新的JavaScript功能，提供良好的开发人员体验，并优化您的应用程序的生产。你需要在你的机器上有Node> = 6。

```javascript
npm install -g create-react-app
create-react-app my-app

cd my-app
npm start
```

如果你有NPM 5.2.0+安装，你可以使用[NPX](https://www.npmjs.com/package/npx)代替。

```javascript
npx create-react-app my-app

cd my-app
npm start
```

创建React App不处理后端逻辑或数据库; 它只是创建一个前端构建管道，所以你可以将它用于任何你想要的后端。它使用诸如[Babel](http://babeljs.io/)和[webpack之](https://webpack.js.org/)类的构建工具，但是使用零配置。

当您准备部署到生产环境时，运行`npm run build`将在`build`文件夹中创建应用程序的优化版本。您可以[从README](https://github.com/facebookincubator/create-react-app#create-react-app-)和[用户指南中](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#table-of-contents)了解有关创建React应用程序的更多信息。

## 将反应添加到现有的应用程序

你不需要重写你的应用程序来开始使用React。

我们建议将React添加到应用程序的一小部分中，例如单个小部件，以便您可以查看它是否适合您的用例。

虽然可以在没有构建管道的情况下使用React，但我们建议将其设置为可以提高生产力。现代构建管道通常由以下部分组成：

- **包管理器**，如[Yarn](https://yarnpkg.com/zh-Hans/)或[NPM](https://www.npmjs.com/)。它可让您充分利用第三方软件包的庞大生态系统，并轻松安装或更新它们。

- **捆绑**，如[的WebPack](https://webpack.js.org/)或[Browserify](http://browserify.org/)。它允许您编写模块化代码，并将它们组合在一起成为小包，以优化加载时间。

- 像[Babel](http://babeljs.io/)这样的**编译器**。它可以让你编写在旧版浏览器中仍然可用的现代JavaScript代码。

### 安装React

> **注意：** 安装完成后，我们强烈建议设置生产构建过程以确保您在生产中使用快速版本的React。

我们建议使用[Yarn](https://yarnpkg.com/)或[npm](https://www.npmjs.com/)来管理前端依赖项。如果您对包管理员不[熟悉](https://yarnpkg.com/en/docs/getting-started)，那么[Yarn文档](https://yarnpkg.com/en/docs/getting-started)是开始使用的好地方。

要安装Yarn React，请运行：

```javascript
yarn init
yarn add react react-dom
```

要使用npm安装React，请运行：

```javascript
npm init
npm install --save react react-dom
```

Yarn和npm都从[npm注册表](http://npmjs.com/)下载软件包。

### 启用ES6和JSX

我们建议使用React with [Babel](http://babeljs.io/)让您在JavaScript代码中使用ES6和JSX。ES6是一组现代化的JavaScript特性，可以使开发变得更加简单，JSX是对与React很好地配合的JavaScript语言的扩展。

在[Babel安装说明](https://babeljs.io/docs/setup/)解释如何在许多不同的构建环境配置通天塔。确保你在你的[配置](http://babeljs.io/docs/usage/babelrc/)中安装[`babel-preset-react`](http://babeljs.io/docs/plugins/preset-react/#basic-setup-with-the-cli-)并[`babel-preset-env`](http://babeljs.io/docs/plugins/preset-env/)启用它们，你很好。[`.babelrc`](http://babeljs.io/docs/usage/babelrc/)

### Hello World与ES6和JSX

我们建议使用像[webpack](https://webpack.js.org/)或[Browserify](http://browserify.org/)这样的[打包](https://webpack.js.org/)[程序](http://browserify.org/)，以便您可以编写模块化代码并将它们捆绑在一起，以便优化加载时间。

最小的React示例如下所示：

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

此代码呈现为ID为的DOM元素`root`，因此您需要`<div id="root"></div>`HTML文件中的某处。

同样，您可以在现有应用程序内的某个DOM元素内部渲染一个React组件，该应用程序使用任何其他JavaScript UI库编写。

了解有关将React与现有代码集成的更多信息。

### 开发和生产版本

默认情况下，React包含许多有用的警告。这些警告在开发中非常有用。

**但是，他们使React的开发版本变得更大更慢，因此您应该在部署应用程序时使用生产版本。**

了解如何判断您的网站是否提供正确版本的React，以及如何最有效地配置生产构建过程：

- 使用创建React App创建生产版本

- 使用单个文件构建创建生产版本

- 使用brunch创建生产版本

- 使用Browserify创建生产版本

- 使用汇总创建生产版本

- 使用webpack创建生产版本

### 使用CDN

如果你不希望使用NPM来管理客户端软件包时，`react`和`react-dom`故宫的包还提供了在单个文件的分发`umd`文件夹，这是一个CDN托管：

```javascript
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
```

以上版本仅用于开发，不适合生产。缩小和优化的React生产版本可在以下网址获得：

```javascript
<script crossorigin src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>
```

要加载的特定版本`react`和`react-dom`，替换`16`用的版本号。

如果您使用Bower，则可以通过`react`软件包获取React 。

#### 为什么`crossorigin`属性？

如果您从CDN提供React，我们建议保留[`crossorigin`](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_settings_attributes)属性设置：

```javascript
<script crossorigin src="..."></script>
```

我们还建议验证您使用的CDN是否设置了`Access-Control-Allow-Origin: *`HTTP标头：

![img](https://ask.qcloudimg.com/http-save/devdocs/2nqmym5uw4.png)

这可以在React 16及更高版本中提供更好的[错误处理体验](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com