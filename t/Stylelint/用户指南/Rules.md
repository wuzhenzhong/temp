# 规则 | Rules



规则确定了linter寻找和抱怨的内容。默认情况下，所有规则默认是关闭的，而且没有默认选项。这些规则遵循统一的命名惯例，协同配合，你可以在[关于规则](http://stylelint.cn/user-guide/about-rules/)中了解更多相关信息。

除了这些规则，还有[插件](http://stylelint.cn/user-guide/plugins/)，它们是社区支持的方法、工具集，非标准的 CSS 特性，或特定的用例。查看[插件](http://stylelint.cn/user-guide/plugins/)了解更多。

在下文中带有的图标的规则可以使用 [stylefmt](https://github.com/morishitter/stylefmt) 进行自动修复。

## 规则清单

以下是 stylelint 的所有规则，并参照[css vocabulary](http://apps.workflower.fi/vocabs/css/en)进行分类。

### 颜色

- [`color-hex-case`](http://stylelint.cn/user-guide/rules/color-hex-case/): 指定十六进制颜色大小写 。
- [`color-hex-length`](http://stylelint.cn/user-guide/rules/color-hex-length/): 指定十六进制颜色是否使用缩写 。
- [`color-named`](http://stylelint.cn/user-guide/rules/color-named/): 要求 (可能的情况下) 或 禁止使用命名的颜色。
- [`color-no-hex`](http://stylelint.cn/user-guide/rules/color-no-hex/): 禁止使用十六进制颜色。
- [`color-no-invalid-hex`](http://stylelint.cn/user-guide/rules/color-no-invalid-hex/): 禁止使用无效的十六进制颜色。

### 字体系列

- [`font-family-name-quotes`](http://stylelint.cn/user-guide/rules/font-family-name-quotes/)：指定字体名称是否需要使用引号引起来。
- [`font-family-no-duplicate-names`](http://stylelint.cn/user-guide/rules/font-family-no-duplicate-names/): 禁止使用重复的字体名称。

### 字体重量

- [`font-weight-notation`](http://stylelint.cn/user-guide/rules/font-weight-notation/)：要求使用数字或命名的 (可能的情况下) `font-weight` 值。

### 功能

- [`function-blacklist`](http://stylelint.cn/user-guide/rules/function-blacklist/)：指定一个禁用的函数的黑名单。
- [`function-calc-no-unspaced-operator`](http://stylelint.cn/user-guide/rules/function-calc-no-unspaced-operator/)：禁止在 `calc` 函数内使用不加空格的操作符。
- [`function-comma-newline-after`](http://stylelint.cn/user-guide/rules/function-comma-newline-after/)：在函数的逗号之后要求有一个换行符或禁止有空白。
- [`function-comma-newline-before`](http://stylelint.cn/user-guide/rules/function-comma-newline-before/)：在函数的逗号之前要求有一个换行符或禁止有空白。
- [`function-comma-space-after`](http://stylelint.cn/user-guide/rules/function-comma-space-after/)：在函数的逗号之后要求有一个空格或禁止有空白。
- [`function-comma-space-before`](http://stylelint.cn/user-guide/rules/function-comma-space-before/)：在函数的逗号之前要求有一个空格或禁止有空白。
- [`function-linear-gradient-no-nonstandard-direction`](http://stylelint.cn/user-guide/rules/function-linear-gradient-no-nonstandard-direction/)：根据[标准语法](https://developer.mozilla.org/en-US/docs/Web/CSS/linear-gradient#Syntax)，禁止 `linear-gradient()` 中无效的方向值。
- [`function-max-empty-lines`](http://stylelint.cn/user-guide/rules/function-max-empty-lines/)：限制函数中相邻的空行数量。
- [`function-name-case`](http://stylelint.cn/user-guide/rules/function-name-case/)：指定函数名称的大小写。
- [`function-parentheses-newline-inside`](http://stylelint.cn/user-guide/rules/function-parentheses-newline-inside/)：在函数的括号内要求有一个换行符或禁止有空白。
- [`function-parentheses-space-inside`](http://stylelint.cn/user-guide/rules/function-parentheses-space-inside/)：在函数的括号内要有一个空格或禁止有空白。
- [`function-url-data-uris`](http://stylelint.cn/user-guide/rules/function-url-data-uris/)：要求或禁止在 url 中使用 data URI。
- [`function-url-no-scheme-relative`](http://stylelint.cn/user-guide/rules/function-url-no-scheme-relative/)：禁止使用相对协议的链接。
- [`function-url-quotes`](http://stylelint.cn/user-guide/rules/function-url-quotes/)：要求或禁止 url 使用引号。
- [`function-url-scheme-whitelist`](http://stylelint.cn/user-guide/rules/function-url-scheme-whitelist/)：指定一个允许的 url 协议的白名单。
- [`function-whitelist`](http://stylelint.cn/user-guide/rules/function-whitelist/)：指定一个允许的函数的白名单。
- [`function-whitespace-after`](http://stylelint.cn/user-guide/rules/function-whitespace-after/)：要求或禁止在函数之后有空白。

### 数

- [`number-leading-zero`](http://stylelint.cn/user-guide/rules/number-leading-zero/)：要求或禁止小于 1 的小数的前导 0 。
- [`number-max-precision`](http://stylelint.cn/user-guide/rules/number-max-precision/)：限制小数位数。
- [`number-no-trailing-zeros`](http://stylelint.cn/user-guide/rules/number-no-trailing-zeros/)：禁止数字中的拖尾 0 。

### 字符串

- [`string-no-newline`](http://stylelint.cn/user-guide/rules/string-no-newline/)：禁止在字符串中使用（非转义的）换行符。
- [`string-quotes`](http://stylelint.cn/user-guide/rules/string-quotes/)：指定字符串使用单引号还是双引号 。

### 长度

- [`length-zero-no-unit`](http://stylelint.cn/user-guide/rules/length-zero-no-unit/): 长度为0时，禁止使用单位 。

### 时间

- [`time-no-imperceptible`](http://stylelint.cn/user-guide/rules/time-no-imperceptible/)：禁止 `animation` 和 `transition` 小于或等于 100ms。

### 单元

- [`unit-blacklist`](http://stylelint.cn/user-guide/rules/unit-blacklist/)：指定一个禁止使用的单位的黑名单。
- [`unit-case`](http://stylelint.cn/user-guide/rules/unit-case/)：指定单位的大小写。
- known units.
- [`unit-no-unknown`](http://stylelint.cn/user-guide/rules/unit-no-unknown/)：禁止使用未知单位。
- [`unit-whitelist`](http://stylelint.cn/user-guide/rules/unit-whitelist/)：指定一个所允许的单位的白名单。

### 值

- [`value-keyword-case`](http://stylelint.cn/user-guide/rules/value-keyword-case/)：指定关键字的值的大小写。
- [`value-no-vendor-prefix`](http://stylelint.cn/user-guide/rules/value-no-vendor-prefix/)：禁止给值添加浏览器引擎前缀。

### 值清单

- [`value-list-comma-newline-after`](http://stylelint.cn/user-guide/rules/value-list-comma-newline-after/)：在值列表的逗号之后要求有一个换行符或禁止有空白。
- [`value-list-comma-newline-before`](http://stylelint.cn/user-guide/rules/value-list-comma-newline-before/)：在值列表的逗号之前要求有一个换行符或禁止有空白。
- [`value-list-comma-space-after`](http://stylelint.cn/user-guide/rules/value-list-comma-space-after/)：在值列表的逗号之后要求有一个空格或禁止有空白。
- [`value-list-comma-space-before`](http://stylelint.cn/user-guide/rules/value-list-comma-space-before/)：在值列表的逗号之前要求有一个空格或禁止有空白。
- [`value-list-max-empty-lines`](http://stylelint.cn/user-guide/rules/value-list-max-empty-lines/)：限制值列表中相邻空行数量。

### Custom property

- [`custom-property-empty-line-before`](http://stylelint.cn/user-guide/rules/custom-property-empty-line-before/)：要求或禁止在自定义属性之前有一行空行。
- [`custom-property-no-outside-root`](http://stylelint.cn/user-guide/rules/custom-property-no-outside-root/)：禁止在 `:root` 规则之外使用自定义属性。
- [`custom-property-pattern`](http://stylelint.cn/user-guide/rules/custom-property-pattern/)：为自定义属性指定一个匹配模式。

### Shorthand property

- [`shorthand-property-no-redundant-values`](http://stylelint.cn/user-guide/rules/shorthand-property-no-redundant-values/)：禁止在简写属性中使用冗余值 。

### Property

- [`property-blacklist`](http://stylelint.cn/user-guide/rules/property-blacklist/)：指定一个禁止使用的属性的黑名单。
- [`property-case`](http://stylelint.cn/user-guide/rules/property-case/)：指定属性的大小写。
- [`property-no-unknown`](http://stylelint.cn/user-guide/rules/property-no-unknown/)：禁止使用未知属性。
- [`property-no-vendor-prefix`](http://stylelint.cn/user-guide/rules/property-no-vendor-prefix/)：禁止属性使用浏览器引擎前缀。
- [`property-whitelist`](http://stylelint.cn/user-guide/rules/property-whitelist/)：指定一个允许使用的属性的白名单。

### 关键帧声明

- [`keyframe-declaration-no-important`](http://stylelint.cn/user-guide/rules/keyframe-declaration-no-important/)：禁止在 keyframe 声明中使用 `!important`。

### 声明

- [`declaration-bang-space-after`](http://stylelint.cn/user-guide/rules/declaration-bang-space-after/)：在感叹号之后要求有一个空格或禁止有空白。
- [`declaration-bang-space-before`](http://stylelint.cn/user-guide/rules/declaration-bang-space-before/)：在感叹号之前要求有一个空格或禁止有空白。
- [`declaration-colon-newline-after`](http://stylelint.cn/user-guide/rules/declaration-colon-newline-after/)：在冒号之后要求有一个换行符或禁止有空白。
- [`declaration-colon-space-after`](http://stylelint.cn/user-guide/rules/declaration-colon-space-after/)：在冒号之后要求有一个空格或禁止有空白 。
- [`declaration-colon-space-before`](http://stylelint.cn/user-guide/rules/declaration-colon-space-before/)：在冒号之前要求有一个空格或禁止有空白 。
- [`declaration-empty-line-before`](http://stylelint.cn/user-guide/rules/declaration-empty-line-before/)：要求或禁止在声明语句之前有空行。
- [`declaration-no-important`](http://stylelint.cn/user-guide/rules/declaration-no-important/)：禁止在声明中使用 `!important`。
- [`declaration-property-unit-blacklist`](http://stylelint.cn/user-guide/rules/declaration-property-unit-blacklist/)：指定一个在声明中禁止使用的属性和单位的黑名单。
- [`declaration-property-unit-whitelist`](http://stylelint.cn/user-guide/rules/declaration-property-unit-whitelist/)：指定一个在声明中允许使用的属性和单位的白名单。
- [`declaration-property-value-blacklist`](http://stylelint.cn/user-guide/rules/declaration-property-value-blacklist/)：指定一个在声明中禁止使用的属性和值的黑名单。
- [`declaration-property-value-whitelist`](http://stylelint.cn/user-guide/rules/declaration-property-value-whitelist/)：指定一个在声明中允许使用的属性和值的白名单。

### 声明块

- [`declaration-block-no-duplicate-properties`](http://stylelint.cn/user-guide/rules/declaration-block-no-duplicate-properties/)：在声明的块中中禁止出现重复的属性。
- [`declaration-block-no-ignored-properties`](http://stylelint.cn/user-guide/rules/declaration-block-no-ignored-properties/)：禁止使用由于其他属性的原因而被忽略的属性。
- [`declaration-block-no-redundant-longhand-properties`](http://stylelint.cn/user-guide/rules/declaration-block-no-redundant-longhand-properties/)：禁止使用可以缩写却不缩写的属性。
- [`declaration-block-no-shorthand-property-overrides`](http://stylelint.cn/user-guide/rules/declaration-block-no-shorthand-property-overrides/)：禁止缩写属性覆盖相关普通写法属性。
- [`declaration-block-properties-order`](http://stylelint.cn/user-guide/rules/declaration-block-properties-order/)：指定声明块中的属性顺序 。**待调整**
- [`declaration-block-semicolon-newline-after`](http://stylelint.cn/user-guide/rules/declaration-block-semicolon-newline-after/)：在声明块的分号之后要求有一个换行符或禁止有空白。
- [`declaration-block-semicolon-newline-before`](http://stylelint.cn/user-guide/rules/declaration-block-semicolon-newline-before/)：在声明块的分号之前要求有一个换行符或禁止有空白。
- [`declaration-block-semicolon-space-after`](http://stylelint.cn/user-guide/rules/declaration-block-semicolon-space-after/)：在声明块的分号之后要求有一个空格或禁止有空白。
- [`declaration-block-semicolon-space-before`](http://stylelint.cn/user-guide/rules/declaration-block-semicolon-space-before/)：在声明块的分号之后前要求有一个空格或禁止有空白。
- [`declaration-block-single-line-max-declarations`](http://stylelint.cn/user-guide/rules/declaration-block-single-line-max-declarations/)：限制单行声明块中声明的数量。
- [`declaration-block-trailing-semicolon`](http://stylelint.cn/user-guide/rules/declaration-block-trailing-semicolon/)：要求或禁止在声明块中使用拖尾分号。

### 块

- [`block-closing-brace-empty-line-before`](http://stylelint.cn/user-guide/rules/block-closing-brace-empty-line-before/)：要求或禁止在闭括号之前有空行。
- [`block-closing-brace-newline-after`](http://stylelint.cn/user-guide/rules/block-closing-brace-newline-after/)：在闭括号之后要求有一个换行符或禁止有空白 。
- [`block-closing-brace-newline-before`](http://stylelint.cn/user-guide/rules/block-closing-brace-newline-before/)：在闭括号之前要求有一个换行符或禁止有空白 。
- [`block-closing-brace-space-after`](http://stylelint.cn/user-guide/rules/block-closing-brace-space-after/)：在闭括号之后要求有一个空格或禁止有空格。
- [`block-closing-brace-space-before`](http://stylelint.cn/user-guide/rules/block-closing-brace-space-before/)：在闭括号之前要求有一个空格或禁止有空格。
- [`block-no-empty`](http://stylelint.cn/user-guide/rules/block-no-empty/)：禁止出现空块。
- [`block-no-single-line`](http://stylelint.cn/user-guide/rules/block-no-single-line/)：禁止出现单行块。
- [`block-opening-brace-newline-after`](http://stylelint.cn/user-guide/rules/block-opening-brace-newline-after/)：在开括号之后要求有一个换行符 。
- [`block-opening-brace-newline-before`](http://stylelint.cn/user-guide/rules/block-opening-brace-newline-before/)：在括开号之前要求有一个换行符或禁止有空白 。
- [`block-opening-brace-space-after`](http://stylelint.cn/user-guide/rules/block-opening-brace-space-after/)：在开括号之后要求有一个空格或禁止有空白 。
- [`block-opening-brace-space-before`](http://stylelint.cn/user-guide/rules/block-opening-brace-space-before/)：在开括号之前要求有一个空格或禁止有空白 。

### 选择

- [`selector-attribute-brackets-space-inside`](http://stylelint.cn/user-guide/rules/selector-attribute-brackets-space-inside/)：在特性选择器的方括号内要求有空格或禁止有空白。
- [`selector-attribute-operator-blacklist`](http://stylelint.cn/user-guide/rules/selector-attribute-operator-blacklist/)：指定一个禁止使用的特性(attribute)操作符的黑名单。
- [`selector-attribute-operator-space-after`](http://stylelint.cn/user-guide/rules/selector-attribute-operator-space-after/)：在特性选择器的操作符之后要求有一个空格或禁止有空白。
- [`selector-attribute-operator-space-before`](http://stylelint.cn/user-guide/rules/selector-attribute-operator-space-before/)：在特性选择器的操作符之前要求有一个空格或禁止有空白。
- [`selector-attribute-operator-whitelist`](http://stylelint.cn/user-guide/rules/selector-attribute-operator-whitelist/)：指定允许使用的特性(attribute)操作符的白名单。
- [`selector-attribute-quotes`](http://stylelint.cn/user-guide/rules/selector-attribute-quotes/)：要求或禁止特性值使用引号。
- [`selector-class-pattern`](http://stylelint.cn/user-guide/rules/selector-class-pattern/)：为类选择器指定一个匹配模式。
- [`selector-combinator-space-after`](http://stylelint.cn/user-guide/rules/selector-combinator-space-after/)：在关系选择符之后要求有一个空格或禁止有空白 。
- [`selector-combinator-space-before`](http://stylelint.cn/user-guide/rules/selector-combinator-space-before/)：在关系选择符之前要求有一个空格或禁止有空白 。
- [`selector-descendant-combinator-no-non-space`](http://stylelint.cn/user-guide/rules/selector-descendant-combinator-no-non-space/)：禁止包含选择符出现非空格字符。
- [`selector-id-pattern`](http://stylelint.cn/user-guide/rules/selector-id-pattern/)：指定一个 id 选择器的匹配模式。
- [`selector-max-compound-selectors`](http://stylelint.cn/user-guide/rules/selector-max-compound-selectors/)：限制复合选择器的数量。
- [`selector-max-specificity`](http://stylelint.cn/user-guide/rules/selector-max-specificity/)：限制选择器的优先级。
- [`selector-nested-pattern`](http://stylelint.cn/user-guide/rules/selector-nested-pattern/)：指定一个嵌套选择器的匹配模式。
- [`selector-no-attribute`](http://stylelint.cn/user-guide/rules/selector-no-attribute/)：禁用特性选择器。
- [`selector-no-combinator`](http://stylelint.cn/user-guide/rules/selector-no-combinator/)：禁用关系选择符。
- [`selector-no-empty`](http://stylelint.cn/user-guide/rules/selector-no-empty/)：禁止出现空选择器。
- [`selector-no-id`](http://stylelint.cn/user-guide/rules/selector-no-id/)：禁用 id 选择器。
- [`selector-no-qualifying-type`](http://stylelint.cn/user-guide/rules/selector-no-qualifying-type/)：禁止使用类型对选择器进行限制。
- [`selector-no-type`](http://stylelint.cn/user-guide/rules/selector-no-type/)：禁用类型选择器。
- [`selector-no-universal`](http://stylelint.cn/user-guide/rules/selector-no-universal/)：禁用通配符选择器。
- [`selector-no-vendor-prefix`](http://stylelint.cn/user-guide/rules/selector-no-vendor-prefix/)：禁止使用浏览器引擎前缀。
- [`selector-pseudo-class-blacklist`](http://stylelint.cn/user-guide/rules/selector-pseudo-class-blacklist/)：指定一个禁止使用的伪类选择器的黑名单。
- [`selector-pseudo-class-case`](http://stylelint.cn/user-guide/rules/selector-pseudo-class-case/)：指定伪类选择器的大小写。
- [`selector-pseudo-class-no-unknown`](http://stylelint.cn/user-guide/rules/selector-pseudo-class-no-unknown/)：禁止使用未知的伪类选择器。
- [`selector-pseudo-class-parentheses-space-inside`](http://stylelint.cn/user-guide/rules/selector-pseudo-class-parentheses-space-inside/)：在伪类选择器的括号内要求使用一个空格或禁止有空白。
- [`selector-pseudo-class-whitelist`](http://stylelint.cn/user-guide/rules/selector-pseudo-class-whitelist/)：指定一个允许使用的伪类选择器的白名单。
- [`selector-pseudo-element-case`](http://stylelint.cn/user-guide/rules/selector-pseudo-element-case/)：指定伪元素的大小写。
- [`selector-pseudo-element-colon-notation`](http://stylelint.cn/user-guide/rules/selector-pseudo-element-colon-notation/):指定伪元素使用单冒号还是双冒号。
- [`selector-pseudo-element-no-unknown`](http://stylelint.cn/user-guide/rules/selector-pseudo-element-no-unknown/)：禁止使用未知的伪元素。
- [`selector-root-no-composition`](http://stylelint.cn/user-guide/rules/selector-root-no-composition/)：禁止 `:root` 混合使用。
- [`selector-type-case`](http://stylelint.cn/user-guide/rules/selector-type-case/)：指定类型选择器的大小写。
- [`selector-type-no-unknown`](http://stylelint.cn/user-guide/rules/selector-type-no-unknown/)：禁用未知的类型选择器。
- [`selector-max-empty-lines`](http://stylelint.cn/user-guide/rules/selector-max-empty-lines/)：限制选择器中相邻空行数量。

### 选择列表

- [`selector-list-comma-newline-after`](http://stylelint.cn/user-guide/rules/selector-list-comma-newline-after/): 要求选择器列表的逗号之后有一个换行符或禁止在逗号之后有空白 。
- [`selector-list-comma-newline-before`](http://stylelint.cn/user-guide/rules/selector-list-comma-newline-before/): 要求选择器列表的逗号之前有一个换行符或禁止在逗号之前有空白 。
- [`selector-list-comma-space-after`](http://stylelint.cn/user-guide/rules/selector-list-comma-space-after/)：要求在选择器列表的逗号之后有一个空格，或禁止有空白 。
- [`selector-list-comma-space-before`](http://stylelint.cn/user-guide/rules/selector-list-comma-space-before/)：要求在选择器列表的逗号之前有一个空格，或禁止有空白 。

### 根规则

- [`root-no-standard-properties`](http://stylelint.cn/user-guide/rules/root-no-standard-properties/)：禁止在 `:root` 中出现标准属性。

### 规则

- [`rule-nested-empty-line-before`](http://stylelint.cn/user-guide/rules/rule-nested-empty-line-before/)：在嵌套的规则中要求或禁止有空行。
- [`rule-non-nested-empty-line-before`](http://stylelint.cn/user-guide/rules/rule-non-nested-empty-line-before/)：在非嵌套的规则之前要求或禁止有空行。

### 媒体功能

- [`media-feature-colon-space-after`](http://stylelint.cn/user-guide/rules/media-feature-colon-space-after/)：在 media 特性中的冒号之后要求有一个空格或禁止有空白。
- [`media-feature-colon-space-before`](http://stylelint.cn/user-guide/rules/media-feature-colon-space-before/)：在 media 特性中的冒号之前要求有一个空格或禁止有空白。
- [`media-feature-name-blacklist`](http://stylelint.cn/user-guide/rules/media-feature-name-blacklist/)：指定禁止使用的 media 特性名称的黑名单。
- [`media-feature-name-case`](http://stylelint.cn/user-guide/rules/media-feature-name-case/)：指定 media 特性名称的大小写。
- [`media-feature-name-no-unknown`](http://stylelint.cn/user-guide/rules/media-feature-name-no-unknown/)：禁止使用未知的 media 特性名称。
- [`media-feature-name-no-vendor-prefix`](http://stylelint.cn/user-guide/rules/media-feature-name-no-vendor-prefix/)：禁止 media 特性名称带有浏览器引擎前缀。
- [`media-feature-name-whitelist`](http://stylelint.cn/user-guide/rules/media-feature-name-whitelist/)：指定允许使用的 media 特性名称的白名单。
- [`media-feature-no-missing-punctuation`](http://stylelint.cn/user-guide/rules/media-feature-no-missing-punctuation/)：禁止非布尔类型的 media 特性缺少标点。
- [`media-feature-parentheses-space-inside`](http://stylelint.cn/user-guide/rules/media-feature-parentheses-space-inside/)：在media 特性的括号内要求有一个空格或禁止有空白。
- [`media-feature-range-operator-space-after`](http://stylelint.cn/user-guide/rules/media-feature-range-operator-space-after/)：在 media 特性的范围操作符之后要求有一个空格或禁止有空白。
- [`media-feature-range-operator-space-before`](http://stylelint.cn/user-guide/rules/media-feature-range-operator-space-before/)：在 media 特性的范围操作符之前要求有一个空格或禁止有空白。

### 自定义媒体

- [`custom-media-pattern`](http://stylelint.cn/user-guide/rules/custom-media-pattern/)：指定一个自定义媒体查询名称的匹配模式。

### 媒体查询列表

- [`media-query-list-comma-newline-after`](http://stylelint.cn/user-guide/rules/media-query-list-comma-newline-after/)：在媒体查询的逗号之后要求有一个换行符或禁止有空白。
- [`media-query-list-comma-newline-before`](http://stylelint.cn/user-guide/rules/media-query-list-comma-newline-before/)：在媒体查询的逗号之前要求有一个换行符或禁止有空白。
- [`media-query-list-comma-space-after`](http://stylelint.cn/user-guide/rules/media-query-list-comma-space-after/)：在媒体查询的逗号之后要求有一个空格或禁止有空白。
- [`media-query-list-comma-space-before`](http://stylelint.cn/user-guide/rules/media-query-list-comma-space-before/)：在媒体查询的逗号之前要求有一个空格或禁止有空白。

### At-rule

- [`at-rule-blacklist`](http://stylelint.cn/user-guide/rules/at-rule-blacklist/)：指定一个禁止使用的 at 规则的黑名单。
- empty line before at-rules .
- [`at-rule-empty-line-before`](http://stylelint.cn/user-guide/rules/at-rule-empty-line-before/)：要求或禁止在 at 规则之前有空行 。
- [`at-rule-name-case`](http://stylelint.cn/user-guide/rules/at-rule-name-case/)：指定 at 规则名称的大小写。
- [`at-rule-name-newline-after`](http://stylelint.cn/user-guide/rules/at-rule-name-newline-after/)：要求在 at 规则之后有一个换行符。
- [`at-rule-name-space-after`](http://stylelint.cn/user-guide/rules/at-rule-name-space-after/)：要求在 at 规则之后有一个空格。
- [`at-rule-no-unknown`](http://stylelint.cn/user-guide/rules/at-rule-no-unknown/)：禁止使用未知的 at 规则。
- [`at-rule-no-vendor-prefix`](http://stylelint.cn/user-guide/rules/at-rule-no-vendor-prefix/)：禁止 at 规则使用浏览器引擎前缀。
- [`at-rule-semicolon-newline-after`](http://stylelint.cn/user-guide/rules/at-rule-semicolon-newline-after/)：要求在 at 规则的分号之后有一个换行符 。
- [`at-rule-whitelist`](http://stylelint.cn/user-guide/rules/at-rule-whitelist/)：指定一个允许使用的 at 规则的白名单。

### `stylelint-disable` 注释

- [`stylelint-disable-reason`](http://stylelint.cn/user-guide/rules/stylelint-disable-reason/)：要求在 `stylelint-disable` 注释之前或之后有一个原因的描述注释。

### 注释

- [`comment-empty-line-before`](http://stylelint.cn/user-guide/rules/comment-empty-line-before/)：要求或禁止在注释之前有空行。
- [`comment-no-empty`](http://stylelint.cn/user-guide/rules/comment-no-empty/)：禁止空注释。
- [`comment-whitespace-inside`](http://stylelint.cn/user-guide/rules/comment-whitespace-inside/)：要求或禁止在注释标签内有空白。
- [`comment-word-blacklist`](http://stylelint.cn/user-guide/rules/comment-word-blacklist/)：指定一个不允许出现在注释中的单词的黑名单。

### 一般/表

- [`indentation`](http://stylelint.cn/user-guide/rules/indentation/)：指定缩进 。
- [`max-empty-lines`](http://stylelint.cn/user-guide/rules/max-empty-lines/)：限制相邻空行的数量。
- [`max-line-length`](http://stylelint.cn/user-guide/rules/max-line-length/)：限制单行的长度。
- [`max-nesting-depth`](http://stylelint.cn/user-guide/rules/max-nesting-depth/)：限制允许嵌套的深度。
- [`no-browser-hacks`](http://stylelint.cn/user-guide/rules/no-browser-hacks/)：禁用与你使用的浏览器无关的 browser hacks。
- [`no-descending-specificity`](http://stylelint.cn/user-guide/rules/no-descending-specificity/)：禁止低优先级的选择器出现在高优先级的选择器之后。
- [`no-duplicate-selectors`](http://stylelint.cn/user-guide/rules/no-duplicate-selectors/)：在一个样式表中禁止出现重复的选择器。
- [`no-empty-source`](http://stylelint.cn/user-guide/rules/no-empty-source/)：禁止空源。
- [`no-eol-whitespace`](http://stylelint.cn/user-guide/rules/no-eol-whitespace/)：禁止行尾空白。
- [`no-extra-semicolons`](http://stylelint.cn/user-guide/rules/no-extra-semicolons/)：禁止多余的分号。
- [`no-indistinguishable-colors`](http://stylelint.cn/user-guide/rules/no-indistinguishable-colors/)：禁用相似的颜色。
- [`no-invalid-double-slash-comments`](http://stylelint.cn/user-guide/rules/no-invalid-double-slash-comments/)：禁用 CSS 不支持的双斜线注释 (`//...`)。
- [`no-missing-end-of-source-newline`](http://stylelint.cn/user-guide/rules/no-missing-end-of-source-newline/)：禁止缺少文件末尾的换行符。
- [`no-unknown-animations`](http://stylelint.cn/user-guide/rules/no-unknown-animations/)：禁止动画名称与 `@keyframes` 声明不符。
- [`no-unsupported-browser-features`](http://stylelint.cn/user-guide/rules/no-unsupported-browser-features/)：禁止使用浏览器不支持的特性。
- 用户指南
  - [规则](http://stylelint.cn/user-guide/rules/)
  - [插件](http://stylelint.cn/user-guide/plugins/)
  - [Processors](http://stylelint.cn/user-guide/processors/)
- [开发指南](http://stylelint.cn/developer-guide/)
- [Demo](http://stylelint.cn/demo/)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-08-30