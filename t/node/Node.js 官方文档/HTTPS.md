# Node.js HTTPS

- `Int8`
- `Uint8`
- `Int16`
- `Uint16`
- `Int32`
- `Uint32`
- `Float`
- `Double`
- `Uint8Clamped`

## HTTPS

```js
稳定性: 3 - 稳定
```

HTTPS是什么？HTTPS是基于TLS/SSL的HTTP协议，在Node.js里它可以作为单独的模块来实现。在本文中，你将了解HTTPS的使用方法。

## 类: https.Server

https.Server是`tls.Server`的子类，并且和`http.Server`一样触发事件。更多信息参见`http.Server`。

### server.setTimeout(msecs, callback)

详情参见http.Server#setTimeout().

### server.timeout

详情参见http.Server#timeout.

## https.createServer(options, requestListener)

返回一个新的HTTPS服务器对象。其中`options`类似于 tls.createServer()。 `requestListener`函数自动加到`'request'`事件里。

例如:

```js
// curl -k https://localhost:8000/
var https = require('https');
var fs = require('fs');

var options = {
  key: fs.readFileSync('test/fixtures/keys/agent2-key.pem'),
  cert: fs.readFileSync('test/fixtures/keys/agent2-cert.pem')
};

https.createServer(options, function (req, res) {
  res.writeHead(200);
  res.end("hello world\n");
}).listen(8000);
```

或：

```js
var https = require('https');
var fs = require('fs');

var options = {
  pfx: fs.readFileSync('server.pfx')
};

https.createServer(options, function (req, res) {
  res.writeHead(200);
  res.end("hello world\n");
}).listen(8000);
```

### server.listen(port, host, callback)

### server.listen(path, callback)

### server.listen(handle, callback)

详情参见http.listen()。

### server.close(callback)

详情参见http.close()。

## https.request(options, callback)

可以给安全web服务器发送请求。

`options`可以是一个对象或字符串。如果`options`是字符串，则会被url.parse()解析。

所有来自http.request()选项都是经过验证的。

例如:

```js
var https = require('https');

var options = {
  hostname: 'encrypted.google.com',
  port: 443,
  path: '/',
  method: 'GET'
};

var req = https.request(options, function(res) {
  console.log("statusCode: ", res.statusCode);
  console.log("headers: ", res.headers);

  res.on('data', function(d) {
    process.stdout.write(d);
  });
});
req.end();

req.on('error', function(e) {
  console.error(e);
});
```

option参数具有以下的值：

- `host`: 请求的服务器域名或IP地址，默认：`'localhost'`

- `hostname`: 用于支持`url.parse()`。`hostname`优于`host`

- `port`: 远程服务器端口。默认： 443。

- `method`: 指定HTTP请求方法。默认：`'GET'`。

- `path`: 请求路径。默认：`'/'`。如果有查询字符串，则需要包含。比如`'/index.html?page=12'`

- `headers`: 包含请求头的对象

- `auth`: 用于计算认证头的基本认证，即`user:password`

- ```
  agent
  ```

  : 控制Agent的行为。当使用了一个Agent的时候，请求将默认为

  ```
  Connection: keep-alive
  ```

  。可能的值为：

  - `undefined` (default): 在这个主机和端口上使用global Agent
  - `Agent` object: 在`Agent`中显式使用passed.
  - `false`: 选择性停用连接池,默认请求为：`Connection: close`

tls.connect()的参数也能指定。但是，globalAgent会忽略他们。

- `pfx`: SSL使用的证书，私钥，和证书Certificate，默认为`null`.
- `key`: SSL使用的私钥. 默认为`null`.
- `passphrase`: 私钥或pfx的口令字符串. 默认为`null`.
- `cert`: 所用公有x509证书. 默认为`null`.
- `ca`: 用于检查远程主机的证书颁发机构或包含一系列证书颁发机构的数组。
- `ciphers`: 描述要使用或排除的密码的字符串，格式请参阅http://www.openssl.org/docs/apps/ciphers.html#CIPHER_LIST_FORMAT
- `rejectUnauthorized`: 如为`true`则服务器证书会使用所给CA列表验证。如果验证失败则会触发`error`事件。验证过程发生于连接层，在`HTTP`请求发送之前。默认为`true`.
- `secureProtocol`: 所用的SSL方法，比如`TLSv1_method`强制使用TLS version 1。可取值取决于您安装的OpenSSL，和定义于[SSL_METHODS](http://www.openssl.org/docs/ssl/ssl.html#DEALING_WITH_PROTOCOL_METHODS)的常量。

要指定这些选项，使用一个自定义`Agent`。

例如:

```js
var options = {
  hostname: 'encrypted.google.com',
  port: 443,
  path: '/',
  method: 'GET',
  key: fs.readFileSync('test/fixtures/keys/agent2-key.pem'),
  cert: fs.readFileSync('test/fixtures/keys/agent2-cert.pem')
};
options.agent = new https.Agent(options);

var req = https.request(options, function(res) {
  ...
}
```

或者不使用`Agent`.

例如:

```js
var options = {
  hostname: 'encrypted.google.com',
  port: 443,
  path: '/',
  method: 'GET',
  key: fs.readFileSync('test/fixtures/keys/agent2-key.pem'),
  cert: fs.readFileSync('test/fixtures/keys/agent2-cert.pem'),
  agent: false
};

var req = https.request(options, function(res) {
  ...
}
```

## https.get(options, callback)

和`http.get()`类似，不过是HTTPS版本的.

`options`可以是字符串对象. 如果`options`是字符串, 会自动使用url.parse()解析。

例如:

```js
var https = require('https');

https.get('https://encrypted.google.com/', function(res) {
  console.log("statusCode: ", res.statusCode);
  console.log("headers: ", res.headers);

  res.on('data', function(d) {
    process.stdout.write(d);
  });

}).on('error', function(e) {
  console.error(e);
});
```

## 类: https.Agent

HTTPS的Agent对象，和http.Agent类似。详情参见https.request()。

## https.globalAgent

所有HTTPS客户端请求的https.Agent全局实例。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-01-01