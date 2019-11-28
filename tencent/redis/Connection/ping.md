# ping

- 贡献者1人

  

```javascript
PING [message]
```

**自1.0.0起可用。**

返回`PONG`如果没有提供参数，否则返回参数的副本作为一个主体。此命令通常用于测试连接是否仍然存在，或测量延迟。

如果客户订购了一个渠道或一个模式，它将返回一个多头，在第一个位置带有“pong”，而在第二个位置带有空批量，除非提供了参数，在这种情况下它会返回一个副本的论点。

## 返回值

[Simple string reply](https://redis.io/topics/protocol#simple-string-reply)

## 例子

redis> PING `"PONG"` redis> PING "hello world" `"hello world"`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18