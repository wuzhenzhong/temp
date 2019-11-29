# 编写处理器 | Writing processors

处理器是挂钩到 stylelint 管道的函数，将代码修改为 stylelint 并在出路时修改结果。

处理器只能与 CLI 和 Node API 一起使用，而不能与 PostCSS 插件一起使用。

处理器模块是接受选项对象并返回具有以下函数的对象的函数，这些函数挂钩到每个文件的处理中：

- **code**：接受两个参数的函数，即文件的代码和文件的路径，并将 stylelint 的字符串返回给 lint。
- **结果**：一个接受两个参数的函数，即文件的 stylelint 结果对象和文件的路径，并且要么改变结果对象（不返回任何参数），要么返回一个新参数。

```javascript
// my-processor.js
module.exports = function(options) {
  return {
    code: function(input, filepath) {
      // ...
      return transformedCode;
    },
    result: function(stylelintResult, filepath) {
      // ...
      return transformedResult;
    }
  };
}
```

处理器可以启用 stylelint 来在非样式表文件中 lint CSS。例如，假设你想`<style>`在 HTML 中的标签内 lint CSS 。如果你只是为你的HTML 代码提供 stylelint，你会遇到问题，因为 PostCSS 不会解析HTML 。相反，您可以创建执行以下操作的处理器：

- 在`code`处理器功能中，从`<style>`HTML代码中的标签中提取CSS 。返回一个 CSS 字符串，其中包含所有提取的 CSS，这是stylelint 将检查的内容。
- 在执行提取时构建源图，因此可以定制警告位置以匹配原始源HTML 。
- 在`result`处理器功能中，使用源图修改每个警告的行/列位置。

处理器选项必须是 JSON 友好的，因为用户需要将它们包含在`.stylelintrc`文件中。

## 共享处理器

- `stylelint-processor`在您的内容中使用关键字`package.json`。
- 处理器发布后，请向我们发送拉取请求，将您的处理器添加到列表中。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-09-03