# FAQ

## 如何禁用规则？

您可以通过将其配置值设置为禁用规则`null`。

例如，要在`stylelint-config-standard`没有`at-rule-empty-line-before`规则的情况下使用：

```javascript
{
  "extends": "stylelint-config-standard",
  "rules": {
    "at-rule-empty-line-before": null
  }
}
```

您还可以为CSS的特定部分禁用规则。有关更多信息，请参阅[配置指南](http://stylelint.cn/user-guide/configuration/#rules)的规则部分。

## 我如何从命令行lint？

请参阅文档的[CLI部分](http://stylelint.cn/user-guide/cli/)。

CLI也可以在[npm运行脚本](http://blog.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/)中使用，以使用stylelint的非全局安装。

## 我如何使用Git预提交钩子？

[lint-staged](https://github.com/okonet/lint-staged)是一个NodeJS脚本，支持对Git staged文件运行stylelint。

## 我如何使用我选择的任务跑步者？

stylelint社区为流行的任务运行者维护了[一些插件](http://stylelint.cn/user-guide/complementary-tools/#build-tool-plugins)，包括gulp，webpack，Broccoli和Grunt。请参阅他们的个人自述文件以开始使用。

如果没有为您选择的任务运行器提供专用的stylelint插件，您可以将stylelint用作PostCSS插件，并使用PostCSS的[众多](https://github.com/postcss/postcss#runners)任务运行器插件。

还有一些通过[文档中](http://stylelint.cn/user-guide/postcss-plugin/)的PostCSS JS API使用PostCSS插件的示例。

但是，使用stylelint作为PostCSS插件会将报告选项限制为[postcss-reporter](https://github.com/postcss/postcss-reporter/)。我们建议使用stylelint CLI或Node API来获得更好的报告。

## 我如何在文本编辑器中lint？

stylelint社区还为流行的编辑器维护了[一些插件](http://stylelint.cn/user-guide/complementary-tools/#editor-plugins)。请参阅他们的个人自述文件以开始使用。

## 我如何lint SCSS，Less或其他非标准语法？

stylelint可以默认*解析*任何以下非标准语法：SCSS，Less和SugarSS。非标准语法可以自动地从下面的文件扩展名推断出`.scss`，`.less`和`.sss`; 否则你可以自己指定语法。

此外，使用CLI或Node API时，stylelint可以接受任何与[PostCSS兼容的语法](https://github.com/postcss/postcss#syntaxes)。但请注意，stylelint无法保证核心规则可以使用上面列出的默认值以外的语法。

请参阅有关如何配置stylelint以解析非标准语法的[文档](http://stylelint.cn/user-guide/css-processors/#parsing-non-standard-syntax)。

## 我应该在通过PostCSS插件或其他处理器处理样式表之前或之后进行lint吗？

我们[建议](http://stylelint.cn/user-guide/css-processors/)在进行任何转换之前对源文件进行linting。

## 我如何在`<style>`标签中lint样式？

[创建处理器](http://stylelint.cn/developer-guide/processors/)或[使用现有](http://stylelint.cn/user-guide/configuration/#processors)[的处理器](http://stylelint.cn/developer-guide/processors/)从HTML `<style>`标签中提取CSS 并将其提供给stylelint。

## 如何自动修复样式警告？

[stylefmt](https://github.com/morishitter/stylefmt)支持stylelint配置文件，可以自动修复许多样式警告。

## 如何管理规则之间的冲突？

每条规则都是独立的，因此有时可以配置规则以使它们彼此冲突。例如，您可以启用两个冲突的黑名单和白名单规则，例如`unit-blacklist`和`unit-whitelist`。

作为配置作者，您有责任解决这些冲突。

## 插件和规则有什么区别？

规则必须符合[标准](http://stylelint.cn/developer-guide/rules/)规定了开发者指南中，包括被只适用于标准的CSS语法，并具有清晰明确的完成状态。而插件是由社区构建的规则或规则集，不符合标准。它可能支持特定的方法或工具集，或适用于*非标准*构造和功能，或适用于特定用例。

例如，我们发现强制执行属性订单，属性分组等的规则更适合作为插件，因为有很多不同的意见关于要执行什么，以及如何执行。当您编写或使用插件时，您可以确保强制执行您自己的特定首选项; 但试图解决太多不同用例的规则变得一团糟。

插件很容易合并到您的配置中。

## 我可以自定义stylelint的消息吗？

是的，您可以使用[`message`辅助选项](http://stylelint.cn/user-guide/configuration/#custom-messages)或[编写自己的格式化程序](http://stylelint.cn/developer-guide/formatters/)。

## 我应该如何使用类似BEM的方法来保护我的CSS？

使用[stylelint-selector-bem-pattern](https://github.com/davidtheclark/stylelint-selector-bem-pattern)插件可确保选择器符合所选的BEM风格模式。

您还可以利用`selector-*`规则禁止某些类型的选择器（例如id选择器）和控制特异性。

如果您正在使用SUITCSS，则可能需要使用[其可共享配置](https://github.com/suitcss/stylelint-config-suitcss)。

## 如何配置`*-pattern`kebab-case等常见CSS命名约定的规则？

使用与您选择的约定相对应的正则表达式：

- kebab-case: `^([a-z][a-z0-9]*)(-[a-z0-9]+)*$`
- lowerCamelCase: `^[a-z][a-zA-Z0-9]+$`
- snake_case: `^([a-z][a-z0-9]*)(_[a-z0-9]+)*$`
- UpperCamelCase: `^[A-Z][a-zA-Z0-9]+$`

例如，对于lowerCamelCase类选择器，请使用`"selector-class-pattern": "^[a-z][a-zA-Z0-9]+$"`。

所有这些模式都不允许以数字，两个连字符或连字符后跟数字开头的CSS标识符。

## 如何将默认严重性更改为“警告”，以便stylelint不会破坏我的构建？

使用[`defaultSeverity`](http://stylelint.cn/user-guide/configuration/#defaultseverity)配置选项。

## 我可以在npm包中捆绑多个可共享配置吗？

用户可以使用`require()`npm包中的任何文件，因此您需要做的就是记录哪些路径指向配置（例如`require('my-package/config-2')`）。

## 如何在块的开放支撑后控制空白？

请参阅解释规则如何工作的文档的[此](http://stylelint.cn/user-guide/about-rules/#-empty-line-before-and--max-empty-lines-rules)部分`*-empty-line-before`。

## 如果我`extends`在配置对象中使用，是否会合并或覆盖每个规则的选项？

他们将被覆盖。

该`extends`机制[在配置文档中详细说明](http://stylelint.cn/user-guide/configuration/#extends)：

> 当一个配置扩展另一个配置时，它从另一个配置属性开始，然后添加并覆盖其中的内容。

这种设计的原因记录在[＃1646中](https://github.com/stylelint/stylelint/issues/1646#issuecomment-232779957)。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-08-30