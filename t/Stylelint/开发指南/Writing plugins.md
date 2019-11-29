# 编写插件 | Writing plugins

插件是社区构建的规则和规则集。

我们建议您熟悉并遵守stylelint 编写规则的惯例，包括名称，选项，消息，测试和文档。

## 插件的解剖

```javascript
// Abbreviated example
var stylelint = require("stylelint")

var ruleName = "plugin/foo-bar"
var messages =  stylelint.utils.ruleMessages(ruleName, {
  expected: "Expected ...",
})

module.exports = stylelint.createPlugin(ruleName, function(primaryOption, secondaryOptionObject) {
  return function(postcssRoot, postcssResult) {
    var validOptions = stylelint.utils.validateOptions(postcssResult, ruleName, { .. })
    if (!validOptions) { return }
    // ... some logic ...
    stylelint.utils.report({ .. })
  }
})

module.exports.ruleName = ruleName
module.exports.messages = messages
```

您的插件的规则名称必须是命名空间，例如`your-namespace/your-rule-name`。如果您的插件只提供一个规则，或者您无法想到一个好的命名空间，那么您可以简单地使用`plugin/my-rule`。此命名空间可确保插件规则永远不会与核心规则冲突。*确保为用户记录插件的规则名称（和命名空间），因为他们需要在配置中使用它。*

`stylelint.createPlugin(ruleName, ruleFunction)` 确保您的插件可以与其他规则一起正确设置。

为了使插件规则能够使用标准配置格式，`ruleFunction`应接受2个参数：primary选项和（可选）辅助选项对象。

`ruleFunction`应该返回一个本质上是一个小[PostCSS插件](https://github.com/postcss/postcss/blob/master/docs/writing-a-plugin.md)的函数：它需要2个参数：PostCSS Root（解析的AST）和PostCSS LazyResult。您必须[了解PostCSS API](https://github.com/postcss/postcss/blob/master/docs/api.md)。

## `stylelint.utils`

stylelint公开了一些有用的实用程序。*有关这些函数的API的详细信息，请查看源代码中的注释和标准规则中的示例。*

### `stylelint.utils.report`

将插件中的警告添加到样式列表将向用户报告的警告列表中。

不要 直接使用 PostCSS 的方法。当您使用时，您的插件将尊重禁用范围和stylelint的其他可能的未来功能，提供更好的用户体验，更符合标准规则。`node.warn()` `stylelint.utils.report`

### `stylelint.utils.ruleMessages`

将您的消息定制为标准 stylelint 规则的格式。

### `stylelint.utils.validateOptions`

验证规则的选项。

### `stylelint.utils.checkAgainstRule`

根据*您自己的规则*中的标准stylelint规则检查CSS 。此功能为希望修改，约束或扩展现有stylelint规则功能的插件作者提供了强大功能和灵活性。

接受选项对象和使用指定规则的警告调用的回调。选项是：

- `ruleName`：您要调用的规则的名称。
- `ruleSettings`：您要调用的规则的设置，格式与`.stylelintrc`配置对象中的格式相同。
- `root`：运行此规则的根节点。

使用警告从您报告的插件规则中创建新警告。`stylelint.utils.report`

例如，假设您要创建一个插件，该插件`at-rule-no-unknown`使用预处理程序提供的at-rules的内置异常列表运行：

```javascript
const allowableAtRules = [..]

function myPluginRule(primaryOption, secondaryOptions) {
  return (root, result) => {
    const defaultedOptions = Object.assign({}, secondaryOptions, {
      ignoreAtRules: allowableAtRules.concat(options.ignoreAtRules || []),
    })

    stylelint.utils.checkAgainstRule({
      ruleName: 'at-rule-no-unknown',
      ruleSettings: [primaryOption, defaultedOptions],
      root: root
    }, (warning) => {
      stylelint.utils.report({
        message: myMessage,   
        ruleName: myRuleName,     
        result: result,        
        node: warning.node,
        line: warning.line,
        column: warning.column,
      })
    })
  }
}
```

## `stylelint.rules`

所有规则功能都可以在`stylelint.rules`。这使您可以根据您的特定需求构建现有规则。

典型的用例是构建规则选项允许的更复杂的条件。例如，您的代码库可能使用特殊注释指令来自定义特定样式表的规则选项。您可以构建一个插件来检查这些指令，然后使用正确的选项运行适当的规则（或者根本不运行它们）。

所有规则共享共同签名。它们是一个接受两个参数的函数：主选项和辅助选项对象。并且该函数返回一个具有 PostCSS 插件签名的函数，期望 PostCSS 根和结果作为其参数。

这是一个插件的简单示例，`color-hex-case`只有`@@check-color-hex-case`在样式表中的某处有特殊指令时才会运行：

```javascript
export default stylelint.createPlugin(ruleName, function (expectation) {
  const runColorHexCase = stylelint.rules["color-hex-case"](expectation)
  return (root, result) => {
    if (root.toString().indexOf("@@check-color-hex-case") === -1) return
    runColorHexCase(root, result)
  }
})
```

## 允许主选项数组

如果您的插件可以接受数组作为其主要选项，则必须通过`primaryOptionArray = true`在规则函数上设置属性来指定它。有关更多信息，请查看“使用规则”文档。

## 外部辅助模块

除了“使用规则”文档中提到的标准解析器之外，还有我们建议使用的stylelint中使用的其他外部模块。这些包括：

- [normalize-selector](https://github.com/getify/normalize-selector)：规范化 CSS 选择器。
- [postcss-resolve-nested-selector](https://github.com/davidtheclark/postcss-resolve-nested-selector)：给定 PostCSS AST 中的（嵌套）选择器，返回已解析选择器的数组。
- [style-search](https://github.com/davidtheclark/style-search)：搜索 CSS（和类似 CSS）字符串，对字符串，注释和函数内是否发生匹配很敏感。

看一下 [stylelint的内部工具](https://github.com/stylelint/stylelint/tree/master/lib/utils)，如果你在插件中遇到了一个，那么请考虑帮助我们将它解压缩到一个外部模块中。

## 测试插件

为了测试您的插件，您可以考虑使用stylelint内部使用的相同规则测试函数：[`stylelint-test-rule-tape`](https://github.com/stylelint/stylelint-test-rule-tape)。

## 插件包

要使单个模块提供多个规则，只需导出一个插件对象数组（而不是单个对象）。

## 共享插件和插件包

- `stylelint-plugin`在您的内容中使用关键字`package.json`。
- 插件发布后，请向我们发送一个Pull请求，将您的插件添加到列表中。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-09-03