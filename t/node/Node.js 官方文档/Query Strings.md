# Node.js Query Strings

## Query String

本节为你介绍Node.js Query Strings。

```js
稳定性: 3 - 稳定
```

该Node.js模块提供了一些处理query strings的工具，你可以通过以下方式访问它：

[纠错](javascript:;)

```js
const querystring = require('querystring');
```

Node.js Query Strings包含的方法如下：

### querystring.stringify(obj, sep, options)

该方法可以将一个对象序列化化为一个query string 。

可以选择重写默认的分隔符(`'&'`) 和分配符 (`'='`)。

Options对象可能包含`encodeURIComponent`属性 (默认：`querystring.escape`)，如果需要，它可以用`non-utf8`编码字符串。

例子:

```js
querystring.stringify({ foo: 'bar', baz: ['qux', 'quux'], corge: '' })
// returns
'foo=bar&baz=qux&baz=quux&corge='

querystring.stringify({foo: 'bar', baz: 'qux'}, ';', ':')
// returns
'foo:bar;baz:qux'

// Suppose gbkEncodeURIComponent function already exists,
// it can encode string with `gbk` encoding
querystring.stringify({ w: '中文', foo: 'bar' }, null, null,
  { encodeURIComponent: gbkEncodeURIComponent })
// returns
'w=%D6%D0%CE%C4&foo=bar'
```

### querystring.parse(str, sep, options)

该方法可以将query string反序列化为对象。

你可以选择重写默认的分隔符(`'&'`) 和分配符 (`'='`)。

Options对象可能包含`maxKeys`属性（默认：1000），用来限制处理过的健值（keys）。设置为0的话，可以去掉键值的数量限制。

Options 对象可能包含`decodeURIComponent`属性（默认：`querystring.unescape`），如果需要，可以用来解码`non-utf8`编码的字符串。

例子:

```js
querystring.parse('foo=bar&baz=qux&baz=quux&corge')
// returns
{ foo: 'bar', baz: ['qux', 'quux'], corge: '' }

// Suppose gbkDecodeURIComponent function already exists,
// it can decode `gbk` encoding string
querystring.parse('w=%D6%D0%CE%C4&foo=bar', null, null,
  { decodeURIComponent: gbkDecodeURIComponent })
// returns
{ w: '中文', foo: 'bar' }
```

### querystring.escape

escape函数供`querystring.stringify`使用，必要时，可以重写。

### querystring.unescape

unescape函数供`querystring.parse`使用。必要时，可以重写。

首先会尝试用`decodeURIComponent`，如果失败，会回退，不会抛出格式不正确的URLs。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-01-01