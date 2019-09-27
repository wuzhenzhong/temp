# .NET Core实战项目之CMS 第十四章 开发篇-防止跨站请求伪造（XSRF/CSRF）攻击处理

0.0762019.01.06 20:00:14字数 2499阅读 112

通过 ASP.NET Core，开发者可轻松配置和管理其应用的安全性。 ASP.NET Core 中包含管理身份验证、授权、数据保护、SSL 强制、应用机密、请求防伪保护及 CORS 管理等等安全方面的处理。 通过这些安全功能，可以生成安全可靠的 ASP.NET Core 应用。而我们这一章就来说道说道如何在ASP.NET Core中处理“跨站请求伪造（XSRF/CSRF）攻击”的，希望对大家有所帮助！

> 本文已收录至《[.NET Core实战项目之CMS 第一章 入门篇-开篇及总体规划](https://www.cnblogs.com/yilezhu/p/9977862.html)》
> 作者：依乐祝
> 原文地址：https://www.cnblogs.com/yilezhu/p/10229954.html

## 写在前面

上篇文章发出来后很多人就去GitHub上下载了源码，然后就来问我说为什么登录功能都没有啊？还有很多菜单点着没反应。这里统一说明一下，是因为我的代码是跟着博客的进度在逐步完善的，等这个系列写完的时候才代表这个CMS系统的完成！因此，现在这个CMS系统还是一个半成品，不过我会尽快来完成的！废话不多说，下面我们先介绍一下跨站请求伪造（XSRF/CSRF）攻击”的概念，然后再来说到一下ASP.NET Core中是如何进行处理的吧！

## 什么是跨站请求伪造（XSRF/CSRF）

在继续之前如果不给你讲一下什么是跨站请求伪造（XSRF/CSRF）的话可能你会很懵逼，我为什么要了解这个，不处理又有什么问题呢？
CSRF（Cross-site request forgery跨站请求伪造，也被称为“One Click Attack”或者Session Riding，通常缩写为CSRF或者XSRF，是一种对网站的恶意利用。尽管听起来像跨站脚本（XSS），但它与XSS非常不同，并且攻击方式几乎相左。XSS利用站点内的信任用户，而CSRF则通过伪装来自受信任用户的请求来利用受信任的网站。与XSS攻击相比，CSRF攻击往往不大流行（因此对其进行防范的资源也相当稀少）和难以防范，所以被认为比XSS更具危险性。
CSRF在 2007 年的时候曾被列为互联网 20 大安全隐患之一。其他安全隐患，比如 SQL 脚本注入，跨站域脚本攻击等在近年来已经逐渐为众人熟知，很多网站也都针对他们进行了防御。然而，对于大多数人来说，CSRF 却依然是一个陌生的概念。即便是大名鼎鼎的 Gmail, 在 2007 年底也存在着 CSRF 漏洞，从而被黑客攻击而使 Gmail 的用户造成巨大的损失。

## 跨站请求伪造（XSRF/CSRF）的场景

这里为了加深大家对“跨站请求伪造（XSRF/CSRF）”的理解可以看如下所示的图：





![img](https://upload-images.jianshu.io/upload_images/2767091-44771aeada1904f1.png?imageMogr2/auto-orient/strip|imageView2/2/w/884/format/webp)

image

如上图所示：

1. 用户浏览位于目标服务器 A 的网站。并通过登录验证。
2. 获取到 cookie_session_id，保存到浏览器 cookie 中。
3. 在未登出服务器 A ，并在 session_id 失效前用户浏览位于 hacked server B 上的网站。
4. server B 网站中的`<img src = "http://www.cnblog.com/yilezhu?creditAccount=1001160141&transferAmount=1000">`嵌入资源起了作用，迫使用户访问目标服务器 A
5. 由于用户未登出服务器 A 并且 sessionId 未失效，请求通过验证，非法请求被执行。

试想一下如果这个非法请求是一个转账的操作会有多恐怖！

## 跨站请求伪造（XSRF/CSRF）怎么处理？

既然跨站请求伪造（XSRF/CSRF）有这么大的危害，那么我们如何在ASP.NET Core中进行处理呢？
其实说白了CSRF能够成功也是因为同一个浏览器会共享Cookies，也就是说，通过权限认证和验证是无法防止CSRF的。那么应该怎样防止CSRF呢？其实防止CSRF的方法很简单，只要确保请求是自己的站点发出的就可以了。那怎么确保请求是发自于自己的站点呢？ASP.NET Core中是以Token的形式来判断请求。我们需要在我们的页面生成一个Token，发请求的时候把Token带上。处理请求的时候需要验证Cookies+Token。这样就可以有效的进行验证了！
其实说到这里可能有部分童鞋已经想到了，`@Html.AntiForgeryToken()` 没错就是它，在.NET Core中起着防止 跨站请求伪造（XSRF/CSRF）的作用，想必大伙都会使用！下面我们再一起看看ASP.NET Core的使用方式吧。

## ASP.NET Core MVC是如何处理跨站请求伪造（XSRF/CSRF）的？

> **警告：**
> ASP.NET Core使用 [ASP.NET Core data protection stack](https://docs.microsoft.com/en-us/aspnet/core/security/data-protection/introduction) 来实现防请求伪造。如果在服务器集群中需配置 ASP.NET Core Data Protection，有关详细信息，请参阅 [Configuring data protection](https://docs.microsoft.com/en-us/aspnet/core/security/data-protection/configuration/overview)。

在ASP.NET Core MVC 2.0或更高版本中，[FormTagHelper](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/working-with-forms#the-form-tag-helper)为HTML表单元素注入防伪造令牌。例如，Razor文件中的以下标记将自动生成防伪令牌：

```xml
        <form method="post">
            ···
        </form>
```

类似地， [IHtmlHelper.BeginForm](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform)默认情况下生成防伪令牌，当然窗体的方法不是 GET。（你懂的）

当Html表单包含`method="post"`并且下面条件之一 成立是会自动生成防伪令牌。

- action属性为空( `action=""`) 或者
- 未提供action属性(`<form method="post">`)。

当然您也可以通过以下方式禁用自动生成HTML表单元素的防伪令牌：

- 明确禁止`asp-antiforgery`，例如

```xml
<form method="post" asp-antiforgery="false">
    ...
</form>
```

- 通过使用标签帮助器[! 禁用语法](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/tag-helpers/intro#opt-out)，从标签帮助器转化为表单元素。

```xml
<!form method="post">
    ...
</!form>
```

- 在视图中移除`FormTagHelper`，您可以在Razor视图中添加以下指令移除`FormTagHelper`：

```css
@removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
```

> **提示：**
> [Razor页面](https://docs.microsoft.com/en-us/aspnet/core/mvc/razor-pages/index)会自动受到XSRF/CSRF的保护。您不必编写任何其他代码，有关详细信息，请参阅[XSRF/CSRF和Razor页面](https://docs.microsoft.com/en-us/aspnet/core/mvc/razor-pages/index#xsrf)。

为抵御 CSRF 攻击最常用的方法是使用*同步器标记模式*(STP)。 当用户请求的页面包含窗体数据使用 STP:

1. 服务器发送到客户端的当前用户的标识相关联的令牌。
2. 客户端返回将令牌发送到服务器进行验证。
3. 如果服务器收到与经过身份验证的用户的标识不匹配的令牌，将拒绝请求。

该令牌唯一且不可预测。 该令牌还可用于确保正确序列化的一系列的请求 (例如，确保请求序列的： 第 1 页–第 2 页–第 3 页)。所有在ASP.NET Core MVC 和 Razor 页模板中的表单都会生成 antiforgery 令牌。 以下两个视图生成防伪令牌的示例：

CSHTML复制

```c
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

显式添加到防伪令牌`<form>`而无需使用标记帮助程序与 HTML 帮助程序元素[@Html.AntiForgeryToken ](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

CSHTML复制

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

在每个前面的情况下，ASP.NET Core 添加类似于以下一个隐藏的表单字段：

CSHTML复制

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core 包括三个[筛选器](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/controllers/filters?view=aspnetcore-2.2)来处理 antiforgery 令牌：

- [ValidateAntiForgeryToken](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
- [AutoValidateAntiforgeryToken](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
- [IgnoreAntiforgeryToken](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

#### 防伪选项

自定义[防伪选项](https://docs.microsoft.com/zh-cn/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions)中`Startup.ConfigureServices`:

C#复制

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-yilezhu";
    options.SuppressXFrameOptionsHeader = false;
});
```

†设置防伪`Cookie`属性使用的属性[CookieBuilder](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.http.cookiebuilder)类。

| 选项                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Cookie](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | 确定用于创建防伪 cookie 的设置。                             |
| [FormFieldName](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | 防伪系统用于呈现防伪令牌在视图中的隐藏的窗体字段的名称。     |
| [HeaderName](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | 防伪系统使用的标头的名称。 如果`null`，系统会认为只有窗体数据。 |
| [SuppressXFrameOptionsHeader](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | 指定是否禁止显示生成`X-Frame-Options`标头。 默认情况下，值为"SAMEORIGIN"生成标头。 默认为 `false`。 |

有关详细信息，请参阅[CookieAuthenticationOptions](https://docs.microsoft.com/zh-cn/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions)。

在我们的CMS系统中的Ajax请求就是使用的自定义HeaderName的方式进行验证的，不知道大家有没有注意到！

#### 需要防伪验证

[ValidateAntiForgeryToken](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)实质上是一个过滤器，可应用到单个操作，控制器或全局范围内。除了具有`IgnoreAntiforgeryToken`属性的操作，否则所有应用了这个属性的Action都会进行防伪验证。如下所示：

C#复制

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

`ValidateAntiForgeryToken`属性所修饰的操作方法包括 HTTP GET 都需要一个Token进行验证。 如果`ValidateAntiForgeryToken`特性应用于应用程序的控制器上，则可以应用`IgnoreAntiforgeryToken`来对它进行重载以便忽略此验证过程。

> 备注:ASP.NET Core 不支持自动将 antiforgery 令牌应用到GET 请求上。

### ASP.NET Core MVC在Ajax中处理跨站请求伪造（XSRF/CSRF）的注意事项

`ValidateAntiForgeryToken` 在进行Token验证的时候Token是从Form里面取的。但是ajax中，Form里面并没有东西。那token怎么办呢？这时候我们可以把Token放在Header里面。相信看了我的源码的童鞋一定对这些不会陌生！
如下代码所示：

```c
$.ajax({
            type: 'POST',
            url: '/ManagerRole/AddOrModify/',
            data: {
                Id: $("#Id").val(),  //主键
                RoleName: $(".RoleName").val(),  //角色名称
                RoleType: $(".RoleType").val(),  //角色类型
                IsSystem: $("input[name='IsSystem']:checked").val() === "0" ? false : true,  //是否系统默认
                Remark: $(".Remark").val()  //用户简介
            },
            dataType: "json",
            headers: {
                "X-CSRF-TOKEN-yilezhu": $("input[name='AntiforgeryKey_yilezhu']").val()
            },
            success: function (res) {//res为相应体,function为回调函数
                if (res.ResultCode === 0) {
                    var alertIndex = layer.alert(res.ResultMsg, { icon: 1 }, function () {
                        layer.closeAll("iframe");
                        //刷新父页面
                        parent.location.reload();
                        top.layer.close(alertIndex);
                    });
                    //$("#res").click();//调用重置按钮将表单数据清空
                } else if (res.ResultCode === 102) {
                    layer.alert(res.ResultMsg, { icon: 5 }, function () {
                        layer.closeAll("iframe");
                        //刷新父页面
                        parent.location.reload();
                        top.layer.close(alertIndex);
                    });
                }
                else {
                    layer.alert(res.ResultMsg, { icon: 5 });
                }
            },
            error: function (XMLHttpRequest, textStatus, errorThrown) {
                layer.alert('操作失败！！！' + XMLHttpRequest.status + "|" + XMLHttpRequest.readyState + "|" + textStatus, { icon: 5 });
            }
        });
```

如上代码所示我是先获取Token代码然后把这些代码放到ajax请求的Head里面再进行post请求即可！

## 开源地址

这个系列教程的源码我会开放在GitHub以及码云上，有兴趣的朋友可以下载查看！觉得不错的欢迎Star

GitHub：https://github.com/yilezhu/Czar.Cms

码云：https://gitee.com/yilezhu/Czar.Cms





如果你觉得这个系列对您有所帮助的话，欢迎以各种方式进行赞助，当然给个Star支持下也是可以滴！另外一种最简单粗暴的方式就是下面这种直接关注我们的公众号了：



![img](https://upload-images.jianshu.io/upload_images/2767091-457480a146263855.png?imageMogr2/auto-orient/strip|imageView2/2/w/258/format/webp)

img

## 总结

今天我先从跨站点请求伪造的概念以及原理入手，然后给大家讲解了如何进行跨站点请求伪造的处理，后面引出了在ASP.NET Core中如何对其进行处理的！同时给大家说了在Ajax处理中的注意事项，希望能对大伙有所帮助！另外如果你有不同的看法欢迎留言，或者加入NET Core千人群637326624讨论。