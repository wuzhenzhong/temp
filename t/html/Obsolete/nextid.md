# nextid

**已废弃**

该功能已从Web标准中删除。尽管一些浏览器可能仍然支持它，但它正在被丢弃。避免使用它并尽可能更新现有的代码; 请参阅本页底部的兼容性表格来指导您的决定。请注意，此功能可能随时停止工作。

**<nextid>**是一个过时的HTML元素，用于使NeXT网页设计工具为其锚点生成自动的NAME标签。它是由该网页编辑工具自动生成的，不需要手动调整或输入。这个元素有一个区别，就是从HTML版本的官方公共DTD中被淘汰，成为“迷失标签”之一的第一个元素。它也可能是所有早期HTML元素中最不被了解的一个。

HTML“0.a” - 从1991年1月10日开始，这个标签还没有被发明，所以没有发现这个时期的例子.HTML“0.c” - 从1991年1月23日到1992年11月23日这个早期版本的HTML `<NEXTID>`以非SGML兼容的形式引入，仅将数字值单独用作“属性”。HTML“0.d” - 从1992年11月26日至1993年5月24日在此期间，NeXT和最古老的DTD显示`<NEXTID>`只取一个数字作为其新引入的属性的值`N`.HTML“1.k” - 版本1（第一版）在这个第一次发布的HTML草稿中，`<NEXTID>`与HTML 2中的相同，最后允许使用一个名称而不是一个数字来表示它的属性值.HTML“1.m” - 版本1（第二版）在下一个发布的HTML草稿中，`<NEXTID><NEXTID>`可以单独deslected用于显示以简单的SGML command.HTML版第2级第1本就像是第2级默认值，但它不包括所有形式的元素，即`<FORM>`，`<INPUT>`，`<TEXTAREA>`，`<SELECT>`，和`<OPTION>`HTML版本2严格等级1本是像普通级别1，但它也排除这些折旧元素，以及`<H*>`在链接（`<A>`元素）中嵌套标题（元素）的结构。HTML版本2级别2这是默认值，包含并允许所有HTML级别2功能和元素和属性HTML版本2严格级别2不包括这些折旧的元素，也禁止这样的结构嵌套一个头（`<H*>`元素）在一个链接（`<A>`元素），或有一个形式`<INPUT>`不在`<P>`HTML版本3.2 等块级元素中的元素`<NEXTID>`已经完全消失，且不会再被听说。

## 属性

像所有其他HTML元素一样，此元素接受全局属性。

**n**参考锚点。

## 示例

用户在目录中输入四个章节标题（也可能在这些章节中写入段落材料）。四个部分中的每一个的标题将被赋予“z0”，“z1”，“z2”和“z3”的名称值。其中的第一个会在这样的目录中产生一个条目，`<A NAME="z0" HREF="#z4">FIRST SECTION NAME</A>`而这个部分的标题就会被标记为：`<H2><A NAME="z4">FIRST SECTION NAME</A></H2>`。对于接下来的三个部分z5，z6和z7（以及名称为z1，z2和z3的目录条目），这将继续进行，每个部分都自动给这些名称定位。用户然后保存并关闭文档。然后，NeXT会在HTML文档的标题内添加一个特殊的标签，`<NEXTID N="z8">`，告知哪里可以继续命名约定。想象一下，网络作者打开文档进行进一步编辑。他们想在第二部分之后添加几个新的部分，最后再添加四个部分。当打开文档时，NeXT编辑器找到并读取这个`<NEXTID N="z8">`标签，并且现在知道在内容列表中给出这些新章节中的第一个z8的名称，并且将z14指定给内容主体。看起来像这样：

```javascript
<HTML>
    <HEAD>
        <TITLE> ... whatever ... </TITLE>
        <LINK, META, BASE, etc. as applicable for the head of this document>
        <NEXTID N="z20">
    </HEAD>
    
    <BODY>
        <A NAME="z0" HREF="#z4">FIRST SECTION HEADING</A>
        <A NAME="z1" HREF="#z5">SECOND SECTION HEADING</A>
        <A NAME="z8" HREF="#z14">NEWLY INSERTED THIRD SECTION HEADING</A>
        <A NAME="z9" HREF="#z15">NEWLY INSERTED FOURTH SECTION HEADING</A>
        <A NAME="z2" HREF="#z6">ORIGINAL THIRD (NOW FIFTH) SECTION HEADING</A>
        <A NAME="z3" HREF="#z7">ORIGINAL FOURTH (NOW SIXTH) SECTION HEADING</A>
        <A NAME="z10" HREF="#z16">SEVENTH SECTION HEADING</A>
        <A NAME="z11" HREF="#z17">EIGHTH SECTION HEADING</A>
        <A NAME="z12" HREF="#z18">NINTH SECTION HEADING</A>
        <A NAME="z13" HREF="#z19">TENTH SECTION HEADING</A>
        <H2><A NAME="z4">FIRST SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z5">SECOND SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z14">NEWLY INSERTED THIRD SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z15">NEWLY INSERTED FOURTH SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z6">ORIGINAL THIRD (NOW FIFTH) SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z7">ORIGINAL FOURTH (NOW SIXTH) SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z16">SEVENTH SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z17">EIGHTH SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z18">NINTH SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z19">TENTH SECTION HEADING</A></H1><P> ... whatever ... </P>
    </BODY>
</HTML>
```

然后他们将这个文档的副本转发给有NeXT编辑器的人，他们删除z7和z19，再添加十个，z20到z29，然后删除z24和z29。那么NEXTID的值在返回修改后是z30：

```javascript
<HTML>
    <HEAD>
        <TITLE> ... whatever ... </TITLE>
        <LINK, META, BASE, etc. as applicable for the head of this document>
        <NEXTID N="z30">
    </HEAD>

    <BODY>
        <A NAME="z0" HREF="#z4">FIRST SECTION HEADING</A>
        <A NAME="z1" HREF="#z5">SECOND SECTION HEADING</A>
        <A NAME="z8" HREF="#z14">NEWLY INSERTED THIRD SECTION HEADING</A>
        <A NAME="z9" HREF="#z15">NEWLY INSERTED FOURTH SECTION HEADING</A>
        <A NAME="z2" HREF="#z6">ORIGINAL THIRD (NOW FIFTH) SECTION HEADING</A>
        <A NAME="z10" HREF="#z16">SEVENTH (NOW SIXTH) SECTION HEADING</A>
        <A NAME="z11" HREF="#z17">EIGHTH (NOW SEVENTH) SECTION HEADING</A>
        <A NAME="z12" HREF="#z18">NINTH (NOW EIGHTH) SECTION HEADING</A>
        <A NAME="z20" HREF="#z25">NEW NINTH SECTION HEADING</A>
        <A NAME="z21" HREF="#z26">NEW TENTH SECTION HEADING</A>
        <A NAME="z22" HREF="#z27">NEW ELEVENTH SECTION HEADING</A>
        <A NAME="e23" HREF="#z28">NEW TWELFTH SECTION HEADING</A>
        <H2><A NAME="z4">FIRST SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z5">SECOND SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z14">NEWLY INSERTED THIRD SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z15">NEWLY INSERTED FOURTH SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z6">ORIGINAL THIRD (NOW FIFTH) SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z16">SEVENTH (NOW SIXTH) SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z17">EIGHTH (NOW SEVENTH) SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z18">NINTH (NOW EIGHTH) SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z25">NEW NINTH SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z26">NEW TENTH SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z27">NEW ELENENTH SECTION HEADING</A></H1><P> ... whatever ... </P>
        <H2><A NAME="z28">NEW TWELFTH SECTION HEADING</A></H1><P> ... whatever ... </P>
    </BODY>
</HTML>
```

## HTML参考

- [Working NEXTID Tag Element Example](http://www.the-pope.com/nextid.html)

- [5.2.6. Next Id: NEXTID](https://tools.ietf.org/html/rfc1866#section-5.2.6)

## 浏览器兼容性

| Feature       | Chrome | Edge | Firefox | Internet Explorer | Opera | Safari |
| :------------ | :----- | :--- | :------ | :---------------- | :---- | :----- |
| Basic Support | No     | No   | No      | No                | No    | No     |

| Feature       | Android | Chrome for Android | Edge mobile | Firefox for Android | IE mobile | Opera Android | iOS Safari |
| :------------ | :------ | :----------------- | :---------- | :------------------ | :-------- | :------------ | :--------- |
| Basic Support | No      | No                 | No          | No                  | No        | No            | No         |

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18