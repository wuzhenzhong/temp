# 编写格式化程序 | Writing formatters

格式化程序是一个函数，它接受这些 stylelint 结果对象的数组并输出一个字符串：

[纠错](javascript:;)

```javascript
// A stylelint result object
{
  source:  "path/to/file.css", // The filepath or PostCSS identifier like <input css 1>
  errored: true, // This is `true` if at least one rule with an "error"-level severity triggered a warning
  warnings: [ // Array of rule violation warning objects, each like the following ...
    {
      line: 3,
      column: 12,
      rule: "block-no-empty",
      severity: "error",
      text: "You should not have an empty block (block-no-empty)"
    },
    ..
  ],
  deprecations: [ // Array of deprecation warning objects, each like the following ...
    {
      text: "Feature X has been deprecated and will be removed in the next major version.",
      reference: "http://stylelint.io/docs/feature-x.md"
    }
  ],
  invalidOptionWarnings: [ // Array of invalid option warning objects, each like the following ...
    {
      text: "Invalid option X for rule Y",
    }
  ],
  ignored: false // This is `true` if the file's path matches a provided ignore pattern
}
```

## `stylelint.formatters`

stylelint 的内部格式化程序是公开的`stylelint.formatters`。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-09-03