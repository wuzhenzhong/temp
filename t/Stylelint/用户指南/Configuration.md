# 配置 | Configuration



检测器需要使用一个 *配置对象*。你可以自定义一个或扩展一个已存在的配置。

## 加载配置对象

stylelint 使用 [cosmiconfig](https://github.com/davidtheclark/cosmiconfig) 来完成查找和加载你的配置对象。从当前工作目录开始，它将按以下顺序查找尽可能的来源：

- `package.json` 中的 `stylelint` 属性
- `.stylelintrc` 文件
- `stylelint.config.js` 文件输出的 JS 对象

`.stylelintrc`文件（不带扩展名）可以是 JSON 或 YAML 格式的。或者，你可以添加一个文件扩展名，来区分 JSON，YAML 或 JS 格式：`.stylelintrc.json`，`.stylelintrc.yaml`，`.stylelintrc.js`。你可能想使用一个扩展名，这样你的文本编辑器可以更好的解释文件，以更好进行语法检查和高亮显示。

一旦发现它们中的任何一个，将不再继续进行查找，进行解析，将使用解析后的对象。

当使用 `config` 或 `configFile` 选项时，配置文件的搜索可能会停止。

## 配置对象

配置对象可以有以下属性。

### `rules`

规则决定检测器要查找什么和要解决什么。stylelint 有[超过 150](http://stylelint.cn/user-guide/rules/)条规则。*所有规则默认都是关闭的*，所以，通过该选项你就可以开启相应规则，进行相应的检测。所有规则必须显式的进行配置，因为 *没有默认值*。

`rules`属性是个对象，其键为规则名称，值为规则配置。每个规则配置符合以下形式之一：

- 一个值 (主要选项)
- 包含两个值的数组 (`[primary option, secondary options]`)
- `null` (关闭规则)

```javascript
{
  "rules": {
    "block-no-empty": null,
    "color-no-invalid-hex": true,
    "comment-empty-line-before": [ "always", {
      "ignore": ["stylelint-commands", "between-comments"]
    } ],
    "declaration-colon-space-after": "always",
    "indentation": ["tab", {
      "except": ["value"]
    }],
    "max-empty-lines": 2,
    "rule-nested-empty-line-before": [ "always", {
      "except": ["first-nested"],
      "ignore": ["after-comment"]
    } ],
    "unit-whitelist": ["em", "rem", "%", "s"]
  }
}
```

指定一个主要选项将开启规则。

#### 从CSS中关闭规则

规则可以通过在你的 CSS 中使用特定的注释临时关闭。例如，你可以关闭所有的规则：

```javascript
/* stylelint-disable */
a {}
/* stylelint-enable */
```

或者你可以关闭个别的规则：

```javascript
/* stylelint-disable selector-no-id, declaration-no-important  */
#id {
  color: pink !important;
}
/* stylelint-enable */
```

你可以使用 `/* stylelint-disable-line */` 注释在个别的行上关闭规则，在其之后你不需要显式的重新开启它们：

```javascript
#id { /* stylelint-disable-line */
  color: pink !important; /* stylelint-disable-line declaration-no-important */
}
```

你也可以使用 `/* stylelint-disable-next-line */` 注释在下一行上关闭规则，在其之后你不需要显式的重新开启它们：

```javascript
#id {
  /* stylelint-disable-next-line declaration-no-important */
  color: pink !important;
}
```

复杂、重叠的禁用和启用模式也是支持的：

```javascript
/* stylelint-disable */
/* stylelint-enable foo */
/* stylelint-disable foo */
/* stylelint-enable */
/* stylelint-disable foo, bar */
/* stylelint-disable baz */
/* stylelint-enable baz, bar */
/* stylelint-enable foo */
```

**警告：** *选择器和值列表* 中的注释目前是被忽略的。

#### 严重程度：错误和警告

默认情况下，所有的规则有一个 `"error"`级别的严重程度。你可以在配置文件中通过添加一个 `defaultSeverity` 属性来改变这种默认情况(见下文)。

使用第二个选项 `severity` 来调整任何特定规则的严重程度。`severity` 可用的值为：

- `"warning"`
- `"warning"`
- `"error"`
- `"error"`

```javascript
// error-level severity examples
{ "indentation": 2 }
{ "indentation": [2] }

// warning-level severity examples
{ "indentation": [2, { "severity": "warning" } ] }
{ "indentation": [2, {
    "except": ["value"],
    "severity": "warning"
  }]
}
```

不同的报告以不同的方式使用这些严重程度级别，比如，以不同的方式进行显示，或以不同的方式退出程序。

#### 自定义消息

当一个规则被触发时，如果你想实现一个自定义的消息，有两种方式可以实现：为规则提供一个 `message` 选项，或写一个自定义的格式化器。

所有的规则接受一个 `message` 作为第二选项，如果提供，将替代提供的任何标准的消息。例如，以下规则配置会使用一些自定义的消息：

```javascript
{
  "color-hex-case": [ "lower", {
    "message": "Lowercase letters are easier to distinguish from numbers"
  } ],
  "indentation": [ 2, {
    "ignore": ["block"],
    "message": "Please use 2 spaces for indentation. Tabs make The Architect grumpy.",
    "severity": "warning"
  } ]
}
```

如果你需要严格的定制，写一个[自定义格式化器](http://stylelint.cn/developer-guide/formatters/)会给你最大控制权。

### `extends`

你的配置可以 *extend* 一个已存在的配置文件(无论是你自己的还是第三方的配置)。当一个配置继承了里一个配置，它将会添加自己的属性并覆盖原有的属性。

你可以继承一个已存在的配置数组，数组中的每一项都优先于下一项(所以，第一项覆盖所有，第二项覆盖除了第一项之外的所有项，最后一项被其他所有项覆盖，等等)。

例如，继承 [`stylelint-config-standard`](https://github.com/stylelint/stylelint-config-standard)，然后将缩进改为 tab 缩进，关闭 `number-leading-zero` 规则：

```javascript
{
  "extends": "stylelint-config-standard",
  "rules": {
    "indentation": "tab",
    "number-leading-zero": null
  }
}
```

或者继承 `stylelint-config-standard` 和 `myExtendableConfig`，并且覆盖缩进规则：

```javascript
{
  "extends": [
    "stylelint-config-standard",
    "./myExtendableConfig"
  ],
  "rules": {
    "indentation": "tab"
  }
}
```

`"extends"` 的值是个“定位器” (或 “定位器” 数组)，也是最终被 `require()` 的，因此，可以使用 Node 的 `require.resolve()` 算法适应任何格式。这意味着一个“定位器”可以是：

- `node_modules`中的某个模块名称 (比如，`stylelint-config-standard`；模块的 `main` 文件必须是一个有效的 JSON 配置)
- 一个带有 `.js` 或 `.json` 扩展名的文件 (which makes sense 如果你在 Node 上下文中创建了一个 JS 对象，并将它传入也是有效的)的绝对路径。
- 一个带有 `.js` 或 `.json` 扩展名的文件的相对路径，相对于引用的配置 (例如，如果 configA 是 `extends: "../configB"`，我们将查找 `configB` 相对于 configA)。

*正因为extends，你可以创建和使用可分享的 stylelint 配置。* 如果你要发布你的配置到 npm，在你的 `package.json` 文件中使用 `stylelint-config` 关键字。

### `plugins`

插件是由社区创建的规则或规则集，支持方法论、工具集，*非标准* 的 CSS 特性，或非常特定的用例。

使用插件的话，在你的配置中添加一个 `"plugins"` 数组，包含“定位器”标识出你要使用的插件。同上面的 `extends`，一个“定位器”可以是一个 npm 模块名，一个绝对路径，或一个相对于要调用的配置文件的路径。

一旦声明了插件，在你的 `"rules"` 对象中，你将需要为插件的规则添加选项，就像其他标准的规则一样。你需要查看插件的文档去了解规则的名称。

```javascript
{
  "plugins": [
    "../special-rule.js"
  ],
  "rules": {
    "plugin/special-rule": "everything"
  }
}
```

一个插件可以提供一个规则或一组规则。如果你使用的插件提供了一组规则，就调用 `"plugins"` 值中的模块，并在 `"rules"` 中使用它的规则。例如：

```javascript
{
  "plugins": [
    "../some-rule-set.js"
  ],
  "rules": {
    "some-rule-set/first-rule": "everything",
    "some-rule-set/second-rule": "nothing",
    "some-rule-set/third-rule": "everything"
  }
}
```

### `processors`

Processors 是 stylelint 的钩子函数，可以以它的方式修改代码，也可以在它们退出时修改结果。

*Processors 只能用在 命令行 和 Node API，不适用于 PostCSS 插件* (PostCSS 插件将忽略它们。)

Processors 可以使 stylelint 检测非样式表文件中的 CSS。例如，你可以检测 HTML 内中 `<style>` 标签中的 CSS，Markdown文件中代码块或 JavaScript 中的字符串。

使用 processors 的话，在你的配置中添加一个 `"processors"` 数组，包含“定位器”标识出你要使用的 processors。同上面的 `extends`，一个“定位器”可以是一个 npm 模块名，一个绝对路径，或一个相对于要调用的配置文件的路径。

```javascript
{
  "processors": ["stylelint-html-processor"],
  "rules": {..}
}
```

如果你的 processor 有选项，把它们放到一个数组里，第一项是“定位器”，第二项是选项对象。

```javascript
{
  "processors": [
    "stylelint-html-processor",
    [ "some-other-processor", { "optionOne": true, "optionTwo": false } ]
  ],
  "rules": {..}
}
```

### `ignoreFiles`

提供一个 glob 或 globs 数组，忽略特定的文件。

(另一种方法是使用 `.stylelintignore` 文件，会在下面描述。)

如果 globs 是绝对路径，就直接使用它们。如果是相对路径，它们将相对：

- `configBasedir`，如果有的话；
- stylelint 使用的配置的文件路径。
- 或 `process.cwd()`。

如果 `ignoreFiles` 属性被继承的配置移除：只有根配置可以忽略文件。

### `defaultSeverity`

所有在第二个选项中没有指定严重级别的规则的默认严重级别。

- `"warning"`
- `"warning"`
- `"error"`
- `"error"`

## `.stylelintignore`

你可以使用一个 `.stylelintignore` 文件(或指定其他的忽略模式文件)忽略指定的文件。

(另一种方式是使用 `config.ignoreFiles`，如上描述。)

你的 `.stylelintignore` 文件中的模式必须匹配 [`.gitignore` 语法](https://git-scm.com/docs/gitignore)。(在幕后使用[`node-ignore`](https://github.com/kaelzhang/node-ignore) 来解析你的模式。) 这就意味着 `*.stylelintignore*` *中模式总是相对于* `*process.cwd()*`*。*

styleline 将在 `process.cwd()` 中查找 `.stylelintignore` 文件。你可以在命令行中使用 `--ignore-path` 和在 JS 中使用 `ignorePath` 选项来指定你一个你要忽略模式的文件路径(绝对或相对于`process.cwd()`)。

- 用户指南
  - [规则](http://stylelint.cn/user-guide/rules/)
  - [插件](http://stylelint.cn/user-guide/plugins/)
  - [Processors](http://stylelint.cn/user-guide/processors/)
- [开发指南](http://stylelint.cn/developer-guide/)
- [Demo](http://stylelint.cn/demo/)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-08-30