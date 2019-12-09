# Node.js Punycode

- 贡献者1人

  

- `event`{String}事件名
- `listener`{Function}事件处理函数

## Punycode

Punycode是根据RFC 3492标准定义的字符编码方案，主要用于把域名从地方语言所采用的Unicode编码转换成为可用于DNS系统的编码。

```js
稳定性: 2 - 不稳定
```

[Punycode.js](https://github.com/bestiejs/punycode.js)从Node.js v0.6.2+开始内置. 使用`require('punycode')`来访问。 (要在其他Node.js版本中访问，先用npm来`punycode`安装)。

## punycode.decode(string)

将一个纯ASCII的Punycode字符串转换为Unicode字符串。

```js
// decode domain name parts
punycode.decode('maana-pta'); // 'mañana'
punycode.decode('--dqo34k'); // '☃-⌘'
```

## punycode.encode(string)

将一个纯Unicode Punycode字符串转换为纯ASCII字符串。

```js
// encode domain name parts
punycode.encode('mañana'); // 'maana-pta'
punycode.encode('☃-⌘'); // '--dqo34k'
```

## punycode.toUnicode(domain)

将一个表示域名的Punycode字符串转换为Unicode。只有域名中的Punycode部分会被转换，也就是说你也可以在一个已经转换为Unicode的字符串上调用它。

```js
// decode domain names
punycode.toUnicode('xn--maana-pta.com'); // 'mañana.com'
punycode.toUnicode('xn----dqo34k.com'); // '☃-⌘.com'
```

## punycode.toASCII(domain)

将一个表示域名的Unicode字符串转换为Punycode。只有域名中的非ASCII部分会被转换，也就是说你也可以在一个已经转换为ASCII的字符串上调用它。

```js
// encode domain names
punycode.toASCII('mañana.com'); // 'xn--maana-pta.com'
punycode.toASCII('☃-⌘.com'); // 'xn----dqo34k.com'
```

## punycode.ucs2

### punycode.ucs2.decode(string)

创建一个包含字符串中每个Unicode符号的数字编码点的数组。由于[JavaScript在内部使用UCS-2](http://mathiasbynens.be/notes/javascript-encoding)， 该函数会按照UTF-16将一对代半数（UCS-2暴露的单独的字符）转换为单独一个编码点。

```js
punycode.ucs2.decode('abc'); // [0x61, 0x62, 0x63]
// surrogate pair for U+1D306 tetragram for centre:
punycode.ucs2.decode('\uD834\uDF06'); // [0x1D306]
```

### punycode.ucs2.encode(codePoints)

创建以一组数字编码点为基础一个字符串。

```js
punycode.ucs2.encode([0x61, 0x62, 0x63]); // 'abc'
punycode.ucs2.encode([0x1D306]); // '\uD834\uDF06'
```

## punycode.version

表示当前Punycode.js版本数字的字符串。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2019-06-27