# Taro.uploadFile(param)

将本地资源上传到服务器。客户端发起一个 HTTPS POST 请求，其中 content-type 为 multipart/form-data。使用前请注意阅读相关说明。

使用方式同 [`wx.uploadFile`](https://developers.weixin.qq.com/miniprogram/dev/api/wx.uploadFile.html)，支持 `Promise` 化使用。

## 参数

### object param

| Property     | Type       | Description                                                  |
| ------------ | ---------- | ------------------------------------------------------------ |
| url          | `string`   | 开发者服务器地址                                             |
| filePath     | `string`   | 要上传文件资源的路径                                         |
| name         | `string`   | 文件对应的 key，开发者在服务端可以通过这个 key 获取文件的二进制内容 |
| [header]     | `object`   | HTTP 请求 Header，Header 中不能设置 Referer                  |
| [formData]   | `object`   | HTTP 请求中其他额外的 form data                              |
| [success()]  | `function` | 接口调用成功的回调函数                                       |
| [fail()]     | `function` | 接口调用失败的回调函数                                       |
| [complete()] | `function` | 接口调用结束的回调函数（调用成功、失败都会执行）             |

## 返回值

### Promise<object res> promise

| Name                             | Type       | Description                 |
| -------------------------------- | ---------- | --------------------------- |
| promise.headersReceive(callback) | `function` | 绑定接收到http header的回调 |
| promise.progress(callback)       | `function` | 绑定请求进度更新的回调      |
| promise.abort()                  | `function` | 中断请求                    |
| res.statusCode                   | `number`   | 请求的返回状态码            |
| res.data                         | `any`      | 服务器的返回数据            |

## 示例代码

```jsx
import Taro from '@tarojs/taro'

const uploadTask = Taro.uploadFile(params).then(...)
```

## API支持度

|       API       | 微信小程序 |  H5  | React Native | 支付宝小程序 | 百度小程序 |
| :-------------: | :--------: | :--: | :----------: | :----------: | :--------: |
| Taro.uploadFile |     ✔️      |  ✔️   |       ️       |      ✔️       |     ✔️      |