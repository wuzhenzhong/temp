# 使用 Sass | Using Sass

## 使用Sass

虽然Bootstrap建立在Less之外，但它也有一个[官方的Sass端口](https://github.com/twbs/bootstrap-sass)。我们将其保存在单独的GitHub存储库中，并使用转换脚本处理更新。

### 包括什么

由于Sass端口有一个单独的回购站，服务对象略有不同，因此该项目的内容与主Bootstrap项目有很大不同。这确保Sass端口尽可能与基于Sass的系统兼容。

| 路径           | 描述                                                         |
| :------------- | :----------------------------------------------------------- |
| lib/           | Ruby gem code (Sass configuration, Rails and Compass integrations) |
| tasks/         | Converter scripts (turning upstream Less to Sass)            |
| test/          | Compilation tests                                            |
| templates/     | Compass package manifest                                     |
| vendor/assets/ | Sass, JavaScript, and font files                             |
| Rakefile       | Internal tasks, such as rake and convert                     |

访问[Sass端口的GitHub存储库](https://github.com/twbs/bootstrap-sass)以查看这些文件的实际运行情况。

### 安装

有关如何安装和使用Bootstrap for Sass的信息，请参阅[GitHub存储库自述文件](https://github.com/twbs/bootstrap-sass)。它是最新的源代码，包含用于Rails，Compass和标准Sass项目的信息。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com