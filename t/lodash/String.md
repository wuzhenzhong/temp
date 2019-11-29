# String

- 贡献者1人

  

### _.camelCase(string='')

转换`string`为[骆驼案件](https://en.wikipedia.org/wiki/CamelCase)。

##### 以来

3.0.0

##### 参数

1. `[string='']` *（字符串）*：要转换的字符串。

##### 返回

*（字符串）*：返回驼峰字符串。

##### 例

```javascript
_.camelCase('Foo Bar');
// => 'fooBar'
 
_.camelCase('--foo-bar--');
// => 'fooBar'
 
_.camelCase('__FOO_BAR__');
// => 'fooBar'
```

### _.capitalize(string='')

将第一个字符转换`string`为大写，其余转换为小写。

##### 以来

3.0.0

##### 参数

1. `[string='']` *（字符串）*：要大写的字符串。

##### 返回

*（字符串）*：返回大写的字符串。

##### 例

```javascript
_.capitalize('FRED');
// => 'Fred'
```

### _.deburr(string='')

去毛刺`string`通过将拉丁文补充-1（[https://en.wikipedia.org/wiki/Latin-1 *补编*（Unicode_block％29＃Character_table](https://en.wikipedia.org/wiki/Latin-1_Supplement_(Unicode_block%29#Character_table)）和[拉丁字母扩充-A](https://en.wikipedia.org/wiki/Latin_Extended-A)字母基本拉丁字母和除去[组合变音符号](https://en.wikipedia.org/wiki/Combining_Diacritical_Marks)。

##### Since

3.0.0

##### 参数

1. `[string='']` *（字符串）*：去毛刺的字符串。

##### 返回

*（字符串）*：返回去毛刺字符串。

##### 例

```javascript
_.deburr('déjà vu');
// => 'deja vu'
```

### _.endsWith(string='', target, position=string.length)

检查是否`string`以给定的目标字符串结束。

##### 以来

3.0.0

##### 参数

1. `[string='']` *（字符串）*：要检查的字符串。
2. `[target]` *（字符串）*：要搜索的字符串。
3. `[position=string.length]` *（数字）*：最多搜索的位置。

##### 返回

*（boolean）*：返回`true`如果`string`以`target`else 结束`false`。

##### 例

```javascript
_.endsWith('abc', 'c');
// => true
 
_.endsWith('abc', 'b');
// => false
 
_.endsWith('abc', 'b', 2);
// => true
```

### _.escape(string='')

将字符“＆”，“<”，“>”，“”和“'” `string`转换为其对应的HTML实体。

**注意：**没有其他字符被转义。为了逃避额外的角色，请使用像[*他*](https://mths.be/he)这样的第三方库。

虽然“>”字符为了对称而被转义，但像“>”和“/”这样的字符不需要在HTML中转义，除非它们是标记或未加引号属性值的一部分，否则没有特殊含义。有关更多详细信息，请参阅[Mathias Bynens的文章](https://mathiasbynens.be/notes/ambiguous-ampersands) *（在“半相关趣味事实”下）*。

使用HTML时，应始终[引用属性值](http://wonko.com/post/html-escaping)以减少XSS向量。

##### 以来

0.1.0

##### 参数

1. `[string='']` *（字符串）*：要转义的字符串。

##### 返回

*（字符串）*：返回转义字符串。

##### 例

```javascript
_.escape('fred, barney, & pebbles');
// => 'fred, barney, &amp; pebbles'
```

### _.escapeRegExp(string='')

转义`RegExp`特殊字符“^”，“$”，“”，“。”，“*”，“+”，“？”，“（”，“）”，“”，“”，“{”，“ }“和”|“ 中`string`。

##### 以来

3.0.0

##### 参数

1. `[string='']` *（字符串）*：要转义的字符串。

##### 返回

*（字符串）*：返回转义字符串。

##### 例

```javascript
_.escapeRegExp('[lodash](https://lodash.com/)');
// => '\[lodash\]\(https://lodash\.com/\)'
```

### _.kebabCase(string='')

转换`string`为[Kebab的情况](https://en.wikipedia.org/wiki/Letter_case#Special_case_styles)。

##### 以来

3.0.0

##### 参数

1. `[string='']` *（字符串）*：要转换的字符串。

##### 返回

*（字符串）*：返回烤肉串字符串。

##### 例

```javascript
_.kebabCase('Foo Bar');
// => 'foo-bar'
 
_.kebabCase('fooBar');
// => 'foo-bar'
 
_.kebabCase('__FOO_BAR__');
// => 'foo-bar'
```

### _.lowerCase(string='')

`string`以空格分隔的字词转换为小写字母。

##### 以来

4.0.0

##### 参数

1. `[string='']` *（字符串）*：要转换的字符串。

##### 返回

*（字符串）*：返回下方的字符串。

##### 例

```javascript
_.lowerCase('--Foo-Bar--');
// => 'foo bar'
 
_.lowerCase('fooBar');
// => 'foo bar'
 
_.lowerCase('__FOO_BAR__');
// => 'foo bar'
```

### _.lowerFirst(string='')

将第一个字符转换`string`为小写。

##### 以来

4.0.0

##### 参数

1. `[string='']` *（字符串）*：要转换的字符串。

##### 返回

*（字符串）*：返回转换后的字符串。

##### 例

```javascript
_.lowerFirst('Fred');
// => 'fred'
 
_.lowerFirst('FRED');
// => 'fRED'
```

### _.pad(string='', length=0, chars=' ')

`string`如果它短于左侧和右侧垫`length`。如果填充字符不能被平均分割，则会被截断`length`。

##### 以来

3.0.0

##### 参数

1. `[string='']` *（字符串）*：要填充的字符串。
2. `[length=0]` *（数字）*：填充长度。
3. `[chars=' ']` *（字符串）*：用作填充的字符串。

##### 返回

*（字符串）*：返回填充的字符串。

##### 例

```javascript
_.pad('abc', 8);
// => '  abc   '
 
_.pad('abc', 8, '_-');
// => '_-abc_-_'
 
_.pad('abc', 3);
// => 'abc'
```

### _.padEnd(string='', length=0, chars=' ')

垫`string`右侧，如果它比短`length`。填充字符如果超过，则会被截断`length`。

##### 以来

4.0.0

##### 参数

1. `[string='']` *（字符串）*：要填充的字符串。
2. `[length=0]` *（数字）*：填充长度。
3. `[chars=' ']` *（字符串）*：用作填充的字符串。

##### 返回

*(string)*: Returns the padded string.

##### 例

```javascript
_.padEnd('abc', 6);
// => 'abc   '
 
_.padEnd('abc', 6, '_-');
// => 'abc_-_'
 
_.padEnd('abc', 3);
// => 'abc'
```

### _.padStart（string =''，length = 0，chars =''）

垫`string`左侧，如果是比较短的`length`。填充字符如果超过，则会被截断`length`。

##### 自从

4.0.0

##### 参数

1. `[string='']` *（字符串）*：要填充的字符串。
2. `[length=0]` *（数字）*：填充长度。
3. `[chars=' ']` *（字符串）*：用作填充的字符串。

##### 返回

*（字符串）*：返回填充的字符串。

##### 示例

```javascript
_.padStart('abc', 6);
// => '   abc'
 
_.padStart('abc', 6, '_-');
// => '_-_abc'
 
_.padStart('abc', 3);
// => 'abc'
```

### _.parseInt(string, radix=10)

转换`string`为指定基数的整数。如果`radix`是`undefined`或 `0`，一个`radix`的`10`使用，除非`value`是一个十六进制，在这种情况下`radix`的`16`使用。

**注意：**此方法与[ES5的实现](https://es5.github.io/#x15.1.2.2)一致`parseInt`。

##### 以来

1.1.0

##### 参数

1. `string` *（字符串）*：要转换的字符串。
2. `[radix=10]` *（数字）*：解释的基数`value`。

##### 返回

*（数字）*：返回转换后的整数。

##### 例

```javascript
_.parseInt('08');
// => 8
 
_.map(['6', '08', '10'], _.parseInt);
// => [6, 8, 10]
```

### _.repeat（string =''，n = 1）

重复给定的字符串`n`时间。

##### 以来

3.0.0

##### 参数

1. `[string='']` *（字符串）*：要重复的字符串。
2. `[n=1]` *（数字）*：重复字符串的次数。

##### 返回

*（字符串）*：返回重复的字符串。

##### 例

```javascript
_.repeat('*', 3);
// => '***'
 
_.repeat('abc', 2);
// => 'abcabc'
 
_.repeat('abc', 0);
// => ''
```

### _.replace（string =''，pattern，replacement）

替换比赛为`pattern`在`string`与 `replacement`。

**注意：**此方法基于[`String#replace`](https://mdn.io/String/replace)。

##### 以来

4.0.0

##### 参数

1. `[string='']` *（字符串）*：要修改的字符串。
2. `pattern` *（RegExp | string）*：要替换的模式。
3. `replacement` *（功能|字符串）*：匹配替换。

##### 返回

*（字符串）*：返回修改后的字符串。

##### 例

```javascript
_.replace('Hi Fred', 'Fred', 'Barney');
// => 'Hi Barney'
```

### _.snakeCase(string='')

转换`string`为[蛇的情况](https://en.wikipedia.org/wiki/Snake_case)。

##### 以来

3.0.0

##### 参数

1. `[string='']` *（字符串）*：要转换的字符串。

##### 返回

*（字符串）*：返回蛇字符串。

##### 例

```javascript
_.snakeCase('Foo Bar');
// => 'foo_bar'
 
_.snakeCase('fooBar');
// => 'foo_bar'
 
_.snakeCase('--FOO-BAR--');
// => 'foo_bar'
```

### _.split(string='', separator, limit)

拆分`string`的`separator`。

**注意：**此方法基于[`String#split`](https://mdn.io/String/split)。

##### 版本

4.0.0

##### 参数

1. `[string='']` *(string)*: The string to split.
2. `separator` *(RegExp|string)*: The separator pattern to split by.
3. `[limit]` *(number)*: The length to truncate results to.

##### 返回

*（数组）*：返回字符串段。

##### 例

```javascript
_.split('a-b-c', '-', 2);
// => ['a', 'b']
```

### _.startCase(string='')

转换`string`为[启动大小写](https://en.wikipedia.org/wiki/Letter_case#Stylistic_or_specialised_usage)。

##### 以来

3.1.0

##### 参数

1. `[string='']` *（字符串）*：要转换的字符串。

##### 返回值

*（字符串）*：返回开始的套用字符串。

##### 例

```javascript
_.startCase('--foo-bar--');
// => 'Foo Bar'
 
_.startCase('fooBar');
// => 'Foo Bar'
 
_.startCase('__FOO_BAR__');
// => 'FOO BAR'
```

### _.startsWith(string='', target, position=0)

检查是否`string`从给定的目标字符串开始。

##### 以来

3.0.0

##### 参数

1. `[string='']` *（字符串）*：要检查的字符串。
2. `[target]` *（字符串）*：要搜索的字符串。
3. `[position=0]` *（数字）*：从中搜索的位置。

##### 返回

*（boolean）*：返回`true`如果`string`以`target`else 开始`false`。

##### 例

```javascript
_.startsWith('abc', 'a');
// => true
 
_.startsWith('abc', 'b');
// => false
 
_.startsWith('abc', 'b', 1);
// => true
```

### _.template(string='', options={})

创建一个编译的模板函数，可以在“插入”分隔符中插入数据属性，在“转义”分隔符中插入HTML转义插值数据属性，并在“评估”分隔符中执行JavaScript。数据属性可以作为模板中的自由变量来访问。如果给定设置对象，则优先于 `_.templateSettings`值。

**注意：**在开发版本中，`_.template`利用[sourceURL](http://www.html5rocks.com/en/tutorials/developertools/sourcemaps/#toc-sourceurl)来更容易地进行调试。

有关预编译模板的更多信息，请参阅[lodash的自定义构建文档](https://lodash.com/custom-builds)。

有关Chrome扩展程序沙箱的更多信息，请参阅[Chrome的扩展程序文档](https://developer.chrome.com/extensions/sandboxingEval)。

##### 版本

0.1.0

##### 参数

1. `[string='']` *(string)*: The template string.
2. `[options={}]` *(Object)*: The options object.
3. `[options.escape=_.templateSettings.escape]` *(RegExp)*: The HTML "escape" delimiter.
4. `[options.evaluate=_.templateSettings.evaluate]` *(RegExp)*: The "evaluate" delimiter.
5. `[options.imports=_.templateSettings.imports]` *(Object)*: An object to import into the template as free variables.
6. `[options.interpolate=_.templateSettings.interpolate]` *(RegExp)*: The "interpolate" delimiter.
7. `[options.sourceURL='lodash.templateSources[n]']` *(string)*: The sourceURL of the compiled template.
8. `[options.variable='obj']` *(string)*: The data object variable name.

##### 返回

*（功能）*：返回已编译的模板功能。

##### 例

```javascript
// Use the "interpolate" delimiter to create a compiled template.

var compiled = _.template('hello <%= user %>!');

compiled({ 'user': 'fred' });
// => 'hello fred!'
 
// Use the HTML "escape" delimiter to escape data property values.

var compiled = _.template('<b><%- value %></b>');

compiled({ 'value': '<script>' });
// => '<b>&amplt;script&ampgt;</b>'
 
// Use the "evaluate" delimiter to execute JavaScript and generate HTML.

var compiled = _.template('<% _.forEach(users, function(user) { %><li><%- user %></li><% }); %>');

compiled({ 'users': ['fred', 'barney'] });
// => '<li>fred</li><li>barney</li>'
 
// Use the internal `print` function in "evaluate" delimiters.

var compiled = _.template('<% print("hello " + user); %>!');

compiled({ 'user': 'barney' });
// => 'hello barney!'
 
// Use the ES template literal delimiter as an "interpolate" delimiter.
// Disable support by replacing the "interpolate" delimiter.

var compiled = _.template('hello ${ user }!');

compiled({ 'user': 'pebbles' });
// => 'hello pebbles!'
 
// Use backslashes to treat delimiters as plain text.

var compiled = _.template('<%= "\\<%- value %\\>" %>');

compiled({ 'value': 'ignored' });
// => '<%- value %>'
 
// Use the `imports` option to import `jQuery` as `jq`.

var text = '<% jq.each(users, function(user) { %><li><%- user %></li><% }); %>';

var compiled = _.template(text, { 'imports': { 'jq': jQuery } });

compiled({ 'users': ['fred', 'barney'] });
// => '<li>fred</li><li>barney</li>'
 
// Use the `sourceURL` option to specify a custom sourceURL for the template.

var compiled = _.template('hello <%= user %>!', { 'sourceURL': '/basic/greeting.jst' });

compiled(data);
// => Find the source of "greeting.jst" under the Sources tab or Resources panel of the web inspector.
 
// Use the `variable` option to ensure a with-statement isn't used in the compiled template.

var compiled = _.template('hi <%= data.user %>!', { 'variable': 'data' });
compiled.source;
// => function(data) {
//   var __t, __p = '';
//   __p += 'hi ' + ((__t = ( data.user )) == null ? '' : __t) + '!';
//   return __p;
// }
 
// Use custom template delimiters.
_.templateSettings.interpolate = /{{([\s\S]+?)}}/g;

var compiled = _.template('hello {{ user }}!');

compiled({ 'user': 'mustache' });
// => 'hello mustache!'
 
// Use the `source` property to inline compiled templates for meaningful
// line numbers in error messages and stack traces.
fs.writeFileSync(path.join(process.cwd(), 'jst.js'), '\

  var JST = {\

    "main": ' + _.template(mainText).source + '\

  };\

');
```

### _.toLower(string='')

`string`整体而言，转换为小写字母，就像[String＃toLowerCase一样](https://mdn.io/toLowerCase)。

##### 版本

4.0.0

##### 参数

1. `[string='']` *（字符串）*：要转换的字符串。

##### 返回

*（字符串）*：返回下方的字符串。

##### 例

```javascript
_.toLower('--Foo-Bar--');
// => '--foo-bar--'
 
_.toLower('fooBar');
// => 'foobar'
 
_.toLower('__FOO_BAR__');
// => '__foo_bar__'
```

### _.toUpper(string='')

`string`整体转换为大写字母，就像[String＃toUpperCase一样](https://mdn.io/toUpperCase)。

##### 版本

4.0.0

##### 参数

1. `[string='']` *（字符串）*：要转换的字符串。

##### 返回

*（字符串）*：返回上方的字符串。

##### 例

```javascript
_.toUpper('--foo-bar--');
// => '--FOO-BAR--'
 
_.toUpper('fooBar');
// => 'FOOBAR'
 
_.toUpper('__foo_bar__');
// => '__FOO_BAR__'
```

### _.trim(string='', chars=whitespace)

从中删除前导和尾部的空格或指定的字符`string`。

##### 版本

3.0.0

##### 参数

1. `[string='']` *（字符串）*：要修剪的字符串。
2. `[chars=whitespace]` *（字符串）*：要修剪的字符。

##### 返回

*（字符串）*：返回修剪后的字符串。

##### 例

```javascript
_.trim('  abc  ');
// => 'abc'
 
_.trim('-_-abc-_-', '_-');
// => 'abc'
 
_.map(['  foo  ', '  bar  '], _.trim);
// => ['foo', 'bar']
```

### _.trimEnd（string =''，chars = whitespace）

从中删除尾部空白或指定的字符`string`。

##### 版本

4.0.0

##### 参数

1. `[string='']` *（字符串）*：要修剪的字符串。
2. `[chars=whitespace]` *（字符串）*：要修剪的字符。

##### 返回

*（字符串）*：返回修剪后的字符串。

##### 例

```javascript
_.trimEnd('  abc  ');
// => '  abc'
 
_.trimEnd('-_-abc-_-', '_-');
// => '-_-abc'
```

### _.trimStart(string='', chars=whitespace)

从中删除前导空格或指定的字符`string`。

##### 版本

4.0.0

##### 参数

1. `[string='']` *（字符串）*：要修剪的字符串。
2. `[chars=whitespace]` *（字符串）*：要修剪的字符。

##### 返回

*（字符串）*：返回修剪后的字符串。

##### 例

```javascript
_.trimStart('  abc  ');
// => 'abc  '
 
_.trimStart('-_-abc-_-', '_-');
// => 'abc-_-'
```

### _.truncate(string='', options={})

`string`如果它长于给定的最大字符串长度，则截断。截断字符串的最后一个字符被替换为缺省为“...”的省略字符串。

##### 版本

4.0.0

##### 参数

1. `[string='']` *（字符串）*：要截断的字符串。
2. `[options={}]` *（对象）*：选项对象。
3. `[options.length=30]` *（数字）*：最大字符串长度。
4. `[options.omission='...']` *（字符串）*：省略表示文本的字符串。
5. `[options.separator]` *（RegExp | string）*：要截断的分隔符模式。

##### 返回

*（字符串）*：返回截断的字符串。

##### 例

```javascript
_.truncate('hi-diddly-ho there, neighborino');
// => 'hi-diddly-ho there, neighbo...'
 
_.truncate('hi-diddly-ho there, neighborino', {
  'length': 24,

  'separator': ' '

});
// => 'hi-diddly-ho there,...'
 
_.truncate('hi-diddly-ho there, neighborino', {
  'length': 24,

  'separator': /,? +/

});
// => 'hi-diddly-ho there...'
 
_.truncate('hi-diddly-ho there, neighborino', {
  'omission': ' [...]'

});
// => 'hi-diddly-ho there, neig [...]'
```

### _.unescape(string='')

相反的`_.escape`; 这种方法的HTML实体转换 `&ampamp;`，`&amplt;`，`&ampgt;`，`&ampquot;`，和 `&amp#39;`在`string`其对应的字符。

**注意：**没有其他HTML实体未转义。为了避免额外的HTML实体使用像[*他*](https://mths.be/he)这样的第三方库。

##### 版本

0.6.0

##### 参数

1. `[string='']` *（字符串）*：unescape的字符串。

##### 返回

*（字符串）*：返回未转义的字符串。

##### 例

```javascript
_.unescape('fred, barney, &amp; pebbles');
// => 'fred, barney, & pebbles'
```

### _.upperCase(string='')

`string`以空格分隔的字词转换为大写字母。

##### 版本

4.0.0

##### 参数

1. `[string='']` *（字符串）*：要转换的字符串。

##### 返回

*（字符串）*：返回上方的字符串。

##### 例

```javascript
_.upperCase('--foo-bar');
// => 'FOO BAR'
 
_.upperCase('fooBar');
// => 'FOO BAR'
 
_.upperCase('__foo_bar__');
// => 'FOO BAR'
```

### _.upperFirst(string='')

将第一个字符转换`string`为大写。

##### 版本

4.0.0

##### 参数

1. `[string='']` *（字符串）*：要转换的字符串。

##### 返回

*（字符串）*：返回转换后的字符串。

##### 例

```javascript
_.upperFirst('fred');
// => 'Fred'
 
_.upperFirst('FRED');
// => 'FRED'
```

### _.words(string='', pattern)

拆分`string`成它的单词的数组。

##### 版本

3.0.0

##### 参数

1. `[string='']` *（字符串）*：要检查的字符串。
2. `[pattern]` *（RegExp | string）*：匹配单词的模式。

##### 返回

*（数组）*：返回单词`string`。

##### 例

```javascript
_.words('fred, barney, & pebbles');
// => ['fred', 'barney', 'pebbles']
 
_.words('fred, barney, & pebbles', /[^, ]+/g);
// => ['fred', 'barney', '&', 'pebbles']
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-03-07