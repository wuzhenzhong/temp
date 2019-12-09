# Node.js TTY

## TTY

[纠错](javascript:;)

```js
稳定性: 2 - 不稳定
```

Node.js的`tty`模块包含`tty.ReadStream`和`tty.WriteStream`类，多数情况下，你不必直接使用这个模块，访问该模块的方法如下：

```js
const tty = require('tty');
```

当node检测到自己正运行于TTY上下文时，`process.stdin`将会是一个`tty.ReadStream`实例，并且`process.stdout`将会是`tty.WriteStream`实例。检测 node是否运行在TTY上下文的好方法是检测`process.stdout.isTTY`：

```js
$ node -p -e "Boolean(process.stdout.isTTY)"
true
$ node -p -e "Boolean(process.stdout.isTTY)" | cat
false
```

## tty.isatty(fd)

如果`fd`和终端相关联返回`true`，否则返回`false`。

## tty.setRawMode(mode)

已经抛弃。使用`tty.ReadStream#setRawMode()`（比如`process.stdin.setRawMode()`）替换。

## Class: ReadStream

`net.Socket`的子类，表示tty的可读部分。通常情况，在任何node程序里（仅当`isatty(0)`为true时），`process.stdin`是`tty.ReadStream`的唯一实例。

### rs.isRaw

`Boolean`值，默认为`false`。它代表当前`tty.ReadStream`实例的"raw"状态。

### rs.setRawMode(mode)

`mode`需是`true`或`false`。它设定`tty.ReadStream`属性为原始设备或默认。`isRaw`将会设置为结果模式。

## Class: WriteStream

`net.Socket`的子类，代表tty的可写部分。通常情况下，`process.stdout`是`tty.WriteStream`唯一实例（仅当`isatty(1)`为true时）。

### ws.columns

TTY当前拥有的列数。触发"resize"事件时会更新这个值。

### ws.rows

TTY当前拥有的行数。触发"resize"事件时会更新这个值。

### Event: 'resize'

```
function () {}
```

行或列变化时会触发`refreshSize()`事件。

```js
process.stdout.on('resize', function() {
  console.log('screen size has changed!');
  console.log(process.stdout.columns + 'x' + process.stdout.rows);
});
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-01-01