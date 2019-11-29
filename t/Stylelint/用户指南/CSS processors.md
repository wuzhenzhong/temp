# CSS processors

linter支持当前和未来的CSS语法。这包括所有标准CSS以及使用标准CSS语法结构的特殊功能，例如特殊的规则，特殊属性和特殊功能。一些*类似* CSS的语言扩展 - 使用非标准语法结构的特性 - 因此受到支持; 然而，由于存在无限的处理可能性，因此linter不能支持所有内容。

您可以在css处理器之前或之后运行linter。根据您使用的处理器，每种方法都有警告：

1. *之前*：某些插件/处理器可能启用与linter不兼容的语法。
2. *之后*：某些插件/处理器可能会生成对您的linter配置无效的CSS，从而导致警告与原始样式表不对应。

*在这两种情况下，您都可以关闭不兼容的linter规则，或者停止使用不兼容的插件/处理器。*您还可以处理插件/处理器作者并请求替代格式化选项，以使其插件/处理器与stylelint兼容。

## 解析非标准语法

默认情况下，linter可以使用特殊的PostCSS解析器*解析*任何以下非标准语法：

- SCSS（使用[`postcss-scss`](https://github.com/postcss/postcss-scss)）
- LESS（使用[`postcss-less`](https://github.com/webschik/postcss-less)）
- SugarSS（使用[`sugarss`](https://github.com/postcss/sugarss)）

*非标准语法可以自动从下列文件扩展名推断出：* *.less，* *.scss，和* *.sss。*但是，如果您需要指定非标准语法，则[CLI](http://stylelint.cn/user-guide/cli/)和[Node API都会](http://stylelint.cn/user-guide/node-api/)公开一个`syntax`选项。

- 如果您使用的是CLI，请使用以下`syntax`标志：`stylelint ... --syntax scss`。
- 如果您正在使用Node API，请传递以下`syntax`选项：`stylelint.lint({ syntax: "sugarss", ... })`。

此外，使用CLI或Node API时，stylelint可以接受与[PostCSS兼容](https://github.com/postcss/postcss#syntaxes)的自定义[语法](https://github.com/postcss/postcss#syntaxes)。对于自定义语法，请分别使用`custom-syntax`和`customSyntax`选项。

- 如果您使用的是CLI，请使用如下`custom-syntax`标志：`stylelint ... --custom-syntax custom-syntax-module`或`stylelint ... --custom-syntax ./path/to/custom-syntax-module`。
- 如果您正在使用Node API，请传递以下`customSyntax`选项：`stylelint.lint({ customSyntax: path.join(process.cwd(), './path/to/custom-syntax-module') , ... })`。

如果你使用[linter](http://stylelint.cn/user-guide/postcss-plugin/)作为[PostCSS插件](http://stylelint.cn/user-guide/postcss-plugin/)，你需要使用特殊的解析器直接使用PostCSS的`syntax`选项，如下所示：

```javascript
var postcss = require("postcss")
var scss = require("postcss-scss")
// or use "postcss-less" or "sugarss"

postcss([
  require("stylelint"),
  require("reporter")
])
  .process(css, {
    from: "lib/app.css",
    syntax: scss
  })
})
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-08-30