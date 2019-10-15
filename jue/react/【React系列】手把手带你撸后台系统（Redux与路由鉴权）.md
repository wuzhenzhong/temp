# 【React系列】手把手带你撸后台系统（Redux与路由鉴权）

## 一、前言

本系列文章计划撰文3篇：

- [【React系列】手把手带你撸后台系统（架构篇）](https://juejin.im/post/5d9b5ddee51d45781b63b8f7)
- [【React系列】手把手带你撸后台系统（Redux与路由鉴权）](https://juejin.im/post/5d9b6100e51d4577fe41b666)
- 【React系列】手把手带你撸后台系统（组件化）

**项目地址：**[github react-admin](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fqiudongwei%2Freact-admin)

**系统预览：**[react-admin system](https://link.juejin.im/?target=https%3A%2F%2Fqiudongwei.github.io%2Freact-admin%2F)

上一篇我们介绍了系统架构，这一篇将继续介绍：

- Redux的应用
- 登录授权
- 路由鉴权

## 二、Redux应用

侧边导航栏（Sidebar）我们实现了根据配置渲染菜单项，现在我们要继续完善它的功能：导航高亮与鉴权。我们通过`redux`管理我们Sidebar的状态，想要`redux`的`store`可用，我们必须使用它的`<Provider />`和`connect()`：

```
// Page.js
import { createStore } from  'redux'
import store from 'store' // 新建store目录
ReactDOM.render(<Provider store={ createStore(store) }>
  <Page />
</Provider>, document.getElementById('root'))
复制代码
```

`<Provider />`的作用是让`store`在整个App中可用，`connect()`的作用是将`store`和`component`连接起来。

```
// Sidebar.js
import { connect } from 'react-redux'
import { updateSidebarState } from 'store/action'
class Sidebar extends Component {
    // 省略代码...
}
const mapStateToProps = (state, owns) => ({
    sidebar: prop('sidebarInfo', state), // 从store中取出sidebarInfo数据
    ...owns
})
const mapDispatchToProps = dispatch => ({
    dispatchSideBar: sidebar => dispatch(updateSidebarState(sidebar)) // 更新数据状态
})
export default connect(
    mapStateToProps,
    mapDispatchToProps
)(SideBar)
复制代码
```

**2.1 初始化Sidebar数据**

初始化Sidebar数据主要做两点：

- 默认的路由配置数据里面没有标识高亮状态，初始化过程增加`active`标志位；
- 为父级导航项增加`key`值标志位，用于检测收合状态；

```
// store/reducer.js
function initSidebar(arr) {
  return map(each => {
      if(each.routes) {
          each.active = false
          each.key = generateKey(each.routes) // 生产唯一key值，与路由path关联
          each.routes = initSidebar(each.routes)
      }
      return each
  }, arr)
}

// 更新sidebar状态
function updateSidebar(arr, key='') {
  return map(each => {
      if(key === each.key) {
          each.active = !!!each.active
      } else if(each.routes) {
          each.routes = updateSidebar(each.routes, key)
      }
      return each
  }, arr)
}

export const sidebarInfo = (state=initSidebar(routes), action) => {
  switch (action.type) {
      case 'UPDATE':
          return updateSidebar(state, action.key)
      default:
          return state
  }
}
复制代码
```

经处理后的路由配置数据新增了`active`和`key`两个属性：

![img](https://user-gold-cdn.xitu.io/2019/10/8/16da926e52a868eb?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**2.2 检测高亮**



渲染侧边导航栏过程需要检测高亮状态：根据当前路由`path`与导航项的`key`值相比较：

```
// Sidebar.js
class Sidebar extends Component {
  constructor(props) {
    // ...
    this.state = {
      routeName: path(['locaotion', 'pathname'], this.props), // 获取当前路由信息
      routes: compose(this.checkActive.bind(this), prop('sidebar'))(this.props) // 返回检测后的路由数据
    }
  }
  // 省略代码...
  checkActive (arr, routeName='') {
        const rName = routeName || path(['location', 'pathname'], this.props)
        if(!rName) return arr
        return map((each) => {
            const reg = new RegExp(rName)
            if(reg.test(each.key)) {
                each.active = true
            } else if (each.routes) {
                each.routes = this.checkActive(each.routes, rName)
            }
            return each
        }, arr)
    }
}

export default connect(
    mapStateToProps,
    mapDispatchToProps
)(withRouter(SideBar))
复制代码
```

**特别注意：** Sidebar组件需要经由`withRouter`包裹后才能在组件内获取路由相关信息。

## 三、登录授权

这里**设定的场景**是：用户的登录数据在当前会话期内有效（`sessionStorage`存储用户信息），用户信息全局可用（`redux`管理）。假定我们存储的用户数据有：

```
{
    username: '',  // 帐号
    permission: [],  // 用户权限列表
    isAdmin: false  // 管理员标识
}
复制代码
```

**3.1 初始化用户信息**

```
// store/reducer.js
const userInfo = getSessionStore('user') || { // 首先从sessionStorage中获取数据
    username: '',
    permission: [],
    isAdmin: false
}
export const user = (state=userInfo, action) => {
    switch (action.type) {
        case 'LOGIN': 
            return pick(keys(state), action.user)
        default:
            return state
    }
}
复制代码
```

**3.2 实现登录**

首先将store的state和action注入login组件：

```
import { connect } from 'react-redux'
import { doLogin } from 'store/action'
import Login from './login'
const mapStateToProps = (state, owns) => ({
    user: state,
    ...owns
})
const mapDispatchToProps = dispatch => ({
    dispatchLogin: user => dispatch(doLogin(user))
})
export default connect(
    mapStateToProps,
    mapDispatchToProps
)(Login)
复制代码
```

继而在login.js中实现登录逻辑：

```
class Login extends Component {
  login () {
      const { dispatchLogin, history } = this.props
      const { form } = this.state
      const user = {
          username: '安歌',
          permission: ['add'],
          isAdmin: false
      }
      dispatchLogin(user) // 更新store存储的用户数据
      setSessionStore('user', user) // 将用户数据存储在sessionStorage
      // login success
      history.push('/front/approval/undo') // 登录重定向
  } 
}
复制代码
```

## 四、路由鉴权

上一篇中我们实现了页面级路由，现在我们需要根据路由配置文件，注册应用级路由。回顾下我们的路由配置：

```
export default [
    {
        title: '我的事务', // 页面标题&一级nav标题
        icon: 'icon-home',
        routes: [{
            name: '待审批',
            path: '/front/approval/undo',
            component: 'ApprovalUndo'
        }, {
            name: '已处理',
            path: '/front/approval/done',
            auth: 'add',
            component: 'ApprovalDone'
        }]
    }
]
复制代码
```

我们根据`path`和`component`信息注册路由，根据auth信息进行路由鉴权。看下我们如何实现各个路由对应的组件。

![img](https://user-gold-cdn.xitu.io/2019/10/8/16da708c5a07b3ed?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

在`views``index.js`



```
// views/index.js
import AsyncComponent from 'components/AsyncComponent'
const ApprovalUndo = AsyncComponent(() => import(/* webpackChunkName: "approvalundo" */ 'views/approval/undo'))
const ApprovalDone = AsyncComponent(() => import(/* webpackChunkName: "approvaldone" */ 'views/approval/done'))
export default {
    ApprovalUndo, ApprovalDone
}
复制代码
```

**说明：** 关于AsyncComponent的说明已在上篇有所介绍。

**4.1 注册路由**

```
// router/index.js 
import routesConf from './config'
import views from 'views'
class CRouter extends Component {
    render () {
        return (
            <Switch>
                { pipe(map(each => {
                    const routes = each.routes
                    return this.generateRoute(routes, each.title)
                }), flatten)(routesConf) }
                <Route render={ () => <Redirect to="/front/404" /> } />
            </Switch>
        )
    }
    generateRoute (routes=[], title='React Admin') { // 递归注册路由
        return map(each => each.component ? (
            <Route 
                key={ each.path }
                exact  // 精确匹配路由
                path={ each.path }
                render={ props => {
                    const reg = /\?\S*g/
                    const queryParams = window.location.hash.match(reg)  // 匹配路由参数
                    const { params } = props.match
                    Object.keys(params).forEach(key => { // 去除?参数
                        params[key] = params[key] && params[key].replace(reg, '')
                    })
                    props.match.params = { ...params }
                    const merge = { ...props, query: queryParams ? queryString.parse(queryParams[0]) : {} }
                    const View = views[each.component]
                    const wrapperView = ( // 包装组件设置标签页标题
                        <DocumentTitle title={ title }>
                            <View { ...merge } />
                        </DocumentTitle>
                    )
                    return wrapperView
                } }
            />
        ) : this.generateRoute(each.routes, title), routes)
    }
}
const mapStateToProps = (state, owns) => ({
    user: prop('user', state),
    ...owns
})
export default connect(
    mapStateToProps
)(CRouter)
复制代码
```

我们的路由配置文件支持多级嵌套，递归注返回的`Route`路由也是嵌套的数组，最后需要借助`flatten`将整个路由数组打平。

**4.2 权限管理**

权限管理分为登录校验和权限校验，默认我们的应用路由都是需要登录校验的。

```
class CRouter extends Component {
  generateRoute (routes=[], title='React Admin') {
    // ...
    // 在上一个版本中直接将wrapperView返回，这个版本包裹了一层登录校验
    return this.requireLogin(wrapperView, each.auth)
  }
  requireLogin (component, permission) {
    const { user } = this.props
    const isLogin = user.username || false  // 登录标识, 从redux取
    if(!isLogin) { // 判断是否登录
        return <Redirect to={'/front/login'} />
    }
    // 如果当前路由存在权限要求，则再进入全权限校验
    return permission ? this.requirePermission(component, permission) : component
  }
  requirePermission (component, permission) {
    const permissions = path(['user', 'permission'], this.props) // 用户权限, 从redux取
    if(!permissions || !this.checkPermission(permission, permissions)) return <Redirect to="/front/autherror" />
    return component
  }
  checkPermission (requirePers, userPers) {
    const isAdmin = path(['user', 'isAdmin'], this.props) // // 超管标识, 从redux取
    if(isAdmin) return true
    if(typeof userPers === 'undefined') return false
    if(Array.isArray(requirePers)) { // 路由权限为数组
        return requirePers.every(each => userPers.includes(each))
    } else if(requirePers instanceof RegExp) { // 路由权限设置为正则
        return userPers.some(each => requirePers.test(each))
    }
    return userPers.includes(requirePers)  // 路由权限设置为字符串
  }
}
复制代码
```

在`checkPermission`函数中实现了字符串、数组和正则类型的校验，因此，在我们路由配置文件中的`auth`可以支持字符串、数组和正则三种方式去设置。

如上，结合`redux`的应用，我们轻松实现可配置、高可复用的路由鉴权功能。系统会根据当前用户的权限列表（一般通过接口由后端返回）与配置文件中定义的权限要求进行校验，如果无权限，则重定向至Permission Error页面。

最后，本系列文章：

- [【React系列】手把手带你撸后台系统（架构篇）](https://juejin.im/post/5d9b5ddee51d45781b63b8f7)
- [【React系列】手把手带你撸后台系统（Redux与路由鉴权）](https://juejin.im/post/5d9b6100e51d4577fe41b666)
- 【React系列】手把手带你撸后台系统（组件化）

**项目地址：**[github react-admin](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fqiudongwei%2Freact-admin)

**系统预览：**[react-admin system](https://link.juejin.im/?target=https%3A%2F%2Fqiudongwei.github.io%2Freact-admin%2F)