# Golang 1.13: 解决国内 go get 无法下载的问题

更新日期: 2019-10-24 阅读次数: 844 字数: 194 分类: [golang](https://www.sunzhongwei.com/category/golang)

 搜索



在[下载并安装 go 1.13 ](https://www.sunzhongwei.com/ubuntu-installation-golang-compiler-tools-and-libraries)之后，安装 golang gin 依赖包的时候，发现长时间没有响应，无法下载，从返回的错误信息看应该是国内无法访问 golang.org。

```
$ go get -u github.com/gin-gonic/gin

package golang.org/x/sys/unix: unrecognized import path "golang.org/x/sys/unix" (https fetch: Get https://golang.org/x/sys/unix?go-get=1: dial tcp 216.239.37.1:443: connect: connection refused)
```

## 解决办法

使用国内七牛云的 go module 镜像。

参考 https://github.com/goproxy/goproxy.cn。

golang 1.13 可以直接执行：

```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

然后再次使用 go get 下载 gin 依赖就可以了。为[七牛云](https://portal.qiniu.com/signup?code=3ldtiuk3qiogi)点个赞。

## 阿里云 Go Module 国内镜像仓库服务

除了七牛云，还可以使用[阿里云](https://www.aliyun.com/1111/2019/group-buying-share?ptCode=8AD59D29D72CF2AD6648FD5DB89CFE06647C88CF896EF535&userCode=yxah0nq9&share_source=copy_link)的 golang 国内镜像。

https://mirrors.aliyun.com/goproxy/

设置方法

```
go env -w GO111MODULE=on
go env -w GOPROXY=https://mirrors.aliyun.com/goproxy/,direct
```

## golang 版本

```
> go version
go version go1.13 linux/amd64
```