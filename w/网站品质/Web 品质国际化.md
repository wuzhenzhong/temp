# Web 品质国际化

## Web 品质 - 国际化

------

网络无国界。

------

## 网络无国界。

*"With the Internet follows an absolute requirement to interchange data in a multiplicity of languages, which in turn utilize a bewildering number of characters."*

H. Alvestrand, Internet工程工作小组 (IETF), 1998年1月。

------

## 国际字符集

所有的 W3C 标准（自从1996年），包括 HTML、XHTML 和 XML 都定义了一个名为 Unicode (ISO 10646) 内部的内部字符集。

所有现代 web 浏览器都在原生地使用此字符集。而大多数在 internet 上传输的文档并没有使用这个 Unicode 字符集。

正因如此，Internet 客户（浏览器）与 Internet 服务器 之间必须有一种在通信中一致使用字符集的方法。

对每个文档在用的字符集进行标记，对于提升网站的品质来说至关重要。

请始终在 <head> 元素内 使用下面的的元元素：

```
<meta charset="x">
```



把 X 替换为你所使用的字符集，比如ISO-8859-1、UTF-8 或者 UTF-16。

------

## 国际日期

请不要使用类似 "04-03-02" 的日期格式。

上面的日期可以表示为2004年3月2日，或者2002年3月4日，亦或者2002年4月3日。

国际标准化 (ISO) 为日期定义的国际标准格式是 "yyyy-mm-dd"，yyyy 是年，mm 是月，dd 是日。

如果您使用了 ISO 的格式，那么大多数访问者都能明白你的日期。