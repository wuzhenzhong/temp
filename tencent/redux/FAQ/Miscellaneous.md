# Redux FAQ：杂项（Miscellaneous）

- 贡献者2人

  

## 目录

- 有没有更大的“真正的” Redux 项目？

- 我如何在 Redux 中实现身份验证？

## 杂项

### 有没有更大的“真正的” Redux 项目？

是的，很多！仅举几例：

- [Twitter的移动网站](https://twitter.com/necolas/status/727538799966715904)

- [Wordpress的新的管理页面](https://github.com/Automattic/wp-calypso)

- [Firefox's new debugger（](https://github.com/jlongster/debugger.html)Firefox的新调试器[）](https://github.com/jlongster/debugger.html)

- [Mozilla's experimental browser testbed（](https://github.com/mozilla/tofino)Mozilla的实验性浏览器测试平台[）](https://github.com/mozilla/tofino)

- [The HyperTerm terminal application（](https://github.com/zeit/hyperterm)HyperTerm终端应用程序[）](https://github.com/zeit/hyperterm)

还有更多！Redux 插件目录有[**一个基于 Redux 的应用程序和示例列表**](https://github.com/markerikson/redux-ecosystem-links/blob/master/apps-and-examples.md)，指向各种实际的应用程序，无论大小。

#### 更多信息

**文档**

- 介绍：示例

### **讨论**

- [Reddit: Large open source react/redux projects?（](https://www.reddit.com/r/reactjs/comments/496db2/large_open_source_reactredux_projects/)Reddit：大型开源反应/ redux 项目？[）](https://www.reddit.com/r/reactjs/comments/496db2/large_open_source_reactredux_projects/)

- [HN: Is there any huge web application built using Redux?（](https://news.ycombinator.com/item?id=10710240)HN：是否有使用 Redux 构建的大型 Web 应用程序？[）](https://news.ycombinator.com/item?id=10710240)

### 我如何在 Redux 中实现身份验证？

认证对于任何实际应用都至关重要。在进行身份验证时，您必须记住，您应该如何组织应用程序并没有什么变化，您应该像使用其他任何功能一样实施身份验证。它相对简单：

1. 创建行动常量`LOGIN_SUCCESS`，`LOGIN_FAILURE`等等。

\2. 创建接收凭据的操作创建者，标识验证是否成功的标志，令牌或错误消息作为有效负载。

\3. 使用 Redux Thunk 中间件或您认为适合的任何中间件创建异步操作创建器，以便向 API 发送网络请求，该 API 在证书有效时返回令牌。然后将令牌保存在本地存储器中，或者在失败时向用户显示响应。您可以从您在上一步中编写的动作创作者执行这些副作用。

\4. 创建一个返回下一个状态为每个可能的认证的情况下（一个减速器`LOGIN_SUCCESS`，`LOGIN_FAILURE`等）。

#### 更多信息

**文章**

- [Authentication with JWT by Auth0（](https://auth0.com/blog/2016/01/04/secure-your-react-and-redux-app-with-jwt-authentication/)通过Auth0使用JWT进行身份验证[）](https://auth0.com/blog/2016/01/04/secure-your-react-and-redux-app-with-jwt-authentication/)

- [Tips to Handle Authentication in Redux（](https://medium.com/@MattiaManzati/tips-to-handle-authentication-in-redux-2-introducing-redux-saga-130d6872fbe7)提示在Redux中处理验证[）](https://medium.com/@MattiaManzati/tips-to-handle-authentication-in-redux-2-introducing-redux-saga-130d6872fbe7)

**例子**

- [react-redux-jwt-auth-example](https://github.com/joshgeller/react-redux-jwt-auth-example)

### **Libraries**

- [Redux Addons Catalog: Use Cases - Authentication（](https://github.com/markerikson/redux-ecosystem-links/blob/master/use-cases.md#authentication)Redux插件目录：用例 - 身份验证[）](https://github.com/markerikson/redux-ecosystem-links/blob/master/use-cases.md#authentication)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18