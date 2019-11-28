# Accessibility

## 为什么可达性？

Web可访问性（也称为[**a11y**](https://en.wiktionary.org/wiki/a11y)）是设计和创建每个人都可以使用的网站。辅助功能支持对于允许辅助技术解释网页是必需的。

React完全支持构建可访问的网站，通常使用标准的HTML技术。

## 标准和准则

### WCAG

[网页内容无障碍指南](https://www.w3.org/WAI/intro/wcag)提供了创建可访问网站的指导方针。

以下WCAG清单提供了一个概述：

- [来自Wuhcag的WCAG清单](https://www.wuhcag.com/wcag-checklist/)

- [来自WebAIM的WCAG清单](http://webaim.org/standards/wcag/checklist)

- [A11Y项目的清单](http://a11yproject.com/checklist.html)

### WAI-ARIA

[网页倡议-无障碍富互联网应用程序](https://www.w3.org/WAI/intro/aria)文档中包含用于构建完全访问的JavaScript控件技术。

请注意，`aria-*`JSX完全支持所有HTML属性。尽管React中的大多数DOM属性和属性都是camelCased的，但这些属性应该是小写的：

```javascript
<input
  type="text" 
  aria-label={labelText}
  aria-required="true"
  onChange={onchangeHandler}
  value={inputValue}
  name="name"
/>
```

## 可访问表单

### Labeling

每个HTML表单控件（例如`<input>`和`<textarea>`）都需要被标记为可访问。我们需要提供也向屏幕阅读器公开的描述性标签。

以下资源向我们展示了如何执行此操作：

- [W3C向我们展示了如何标注元素](https://www.w3.org/WAI/tutorials/forms/labels/)

- [WebAIM向我们展示了如何给元素添加标签](http://webaim.org/techniques/forms/controls)

- [Paciello集团解释可访问的名称](https://www.paciellogroup.com/blog/2017/04/what-is-an-accessible-name/)

虽然这些标准的HTML实践可以直接在React中使用，但请注意，该`for`属性是按照`htmlFor`JSX 编写的：

```javascript
<label htmlFor="namedInput">Name:</label>
<input id="namedInput" type="text" name="name"/>
```

### 通知用户错误

所有用户都需要理解错误情况。以下链接向我们展示了如何将错误文本展示给屏幕阅读器：

- [W3C演示用户通知](https://www.w3.org/WAI/tutorials/forms/notifications/)

- [WebAIM查看表单验证](http://webaim.org/techniques/formvalidation/)

## 焦点控制

确保您的Web应用程序只能通过键盘完全操作：

- [WebAIM讨论键盘辅助功能](http://webaim.org/techniques/keyboard/)键盘焦点和焦点outlineKeyboard焦点是指DOM中的当前元素，该元素被选为接受来自键盘的输入。我们将它视为与以下图像中显示的焦点轮廓类似的焦点轮廓：

![img](https://ask.qcloudimg.com/http-save/devdocs/z5onx40u5e.png)

仅使用可删除此轮廓的CSS，例如通过设置`outline: 0`，如果您将其替换为另一个焦点轮廓实现。跳过所需内容的机制提供机制允许用户跳过应用程序中的导航部分，因为这有助于加速键盘导航。跳过链接或跳过导航链接是隐藏的导航链接，只有当键盘用户与网页进行交互时才会显示链接。使用内部页面锚点和一些样式，它们非常容易实现：

- [WebAIM - 跳过导航链接](http://webaim.org/techniques/skipnav/)

同样，使用地标元素和角色（例如`<main>`和`<aside>`）来划分页面区域，因为辅助技术允许用户快速导航到这些部分。

阅读更多关于使用这些元素来增强可访问性的信息：

- [Deque University - HTML 5和ARIA地标以](https://dequeuniversity.com/assets/html/jquery-summit/html5/slides/landmarks.html)编程方式管理焦点我们的React应用程序在运行时不断修改HTML DOM，有时会导致键盘焦点丢失或设置为意外元素。为了修复这个问题，我们需要以正确的方向以编程方式推动键盘焦点。例如，通过将键盘焦点重置为在该模式窗口关闭之后打开模式窗口的按钮.Mozilla开发人员网络将查看此内容并描述我们如何构建[键盘可导航的JavaScript小部件](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Keyboard-navigable_JavaScript_widgets)。为了在React中设置焦点，我们可以使用Refs到组件。使用这个，我们首先在组件类的JSX中创建一个ref元素：render（）{//使用`ref`回调来存储引用文本在实例字段中输入DOM //元素（例如，this.textInput）。return（<input type =“text”ref = {（input）=> {this.textInput = input;}} />）; }然后，我们可以在需要时将其聚焦在我们的组件的其他地方：focus（）{//使用原始DOM API显式地聚焦文本输入this.textInput.focus（）; }一个很好的焦点管理例子就是[反应式分析模式](https://github.com/davidtheclark/react-aria-modal)。这是一个相当罕见的完全无障碍模式窗口的例子。它不仅将初始焦点设置为取消按钮（防止键盘用户意外激活成功操作）并将模式中的键盘焦点陷入困境，还将焦点重置回最初触发模式的元素。注意：虽然这是非常重要的辅助功能，但它也是一种应该谨慎使用的技术。用它修复键盘焦点流时，不要试图预测用户如何使用应用程序。更复杂的小部件更复杂的用户体验不应该意味着更难访问。尽管通过尽可能接近HTML进行编码最容易实现可访问性，但即使是最复杂的小部件也可以被无缝地编码。我们需要知识[ARIA角色](https://www.w3.org/TR/wai-aria/roles)以及[ARIA国家和地产](https://www.w3.org/TR/wai-aria/states_and_properties)。这些工具箱充满了JSX完全支持的HTML属性，使我们能够构建完全可访问的，功能强大的React组件。每种类型的小部件都有一个特定的设计模式，并且希望用户和用户代理以某种方式运行：

- [WAI-ARIA创作实践 - 设计模式和小工具](https://www.w3.org/TR/wai-aria-practices/#aria_ex)

- [Heydon Pickering - ARIA示例](http://heydonworks.com/practical_aria_examples/)

- [包容性组件](https://inclusive-components.design/)

## 其他要点

### 设置语言

当屏幕阅读器软件使用它来选择正确的语音设置时，请指明页面文本的人类语言：

- [WebAIM - 文档语言](http://webaim.org/techniques/screenreader/#language)设置文档标题设置文档`<title>`以正确描述当前页面内容，因为这可以确保用户始终了解当前页面上下文：

- [WCAG - 理解文档标题要求](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-title.html)

我们可以使用[React Document Title组件](https://github.com/gaearon/react-document-title)在React中进行设置。

### 色彩对比

确保您网站上的所有可读文本都具有足够的色彩对比度，以保持低视力用户的最大可读性：

- [WCAG - 理解色彩对比要求](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-contrast.html)

- [关于色彩对比的一切以及为什么你应该重新思考它](https://www.smashingmagazine.com/2014/10/color-contrast-tips-and-tools-for-accessibility/)

- [A11yProject - 什么是色彩对比](http://a11yproject.com/posts/what-is-color-contrast/)

手动计算网站中所有案例的适当颜色组合可能非常繁琐，因此您可以[使用Colorable](http://jxnblk.com/colorable/)来[计算整个可访问的调色板](http://jxnblk.com/colorable/)。

下面提到的ax和WAVE工具都包括颜色对比测试，并将报告对比度误差。

如果你想扩展你的对比测试能力，你可以使用这些工具：

- [WebAIM - 颜色对比检查器](http://webaim.org/resources/contrastchecker/)

- [Paciello Group - 色彩对比分析仪](https://www.paciellogroup.com/resources/contrastanalyser/)

## 开发和测试工具

我们可以使用许多工具来协助创建可访问的Web应用程序。

### 键盘

到目前为止，最简单也是最重要的检查之一是测试整个网站是否可以与键盘一起使用和使用。做到这一点：

1. 拔出鼠标。

\2. 使用`Tab`和`Shift+Tab`浏览。

\3. 使用`Enter`激活的元素。

\4. 根据需要，使用键盘上的箭头键与某些元素（如菜单和下拉菜单）进行交互。

### 发展援助

我们可以直接在我们的JSX代码中检查一些辅助功能。智能感知IDE通常会为ARIA角色，状态和属性提供智能感知检查。我们也可以访问以下工具：

#### eslint-plugin-jsx-a11y

[eslint-插件- JSX-A11Y](https://github.com/evcohen/eslint-plugin-jsx-a11y)为ESLint插件提供了关于您的JSX访问性问题，AST掉毛反馈。许多IDE允许您将这些发现直接集成到代码分析和源代码窗口中。

[创建React App](https://github.com/facebookincubator/create-react-app)包含此插件并激活了一部分规则。如果您希望启用更多可访问性规则，则可以`.eslintrc`使用以下内容在项目的根目录中创建一个文件：

```javascript
{
  "extends": ["react-app", "plugin:jsx-a11y/recommended"],
  "plugins": ["jsx-a11y"]
}
```

### 测试浏览器的可访问性

有许多工具可以在浏览器的网页上运行可访问性审计。请将它们与其他可访问性检查结合使用，因为它们只能测试HTML的技术可访问性。

#### aXe, aXe-core and react-axe

Deque Systems为您的应用程序的自动化和端对端可访问性测试提供了[ax-core](https://www.deque.com/products/axe-core/)。该模块包含Selenium的集成。

[Accessibility Engine](https://www.deque.com/products/axe/)或ax，是构建于其上的可访问性检查器浏览器扩展`aXe-core`。

您还可以使用[react-ax](https://github.com/dylanb/react-axe)模块在开发和调试时直接向控制台报告这些可访问性结果。

#### WebAIM WAVE

[网站可访问性评估工具](http://wave.webaim.org/extension/)是另一个可访问浏览器扩展。

#### 辅助功能检查员和辅助功能树

[辅助功能树](https://www.paciellogroup.com/blog/2015/01/the-browser-accessibility-tree/)是DOM树的子集，其中包含每个DOM元素的可访问对象，这些元素应该接触辅助技术，例如屏幕阅读器。

在某些浏览器中，我们可以轻松查看辅助功能树中每个元素的辅助功能信息：

- [在Chrome中激活辅助功能检查器](https://gist.github.com/marcysutton/0a42f815878c159517a55e6652e3b23a)

- [在OS X Safari中使用辅助功能检查器](https://developer.apple.com/library/content/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html)

### 屏幕阅读器

使用屏幕阅读器进行测试应构成您的辅助功能测试的一部分

请注意，浏览器/屏幕阅读器组合很重要。建议您在最适合您选择的屏幕阅读器的浏览器中测试您的应用程序。

#### Firefox中的NVDA

[NonVisual Desktop Access](https://www.nvaccess.org/)或NVDA是一款广泛使用的开源Windows屏幕阅读器。

请参阅以下有关如何最佳使用NVDA的指南：

- [WebAIM - 使用NVDA评估Web可访问性](http://webaim.org/articles/nvda/)

- [Deque - NVDA键盘快捷键](https://dequeuniversity.com/screenreaders/nvda-keyboard-shortcuts)

#### Safari中的VoiceOver

VoiceOver是Apple设备上的集成屏幕阅读器。

有关如何激活和使用VoiceOver，请参阅以下指南：

- [WebAIM - 使用VoiceOver评估Web可访问性](http://webaim.org/articles/voiceover/)

- [Deque - 适用于OS X键盘快捷键的VoiceOver](https://dequeuniversity.com/screenreaders/voiceover-keyboard-shortcuts)

- [Deque - 适用于iOS快捷键的VoiceOver](https://dequeuniversity.com/screenreaders/voiceover-ios-shortcuts)

#### JAWS在Internet Explorer中

[使用Speech](http://www.freedomscientific.com/Products/Blindness/JAWS)或JAWS进行[工作访问](http://www.freedomscientific.com/Products/Blindness/JAWS)是Windows上一种多用途的屏幕阅读器。

请参阅以下关于如何最佳使用JAWS的指南：

- [WebAIM - 使用JAWS评估Web可访问性](http://webaim.org/articles/jaws/)

- [Deque - JAWS键盘快捷键](https://dequeuniversity.com/screenreaders/jaws-keyboard-shortcuts)

```js
 © 2013–2017 Facebook Inc.
```

Licensed under the Creative Commons Attribution 4.0 International Public License.

https://reactjs.org/docs/accessibility.html

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18