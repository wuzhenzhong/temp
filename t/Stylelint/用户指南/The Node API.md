# The Node API



stylelint 模块中的 `lint()` 函数提供 Node API。

```javascript
stylelint.lint(options)
  .then(function(resultObject) { .. });
```

## 安装

stylelint 是一个 [npm 包](https://www.npmjs.com/package/stylelint)。通过以下命令进行安装：

```javascript
npm install stylelint
```

## 选项

Options 是一个对象类型，带有以下属性。

虽然 `files` 和 `code` 是可选的，你必须使用其中一个，不能两个都不使用。所有其他的选项都是可选的。

### `code`

要检测的 CSS 文本字符串。

### `codeFilename`

如果使用 `code` 直接把源字符串传过去，你可以使用 `codeFilename` 将代码和一个特定的文件名联系起来。

这会非常有用，比如，当一个文本编辑器插件直接操作代码，但需要使用配置中的 `ignoreFiles` 功能忽略这些代码。

### `config`

A [stylelint configuration object](http://stylelint.cn/user-guide/configuration/).

一个 [stylelint 配置对象](http://stylelint.cn/user-guide/configuration/)。

如果没有 `config` 或 `configFile`，stylelint 将查找 `.stylelintrc` 配置文件。

### `configBasedir`

一个定义 `extends` 和 `plugins` 的相对路径的目录的绝对路径。

如果 `config` 对象使用相对路径，比如，对于 `extends` 或 `plugins`，你需要传递 `configBasedir`。反之，不需要。

### `configFile`

一个包含你的[stylelint 配置对象](http://stylelint.cn/user-guide/configuration/)的 JSON，YAML 或 JS 文件的路径。

它应该是绝对路径或是相对于你的程序运行的目录（`process.cwd()`）的相对路径。我们推荐使用绝对路径。

### `configOverrides`

部分 stylelint 配置对象的属性将会覆盖已存在的通过 `config` 选项或 `.stylelintrc` 文件加载的配置对象。

`configOverrides` 和 `config` 选项的不同点在于：如果使用了 `config` 对象，stylelint 就不会去查找 `.stylelintrc` 文件了，而是使用你传入的 `config` 对象；但是，如果你想加载 `.stylelintrc` 文件而且像覆盖特定的部分，`configOverrides`就派上用场了。

### `files`

一个文件 glob，或文件列表 glob。最终通过[node-glob](https://github.com/isaacs/node-glob)指出哪些文件是你想要检测的。

相对的 glob 被认为是相对于 `process.cwd()`。

`node_modules` and `bower_components` are always ignored.

`node_modules` 和 `bower_components` 总是被忽略的。

### `formatter`

选项：`"json"|"string"|"verbose"`，或一个函数。默认为 `"json"`。

指定你想使用的格式化器来格式化你的检测结果。

如果你传递一个函数，它必须符合[开发者指南](http://stylelint.cn/developer-guide/formatters/)描述的特征。

### `ignoreDisables`

如果为 `true`，所有的禁用注释(比如，`/* stylelint-disable block-no-empty */`) 将被忽略。

你可以使用该选项查看不使用这样例外的情况下，你的检测结果是怎样的。

### `reportNeedlessDisables`

如果为 `true`，`ignoreDisables` 也将被设置为 `true`， 返回的结果数据将包含一个 `needlessDisables` 属性，该属性的值是个对象列表，每个对象，告诉你哪些 stylelint 禁用注释不在阻塞检测警告。

使用该报告来清理你的代码块，达到只有 stylelint 禁用注释在起作用的目的。

*推荐通过命令行来使用此选项。* 它将在控制台输入一个清晰的报告。

### `ignorePath`

一个文件的路径，该文件包含要忽略文件的模式。该路径可以是绝对或相对于 `process.cwd()` 的路径。默认情况下，stylelint 会查找 在`process.cwd()` 中查找 `.stylelintignore`。查看[配置](http://stylelint.cn/user-guide/configuration/#stylelintignore)。

### `syntax`

Options: `"scss"|"less"|"sugarss"`

选项：`"scss"|"less"|"sugarss"`

指定一个非标准的语法，用来解析源样式表。

如果你不指定一个语法，非标准的语法将会根据文件扩展名 `.scss`，`.less` 和 `.sss` 自动推断出来。

如果你想使用一个自定义的语法，查看下面的 [`customSyntax`](http://stylelint.cn/user-guide/node-api/#customsyntax) 选项。

### `customSyntax`

一个自定义的 [PostCSS-compatible syntax](https://github.com/postcss/postcss#syntaxes) 模块的绝对路径。

但是请注意，stylelint 不保证核心规则能与自定义的语法正常工作，除非是上面 `syntax` 选项列出的默认语法。

## The returned promise

`stylelint.lint()` 返回一个 Promise，它 resolve 一个包含以下属性的对象。

### `errored`

布尔类型。如果为 `true`，至少有一条规则是错误级别的警告。

### `output`

一个展示格式化后的警告（使用默认或你指定的格式化器）的字符串。

### `postcssResults`

一个包含所有 [PostCSS LazyResults](https://github.com/postcss/postcss/blob/master/docs/api.md#lazyresult-class) 的列表，它们是在处理过程中积累起来的。

### `results`

一个包含所有 stylelint 结果对象（格式化器处理过的对象）的列表。

## 语法错误

当你的 CSS 包含语法错误时，`stylelint.lint()` 不会 reject 对应的 Promise。它将 resolve 一个包含语法错误信息的对象(查看 [返回的 promise](http://stylelint.cn/user-guide/node-api/#the-returned-promise))。

## 用法示例

如果 `myConfig` *不* 包含 `extends` 或 `plugins` 的相对路径，你可以不使用 `configBasedir`：

```javascript
stylelint.lint({
  config: myConfig,
  files: "all/my/stylesheets/*.css"
})
  .then(function(data) {
    // do things with data.output, data.errored,
    // and data.results
  })
  .catch(function(err) {
    // do things with err e.g.
    console.error(err.stack);
  });;
```

如果 `myConfig` 包含 `extends` 或 `plugins` 的相对路径，你必须使用 `configBasedir`：

```javascript
stylelint.lint({
  config: myConfig,
  configBasedir: path.join(__dirname, "configs"),
  files: "all/my/stylesheets/*.css"
}).then(function() { .. });
```

你可能想使用 CSS 字符串而不是一个文件 glob，而且想使用字符串格式化器而不是默认的 JSON：

```javascript
stylelint.lint({
  code: "a { color: pink; }",
  config: myConfig,
  formatter: "string"
}).then(function() { .. });
```

你可能想要使用你自定义的格式化函数解析 `.scss` 源文件：

```javascript
stylelint.lint({
  config: myConfig,
  files: "all/my/stylesheets/*.scss",
  formatter: function(stylelintResults) { .. },
  syntax: "scss"
}).then(function() { .. });
```

同样的模式可以被用来读取 Less 或 [SugarSS](https://github.com/postcss/sugarss) 语法。

- 用户指南
  - [规则](http://stylelint.cn/user-guide/rules/)
  - [插件](http://stylelint.cn/user-guide/plugins/)
  - [Processors](http://stylelint.cn/user-guide/processors/)
- [开发指南](http://stylelint.cn/developer-guide/)
- [Demo](http://stylelint.cn/demo/)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-08-30