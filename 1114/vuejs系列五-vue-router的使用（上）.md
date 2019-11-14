# vuejs系列五-vue-router的使用（上）

# 前言

一个web应用路由定义的是否合理是判断这个应用是否合格的基础条件之一，在spa开发模式之前，前端开发基本不用考虑路由的定义这块基本都是后台在完成，但随着spa的推广前后端分离的大趋势下，前端路由定义的任务便落在的我们前端开发者身上。本节我们就来聊聊vue中vue-router的路由定义与配置。

# vue-router起到的作用

- 路由匹配：组件 (components) 映射到路由 (routes)，然后告诉 Vue Router 在哪里渲染它们。
- 动态路由：以多个路由映射到同一个组件，即把某种模式匹配到的所有路由，全都映射到同个组件。
- 路由拦截（导航守卫）：登录认证以及权限认证都需要导航守卫来控制。
- 前端控制路由

# vue-router使用方式

## 引入

- 将vue-router引入
- 定义路由组件

```
import Foo from './Foo.vue'
import Test from './Test.vue'
复制代码
```

- 配置路由和组件到映射

```
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Test }
]
复制代码
```

- 将路由配置文件放到VueRouter对象中

```
let router =  new VueRouter({
  routes,
  //默认为hash模式
  mode:'history'
})
export default router
复制代码
```

- 将路由实例挂载到vue对象上

```
const router = new VueRouter({
  routes // (缩写) 相当于 routes: routes
})
const app = new Vue({
  router
}).$mount('#app')

复制代码
```

## 主要api详解

- 配置路由

```
interface RouteConfig = {
  path: string,//路径
  component?: Component,//路径对应加载对组件
  name?: string, // 命名路由
  components?: { [name: string]: Component }, // 命名视图组件
  redirect?: string | Location | Function,//重订向对路由
  props?: boolean | Object | Function,
  alias?: string | Array<string>,//别名
  children?: Array<RouteConfig>, // 嵌套路由的自路由
  beforeEnter?: (to: Route, from: Route, next: Function) => void,//路由守卫
  meta?: any,//其他项 挂载在this.$route上

  // 2.6.0+
  caseSensitive?: boolean, // 匹配规则是否大小写敏感？(默认值：false)
  pathToRegexpOptions?: Object // 编译正则的选项
}

复制代码
```

- 路由模式：改变视图的同时不会向后端发出请求

  ```
  mode:'history'//默认为hash模式
  复制代码
  ```

  ### hash模式是带有#的路由模式，一般我们生产环境部署都不会采用他。

  - 优点：当没有匹配的路由时不会404，即服务端对hash模式影响不大
  - 缺点：带有#号，不符合正常路由规范

  ### history模式是你实际配置对路由，无#号

  - 优点1：设置的新 URL 可以是与当前 URL 同源的任意 URL；而 hash 只可修改 # 后面的部分，因此只能设置与当前 URL 同文档的 URL
  - 优点2: 实际设置路由，无#号。
  - 优点3: 添加记录（与hash路由相比，无论变化路由是否相同都会添加到记录栈中）
  - 优点4: 添加任意类型到记录（hash只能添加短字符串）

  ### 路由跳转

- 常用api如下：

  ```
    router.push
    router.replace
    router.go
    router.back
    router.forward
  复制代码
  ```

- 路由导航守卫 vue-router中常用到路由导航守卫有beforeEach和afterEach

  ### beforeEach

  - 路由前置导航守卫，回调函数有三个参数from to next在调用next前 路由不会被解析。

  ```
  const router = new VueRouter({ ... })
  router.beforeEach((to, from, next) => {
    // ...
    next()
  })
  复制代码
  ```

  ### afterEach路由后置钩子

- 过渡动效：transition组件,使用方式如下

  ```
  <transition>
    <router-view></router-view>
  </transition>
  复制代码
  ```

- 滚动:vue-router有个可以控制路由跳转到新页面是的滚动位置的属性

```
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
复制代码
```

注意: 这个功能只在支持 history.pushState 的浏览器中可用。详细请查看[官方文档](https://router.vuejs.org/zh/guide/advanced/scroll-behavior.html#异步滚动)

# 总结

路由配置现在已经是前端工程中必不可少的配置，合理的路由配置能大大提升用户体验度，并且还能改善项目结构。