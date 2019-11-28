# DOM Elements

- 贡献者1人

  

React 实现了一套与浏览器无关的 DOM 系统，兼顾了性能和跨浏览器的兼容性。借此机会，我们清理了浏览器 DOM 实现中一些不一致的问题。

在React中，所有DOM属性和属性（包括事件处理程序）都应该是camelCased的。例如，HTML属性`tabindex`对应`tabIndex`于React中的属性。唯一的例外是`aria-*`和`data-*`属性，应小写。例如，你可以继续`aria-label`作为`aria-label`。

## 属性差异

React和HTML之间有许多不同的属性：

### checked

该`checked`属性由`<input>`类型`checkbox`或的组件支持`radio`。您可以使用它来设置组件是否被选中。这对于构建受控组件非常有用。`defaultChecked`是不受控制的等价物，用于设置组件是否在第一次安装时进行检查。

### className

要指定一个CSS类，请使用该`className`属性。这适用于所有常规的DOM和SVG元素一样`<div>`，`<a>`和其他。

如果您使用Web组件的反应（这是不常见的），请改用该`class`属性。

### dangerouslySetInnerHTML

`dangerouslySetInnerHTML`是React `innerHTML`在浏览器DOM中使用的替代品。一般来说，从代码中设置HTML是有风险的，因为很容易让您的用户无意中发现[跨站脚本](https://en.wikipedia.org/wiki/Cross-site_scripting)攻击[（XSS）](https://en.wikipedia.org/wiki/Cross-site_scripting)。因此，您可以直接从React中设置HTML，但您必须输入`dangerouslySetInnerHTML`并使用`__html`密钥传递对象，以提醒自己危险。例如：

```javascript
function createMarkup() {
  return {__html: 'First &middot; Second'};
}

function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarkup()} />;
}
```

### htmlFor

既然`for`是JavaScript中的保留字，React元素就会用到`htmlFor`。

### onChange

`onChange`事件的行为与您所期望的一样：每当表单字段发生更改时，将触发此事件。我们故意不使用现有的浏览器行为，因为`onChange`它的行为不当，React依靠此事件来实时处理用户输入。

### selected

`selected`属性由`<option>`组件支持。您可以使用它来设置组件是否被选中。这对于构建受控组件非常有用。

### style

`style`属性接受带有camelCased属性的JavaScript对象而不是CSS字符串。这与DOM `style`JavaScript属性一致，效率更高，并可防止XSS安全漏洞。例如：

```javascript
const divStyle = {
  color: 'blue',
  backgroundImage: 'url(' + imgUrl + ')',
};

function HelloWorldComponent() {
  return <div style={divStyle}>Hello World!</div>;
}
```

请注意，样式不是自动复制的。要支持旧版浏览器，您需要提供相应的样式属性：

```javascript
const divStyle = {
  WebkitTransition: 'all', // note the capital 'W' here
  msTransition: 'all' // 'ms' is the only lowercase vendor prefix
};

function ComponentWithTransition() {
  return <div style={divStyle}>This should work cross-browser</div>;
}
```

样式键是驼峰式的，以便与从JS（例如`node.style.backgroundImage`）访问DOM节点上的属性一致。[除`ms`](http://www.andismith.com/blog/2012/02/modernizr-prefixed/)大写字母[之外的](http://www.andismith.com/blog/2012/02/modernizr-prefixed/)供应商前缀应以大写字母开头。这就是为什么`WebkitTransition`有一个大写“W”。

React会自动将“px”后缀附加到某些内联样式属性。例如：

```javascript
// This:
<div style={{ height: 10 }}>
  Hello World!
</div>;

// Becomes:
<div style="height: 10px;">
  Hello World!
</div>
```

不是所有的样式属性都转换为像素字符串。某些那些仍然无单位（例如`zoom`，`order`，`flex`）。无单位属性的完整列表可以在[这里](https://github.com/facebook/react/blob/4131af3e4bf52f3a003537ec95a1655147c81270/src/renderers/dom/shared/CSSProperty.js#L15-L59)看到。

### suppressContentEditableWarning

通常情况下，当有孩子的元素也被标记为时会有警告`contentEditable`，因为它不起作用。该属性会抑制该警告。除非你正在构建一个像手动管理的[Draft.js](https://facebook.github.io/draft-js/)这样的库，否则不要使用它`contentEditable`。

### value

`value`属性由`<input>`和`<textarea>`组件支持。您可以使用它来设置组件的值。这对于构建受控组件非常有用。`defaultValue`是不受控制的等价物，用于设置组件首次安装时的值。

## 所有支持的HTML属性

截至React 16，完全支持任何标准[或自定义](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html) DOM属性。

React一直为DOM提供以JavaScript为中心的API。由于React组件经常采用自定义和DOM相关的道具，因此React `camelCase`像使用DOM API一样使用约定：

```javascript
<div tabIndex="-1" />      // Just like node.tabIndex DOM API
<div className="Button" /> // Just like node.className DOM API
<input readOnly={true} />  // Just like node.readOnly DOM API
```

这些道具的工作方式与相应的HTML属性类似，但上面记录的特例除外。

React支持的一些DOM属性包括：

```javascript
accept acceptCharset accessKey action allowFullScreen allowTransparency alt
async autoComplete autoFocus autoPlay capture cellPadding cellSpacing challenge
charSet checked cite classID className colSpan cols content contentEditable
contextMenu controls controlsList coords crossOrigin data dateTime default defer
dir disabled download draggable encType form formAction formEncType formMethod
formNoValidate formTarget frameBorder headers height hidden high href hrefLang
htmlFor httpEquiv icon id inputMode integrity is keyParams keyType kind label
lang list loop low manifest marginHeight marginWidth max maxLength media
mediaGroup method min minLength multiple muted name noValidate nonce open
optimum pattern placeholder poster preload profile radioGroup readOnly rel
required reversed role rowSpan rows sandbox scope scoped scrolling seamless
selected shape size sizes span spellCheck src srcDoc srcLang srcSet start step
style summary tabIndex target title type useMap value width wmode wrap
```

同样，所有SVG属性都得到完全支持：

```javascript
accentHeight accumulate additive alignmentBaseline allowReorder alphabetic
amplitude arabicForm ascent attributeName attributeType autoReverse azimuth
baseFrequency baseProfile baselineShift bbox begin bias by calcMode capHeight
clip clipPath clipPathUnits clipRule colorInterpolation
colorInterpolationFilters colorProfile colorRendering contentScriptType
contentStyleType cursor cx cy d decelerate descent diffuseConstant direction
display divisor dominantBaseline dur dx dy edgeMode elevation enableBackground
end exponent externalResourcesRequired fill fillOpacity fillRule filter
filterRes filterUnits floodColor floodOpacity focusable fontFamily fontSize
fontSizeAdjust fontStretch fontStyle fontVariant fontWeight format from fx fy
g1 g2 glyphName glyphOrientationHorizontal glyphOrientationVertical glyphRef
gradientTransform gradientUnits hanging horizAdvX horizOriginX ideographic
imageRendering in in2 intercept k k1 k2 k3 k4 kernelMatrix kernelUnitLength
kerning keyPoints keySplines keyTimes lengthAdjust letterSpacing lightingColor
limitingConeAngle local markerEnd markerHeight markerMid markerStart
markerUnits markerWidth mask maskContentUnits maskUnits mathematical mode
numOctaves offset opacity operator order orient orientation origin overflow
overlinePosition overlineThickness paintOrder panose1 pathLength
patternContentUnits patternTransform patternUnits pointerEvents points
pointsAtX pointsAtY pointsAtZ preserveAlpha preserveAspectRatio primitiveUnits
r radius refX refY renderingIntent repeatCount repeatDur requiredExtensions
requiredFeatures restart result rotate rx ry scale seed shapeRendering slope
spacing specularConstant specularExponent speed spreadMethod startOffset
stdDeviation stemh stemv stitchTiles stopColor stopOpacity
strikethroughPosition strikethroughThickness string stroke strokeDasharray
strokeDashoffset strokeLinecap strokeLinejoin strokeMiterlimit strokeOpacity
strokeWidth surfaceScale systemLanguage tableValues targetX targetY textAnchor
textDecoration textLength textRendering to transform u1 u2 underlinePosition
underlineThickness unicode unicodeBidi unicodeRange unitsPerEm vAlphabetic
vHanging vIdeographic vMathematical values vectorEffect version vertAdvY
vertOriginX vertOriginY viewBox viewTarget visibility widths wordSpacing
writingMode x x1 x2 xChannelSelector xHeight xlinkActuate xlinkArcrole
xlinkHref xlinkRole xlinkShow xlinkTitle xlinkType xmlns xmlnsXlink xmlBase
xmlLang xmlSpace y y1 y2 yChannelSelector z zoomAndPan
```

只要它们完全小写，您也可以使用自定义属性。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18