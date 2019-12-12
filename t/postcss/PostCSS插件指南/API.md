# API

PostCSS插件是一个接收并通常从PostCSS解析器转换CSS AST的函数。

所有PostCSS插件都*必须*遵守以下规则。

另请参阅[ClojureWerkz](http://blog.clojurewerkz.org/blog/2013/04/20/how-to-make-your-open-source-project-really-awesome/)对开源项目[的建议](http://blog.clojurewerkz.org/blog/2013/04/20/how-to-make-your-open-source-project-really-awesome/)。

## 1. API

### 1.1清除带`postcss-`前缀的名称

只需阅读其名称即可清楚插件的用途。如果您为CSS 4 Custom Media编写了一个转换器，那`postcss-custom-media` 将是一个好名字。如果你写了一个插件来支持mixins，那`postcss-mixins`将是一个好名字。

前缀`postcss-`表明该插件是PostCSS生态系统的一部分。

对于可以作为独立工具运行的插件，此规则不是必需的，用户不必知道它是由PostCSS提供的 - 例如，[cssnext](http://cssnext.io/)和[Autoprefixer](https://github.com/postcss/autoprefixer)。

### 1.2 做一件事，做得好

不要创建多工具插件。捆绑到插件包中的几个小型单用插件通常是更好的解决方案。

例如，[cssnext](http://cssnext.io/)包含许多小插件，每个插件对应一个W3C规范。而[cssnano](https://github.com/ben-eb/cssnano)包含每个及其优化的一个单独的插件。

### 1.3 不要使用mixins

像Compass这样的预处理器库提供了一个带有mixins的API。

PostCSS插件是不同的。插件不能只是[postcss-mixin的](https://github.com/postcss/postcss-mixins)一组[mixins](https://github.com/postcss/postcss-mixins)。

要实现您的目标，请考虑转换有效的CSS或使用自定义的规则和自定义属性。

### 1.4 创建插件`postcss.plugin`

通过将函数包装在此方法中，您将挂钩到一个常见的插件API：

```javascript
module.exports = postcss.plugin('plugin-name', function (opts) {
    return function (root, result) {
        // Plugin code
    };
});
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-09-14