# Navigator

##### 页面链接

`<Navigator />` 组件的 RN 版本尚未实现。

#### 组件属性

| Name            | Type       | Description                                                  |
| --------------- | ---------- | ------------------------------------------------------------ |
| frontColor      | `string`   | 前景颜色值，包括按钮、标题、状态栏的颜色，仅支持 #ffffff 和 #000000 |
| backgroundColor | `string`   | 背景颜色值，有效值为十六进制颜色                             |
| animation       | `Object`   | 动画效果                                                     |
| [success]       | `function` | 接口调用成功的回调函数                                       |
| [fail]          | `function` | 接口调用失败的回调函数                                       |
| [complete]      | `function` | 接口调用结束的回调函数（调用成功、失败都会执行）             |

> 小程序端参数支持详见各小程序官网

[微信小程序 Navigator](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html)。

[百度小程序 Navigator](https://smartprogram.baidu.com/docs/develop/component/nav/#navigator)。

[支付宝小程序 Navigator](https://docs.alipay.com/mini/component/navigator)。

[字节跳动小程序 Navigator](https://developer.toutiao.com/docs/comp/navigator.html)。