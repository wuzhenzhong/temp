# evalsha

```javascript
EVALSHA sha1 numkeys key [key ...] arg [arg ...]
```

**自2.6.0起可用。**

**时间复杂度：**取决于执行的脚本。

通过其 SHA1摘要评估缓存在服务器端的脚本。使用 SCRIPT LOAD 命令将脚本缓存在服务器端。该命令在其他方面与 EVAL 相同。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18