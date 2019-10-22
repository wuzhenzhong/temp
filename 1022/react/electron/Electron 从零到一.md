# Electron 从零到一

> 蘑菇街前端团队正式入驻掘金，希望大家不要吝啬你们手中的赞(比心)!

## 零、 介绍

Electron 我们会出一个系列，这篇文章主要是介绍 Electron 基础相关的东西，后续会出我们在实战中更多的技巧以及问题的解决方案，希望大家持续关注我们。

本文主要包括：

1. Electron 简介
2. 快速开始
3. 进程
4. 打包
5. 踩坑总结

## 一、[Electron](https://electronjs.org/) 简介

`Electron` 是一个赋力前端进行跨平台开发的框架,让开发人员使用 JavaScript, HTML 和 CSS 等前端技术构建跨平台的桌面应用。 `Electron` 通过将 `Chromium` 和 `Node.js` 合并到同一个运行时环境中，并将其打包为 Mac，Windows 和 Linux 系统下的应用，而开发人员只需关注前端代码的开发。

## 二、快速开始 Electron + React

electron 提供了一个名为 [electron-quick-start](https://github.com/electron/electron-quick-start) 的项目，可以 clone 下来当成模版使用，本文还是使用 create-react-app 来一步一步学习。

### 创建一个 react 项目

```
# 安装 create-react-app 命令,如果已将安装请忽略
npm install -g create-react-app

# 创建 electron-react 项目
create-react-app electron-react

# 启动项目
cd electron-react && npm start
复制代码
```

浏览器打开 localhost:3000 出现下面的画面：

### 配置 electron 环境

在 public 文件夹下新建 index.html，随便写点内容：

```
...
<div>hello world</div>
...
复制代码
```

接下来创建 electron 主线程文件，public/main.js，建议写在 public 路径下面。

```
const {app, BrowserWindow} = require('electron')

// 创建全局变量并在下面引用，避免被GC
let win

function createWindow () {
    // 创建浏览器窗口并设置宽高
    win = new BrowserWindow({ width: 800, height: 600 })
    
    // 加载页面
    win.loadFile('./index.html')
    
    // 打开开发者工具
    win.webContents.openDevTools()
    
    // 添加window关闭触发事件
    
    win.on('closed', () => {
        win = null  // 取消引用
    })
}

// 初始化后 调用函数
app.on('ready', createWindow)  

// 当全部窗口关闭时退出。
app.on('window-all-closed', () => {
   // 在 macOS 上，除非用户用 Cmd + Q 确定地退出，
   // 否则绝大部分应用及其菜单栏会保持激活。
   if (process.platform !== 'darwin') {
        app.quit()
   }
})
  
app.on('activate', () => {
// 在macOS上，当单击dock图标并且没有其他窗口打开时，
// 通常在应用程序中重新创建一个窗口。
    if (win === null) {
      createWindow()
    }
})

复制代码
```

最后，修改 package.json 中的 main 字段对应的路径, 并添加 start 命令

```
{
    ...
    "main": "main.js",
    "scripts": "electron ."
}
复制代码
```

执行 npm start, 就会弹出如下界面：



![png](https://user-gold-cdn.xitu.io/2019/10/21/16dedc0e2c93e19a?imageslim)



这里我简单写了一个页面，大家也可以写一写自己感兴趣的东西。

于是，一个简单的桌面应用就开发好了，真的 so easy.

## 三、 进程

electron 的进程分为`主进程`和`渲染进程`

### 主进程&渲染进程

先来看看 electron 项目基本目录结构

```
app
└─public
    └─index.html---------------入口文件
├─main.js----------------------程序启动入口，主进程
├─ipc--------------------------进程间模块
├─appNetwork-------------------应用通信模块
└─src--------------------------窗口管理，渲染进程
    ├─components---------------通用组件模块
    ├─store--------------------数据共享模块
    ├─statics------------------静态资源模块
    └─pages----------------------窗口业务模块
      ├─窗口A----------------窗口
      └─窗口B----------------窗口
复制代码
```

package.json 中的 `main` 字段对应的文件的进程是`主进程`。Electron集成了Chromium来展示窗口界面，窗口中所看到的内容使用的都是HTML渲染出来的。 Chromium本身是多进程渲染页面的架构（在默认情况下，Chromium的默认策略是对每一个tab新开一个进程，以确保每个页面是独立且互不影响的。避免一个页面的崩溃导致全部页面无法使用），所以Electron在展示窗口时，也会使用到Chromium的多进程架构。而这种多进程渲染架构在Electron中，就被称之为`渲染进程（render process）`。

### 进程间通信

在 electron 中，GUI 相关的模块（如 dialog,menu 等）仅在主进程可用，在渲染进程中不可用。为了在渲染进程中使用它们，需要使用 `ipc` 模块向主进程发送消息，下面是几种进程间通讯的方法。

1. [ipcMain & ipcRenderer](https://electronjs.org/docs/api/ipc-main#sending-messages)

从主进程到渲染进程的异步通信，也可以将消息从主进程发送到渲染进程，参考 [webContents.send](https://electronjs.org/docs/api/web-contents#contentssendchannel-arg1-arg2-).

> 发送消息时，事件名称为 channel。
>
> 回复同步消息时，需要设置 event.returnValue。
>
> 将异步消息发送回发送方，可以使用 event.reply(...)，这个辅助方法将自动处理来自渲染进程的消息，然而 event.sender.send(...) 这个方法则始终将消息发送给主进程。

下面是在渲染和主进程之间发送和处理消息的一个例子：

```
// 在主进程中
const { ipcMain } = require('electron')
ipcMain.on('asynchronous-message', (event, arg) => {
    console.log(arg); // 输出 'ping'
    event.reply('asynchronous-reply', 'pong');
})

ipcMain.on('synchronous-message', (event, arg) => {
    console.log(arg) // 输出 ‘ping’
    event.returnValue = 'pong'
})
复制代码
// 在渲染进程（网页）中
const { ipcRenderer } = require('electron)
console.log(ipcRenderer.sendSync('synchronous-message', 'ping')) // 输出 'pong'

ipcRenderer.on('asynchronous-reply', (event, arg) => {
    console.log(arg); // 输出 'pong'
})

ipcRenderer.send('asynchronous-message', 'ping')
复制代码
```

1. [remote 模块](https://electronjs.org/docs/api/remote)

`remote` 为`渲染进程`和主进程通信提供了一种简单的方法。你可以调用 main 进程对象的方法，而不必显式发送进程间消息。例如：从渲染进程创建浏览器窗口

```
const { BrowserWindow } = require('electron').remote
let win = new BrowserWindow({ width: 800, height: 600 })
win.loadUrl('https://www.mogu.com')
复制代码
```

**注意：** 反过来（如果需要从主进程访问渲染进程），可以使用 [webContents.executeJavascript](https://electronjs.org/docs/api/web-contents#contentsexecutejavascriptcode-usergesture-callback)。

1. [webContents](https://electronjs.org/docs/api/web-contents#contentssendchannel-arg1-arg2-)

> 通过 `channel` 向渲染进程发送异步消息，可以发送任意参数。在内部，参数会被序列化为 JSON，因此参数对象伤的函数和原型链不会被发送

除了以上这些方法，也可以使用 localStorage、sessionStorage 等。

## 四、打包

开发完成后，还需要将应用打包成可执行文件，这一环节的坑还是学习 electron 到现在踩的最多的。

目前主流的打包工具有 [electron-packager](https://github.com/electron/electron-packager#usage) 和 [electron-builder](https://github.com/electron-userland/electron-builder)

### electron-packager

**安装依赖：**

```
npm i electron-packager --save-dev
```

**打包：**

```
electron-packager <sourcedir> <appname> --platform=<platform> --arch=<arch> [optional flags...]
```

也可以直接运行 `npm run electron-packager .` 打包。

### electron-builder

官方解释：

> A complete solution to package and build a ready for distribution Electron, Proton Native or Muon app for macOS, Windows and Linux with “auto update” support out of the box.

简单的说，electron-builder 有比 electron-packager 更丰富的功能，支持更多的平台，同时也支持了自动更新。除了这几点外，electron-builder 打出的包更为轻量，并且可以打包出不暴露源码的 setup 安装程序。另外使用下来感觉比 electron-packager 的坑要少一点。

**安装依赖：**

```
npm i electron-builder --save-dev

复制代码
```

**打包：**

- 在项目的 `package.json` 文件中定义 `build` 字段

```
{
    "build": {
        "appId": "com.xxx.app",
        "extends": null,
        "files": [
            "build/**/*"
        ],
        "mac": {
            "icon": "icons/icon.icns"
        },
        "win": {
            "target": "nsis",
            "icon": "icons/icon.png"
        }
    }
}

复制代码
```

这是最基础的配置，当然打包过程中可能会碰到其他的问题需要修改配置。通常 files 配置只写一个 build 文件夹是不够的，要根据项目结构和打包情况添加其他路径。

- 添加 scripts 命令

```
{
    "scripts": {
        "pack": "electron-builder"
    }
}

复制代码
```

- 运行 npm run pack 打包

打包完成后在 dist 目录下有可执行文件，打开后如果没有报错，则说明打包成功。

## 五、踩坑总结

大部分都是打包遇到的坑

- **使用 electron-packager 打包报错**

Generated checksum for "electron-v6.0.2-darwin-x64.zip" did not match expected checksum。node 版本升级到 8.x 以上就好。

- **打开打包生成的可执行文件报错**



![png](https://user-gold-cdn.xitu.io/2019/10/21/16dedc0e2ec32630?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



出现这种问题可能有以下几个原因：

1. 项目中可能直接访问了本地路径, 浏览器为了安全考虑不允许访问。
2. package.json 中的 build 配置问题，假如 main.js 在一个很深的路径中，需要在下面单独添加 main.js 的路径

```
"build": {
    ...
+   "public/main.js"
    ...
}

复制代码
```

1. webpack 配置中的路径直接使用了 __dirname, 可以使用 remote 模块的 [getAppPath](https://electronjs.org/docs/api/app#appgetapppath) 方法取得路径

```
const remote = require('remote')
const app = remote.require('app')
console.log(app.getAppPath());

复制代码
```

参考 [github.com/electron/el…](https://github.com/electron/electron/issues/5107)

- **dependencies & devDependencies**

在 electron 打包时，一定要分清哪些是生产环境依赖，哪些是开发环境依赖。避免出现此类错误：

![png](https://user-gold-cdn.xitu.io/2019/10/21/16dedc0e2df1c366?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



- **关于打包慢的问题：npm & cnpm**

cnpm 装的各种 node_modules，这种方式下所有的包都是扁平化的安装，一下子 node_modules 展开就有非常多的文件，导致打包的过程非常慢。但是如果该用 npm 来安装 node_modules 的话，所有的包都是树状结构，层级变深。但是打包速度会快很多。具体见：[electron打包过了2小时都没好？](https://segmentfault.com/q/1010000006197710)

## 推荐文章

1. [imweb.io/topic/5b681…](https://imweb.io/topic/5b6817b5f6734fdf12b4b09c)
2. [juejin.im/post/5ba06b…](https://juejin.im/post/5ba06b67f265da0ae343e89c#heading-33)

## 参考文章

1. [juejin.im/post/5c9041…](https://juejin.im/post/5c9041205188252d9c5bae83)
2. [electronjs.org](https://electronjs.org/)
3. [juejin.im/post/5b86b7…](https://juejin.im/post/5b86b7fd6fb9a019c476fc06)
4. [imweb.io/topic/5b681…](https://imweb.io/topic/5b6817b5f6734fdf12b4b09c)
5. [juejin.im/post/5ba06b…](https://juejin.im/post/5ba06b67f265da0ae343e89c#heading-36)