# nodeJS简单介绍及使用nodeJS操纵数据库

0.4292017.07.14 22:51:37字数 3217阅读 9503

# NodeJS的基本概念

## NodeJS是什么？

`官网:
[https://nodejs.org/en/](https://link.jianshu.com/?t=https://nodejs.org/en/)

Node.js® is a JavaScript runtime built on
Chrome's V8 JavaScript engine. Node.js uses
an event-driven, non-blocking I/O model that
makes it lightweight and efficient. Node.js'
package ecosystem, npm, is the largest
ecosystem of open source libraries in the
world.`

js能做什么事?
js ---> 浏览器中运行(面向过程--->面向对象)
js ---> 后台开发

NodeJS就是使用js代码，来做后台开发

使用NodeJS可以开启一个Web服务，给浏览器提供数据去
展示，并且接收浏览器提交过来的用户产生的数据，存储
到数据库中，方便后面使用。

## NodeJS能做什么？

1、提供数据给浏览器展示
2、保存用户提交过来的数据
3、数据统计与分析

## 前端开发者为什么要学习nodeJS

1、要和后台打交道，明白后台提供给我们的数据是经过哪几
2、以后在开发过程中方便调试
3、迈向全栈工程师的开始
4、搭建个人网站

------

# Node服务器软件的安装与配置

## Node.exe的安装

下载nodeJS，安装

检测是否安装成功 node -v

另外一种安装我们node的方式
使用nvm这个软件来安装
node version manger,如果你想同时安装多个node版本
教程:[http://www.jianshu.com/p/07c3456e875a](https://www.jianshu.com/p/07c3456e875a)

步骤:
1、安装nvm这个软件:
[https://github.com/coreybutler/nvm-windows/releases](https://link.jianshu.com/?t=https://github.com/coreybutler/nvm-windows/releases)

2、使用上面装好的nvm软件，安装我们需要的node版本了
指令:
nvm install 具体的版本号就行了
nvm uninstall 具体的版本号
nvm list 查看当前安装了哪些版本
nvm use 具体版本号，切换到某个版本

建议:
安装一个高一点的稳定的版本即可，因为软件都是向下兼容

------

# 系统环境变量及其作用

## 系统环境变量

每个系统都会提供一种叫做环境变量的东西，用来简化我们去
访问某一个应用程序可执行文件(.exe)的操作

我们配置了环境变量能做到什么事呢？
在我们终端的任何一个目录下，都可以访问，配置在系统
环境变量里面的可执行文件

如何将一个软件的可执行文件配置在我们的系统环境变量中?
步骤：
1、拷贝一个可执行文件所在的目录，比如：
node.exe所在的目录 `C:\Program Files\nodejs`

2、系统 > 高级系统设置 > 高级 > 环境变量 >
系统变量 > Path > 填写上你的目录

注意事项:
如果更改了系统的环境变量，就必须把终端重新启动

------

# 启动node.exe执行js代码

## 启动(相当于启动Apache服务器)

1、在我们的node的安装目录下，去双击我们node.exe

2、在终端输入 node即可 node.exe

## 退出我们的node.exe

1、在终端中输入.exit
2、连续按住两次 CTRL + C

## 怎么去执行js代码

1、直接在我们启动的node.exe中写代码(在开启的`REPL`环境中写代码执行)
缺点:
书写不方便，阅读起来也不方便
因为在我们的cmd中写的代码，是放在内存中的，
一旦我们退出了node.exe，原先写的代码都没有了

2、把我们写好的代码放在一个单独的js文件中去执行

在终端中输入 node.exe +执行的文件名称

注意:
1、我们js代码不是在终端中运行的，只是借助终端
去启动我们node.exe，并且最终将结果展现在终端里面而已

2、在运行时候，首先你的终端的目录得切换到你要
执行的文件的目录下面去，然后使用node 文件名称执行即可
我们nodejs的代码是在一个叫做`REPL`环境中，执行的

------

# REPL

## JS的执行

执行js在浏览器端，我是是要依靠浏览器(js的解析引擎)

在服务器端 nodejs开启的REPL环境

官网的解释:
参考:[http://shouce.qdfuns.com/nodejs/repl.html](https://link.jianshu.com/?t=http://shouce.qdfuns.com/nodejs/repl.html)

REPL就是当通过node.exe启动之后开辟的一块内存空间，
在这块内容空间里面就可以解释执行我们的js代码

例如:
在终端中输入了 node abc.js 做的事情就是，将abc.js中
写好的js的逻辑代码扔在启动好的node的内容空间中去运行，
我们把启动好的node的这块内存空间称之为REPL环境

------

# 模块化思想

## 为什么前端需要有模块化

1、解决全局变量名污染的问题
2、把相同功能的代码放在一个模块(一个js文件中)方便后期维护
3、便于复用

## NodeJS中如何体现模块化

1、Node本身是基于CommonJS规范，
参考:[http://javascript.ruanyifeng.com/nodejs/module.html#toc0](https://link.jianshu.com/?t=http://javascript.ruanyifeng.com/nodejs/module.html#toc0)

2、Node作者在设计这门语言的时候，就严格按照CommonJS
的规范，将它的API设计成模块化了，比如它将开启Web服务这
个功能所有代码都放入一个http模块中

3、Node本质来说就是将相同功能的代码放入到一个.js文件中管理

## 常用NodeJS中的模块

```undefined
模块              作用
http              开启一个Web服务，给浏览器提供服务
url            给浏览器发送请求用，还可以传递参数(GET)
querystring       处理浏览器通过GET/POST发送过来的参数
path              查找文件的路径
fs              在服务器端读取文件用的

上面五大核心模块加上其它一些第三方的模块，就可以完成基本的数据库操作了
```

------

# nodeJS核心模块及其操作

## http

```jsx
使用http模块开启web服务
步骤:
    //1、导入我们需要的核心模块(NodeJS提供的模块我们称之为核心模块)
        var http = require('http');
    
    //2、利用获取到的核心模块的对象，创建一个server对象
        var server = http.createServer();

    //3、利用server对象监听浏览器的请求，并且处理(请求-处理-响应)
        server.on('request',function(req,res){
             res.end("welcome");
          });

    //4、开启web服务开始监听
        server.listen(8080,'127.0.0.1',function(){
            console.log('开启服务器成功');
          });
```

## url

1、导入url这个核心模块

2、调用url.parse(url字符串,true)，如果是true的话代表把我们
的username=zhangsan&pwd=123 字符串解析成js对象

```jsx
 // 使用url模块获取url中的一些相关信息
const url = require('url')
var testURL = http://127.0.0.1:8899/login?username=zhangsan&pwd=123 
console.log(url.parse(testURL,true))//{username:zhangsan,pwd:123}
```

## QueryString

作用:
将GET/POST传递过来的参数，进行解析
GET : ?username=zhangsan&pwd=123
POST : username=zhangsan&pwd=123

```jsx
使用:
    const querystring = require('querystring')
    
    const paramsObj = querystring.parse(键值对的字符串)
```

------

## GET&POST

相同点:
都是HTTP协议的方法
都能传递参数给服务器

不同点:
1、传参的方式不一样
GET 放在路径后面 ?开始，后面键值对
POST 放在请求体 键值对的方式

2、传参的限制不一样
GET 2048B
POST 2M

3、GET有缓存，POST没有

4、GET传参不安全，POST相对安全

建议：
如果只是单纯的获取数据，就用GET，因为GET有缓存效率高

如果是要向服务器提交数据，就用POST

------

## fs&path

### path

作用：获取路径

```csharp
path.join(__dirname,'你要读取的文件夹下面的文件名称即可')

__dirname全局属性，代表当前文件所在的文件夹路径

path.join会自动判断文件的路径，并且给他加上`/`
```

### fs

```css
作用:读取服务器硬盘上面的某一个文件(操作文件)

fs.readFile ： 异步读取服务器硬盘上面的某一个文件
```

------

```css
fs:node去读取服务器硬盘中的文件(操作文件)

path:获取文件的路径

上面两个基本上配合起来用
```

------

### 自定义模块

CommonJS规范认为，一个.js文件就可以看成一个模块，如果我们想把模块中定义的变量，方法，对象给外面的js使用，就必须使用CommonJS提供module将我们需要给外面用的东西，导出去

### 注意点

在commonjs中导入模块用 require
在commonjs中在模块中导出 使用module.exports
如果是自定义模块，在导入自定义模块的时候，得把路径写完整
require导入的东西，就是别的文件modulu.exports导出的东西

------

# Express 框架

## 基本概念

它是对HTTP封装，用来简化我们网络功能那一块

官网:[http://www.expressjs.com.cn/](https://link.jianshu.com/?t=http://www.expressjs.com.cn/)
官方解释:
基于 Node.js 平台，快速、开放、极简的 web 开发框架。

## 重点

1、如何去接收GET/POST传递过来的参数
2、如何通过Express进行分门别类的处理路由
3、静态资源的处理

## 使用

1、Hello World 案例

步骤:
1、导入包
2、创建一个app
3、请求处理响应
4、开启web服务，开始监听

2、获取GET/POST参数
GET参数：登录 [http://127.0.0.1:3000/login?username=zhangsan&pwd=123](https://link.jianshu.com/?t=http://127.0.0.1:3000/login?username=zhangsan&pwd=123)

可以直接在我们的req.query中就可以获取了

POST参数：因为express没有直接提供获取POST参数的方法，需要借助一个第三方包 body-parser
参考:
[https://www.npmjs.com/package/body-parser](https://link.jianshu.com/?t=https://www.npmjs.com/package/body-parser)

步骤:
1、npm install body-parser --save
2、导包
3、实现某些方法

最后通过req.body即可以获取到post提交过来的参数

## 路由处理

前端路由:
作用:当触发了某个超链接之后，根据路由的配置，决定
跳转到哪个页面，最终将这个页面呈现出来

后台的路由
作用:就是用来分门别类的出路用户发送过来的请求

```cpp
    http://127.0.0.1:3000/login
    http://127.0.0.1:3000/register
    
    http://127.0.0.1:3000/getGoodsList
    http://127.0.0.1:3000/getGoodsInfo
    
    jd购物
    男士:(专门创建一个man.js文件来实现男士区域商品的请求)
        http://www.jd.com/man/xz
        http://www.jd.com/man/ld
        http://www.jd.com/man/px
        
    女士:(专门创建一个girl.js文件来实现女士区域商品的请求)
        http://www.jd.com/girl/xs
        http://www.jd.com/girl/bag
        http://www.jd.com/girl/kh
```

### express中代码实现?

步骤:
1、先要创建一个单独的路由(js文件)，来处理某一类
请求下面的所有用户请求，并且需要导出去
1.1 导入包 express
1.2 创建一个路由对象
`const manRouter = express.Router()`
1.3 在具体的路由js中处理属于我们该文件的路由
manRouter.get(xxx)
manRouter.post(xxx)
1.4 将上面创建的路由对象导出去，在入口文件中使用

2、在入口文件中，导入我们的路由文件，并且使用就可以了

~~~php
//导入路由文件
const manRouter = require(path.join(__dirname,"man/manRouter.js"))
//在入口文件中使用
app.use('/man',manRouter)
                ```
                
## Express中静态资源的处理
Express希望对我们后台静态资源处理，达到简单的目的，
    然后只希望我们程序员写一句话就能搞定
    
步骤:
        1、在我们入口文件中设置静态资源的根目录
            注意点:一定要在路由处理之前设置
~~~

app.use(express.static(path.join(__dirname,'statics')))
\```

2、在我们的页面中，按照我们Express的规则来请求后台
静态资源数据
写link的href,script的src写的时候，除开静态资源根
路径之外，按照他在服务器上面的路径规则写

------

# mongodb数据库

## 数据库

保存数据的仓库，数据库本质也是一个文件，只是说和普通的
文件不太一样，他有自己的存储规则，让我们保存数据和查询
数据更加方便

## 存储文件的介质

localStorage 文本文件
大型数据或是海量数据的时候必须要用到数据库

## 数据库的分类

客户端：
iOS/Android/前端
iOS/Android SQLite 在iOS/Android存储App的数据

服务端：
关系型数据库
部门---员工
mysql
sqlserver
oracle

非关系型数据库
JSON对象的形式来存储

MongoDB : 简单，你会js、JSON就能操作
Redis
Memcached

### 数据库的作用

1、保存应用程序产生的数据(用户注册数据，用户的个人信息等等)
2、当应用程序需要数据的时候，提供给应用程序去展示

------

## 安装mongodb服务端

步骤：
1、安装mongodb服务端软件
2、设置mongodb的环境变量，重启终端验证 mongo -version
3、建立一个文件夹，用来存储mongodb数据库产生的数
据(建议放在C盘根目录 mongodb_datas)
4、启动
mongod --dbpath c:/mongodb_datas

## 启动服务端有几种方式

1、方式一，直接在cmd中输入 `mongod --dbpath c:/mongodb_datas`
32位: `mongod --dbpath c:/mongodb_datas --journal --storageEngine=mmapv1`

2、方式二，可以把`mongod --dbpath c:/mongodb_datas`做成一个批处理文件
`32位: mongod --dbpath c:/mongodb_datas --journal --storageEngine=mmapv1`

## 使用robomongo这个小机器人来操作我们的数据库中的数据

步骤:
1、连接到我们mongodb数据库服务端，并且连接成功之
后，服务端会给我们返回一个操作数据库的db对象

2、拿着上一步返回的db对象，对mongodb数据库中的数据进行操作了

连接成功之后，我们要来操作数据的话
1、创建一个数据库 (相当于在excel中创建空白工作簿)
2、创建集合 (相当于在excel创建工作表单)
数据的一个集合，把相关联的数据放在一个集合中
3、确立表头，插入数据、删除数据、修改数据、查询数据

## MongoDB数据库中的概念

数据库 ： 一个App中对应一个数据库

集合：相当于Excel中表单，一堆数据的集合，相关联的数据，
会放在一个集合中

文档：相当于excel中的每一行数据

一个数据中可以有多个集合(学生集合、食品集合)
一个集合可以有多条文档(多条数据)

### 在NodeJS中使用mongodb这个第三方包来操作我们mongodb数据库中的数据

参考：
[https://www.npmjs.com/package/mongodb](https://link.jianshu.com/?t=https://www.npmjs.com/package/mongodb)

前提准备:
1、使用`npm i mongodb --save`来安装

正式集成:
1、导入包
2、拿到我们mongoClient对象
3、使用mongoClient连接到mongodb的服务端，返回操作数据库的db对象
4、通过db对象，拿到数据集合

~~~go
db.collection('集合的名称')
            ```
5、调用集合的增，删，改，查的方法，来操作数据库中的数据
            

------------------------------------------
~~~