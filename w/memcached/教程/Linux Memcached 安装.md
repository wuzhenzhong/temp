# Linux Memcached 安装

Memcached 支持许多平台：Linux、FreeBSD、Solaris、Mac OS，也可以安装在Windows上。

Linux系统安装memcached，首先要先安装libevent库。

```
sudo apt-get install libevent libevent-deve          自动下载安装（Ubuntu/Debian）

yum install libevent libevent-deve                      自动下载安装（Redhat/Fedora/Centos）
```

------

## 安装 Memcached

### 自动安装

**Ubuntu/Debian**

```
sudo apt-get install memcached
```

**Redhat/Fedora/Centos**

```
yum install memcached
```

**FreeBSD**

```
portmaster databases/memcached
```

### 源代码安装

从其官方网站（http://memcached.org）下载memcached最新版本。

```
wget http://memcached.org/latest                    下载最新版本

tar -zxvf memcached-1.x.x.tar.gz                    解压源码

cd memcached-1.x.x                                  进入目录

./configure --prefix=/usr/local/memcached           配置

make && make test                                   编译

sudo make install                                   安装
```

------

## Memcached 运行

Memcached命令的运行：

```
$ /usr/local/memcached/bin/memcached -h                           命令帮助
```

注意：如果使用自动安装 memcached 命令位于 **/usr/local/bin/memcached**。

**启动选项：**

- -d是启动一个守护进程；
- -m是分配给Memcache使用的内存数量，单位是MB；
- -u是运行Memcache的用户；
- -l是监听的服务器IP地址，可以有多个地址；
- -p是设置Memcache监听的端口，，最好是1024以上的端口；
- -c是最大运行的并发连接数，默认是1024；
- -P是设置保存Memcache的pid文件。

### （1）作为前台程序运行：

从终端输入以下命令，启动memcached:

```
/usr/local/memcached/bin/memcached -p 11211 -m 64m -vv

slab class   1: chunk size     88 perslab 11915

slab class   2: chunk size    112 perslab  9362

slab class   3: chunk size    144 perslab  7281

中间省略

slab class  38: chunk size 391224 perslab     2

slab class  39: chunk size 489032 perslab     2

<23 server listening

<24 send buffer was 110592, now 268435456

<24 server listening (udp)

<24 server listening (udp)

<24 server listening (udp)

<24 server listening (udp)
```

这里显示了调试信息。这样就在前台启动了memcached，监听TCP端口11211，最大内存使用量为64M。调试信息的内容大部分是关于存储的信息。

### （2）作为后台服务程序运行：

```
# /usr/local/memcached/bin/memcached -p 11211 -m 64m -d
```

或者

```
/usr/local/memcached/bin/memcached -d -m 64M -u root -l 192.168.0.200 -p 11211 -c 256 -P /tmp/memcached.pid
```