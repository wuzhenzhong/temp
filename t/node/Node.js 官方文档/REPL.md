# Node.js REPL

## REPL

REPL即Node自带的交互式解释器，它可以实现如下的任务：

- 读取（Read）- 可以读取用户的输入，解析输入的Javascript数据结构并存储在内存中。
- 执行（Eval）- 可以执行输入的Javascript数据结构。
- 打印（Print）- 打印输出结果。
- 循环（Loop）- 对上述的步骤进行循环，如果需要退出，则用户需要两次按下ctrl-c按钮。

```js
稳定性: 3 - 稳定
```

Read-Eval-Print-Loop (REPL 读取-执行-输出循环)即可作为独立程序，也可以集成到其他程序中。

REPL提供了一种交互的执行JavaScript并查看输出结果的方法。可以用来调试，测试，或仅是用来试试。

在命令行中不带任何参数的执行`node`，就是REPL模式。它提供了简单的emacs行编辑。

```js
mjr:~$ node
Type '.help' for options.
> a = [ 1, 2, 3];
[ 1, 2, 3 ]
> a.forEach(function (v) {
...   console.log(v);
...   });
1
2
3
```

若想使用高级的编辑模式，使用环境变量`NODE_NO_READLINE=1`打开node。这样会开启REPL模式，允许你使用`rlwrap`。

例如，你可以添加以下代码到你的bashrc文件里。

```js
alias node="env NODE_NO_READLINE=1 rlwrap node"
```

## repl.start(options)

启动并返回一个`REPLServer`实例。它继承自Readline Interface。接收的参数"options"有以下值：

- `prompt`- 所有输入输出的提示符和流，默认是`>`.
- `input`- 需要监听的可读流，默认为`process.stdin`.
- `output`- 用来输出数据的可写流，默认为`process.stdout`.
- `terminal`- 如果`stream`被当成TTY，并且有ANSI/VT100转义，传输`true`。默认在实例的输出流上检查`isTTY`。
- `eval`- 用来对每一行进行求值的函数。默认为`eval()`的异步封装。参见后面的自定义`eval`例子。
- `useColors`- 写函数输出是否有颜色。如果设定了不同的`writer`函数则无效。默认为 repl 的`terminal`值。
- `useGlobal`- 如果为`true`，则repl将会使用全局对象，而不是在独立的上下文中运行scripts。默认为`false`。
- `ignoreUndefined`- 如果为`true`，repl不会输出未定义命令的返回值。默认为`false`。
- `writer`- 每个命令行被求值时都会调用这个函数，它会返回格式化显示内容（包括颜色）。默认是`util.inspect`。

如果有以下特性，可以使用自己的`eval`函数：

```js
function eval(cmd, context, filename, callback) {
  callback(null, result);
}
```

在同一个node的运行实例上，可以打开多个REPLs。每个都会共享一个全局对象，但会有独立的I/O。

以下的例子，在stdin、 Unix socket和 TCP socket上开启REPL ：

```js
var net = require("net"),
    repl = require("repl");

connections = 0;

repl.start({
  prompt: "node via stdin> ",
  input: process.stdin,
  output: process.stdout
});

net.createServer(function (socket) {
  connections += 1;
  repl.start({
    prompt: "node via Unix socket> ",
    input: socket,
    output: socket
  }).on('exit', function() {
    socket.end();
  })
}).listen("/tmp/node-repl-sock");

net.createServer(function (socket) {
  connections += 1;
  repl.start({
    prompt: "node via TCP socket> ",
    input: socket,
    output: socket
  }).on('exit', function() {
    socket.end();
  });
}).listen(5001);
```

从命令行运行这个程序，将会在stdin上启动REPL。其他的REPL客户端可能通过Unix socket或TCP socket连接。`telnet`常用于连接TCP socket，`socat`用于连接Unix和TCP sockets

从Unix socket-based服务器启动REPL（而非stdin），你可以建立长连接，不用重启它们。

通过`net.Server`和`net.Socket`实例运行"full-featured" (`terminal`) REPL的例子，参见: https://gist.github.com/2209310

通过`curl(1)`实例运行REPL的例子，参见: https://gist.github.com/2053342

### Event: 'exit'

```
function () {}
```

当用户通过预定义的方式退出REPL将会触发这个事件。预定义的方式包括，在repl里输入`.exit`，按Ctrl+C两次来发送SIGINT信号，或者在`input`流上按Ctrl+D 来发送"end"。

监听`exit`的例子：

```js
r.on('exit', function () {
  console.log('Got "exit" event from repl!');
  process.exit();
});
```

### Event: 'reset'

```
function (context) {}
```

重置REPL的上下文的时候触发。当你输入`.clear`会重置。如果你用`{ useGlobal: true }`启动repl，那这个事件永远不会被触发。

监听`reset`的例子：

```js
// Extend the initial repl context.
r = repl.start({ options ... });
someExtension.extend(r.context);

// When a new context is created extend it as well.
r.on('reset', function (context) {
  console.log('repl has a new context');
  someExtension.extend(context);
});
```

## REPL 特性

在REPL里， Control+D会退出。可以输入多行表达式。支持全局变量和局部变量的TAB自动补全。

特殊变量`_`(下划线)包含上一个表达式的结果。

```js
> [ "a", "b", "c" ]
[ 'a', 'b', 'c' ]
> _.length
3
> _ += 1
4
```

REPL支持在全局域里访问任何变量。将变量赋值个和`REPLServer`关联的上下文对象，你可以显示的讲变量暴露给REPL，例如：

```js
// repl_test.js
var repl = require("repl"),
    msg = "message";

repl.start("> ").context.m = msg;
```

`context`对象里的东西，会以局部变量的形式出现：

```js
mjr:~$ node repl_test.js
> m
'message'
```

有一些特殊的REPL命令：

- `.break` - 当你输入多行表达式时，也许你走神了或者不想完成了，`.break`可以重新开始。
- `.clear` - 重置`context`对象为空对象，并且清空多行表达式。
- `.exit` - 关闭输入/输出流，会让REPL退出。
- `.help` - 打印这些特殊命令。
- `.save` - 保存当前REPL会话到文件。.save ./file/to/save.js
- `.load`- 加载一个文件到当前REPL会话.load ./file/to/load.js

下面的组合键在REPL中有以下效果：

- `<ctrl>C`- 和`.break`键类似，在一个空行连按两次会强制退出。
- `<ctrl>D`- 和`.exit`键类似。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-01-01