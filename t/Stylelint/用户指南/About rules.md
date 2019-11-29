# 关于规则 | About rules

我们一直非常谨慎地遵守规则。

这些规则旨在相互协作，以便实施严格的约定。

## 关于规则名称

- 由连字符分隔的小写单词组成。
- 分为两部分：
  - 首先介绍了什么[*事情*](http://apps.workflower.fi/vocabs/css/en)该规则适用于。
  - 第二部分描述了规则检查的内容。

```javascript
"number-leading-zero"
// ↑          ↑
// the thing  what the rule is checking
```

- 除非规则适用于整个样式表：

```javascript
"no-eol-whitespace"
"indentation"
//    ↑
// what the rules are checking
```

### 没有规则

大多数规则允许您选择是否要求*或*禁止某些内容。

例如，无论是数字*必须*或*绝不能*有前导零：

- ```
  number-leading-zero
  ```

  :

   

  ```
  string - "always"|"never"
  ```

  - `"always"`- *必须始终*有一个前导零。
  - `"never"`- *绝不能*有领先的零。

```javascript
a { line-height: 0.5; }
/**              ↑
 * This leading zero */
```

但是，有些规则*只是不允许*某些事情。`*-no-*`用于识别这些规则。

例如，是否应该禁止空块：

- `block-no-empty`- 块*不能*为空。

```javascript
a {   }
/** ↑
 * Blocks like this */
```

请注意，对于这样的规则，如果有一个强制执行相反的选项，即每个块*必须*为空，则没有意义。

### 最大规则

`*-max-*`当规则*设置某个限制时*使用。

例如，指定“。”之后的最大位数。在一个数字：

- `number-max-precision`: `int`

```javascript
a { font-size: 1.333em; }
/**             ↑
 * The maximum number of digits after this "." */
```

### 空白规则

空格规则允许您指定在样式表的某个特定部分中是否必须使用空行，单个空格，换行符或无空格。

空白规则组合了两组关键字：

1. `before`，`after`和`inside`用于指定其中空白（如果有的话）的预期。
2. `empty-line`，`space`和`newline`用于指定是否单个空行，一个单一的空间，单个换行或没有空间预期在那里。

例如，指定样式表中的所有注释之前是否必须包含单个空行或没有空格：

- `comment-empty-line-before`: `string` - `"always"|"never"`

```javascript
a {}
              ←
/* comment */ ↑
              ↑
/**           ↑
 * This empty line  */
```

此外，一些空格规则使用另一组关键字：

1. `comma`，`colon`，`semicolon`，`opening-brace`，`closing-brace`，`opening-parenthesis`，`closing-parenthesis`，`operator`或`range-operator`如果标点符号中的一个特定的片，使用*的东西*被针对性。

例如，指定函数中的逗号后是否必须包含单个空格或空格：

- `function-comma-space-after`: `string` - `"always"|"never"`

```javascript
a { transform: translate(1, 1) }
/**                       ↑
 * The space after this commas */
```

标点符号的复数用于`inside`规则。例如，指定单个空格或无空格必须位于函数括号内：

- `function-parentheses-space-inside`: `string` - `"always"|"never"`

```javascript
a { transform: translate( 1, 1 ); }
/**                     ↑      ↑
 * The space inside these two parentheses */
```

## 规则协同工作

规则可以一起使用以强制执行严格的约定。

### `*-newline/space-before`和`*-newline/space-after`规则

假设您希望之前没有空格，并且每个声明中的冒号后面都有一个空格：

```javascript
a { color: pink; }
/**      ↑
 * No space before and a single space after this colon */
```

您可以通过以下方式强制执行：

```javascript
"declaration-colon-space-after": "always",
"declaration-colon-space-before": "never"
```

有些*东西*（例如声明块和值列表）可以跨越多行。在这些情况下`newline`，可以使用规则和额外选项来提供灵活性。

例如，这是一整套`value-list-comma-*`规则及其选项：

- `value-list-comma-space-after`: `"always"|"never"|"always-single-line"|"never-single-line"`
- `value-list-comma-space-before`: `"always"|"never"|"always-single-line"|"never-single-line"`
- `value-list-comma-newline-after`: `"always"|"always-multi-line|"never-multi-line"`
- `value-list-comma-newline-before`: `"always"|"always-multi-line"|"never-multi-line"`

`*-multi-line`和`*-single-line`参考价值表（*事物*）。例如，给定：

```javascript
a,
b {
  color: red;
  font-family: sans, serif, monospace; /* single line value list */
}              ↑                    ↑
/**            ↑                    ↑
 *  The value list start here and ends here */
```

此示例中只有一个单行值列表。选择器是多行的，声明块也是如此，同样也是规则。但值列表不是，这是什么`*-multi-line`，并`*-single-line`请参见本规则的情况下。

#### 例A

假设您只想允许单行值列表。并且您希望之前没有空格，并且在逗号后面只有一个空格：

```javascript
a {
  font-family: sans, serif, monospace;
  box-shadow: 1px 1px 1px red, 2px 2px 1px 1px blue inset, 2px 2px 1px 2px blue inset;
}
```

您可以通过以下方式强制执行：

```javascript
"value-list-comma-space-after": "always",
"value-list-comma-space-before": "never"
```

#### 例B

假设您要同时允许单行和多行值列表。您希望单行列表中的逗号后面有一个空格，而单行和多行列表中的逗号之前没有空格：

```javascript
a {
  font-family: sans, serif, monospace; /* single-line value list with space after, but no space before */
  box-shadow: 1px 1px 1px red, /* multi-line value list ... */
    2px 2px 1px 1px blue inset, /* ... with newline after, ...  */
    2px 2px 1px 2px blue inset; /* ... but no space before */
}
```

您可以通过以下方式强制执行：

```javascript
"value-list-comma-newline-after": "always-multi-line",
"value-list-comma-space-after": "always-single-line",
"value-list-comma-space-before": "never"
```

#### 例C

假设您要同时允许单行和多行值列表。您希望在单行列表中的逗号之前没有空格，并且在两个列表中的逗号后面始终是空格：

```javascript
a {
  font-family: sans, serif, monospace;
  box-shadow: 1px 1px 1px red
    , 2px 2px 1px 1px blue inset
    , 2px 2px 1px 2px blue inset;
}
```

您可以通过以下方式强制执行：

```javascript
"value-list-comma-newline-before": "always-multi-line",
"value-list-comma-space-after": "always",
"value-list-comma-space-before": "never-single-line"
```

#### 例D

最后，规则足够灵活，可以对单行和多行列表强制执行完全不同的约定。假设您要同时允许单行和多行值列表。您希望单行列表在冒号之前和之后具有单个空格。您希望多行列表在逗号之前有一个换行符，但之后没有空格：

```javascript
a {
  font-family: sans , serif , monospace; /* single-line list with a single space before and after the comma */
  box-shadow: 1px 1px 1px red /* multi-line list ... */
    ,2px 2px 1px 1px blue inset /* ... with newline before, ...  */
    ,2px 2px 1px 2px blue inset; /* ... but no space after the comma */
}
```

您可以通过以下方式强制执行：

```javascript
"value-list-comma-newline-after": "never-multi-line",
"value-list-comma-newline-before": "always-multi-line",
"value-list-comma-space-after": "always-single-line",
"value-list-comma-space-before": "always-single-line"
```

### `*-empty-line-before`和`*-max-empty-lines`规则

这些规则共同控制允许空行的位置。

每一*件事*负责将自身推离*前面的东西*，而不是推动*后续的事情*了。这种一致性是为了避免冲突，这就是为什么`*-empty-line-after`stylelint 中没有任何规则。

假设您要强制执行以下操作：

```javascript
a {
  background: green;
  color: red;

  @media (min-width: 30em) {
    color: blue;
  }
}

b {
  --custom-property: green;

  background: pink;
  color: red;
}
```

你可以这样做：

```javascript
"at-rule-empty-line-before": ["always", {
  "except": ["first-nested"]
}],
"custom-property-empty-line-before": [ "always", {
  "except": [
    "after-custom-property",
    "first-nested"
  ]
}],
"declaration-empty-line-before": ["always", {
  "except": [
    "after-declaration",
    "first-nested"
  ]
}],
"block-closing-brace-empty-line-before": "never",
"rule-non-nested-empty-line-before": ["always-multi-line"]
```

我们建议您将主要选项（例如`"always"`或`"never"`）设置为最常见的`except`选项，并使用可选的辅助选项定义例外。有对许多值`except`选项例如`first-nested`，`after-comment`等等。

该`*-empty-line-before`规则控制是否存在绝不是一个空行，还是必须有*一个或多个*，前后空行*的事*。该`*-max-empty-lines`规则通过控制补充这一*数量*的内空行*的事*。该`max-empty-lines`规则用于在整个源中设置限制。一个*更严格的*限制然后可以中设置*的东西*用的喜欢`function-max-empty-lines`，`selector-max-empty-lines`和`value-list-max-empty-lines`。

例如，假设您要强制执行以下操作：

```javascript
a,
b {
  box-shadow:
    inset 0 2px 0 #dcffa6,
    0 2px 5px #000;
}

c {
  transform:
    translate(
      1,
      1
    );
}
```

即整个源中最多1个空行，但函数，选择器列表和值列表中没有空行。

你可以这样做：

```javascript
"function-max-empty-lines": 0,
"max-empty-lines": 1,
"selector-list-max-empty-lines": 0,
"value-list-max-empty-lines": 0
```

### `*-whitelist`, `*-blacklist`, `color-named` and applicable `*-no-*` rules

这些规则一起工作以（dis）允许语言特征和结构。

有一些`*-whitelist`和`*-blacklist`规则针对CSS语言的主要构造：at-rules，函数，声明（即属性 - 值对），属性和单元。这些规则可用于（dis）允许使用这些结构的任何语言特征（例如`@media`，`rgb()`）。但是，有些功能没有被这些`*-whitelist`和`*-blacklist`规则捕获（或者是，但需要复杂的正则表达式来配置）。存在单独的规则，通常是`*-no-*`规则（例如`color-no-hex`和`selector-no-id`），以禁止这些特征中的每一个。

假设您要禁止`@debug`语言扩展。您可以使用`at-rule-blacklist`或`at-rule-whitelist`规则执行此操作，因为`@debug`语言扩展使用at-rule构造，例如

```javascript
"at-rule-blacklist": ["debug"]
```

无论出于何种原因，您都希望不要使用整个规则构造。你可以这样做：

```javascript
"at-rule-whitelist": []
```

假设你想设置为禁止值`none`的`border`属性。你可以使用`declaration-property-value-blacklist`或者`declaration-property-value-whitelist`例如

```javascript
"declaration-property-value-blacklist": [{
  "/^border/": ["none"]
}]
```

#### 颜色

大多数`<color>`值都是*函数*。因此，可以（dis）允许使用`function-blacklist`或者`function-whitelist`规则。还有两种不是函数的颜色表示：命名颜色和十六进制颜色。（dis）允许这两个具体规则：`color-named`和`color-no-hex`。

假设您希望使用命名颜色强制执行，*如果您选择的颜色存在，*并使用`hwb`颜色，如果没有，例如：

```javascript
a {
  background: hwb(235, 0%, 0%); /* there is no named color equivalent for this color */
  color: black;
}
```

如果您采用白名单方法，可以使用以下方法：

```javascript
"color-named": "always-where-possible",
"color-no-hex": true,
"function-whitelist": ["hwb"]
```

Or, if you're taking a blacklisting approach:

```javascript
"color-named": "always-where-possible",
"color-no-hex": true,
"function-blacklist": ["/^rgb/", "/^hsl/", "gray"]
```

This approach scales to when language extensions (that use the two built-in extendable syntactic constructs of at-rules and functions) are used. For example, say you want to disallow all standard color presentations in favour of using a custom color representation function, e.g. `my-color(red with a dash of green / 5%)`. You can do that with:

```javascript
"color-named": "never",
"color-no-hex": true,
"function-whitelist": ["my-color"]
```

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2018-08-30