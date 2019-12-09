# Node.js UDP/Datagram

## UDP/Datagram Sockets

```js
稳定性: 3 - 稳定
```

Node.js的dgram模块提供了UDP数据报套接字的实现。

使用数据报文sockets(Datagram sockets)的方式是调用`require('dgram')`。

重要提醒：`dgram.Socket#bind()`的行为在v0.10做了改动 ，它总是异步的。如果你的代码像下面的一样：

```js
var s = dgram.createSocket('udp4');
s.bind(1234);
s.addMembership('224.0.0.114');
```

现在需要改为：

```js
var s = dgram.createSocket('udp4');
s.bind(1234, function() {
  s.addMembership('224.0.0.114');
});
```

## dgram.createSocket(type, callback)

- `type`字符串。 'udp4'或'udp6'
- `callback`函数。附加到`message`事件的监听器。可选参数。
- 返回：Socket对象

创建指定类型的数据报文(datagram) Socket。有效类型是`udp4`和`udp6`

接受一个可选的回调，会被添加为`message`的监听事件。

如果你想接收数据报文(datagram)可以调用`socket.bind()`。`socket.bind()`将会绑定到所有接口（"all interfaces"）的随机端口上（`udp4`和`udp6` sockets都适用）。你可以通过`socket.address().address`和`socket.address().port`获取地址和端口。

## dgram.createSocket(options, callback)

- `options`对象
- `callback`函数。给`message`事件添加事件监听器。
- 返回：Socket对象

参数`options`必须包含`type`值(`udp4`或`udp6`)，或可选的boolean值`reuseAddr`。

当`reuseAddr`为 true 时，`socket.bind()`将会重用地址，即使另一个进程已经绑定socket。`reuseAddr`默认为`false`。

回调函数为可选参数，作为`message`事件的监听器。

如果你想接受数据报文(datagram)，可以调用`socket.bind()`。`socket.bind()`将会绑定到所有接口（"all interfaces"）地址的随机端口上（`udp4`和`udp6` sockets都适用）。你可以通过`socket.address().address`和`socket.address().port`获取地址和端口。

## Class: dgram.Socket

报文数据Socket类封装了数据报文(datagram)函数。必须通过`dgram.createSocket(...)`函数创建。

### Event: 'message'

- `msg`缓存对象. 消息。
- `rinfo`对象. 远程地址信息。

当socket上新的数据报文(datagram)可用的时候，会触发这个事件。`msg`是一个缓存，`rinfo`是一个包含发送者地址信息的对象。

```js
socket.on('message', function(msg, rinfo) {
  console.log('Received %d bytes from %s:%d\n',
              msg.length, rinfo.address, rinfo.port);
});
```

### Event: 'listening'

当socket开始监听数据报文(datagram)时触发。在UDP socket创建时触发。

### Event: 'close'

当socket使用`close()`关闭时触发。在这个socket上不会触发新的消息事件。

### Event: 'error'

- `exception`Error对象

当发生错误时触发。

### socket.send(buf, offset, length, port, address, callback)

- `buf`缓存对象或字符串. 要发送的消息。
- `offset`整数。消息在缓存中得偏移量。
- `length`整数。消息的比特数。
- `port`整数。端口的描述。
- `address`字符串。目标的主机名或IP地址。
- `callback`函数。当消息发送完毕的时候调用。可选。

对于UDP socket，必须指定目标端口和地址。`address`参数可能是字符串，它会被DNS解析。

如果忽略地址或者地址是空字符串，将使用`'0.0.0.0'`或`'::0'`替代。依赖于网络配置，这些默认值有可能行也可能不行。

如果socket之前没被调用`bind`绑定，则它会被分配一个随机端口并绑定到所有接口（"all interfaces"）地址(`udp4`sockets的`'0.0.0.0'` ，`udp6`sockets的`'::0'`)

回调函数可能用来检测DNS错误，或用来确定什么时候重用`buf`对象。注意，DNS查询会导致发送tick延迟。通过回调函数能确认数据报文(datagram)是否已经发送的。

考虑到多字节字符串情况，偏移量和长度是字节长度byte length，而不是字符串长度。

下面的例子是在`localhost`上发送一个UDP包给随机端口：

```js
var dgram = require('dgram');
var message = new Buffer("Some bytes");
var client = dgram.createSocket("udp4");
client.send(message, 0, message.length, 41234, "localhost", function(err) {
  client.close();
});
```

**关于UDP数据报文(datagram) 尺寸**

`IPv4/v6`数据报文(datagram)的最大长度依赖于`MTU` (*Maximum Transmission Unit*)和`Payload Length`的长度。

- `Payload Length`内容为16位宽，它意味着Payload的最大字节说不超过64k，其中包括了头信息和数据（65,507字节 = 65,535 − 8字节UDP头 − 20字节IP 头）；对于环回接口（loopback interfaces）这是真的，但对于多数主机和网络来说不太现实。
- `MTU`能支持数据报文(datagram)的最大值（以目前链路层技术来说）。对于任何连接，`IPv4`允许的最小值为`68`的`MTU`，推荐值为`576`（通常推荐作拨号应用的`MTU`）,无论他们是完整接收还是碎片接收。 对于`IPv6`，`MTU`的最小值为`1280`字节，最小碎片缓存大小为`1500`字节。16字节实在是太小，所以目前链路层一般最小`MTU`大小为`1500`。

我们不可能知道一个包可能进过的每个连接的MTU。通常发送一个超过接收端MTU大小的数据报文(datagram)会失效。（数据包会被悄悄的抛弃，不会通知发送端数据包没有到达接收端）。

### socket.bind(port, address)

- `port`整数
- `address`字符串，可选
- `callback`没有参数的函数，可选。绑定时会调用回调。

对于UDP socket，在一个端口和可选地址上监听数据报文(datagram)。如果没有指定地点，系统将会参数监听所有的地址。绑定完毕后，会触发"listening" 事件，并会调用传入的回调函数。指定监听事件和回调函数非常有用。

一个绑定了的数据报文socket会保持node进程运行来接收数据。

如果绑定失败，会产生错误事件。极少数情况（比如绑定一个关闭的socket）。这个方法会抛出一个错误。

以下是UDP服务器监听端口41234的例子：

```js
var dgram = require("dgram");

var server = dgram.createSocket("udp4");

server.on("error", function (err) {
  console.log("server error:\n" + err.stack);
  server.close();
});

server.on("message", function (msg, rinfo) {
  console.log("server got: " + msg + " from " +
    rinfo.address + ":" + rinfo.port);
});

server.on("listening", function () {
  var address = server.address();
  console.log("server listening " +
      address.address + ":" + address.port);
});

server.bind(41234);
// server listening 0.0.0.0:41234
```

### socket.bind(options, callback)

- ```
  options
  ```

  {对象} - 必需. 有以下的属性:

  - `port`{Number} - 必需.
  - `address`{字符串} - 可选.
  - `exclusive`{Boolean} - 可选.

- `callback`{函数} - 可选.

## TTY

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

- `multicastAddress`字符串
- `multicastInterface`字符串，可选

和`addMembership`相反 - 用`IP_DROP_MEMBERSHIP`选项告诉内核离开广播组 。如果没有指定`multicastInterface`，操作系统会移除所有可用的接口关系。

### socket.unref()

在socket上调用`unref`允许程序退出，如果这是在事件系统中唯一的活动socket。如果socket已经`unref`，再次调用`unref`将会无效。

### socket.ref()

和`unref`相反，如果这是唯一的socket，在一个之前被unref了的socket上调用ref将不会让程序退出（缺省行为）。如果一个socket已经被ref，则再次调用ref将会无效。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-01-01