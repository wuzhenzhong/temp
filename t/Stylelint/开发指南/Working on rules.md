# 按规则工作 | Working on rules

**请帮助我们创建，增强和调试stylelint规则！**

已有超过一百条规则，因此stylelint *需要*社区贡献才能继续改进。

如果你喜欢stylelint和开源软件（因为你正在读这个，你几乎肯定会这样做），请考虑花一些时间来投入。你不仅会帮助stylelint茁壮成长，你还会学习一两件事 - 关于CSS ，PostCSS，Node，ES2015，单元测试，开源软件等。

**我们希望尽一切努力鼓励捐款！**因此，如果您想参与，但最终因为某种原因而无法参与，请提出问题，并就我们可以采取哪些措施向您提供反馈，以便更好地鼓励您。

另外：我们希望您参与该项目不是一次性的。*我们希望在组织中添加更多成员，并看到更多常客弹出问题并提出请求！*

## 创建新规则

首先，针对新规则打开您的想法[问题](https://github.com/stylelint/stylelint/issues/new)。

通常我们会对规则的目的，名称，选项和适用性进行一些讨论。

### 纳入标准

我们讨论规则是否符合以下标准以包含在stylelint中：

- 仅适用于标准CSS语法。
- 对大多数用户有用。
- 有一个明确和明确的完成状态。
- 有一个单一的目的。
- 是独立的，不依赖于其他规则。
- 不包含与其他规则重叠的功能。

否则，它应该是一个插件。但是，插件也应该尽量遵守后三个标准。

### 命名规则

查看规则用户指南以熟悉规则命名约定。

我们注意确保准确一致地命名所有规则。我们在这方面的目标是确保规则易于查找和理解，并防止我们以后想要更改名称。

*命名规则是为了鼓励明确而非隐含的选项。*例如，`color-hex-case: "upper"|"lower"`而不是`color-hex-uppercase: "always"|"never"`。因为`color-hex-uppercase: "never"` *暗示*总是小写，而`color-hex-case: "lower"`使它*明确*。

### 确定选项

#### 主要

每个规则都*必须有*一个**主要选项**。

- 在`"color-hex-case": "upper"`，主要选项是`"upper"`。
- 在`"indentation": [2, { "except": ["block"] }]`，主要选项是`2`。

如果您的规则可以接受数组作为其主要选项，则必须通过`primaryOptionArray = true`在规则函数上设置属性来指定它。例如：

```javascript
function rule(primary, secondary) {
  return (root, result) => {..}
}
rule.primaryOptionArray = true
export default rule
// or, for plugins: stylelint.createPlugin(ruleName, rule)
```

这里有一点需要注意：如果您的规则接受主选项数组，则它也不能接受主选项对象。只要有可能，如果您希望规则接受主选项数组，您应该只创建一个数组，而不是允许各种数据结构。

#### 次要

某些规则需要额外的灵活性来处理各种用例。这些可以使用**可选的辅助选项对象**。

- 在`"color-hex-case": "upper"`，没有辅助选项对象。
- 在`"indentation": [2, { "except": ["block"] }]`，辅助选项对象是`{ "except": ["block"] }`。

最典型的次要选择是`"ignore": []`和`"except": []`; 但一切皆有可能。

如果您不忽略或例外，规则的次要选项可以是任何内容。例如，`resolveNestedSelectors: true|false`在某些`selector-*`规则中用于更改规则处理嵌套选择器的方式。

##### 关键字`"ignore"`和`"except"`

`"ignore"`并`"except"`接受一组预定义的关键字选项，例如`["relative", "first-nested", "descendant"]`。

- 使用`"ignore"`时，你希望规则简单地跳过，在一个特定的模式。
- `"except"`如果要反转特定模式的主选项，请使用此选项。

##### 用户自定义 `"ignore*"`

在接受*用户定义*的要忽略的事物列表时，请使用更具体的辅助选项名称。如果规则检查规则并且您希望允许用户指定要忽略哪些特定的规则类型，则采用`"ignore<Things>": []`例如使用的形式`"ignoreAtRules": []`。

### 确定警告消息

消息采用以下形式之一：

- “预期[某事] [在某些情况下]”。
- “出乎意料的[某事] [在某些情况下]。”

查看其他规则的消息，以收集更多的约定和模式。

### 写下规则

*在编写规则时，请始终查看其他类似的约定和模式规则，以便从模拟开始。*

您将使用简单的 [PostCSS API ](http://api.postcss.org/)来导航和分析CSS语法树。我们建议使用`walk`迭代器（例如`walkDecls`），而不是使用`forEach`循环遍历节点。

根据规则，我们还建议使用[postcss-value-parser ](https://github.com/TrySound/postcss-value-parser)和 [postcss-selector-parser](https://github.com/postcss/postcss-selector-parser)。使用这些解析器而不是正则表达式或`indexOf`搜索有很多好处（即使它们并不总是最高性能的方法）。

stylelint具有许多在现有规则中使用的 [实用程序函数](https://github.com/stylelint/stylelint/tree/master/lib/utils)，也可能对您有用。请仔细查看，以便了解可用的内容。（如果你有一个新的功能，你认为可能通常有用，让我们把它添加到列表中！）

特别是，您肯定希望使用，`validateOptions()`以便警告用户无效选项。（查看选项验证示例的其他规则将有很大帮助。）

### 编写测试

每条规则必须附带包含以下内容的测试：

- 所有被视为警告的模式。
- 应该所有的模式*不*被认为是警告。

编写 stylelint 测试很容易，所以*尽可能多地编写*。

#### 清单

请仔细检查此清单，确保测试涵盖每个点。特别是*考虑边缘情况*。这些都是规则的缺陷和缺点总是出现的地方。

##### 最佳做法

- 确保您在多个位置测试错误，而不是每次都在同一个位置。
- 确保使用逼真（如果简单）CSS，并避免使用省略号。
- 确保您默认使用标准CSS语法，并且在测试特定的非标准语法时仅交换解析器。
- 从PostCSS AST访问原始字符串时，请使用`node.raws`而不是`node.raw()`。这将确保字符串与原始字符串完全对应。

##### 通常被忽视的边缘情况

- 请问你的规则处理变量（`$sass`，`@less`或`var(--custom-property)`）？
- 你的规则如何处理CSS字符串（例如`content: "anything goes";`）？
- 您的规则如何处理CSS注释（例如`/* anything goes */`）？
- 您的规则如何处理`url()`函数，包括数据URI（例如`url(anything/goes.jpg)`）？
- 您的规则如何处理供应商前缀（例如`@-webkit-keyframes name {}`）？
- 您的规则如何处理区分大小写（例如`@KEYFRAMES name {}`）？
- 您的规则如何处理与伪元素*组合*的伪类（例如`a:hover::before`）？
- 您的规则如何处理嵌套（例如，您是解决`& a {}`，还是按原样检查？）？
- 您的规则如何处理空格和标点符号（例如`rgb(0,0,0)`与之比较`rgb(0, 0, 0)`）？

#### 运行测试

您可以通过以下方式运行测试：

```javascript
npm test
```

但是，这会运行所有25,000多个单元测试并且还会进行 linting。

您可以使用交互式测试提示来仅针对一组选定的规则（您在开发期间要执行的操作）运行测试。例如，仅针对`color-hex-case`和`color-hex-length`规则运行测试：

1. 使用`npm run watch`启动提示。
2. 按`p`以按文件名正则表达式模式过滤。
3. 输入`color-hex-case|color-hex-length`ie由管道符号（`|`）分隔的每个规则名称。

### 写自述文件

每条规则必须附有自述文件，符合以下格式：

1. 规则名称。
2. 单行描述。
3. 原型代码示例。
4. 扩展说明（如有必要）。
5. 选项。
6. 被视为警告的示例模式（对于每个选项值）。
7. *不*被视为警告的示例模式（对于每个选项值）。
8. 可选选项（如果适用）。

查看其他规则的自述文件以收集更多传统模式。

#### 单行描述

采取以下形式：

- “禁止......”（适用于`no`规则）。
- “限制......”（适用于`max`规则）。
- “要求......”（适用于接受`"always"`和`"never"`选项的规则）。
- “指定......”（其他一切）。

#### 示例模式

- 使用完整的 CSS 模式，即避免省略号（`...`）
- `css`默认情况下，使用标准 CSS 语法（并使用代码栅栏）。
- 使用可能的最少代码来传递模式，例如，如果规则目标选择器然后使用空规则，例如`{}`。
- 使用`{}`，而不是`{ }`空的规则。
- `a`默认情况下使用类型选择器。
- `@media`默认情况下使用at-rules。
- `color`默认情况下使用该属性。
- 使用 *FOO*，*酒吧*和*巴兹*的名称例如`.foo`，`#bar` `--baz`

### 连接规则

最后一步是在以下位置添加对新规则的引用：

- [规则`index.js`文件](https://github.com/stylelint/stylelint/blob/master/lib/rules/index.js)
- 规则列表
- 示例配置

一旦有东西要显示，你就会创建一个[拉取请求](https://github.com/stylelint/stylelint/compare)来继续对话。

## 向现有规则添加选项

首先，打开有关您要添加的选项[的问题](https://github.com/stylelint/stylelint/issues/new)。我们将在那里讨论它的功能和名称。

一旦我们就方向达成一致，您就可以处理拉取请求。以下是您需要采取的步骤：

1. 更改规则的验证以允许新选项。
2. 添加规则一些逻辑（尽可能少）使选项工作。
3. 添加新的单元测试以测试该选项。
4. 添加有关新选项的文档。

## 修复现有规则中的错误

修复错误通常很容易。这是一个有效的过程：

1. 编写失败的单元测试，以示例错误。
2. 在这些新测试通过之前，先处理规则。

而已！**如果您无法自己弄清楚如何修复错误，那么** **在您的失败测试用例中提交拉取请求仍然非常** **有用。**这意味着其他人可以直接进入并帮助解决规则的逻辑问题。

## 提高新规则或现有规则的性能

有一种简单的方法可以在任何给定规则上运行基准测试，并为其提供任何有效配置：

```javascript
npm run benchmark-rule -- [rule-name] [config]
```

如果`config`参数不是字符串或布尔值，则它必须是用引号括起来的有效 JSON。

```javascript
npm run benchmark-rule -- selector-combinator-space-after never
npm run benchmark-rule -- selector-combinator-space-after always
npm run benchmark-rule -- selector-no-combinator true
npm run benchmark-rule -- block-opening-brace-space-before "[\"always\", {\"ignoreAtRules\": [\"else\"]}]"
```

该脚本加载 Bootstrap 的 CSS（来自其CDN）并通过配置的规则运行它。

它最终将打印一些简单的统计数据：

```javascript
Warnings: 1441
Mean: 74.17598357142856 ms
Deviation: 16.63969674310928 ms
```

你能用这个做什么？**在编写新规则或重构现有规则时，请使用这些度量来确定代码的效率。**

stylelint规则可以多次重复它的核心逻辑（例如，检查庞大的CSS代码库中每个声明的每个值节点）。所以值得关注性能并尽我们所能来改进它！

**如果你只想要一个快速的小项目，这是一个很好的贡献方式。**尝试选择规则并查看是否有任何可以做的事情来加快速度。

确保在PR中包含基准测量！

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-09-03