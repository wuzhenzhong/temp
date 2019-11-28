# Static Type Checking

像[Flow](https://flowtype.org/)和[TypeScript](https://www.typescriptlang.org/)这样的静态类型检查程序可以在运行代码之前识别某些类型的问题。他们还可以通过添加自动完成功能来改善开发人员的工作流程。出于这个原因，我们建议使用Flow或TypeScript而不是`PropTypes`更大的代码库。

## 流

[纠错](javascript:;)

以下是将流程添加到您的React应用程序的说明。（您可以[在这里](https://flow.org/en/docs/react/)了解更多关于使用Flow with React的[信息](https://flow.org/en/docs/react/)。）

### 与巴别尔一起使用流

首先安装Babel。如果你还没有这样做，这里有一个[有用的设置指南](http://babeljs.io/docs/setup/)。

接下来安装`babel-preset-flow`有两种[纱线](https://yarnpkg.com/)或[NPM](https://www.npmjs.com/)。

```javascript
yarn add --dev babel-preset-flow
# or
npm install --save-dev babel-preset-flow
```

然后添加`flow`到您的Babel预设配置。

```javascript
{
  "presets": ["flow"]
}
```

### 通过创建React App使用Flow

[创建React App](https://github.com/facebookincubator/create-react-app)默认支持Flow。只需[安装Flow](https://flow.org/en/docs/install/)并`.flowconfig`通过运行创建一个文件`flow init`。

```javascript
create-react-app my-app
cd my-app
yarn add --dev flow-bin
yarn run flow init
```

流程现在将作为`create-react-app`脚本的一部分运行。

## TypeScript

你可以[在这里](https://github.com/Microsoft/TypeScript-React-Starter#typescript-react-starter)了解更多关于使用TypeScript和React的知识。

### 使用TypeScript和Create React App

[react-scripts-ts](https://www.npmjs.com/package/react-scripts-ts)自动配置`create-react-app`项目以支持TypeScript。你可以像这样使用它：

```javascript
create-react-app my-app --scripts-version=react-scripts-ts
```

你也可以尝试[打字稿 - 反应 - 启动器](https://github.com/Microsoft/TypeScript-React-Starter#typescript-react-starter)。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18