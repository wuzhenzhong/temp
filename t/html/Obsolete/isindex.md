# isindex

**已废弃**



该特性已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用该特性。



`**<isindex>**`**元素**的作用是使浏览器显示一个对话框，提示用户输入单行文本。在W3C的规范中建议，`<isindex>`元素最好被放置在[``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/head)标签块内，但是对于浏览器来说，<isindex>标签在页面任何位置都没有关系。



从HTML4规范开始，就不建议在您编写的HTML文档内使用<isindex>元素，您可以用<input>标签来替代。



## 属性

本元素支持 [全局属性](https://developer.mozilla.org/en-US/docs/HTML/Global_attributes).



**prompt**该属性用作在输入框前添加一个输入提示文本。

**action**本属性可以规定文本框中的值发送向一个没有被W3C规范的URL。



## 示例

```javascript
<head>
  <isindex prompt="Search Document... " />
</head>
```

## 历史

1992年6月，Dan Connolly [宁愿](http://1997.webhistory.org/www.lists/www-talk.1992/0080.html)使用不同的[锚](https://developer.mozilla.org/wiki/HTML/Elements/a)类型而不是isindex。

1992年11月，丹·康诺利（Dan Connolly）发表了[链接索引而不是文件](http://lists.w3.org/Archives/Public/www-talk/1992NovDec/thread.html#31)，索引链接比文档链接要多。在这个线程中，提出了不同类型的解决方案。参考Dynatext浏览器[提到](http://lists.w3.org/Archives/Public/www-talk/1992NovDec/0039.html)了查询表单的问题：“浏览器显示切换按钮，文本字段等。用户填写字段，单击确定，查询结果出现在目录窗口中。

在1992年11月，关于[isindex的](http://lists.w3.org/Archives/Public/www-talk/1992NovDec/thread.html#42)一个主题，Kevin Hoadley [质疑](http://lists.w3.org/Archives/Public/www-talk/1992NovDec/0042.html)是否需要isindex元素，并建议放弃它。他建议有一个[输入](https://developer.mozilla.org/wiki/HTML/Elements/input)元素（Steve Putz [支持](http://lists.w3.org/Archives/Public/www-talk/1992NovDec/0053.html)的想法）。Tim Berners-Lee [解释](http://lists.w3.org/Archives/Public/www-talk/1992NovDec/0044.html)了isindex的目的是导致搜索结果汇总。凯文[回答说](http://lists.w3.org/Archives/Public/www-talk/1992NovDec/0048.html)，他不喜欢isindex的布尔性质，并希望一个系统，一切都可搜索，并建议扩展当前的WWW框架与特定的httpd配置，并定义一些URI映射创建搜索查询。

在2016年，有人建议[从规范中](https://github.com/w3c/html/issues/240)[删除`isindex`](https://github.com/w3c/html/issues/240)。

## HTML参考

- [HTML5](http://www.w3.org/TR/html5)将其归类为[不符合要求的功能](http://www.w3.org/TR/html5/obsolete.html#obsolete)。

- [ISINDEX](http://www.w3.org/TR/html401/interact/forms.html#h-17.8)元素在[HTML 4.01中](http://www.w3.org/TR/html401/)已弃用

- [ISINDEX](http://www.w3.org/TR/REC-html32#isindex)在[HTML 3.2中](http://www.w3.org/TR/REC-html32)

- [ISINDEX](http://www.w3.org/MarkUp/html-spec/html-spec_5.html#SEC5.2.3)在[HTML 2.0](http://www.w3.org/MarkUp/html-spec/html-spec_5.html)以及行为的在说明书中[查询和索引](http://www.w3.org/MarkUp/html-spec/html-spec_7.html#SEC7.5)（HTML 2.0）

- [ISINDEX](http://www.w3.org/MarkUp/HTMLPlus/htmlplus_51.html)在[HTML +](http://www.w3.org/MarkUp/HTMLPlus/htmlplus_1.html)

## 浏览器兼容性

| Feature       | Chrome | Edge | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :--- | :------ | :---------------- | :---- | :----- |
| Basic Support | No     | No   | No      | No                | No    | No     |
| action        | No     | No   | No      | No                | No    | No     |
| prompt        | No     | No   | No      | No                | No    | No     |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | No      | No                 | No          | No                  | No        | No            | No         |
| action        | No      | No                 | No          | No                  | No        | No            | No         |
| prompt        | No      | No                 | No          | No                  | No        | No            | No         |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18