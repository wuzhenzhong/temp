# auth

```javascript
AUTH password
```

**自1.0.0起可用。**

请求在受密码保护的 Redis 服务器中进行身份验证。在允许客户端执行命令之前，可以指示 Redis 需要密码。这是使用`requirepass`配置文件中的指令完成的。

如果`password`与配置文件中的密码相匹配，则服务器将回复`OK`状态码并开始接受命令。否则，返回错误，客户端需要尝试新的密码。

**注意**：由于 Redis 的高性能特性，可以在很短的时间内并行尝试大量密码，因此请确保生成强大且密码非常长的密码，以防止此类攻击行为发生。

## 返回值

[Simple string reply](https://redis.io/topics/protocol#simple-string-reply)

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18