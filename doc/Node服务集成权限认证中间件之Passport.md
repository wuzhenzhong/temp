# Node服务集成权限认证中间件之Passport

0.0812017.03.12 23:39:18字数 1009阅读 4174

# 关于权限认证

> 权限认证是几乎任何系统都会存在的一个重要模块，但是往往权限认证又会是比较麻烦且复杂的模块。所以很多的权限认证框架因此诞生，在Java的Spring应用中，就有包括Shrio和SpringSecurity等十分成熟可靠的权限认证框架。

> 那么在Node中有没有类似的实现框架，解放Node服务开发人员的双手呢
>
> 答案当然是：有...

# Passport

> passport是Node下的权限认证框架，轻量，简单，拓展性强。废话少说，直接来看使用，下面会以一个本地用户名密码认证模块为例，讲解开发步骤——

## 实现目标

> 登录的用户信息存储在服务器的session中，然后访问时从session中获取用户信息，如果没有获取到用户信息，则认为该请求非法，以上，集成passport之后，会全自动完成，无须人工干预。

## 准备工作

> npm install passport --save
>
> npm install passport-local --save
>
> npm install express-session --save



## 第一步：加载passport；需要在app.js中引入passport，然后提供给express作为中间件使用

```js
var expressSession = require('express-session');

var passport = require('./auth/passport_config.js');

// 使服务器支持session

app.use(expressSession({

secret: 'cheneyxu',

resave: false,

saveUninitialized: false

}));

// 初始化调用 passport

app.use(passport.initialize());

app.use(passport.session());
```



## 第二步：在路由中加载passport作为中间件提供认证服务；需要在router.js中增加两个路由处理

```js
var passport = require('../auth/passport_config.js');

// 登录认证

router.post('/user/login', passport.authenticate('local'), function(req, res) {

log.info(req.user);

res.send("success");

});

// 认证测试

router.get('/user/testauth', passport.authenticateMiddleware(), function(req, res) {

res.send('允许访问');

});
```



## 第三步：实现passport_config.js的核心配置文件

```js
var passport = require('passport');

var LocalStrategy = require('passport-local');

var UserModel = require('../model/UserModel');// 实体数据库模块

var log = require('tracer').colorConsole({ level: require('config').get('log').level });// 日志

// 用户名密码验证策略

passport.use(new LocalStrategy(

/**

\* @param username 用户输入的用户名

\* @param password 用户输入的密码

\* @param done 验证验证完成后的回调函数，由passport调用

*/

function(username, password, done) {

UserModel.findOne({ username: username }).then(function(result) {

if (result != null) {

if (result.password == password) {

return done(null, result);

} else {

log.error('密码错误');

return done(null, false, { message: '密码错误' });

}

} else {

log.error('用户不存在');

return done(null, false, { message: '用户不存在' });

}

}).catch(function(err) {

log.error(err.message);

return done(null, false, { message: err.message });

});

}

));

// serializeUser 用户登录验证成功以后将会把用户的数据存储到 session 中

passport.serializeUser(function(user, done) {

done(null, user);

});

// deserializeUser 每次请求的时将从 session 中读取用户对象，并将其封装到 req.user

passport.deserializeUser(function(user, done) {

return done(null, user);

});

// 这是封装了一个中间件函数到 passport 中，可以在需要拦截未验证的用户的请求的时候调用

passport.authenticateMiddleware = function authenticationMiddleware() {

return function(req, res, next) {

if (req.isAuthenticated()) {

return next();

}

// res.redirect('/user/login');

res.send('非法访问');

}

};

module.exports = passport;
```



#### 就这样把大象放进冰箱的三步，完成了passport的本地用户名密码方式的集成，关于passport更详细的使用方法会在今后的文章中陆续说明，passport远远不止能实现用户名密码认证，更能实现OAuth，OpenID,甚至是其他网络登录认证服务例如facebook，twiter等，官方文档传送门如下：

[passport官方文档](https://link.jianshu.com/?t=http%3A%2F%2Fwww.passportjs.org%2Fdocs)

最后，本文实现的代码已经集成到了个人的轻量极简风格的Node无后端框架中，传送门如下：

[XModel——无后端框架只需要写实体类，然后直接RESTful请求，全自动CRUD](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fcheneyweb%2Fexpress-xmodel)

如果有任何的批评建议，BUG反馈，问题反馈，或是想法建议，帮助支持，个人都十分欢迎，我个人的联系方式如下：）

> 作者：CheneyXu
>
> 关于：[XServer官网](https://link.jianshu.com/?t=http%3A%2F%2Fwww.xserver.top)