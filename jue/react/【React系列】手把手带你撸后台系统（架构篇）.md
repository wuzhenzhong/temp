# 【React系列】手把手带你撸后台系统（架构篇）

## 一、前言

本系列文章计划撰文3篇：

- [【React系列】手把手带你撸后台系统（架构篇）](https://juejin.im/post/5d9b5ddee51d45781b63b8f7)
- [【React系列】手把手带你撸后台系统（Redux与路由鉴权）](https://juejin.im/post/5d9b6100e51d4577fe41b666)
- 【React系列】手把手带你撸后台系统（组件化）

**项目地址：**[github react-admin](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fqiudongwei%2Freact-admin)

**系统预览：**[react-admin system](https://link.juejin.im/?target=https%3A%2F%2Fqiudongwei.github.io%2Freact-admin%2F)

本系列文章将介绍从零开始搭建一个高可复用的后台架构系统，让每一个人都能轻松搭出自己的后台。系统功能包含**登录授权**、**路由鉴权**与**组件化**，涉及`react-router`与`react-redux`的应用。系统最终实现的效果：



![img](https://user-gold-cdn.xitu.io/2019/10/7/16da6e79c49f9b3e?imageslim)

本篇介绍的内容包含：



- 脚手架环境搭建与构建配置说明
- 基础架构设计
- 侧边栏实现
- 初探路由

## 二、React环境搭建

### 2.1 脚手架环境

创建一个React App，目测有3到4种方法，这是官网文档的说明，可以移步查看。我们这里主要介绍`npx`和`npm`两种方式。

**2.1.1 什么是npx？**

`npx`是`npm@5.2`版本新增的命令，如果你的`npm`版本是高于这个版本的，那么你可以直接使用`npx`命令了；否则就需要全局安装：`npm install -g npx`。但个人建议是**升级npm**：`npm install -g npm@next`。

`npx`的愿景就是：**让天下没有难调用的模块**。譬如我们项目中集成了`Eslint`模块，我们想要在命令行调用该模块必须像这样：`./node_modules/.bin/eslint --init`；有了`npx`，我们可以直接：`npx eslint --init`。

同时，**`npx`还能避免全局安装，真正的属于用完即走。**早先以前，我们使用`create-react-app`创建`react`应用，必须要全局安装该模块，`react script`才可用。现在，通过`npx`执行`npx create-react-app my-app`，`npx`会将`create-react-app`下载在一个临时目录，并让`react script`全局可用，用完之后再删除，待下次需要时再重新下载。

**使用npx创建本后台应用：**`npx create-react-app my-admin`

**2.1.2 npm init经历了什么？**

在@5.2版本之前，执行`npm init`会在当前目录创建一个`package.json`文件，而在@5.2及之后的版本，我们可以通过`npm init <initializer>`命令创建一个应用，譬如：`npm init react-app my-app`。其本质也是内部调用`npx`命令，会自动将`react-app`补全为`create-react-app`，继而下载并创建应用。

**使用npm init创建本后台应用：**`npm init react-app my-admin`

### 2.2 构建配置

在上一步骤创建应用之后，默认的目录架构像下面这样：

![img](https://user-gold-cdn.xitu.io/2019/10/7/16da6e881f4535ae?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

执行`npm eject``config``script``webpack`

![image](https://user-gold-cdn.xitu.io/2019/10/7/16da6e8d8e6b593f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



其次，我们还可以在一些特殊文件中定义配置信息：新建`jsconfig.json`文件，并填充如下内容：

```
{
    "compilerOptions": {
      "baseUrl": "src"  // 编译根路径
    }
}
复制代码
```

### 2.3 目录说明

最终我们系统的目录架构如下：

```
├── config   // 配置相关   
├── script   // 构建脚本 
├── public   // 应用对外目录 
├── src      // 源代码 
│   ├── components  // 公共组件
│   ├── font        // 字体相关
│   ├── js          // js库
│   ├── router      // 路由相关
│   ├── scss        // 样式表
│   ├── store       // redux相关
│   ├── views       // 页面应用
│   ├── index.js    // 入口文件
│   ├── Page.js     // 页面路由入口
│   ├── App.js      // 主应用入口
复制代码
```

## 三、基础架构

从主应用入口文件·App.js·开始，根据我们系统后台的布局，用以下代码替换原有代码：

```
import React, { Component } from 'react'
class App extends Component {
  render () {
    return (
      <div className="container">
        <section className="sidebar">
          侧边导航栏
        </section>
        <section className="main">
          <header className="header">
            <span className="username">Hi, 安歌</span>
          </header>
          <div className="wrapper">
            主体内容
          </div>
          <footer className="footer">
            <span className="copyright">Copyright@2019 安歌</span>
          </footer>
        </section>
      </div>
    )
  }
}
export default App
复制代码
```

加上样式：在`scss`目录新建`index.scss`，同时删除根目录的`App.css`文件（根据个人习惯选择`scss`或其他的预编译语言），并在`index.js`中添加样式表的引用。可以得到如下效果：

![img](https://user-gold-cdn.xitu.io/2019/10/7/16da6ea4ff125244?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## 四、侧边导航栏

4.1 在`components`目录新建`SideBar.js`组件，同时在`router`目录下新建路由配置文件`config.js`，这份配置文件由侧边栏跟路由共用：

```
// Sidebar.js: 侧栏导航组件，侧栏菜单在router/config.js配置
import React, { Component } from 'react'
class SideBar extends Component {
    constructor (props) {
        super(props)
        this.state = {
            routes: []  // 路由列表
        }
    }
    render () {
        return (
            <ul className="sidebar-wrapper">
                侧边栏
            </ul>
        )
    }
}
export default SideBar
复制代码
```

4.2 定义路由配置文件：**支持多级嵌套，routes与component不能同级共存**，如果存在子菜单，则用`routes`字段，否则使用`component`字段。

```
// router/config.js
export default [
    {
        title: '我的事务', // 页面标题&一级nav标题
        icon: 'icon-home',
        routes: [{
            name: '待审批',  // 次级nav标题
            path: '/front/approval/undo', // 路由url
            component: 'ApprovalUndo'  // 路由组件
        }, {
            name: '已处理',
            path: '/front/approval/done',
            auth: 'add',  // 访问所需权限
            component: 'ApprovalDone'
        }]
    },
    // ...
]
复制代码
```

4.3 使用递归渲染侧边栏

```
// SideBar.js
import React, { Component, Fragment } from 'react'
class SideBar extends Component {
    constructor (props) {
        // ...
        this.generateSidebar = this.generateSidebar.bind(this)
    }
    render () {
        return ( // 渲染侧边栏
            <ul className="sidebar-wrapper">
                { map(this.generateSidebar, this.state.routes) }
            </ul>
        )
    }
    generateSidebar (item) { // 一级nav
        return <li className="sidebar-item" key={item.title}>
            <div className={ className({
                'sidebar-item-name': true,
                'on': item.active  /* 当前菜单展开/收起标识 */
            }) }>
                <span>
                    { item.title }
                </span>
            </div>
            <ul className="sidebar-sub">
                { this.generateSubMenu(item.routes) }
            </ul>
        </li>
    }
    generateSubMenu (routes) { // 子级nav
        return map(each => <li className="sidebar-sub-item" key={ each.name }>
            { each.component ? <a href={ each.path }>{ each.name }</a> : (
                <Fragment>
                    <div className={ className({
                        'sidebar-item-name': true,
                        'on': each.active // 当前菜单展开/收起标识
                    }) }>
                        { each.name }
                    </div>
                    <ul className="sidebar-sub">
                        { this.generateSubMenu(each.routes) }
                    </ul>
                </Fragment>
            ) }
        </li>, routes)
    }
}
复制代码
```

加上样式之后如下图：

![img](https://user-gold-cdn.xitu.io/2019/10/7/16da6eb89cce84a3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

`SideBar.js`中预留两个API留待下篇解说：

- `checkActive`: 根据当前访问路由检测菜单项的收合状态；
- `hasPer`: 检测当前用户是否有菜单项的访问权限，有则渲染，无则跳过渲染；

## 五、基础路由

我们需要借助`react-router`实现几个页面：登录、404以及权限错误。这些属于页面级路由，可以归在`views`目录下，这里我们在`views`目录新建`login`目录作为登录应用、`components`目录下新建`404.js`和`AuthError.js`，将其视作组件：

```
├── components   // 公共组件 
│   ├── 404.js          // not found
│   ├── AuthError.js    // permission error
├── views       // 公共组件 
│   ├── login           // 登录应用
│   │   ├── index.js    // 登录页面入口
复制代码
```

Page.js注册路由：

```
// Page.js
import React, { Component } from 'react'
import { BrowserRouter as Router, Switch, Route, Redirect } from 'react-router-dom'
import App from 'App'
import AsyncComponent from 'components/AsyncComponent'
const Login = AsyncComponent(() => import(/* webpackChunkName: "login" */ 'views/login'))
const NotFound = AsyncComponent(() => import(/* webpackChunkName: "404" */ 'components/404'))
const AuthError = AsyncComponent(() => import(/* webpackChunkName: "autherror" */ 'components/AuthError'))
class Page extends Component {
    render () {
        return (
            <Router>
                <Switch>
                    <Route exact path="/" render={ () => <Redirect to="/front/approval/undo" push /> } />
                    <Route path="/front/404" component={ NotFound }/>
                    <Route path="/front/autherror" component={ AuthError } />
                    <Route path="/front/login" render={ () => {
                        const isLogin = false // 登录状态从redux获取
                        return isLogin ?  <Redirect to="/front/approval/undo" /> : <Login />
                    } } />
                    <Route render={ () => <App /> } />
                </Switch>
            </Router>
        )
    }
}
export default Page
复制代码
```

`Route`接口用于注册路由并定义渲染逻辑，`Page.js`引入了主应用`App.js`文件，因此主入口文件index.js需要引入`Page.js`并渲染（将原来的引入`App.js`改成`Page.js`即可）。

## 六、异步组件

在上一章节基础路由部分我们用到了一个组件函数`AsyncComponent`，它是一个工厂函数，用于异步解析我们的组件定义。在大型应用中，我们会使用很多的异步组件，譬如`() => import(/* webpackChunkName: "login" */ 'views/login')`是一个异步组件，它返回一个`promise`，在React里面，我们需要自己写一个工厂函数来解析，让`Promise`变为React可用的组件。`AsyncComponent`的逻辑很简单：异步解析和渲染：

```
// AsyncComponent.js
import React, { Component } from 'react'
export default function asyncComponent (importComp) {
    class AsyncComponent extends Component {
        constructor (props) {
            super(props)
            this.state = {
                component: null
            }
        }
        async componentDidMount () {
            const { default: component } = await importComp()
            this.setState({
                component: component
            })
        }
        render () {
            const C = this.state.component
            return C ? <C {...this.props} /> : null
        }
    }
    return AsyncComponent
}
复制代码
```

如上一顿操作之后，我们访问`/front/login`、`/front/404`、`/front/autherror`就能分别访问到登录、Page Not Found和Permission Error页面了，访问根路径会被重定向到`/front/approval/undo`。

以上几个路由属于页面级路由，访问无需授权，在下一篇中我们介绍应用级路由，并实现路由鉴权功能。

其他系列文章

- [【React系列】手把手带你撸后台系统（Redux与路由鉴权）](https://juejin.im/post/5d9b6100e51d4577fe41b666)
- 【React系列】手把手带你撸后台系统（组件化）

**项目地址：**[github react-admin](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fqiudongwei%2Freact-admin)

**系统预览：**[react-admin system](https://link.juejin.im/?target=https%3A%2F%2Fqiudongwei.github.io%2Freact-admin%2F)