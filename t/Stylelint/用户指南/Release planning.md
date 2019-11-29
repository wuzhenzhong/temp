# 计划与版本发布 | Release planning

有时，即将发布的版本需要一些额外的计划（以及来自stylelint社区的帮助），以使过渡尽可能顺利。

## `8.0.0`

[纠错](javascript:;)

在`8.0.0`我们将删除：

- the `declaration-block-properties-order` rule.

该`declaration-block-properties-order`规则将被[`stylelint-order`](https://github.com/hudochenkov/stylelint-order)一个社区插件包订购规则所取代。一个包含在声明块中对属性，属性组和*其他内容*进行排序的规则。请考虑做出贡献，`stylelint-order`确保其稳健并处理您的特定用例。`8.0.0`一旦准备好，这将提供更平滑的过渡。

## `7.0.0`

在`7.0.0`我们将删除：

- 规则中的`emptyLineBefore`选项`declaration-block-properties-order`。
- 规则中的`hierarchicalSelectors`选项`indentation`。
- 在`-e`和`--extract`CLI标志和`extractStyleTagsFromHtml`节点API选项。

这是为了确保棉绒的开发保持可持续性。

`declaration-block-properties-order`顾名思义，该规则将仅检查声明块中属性的顺序。它不会关注声明之间的空格。因此，`emptyLineBefore`“组对象”配置功能中的选项，即：

```javascript
[
  {
    "emptyLineBefore": "always",
    "properties": [
      "height",
      "width",
    ],
  }, {
    "emptyLineBefore": "always",
    "properties": [
      "color",
      "font",
    ],
  }
]
```

将被删除`7.0.0`。这是社区开发一个更强大和开放式插件的机会，用于指定*块*的*结构*。还有机会将这样的插件与现有的块排序PostCSS插件对齐，例如[`postcss-sorting`](https://github.com/hudochenkov/postcss-sorting)，其支持指定嵌套规则的顺序和块内的规则。

该`indentation`规则仅检查块级缩进的更常见用例。因此，该`hierarchicalSelectors`选项将被删除。如果您使用该`hierarchicalSelectors`选项，请考虑为此特定代码样式创建插件并与社区共享。

的`-e`和`--extract`标志和`extractStyleTagsFromHtml`节点API选项将通过一个可扩展的处理器系统来代替。如果您当前使用这些标志或此选项从HTML文件中提取CSS代码，请考虑为社区[构建处理器](http://stylelint.cn/developer-guide/processors/)。

一切顺利，如果有需要，社区将创建这些插件和处理器，而stylelint团队专注于开发`7.0.0`。`7.0.0`一旦准备好，这将提供更平滑的过渡。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-08-30