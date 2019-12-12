# API

PostCSS runner是一个通过用户定义的插件列表处理CSS的工具; 例如，或  。这些规则对于任何此类参赛者都是强制性的。[`postcss-cli`](https://github.com/postcss/postcss-cli)[`gulp‑postcss`](https://github.com/w0rm/gulp-postcss)

对于单插件工具，例如，这些规则不是强制性的，但强烈建议使用。[`gulp-autoprefixer`](https://github.com/sindresorhus/gulp-autoprefixer)

另请参阅[ClojureWerkz](http://blog.clojurewerkz.org/blog/2013/04/20/how-to-make-your-open-source-project-really-awesome/)对开源项目[的建议](http://blog.clojurewerkz.org/blog/2013/04/20/how-to-make-your-open-source-project-really-awesome/)。

## 1. API

### 1.1 接受插件参数中的函数

如果您的跑步者使用配置文件，则必须使用JavaScript编写，以便它可以支持接受函数的插件，例如：[`postcss-assets`](https://github.com/borodean/postcss-assets)

```javascript
module.exports = [
    require('postcss-assets')({
        cachebuster: function (file) {
            return fs.statSync(file).mtime.getTime().toString(16);
        }
    })
];
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-09-14