# Processors

处理器是社区包，它使stylelint能够在非样式表文件中输入CSS。例如，您可以`<style>`在HTML 中的标签内部标记CSS ，使用Markdown中的代码块或JavaScript中的字符串。

*处理器只能与CLI和Node API一起使用，而不能与PostCSS插件一起使用。*（PostCSS插件会忽略它们。）

[纠错](javascript:;)

- [stylelint-processor-arbitrary-tags](https://github.com/mapbox/stylelint-processor-arbitrary-tags)：用户指定标签内的Lint。
- [stylelint-processor-html](https://github.com/ccbikai/stylelint-processor-html)：HTML `<style>`标记内的Lint 。
- [stylelint-processor-markdown](https://github.com/mapbox/stylelint-processor-markdown)：Markdown的[围栏代码块中的](https://help.github.com/articles/creating-and-highlighting-code-blocks/) Lint 。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-08-30