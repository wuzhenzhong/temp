# keygen

**已废弃**



该特性已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用该特性。



HTML `<keygen>` 元素是为了方便生成密钥材料和提交作为[HTML form](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Forms) 的一部分的公钥.这种机制被用于设计基于 Web 的证书管理系统。按照预想，`<keygen>` 元素将用于 HTML 表单与其他的所需信息一起构造一个证书请求，该处理的结果将是一个带有签名的证书。



Web浏览器制造商目前正在讨论是否保留这个功能。在达成决定之前，最好继续将此功能视为已弃用或消失。

| 内容类别     | 流量内容，措辞内容，互动内容，列出，可引用，可提交，可重置的表格相关元素，可触及的内容。 |
| :----------- | :----------------------------------------------------------- |
| 允许的内容   | 没有，这是一个空的元素。                                     |
| 标记遗漏     | 必须有开始标签，并且不得有结束标签。                         |
| 允许父母     | 任何接受短语内容的元素。                                     |
| 允许ARIA角色 | 没有                                                         |
| DOM界面      | HTMLKeygenElement                                            |

## 属性

这个元素包含全局属性。

**autofocus**这个布尔属性可以让你指定当页面加载时控件应该有输入焦点，除非用户覆盖它，例如通过输入不同的控件。文档中只有一个表单元素可以具有`autofocus`属性，这是一个布尔值。

**challenge**与公钥一起提交的质询字符串。如果未指定，则缺省为空字符串。**disabled**此布尔属性指示表单控件不可用于交互。

**form**该元素与其关联的表单元素（其*表单所有者*）。属性的值必须是同一个文档中`id`的一个`<form>`元素的值。如果未指定此属性，则此元素必须是元素的后代`<form>`。这个属性可以让你放置`<keygen>`元素中任何位置的元素，而不仅仅是它们的表单元素的后代。

**keytype**生成的密钥的类型。默认值是`RSA`。

**name**与表单数据一起提交的控件的名称。

元素写成如下：

```javascript
<keygen name="name" challenge="challenge string" keytype="type" keyparams="pqg-params">
```

`keytype`参数用于指定要生成哪种类型的密钥。有效值是“ `RSA`”，这是默认值，“ `DSA`”和“ `EC`”。将`name`和`challenge`属性都需要在所有情况下。该`keytype`属性对于RSA密钥生成是可选的，并且对于生成DSA和EC密钥是必需的。

`keyparams`属性是DSA和EC密钥生成所必需的，在生成RSA密钥时将被忽略。`PQG`是一个同义词`keyparams`。也就是说，你可以指定`keyparams="pqg-params"`或`pqg="pqg-params"`。

对于RSA密钥，`keyparams`不使用该参数（如果存在则忽略）。用户可以被给予RSA关键优势的选择。目前，用户被给予“高”强度（2048比特）和“中等”强度（1024比特）之间的选择。

对于DSA密钥，`keyparams`参数指定将在keygen过程中使用的DSA PQG参数。`pqg`参数的值是IETF [RFC 3279中](ftp://ftp.rfc-editor.org/in-notes/rfc3279.txt)规定的BASE64编码的DER编码的Dss-Parms 。用户可以选择DSA密钥大小，允许用户选择DSA标准中定义的大小之一。

对于EC密钥，`keyparams`参数指定将在其上生成密钥的椭圆曲线的名称。它通常是[nsKeygenHandler.cpp中](http://mxr.mozilla.org/mozilla-central/source/security/manager/ssl/src/nsKeygenHandler.cpp?mark=179-185,187-206,208-227,229-256#177)的表中的一个字符串。（请注意，在任何特定的浏览器中，只有名为的曲线的子集可能实际上受支持。）如果`keyparams`参数字符串不是可识别的曲线名称字符串，则根据用户选择的关键强度（低，中，高），使用名为“ `secp384r1`” 的曲线为高，曲线名为“ `secp256r1`”为中等键。（注意：关键强度数量的选择，每个强度的默认值，以及用户提供选择的UI都超出了本规范的范围。）

`<keygen>`元素仅在HTML表单中有效。这将导致某种选择被呈现给用户以选择密钥大小。用于选择的UI可以是菜单，单选按钮或可能的其他东西。浏览器提供了几个可能的关键优势。目前，有两个优势，高和中。如果用户的浏览器被配置为支持加密硬件（例如“智能卡”），则用户也可以选择在何处生成密钥，即在智能卡中或软件中并存储在磁盘上。

当按下提交按钮时，会生成所选尺寸的密钥对。私钥被加密并存储在本地密钥数据库中。

```javascript
   PublicKeyAndChallenge ::= SEQUENCE {
       spki SubjectPublicKeyInfo,
       challenge IA5STRING
   }
   SignedPublicKeyAndChallenge ::= SEQUENCE {
       publicKeyAndChallenge PublicKeyAndChallenge,
       signatureAlgorithm AlgorithmIdentifier,
       signature BIT STRING
   }
```

公钥和挑战字符串是DER编码的`PublicKeyAndChallenge`，然后用私钥进行数字签名产生一个`SignedPublicKeyAndChallenge`。`SignedPublicKeyAndChallenge`是[Base64](https://developer.mozilla.org/en-US/docs/Glossary/Base64)编码，该ASCII数据最终提交到服务器作为一种形式的名称/值对的值，其中由指定的名称是名称`name`的属性`keygen`元素。如果没有提供挑战字符串，那么它将被编码为`IA5STRING`长度为零的字符串。

下面是一个表单提交的示例，它将通过HTTP服务器传递给CGI程序：

```javascript
   commonname=John+Doe&email=doe@foo.com&org=Foobar+Computing+Corp.&
   orgunit=Bureau+of+Bureaucracy&locality=Anytown&state=California&country=US&
   key=MIHFMHEwXDANBgkqhkiG9w0BAQEFAANLADBIAkEAnX0TILJrOMUue%2BPtwBRE6XfV%0AWtKQbsshxk5ZhcUwcwyvcnIq9b82QhJdoACdD34rqfCAIND46fXKQUnb0mvKzQID%0AAQABFhFNb3ppbGxhSXNNeUZyaWVuZDANBgkqhkiG9w0BAQQFAANBAAKv2Eex2n%2FS%0Ar%2F7iJNroWlSzSMtTiQTEB%2BADWHGj9u1xrUrOilq%2Fo2cuQxIfZcNZkYAkWP4DubqW%0Ai0%2F%2FrgBvmco%3D
```

## 示例

- [带有RSA KEYGEN元素的示例表单](https://bugzilla.mozilla.org/attachment.cgi?id=380749)

- [带有DSA KEYGEN元素和PQG参数的样品表格](https://bugzilla.mozilla.org/attachment.cgi?id=380750)

- [带有DSA KEYGEN元素但没有PQG参数的示例表单](https://bugzilla.mozilla.org/attachment.cgi?id=380751)

- [带有EC KEYGEN元素的示例表单](https://bugzilla.mozilla.org/attachment.cgi?id=380752)

## 规范

| 规范                                   | 状态     | 评论 |
| :------------------------------------- | :------- | :--- |
| HTML生活标准该规范中'<keygen>'的定义。 | 生活水平 |      |
| HTML5该规范中'<keygen>'的定义。        | 建议     |      |

## 浏览器兼容性

| Feature       | Chrome     | Edge  | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :--------- | :---- | :------ | :---------------- | :---- | :----- |
| Basic Support | (Yes) — 57 | (Yes) | 1       | No                | 3.0   | 1.2    |

| Feature       | Android    | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :--------- | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | (Yes) — 57 | (Yes) — 57         | (Yes)       | 1                   | No        | ?             | No         |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18