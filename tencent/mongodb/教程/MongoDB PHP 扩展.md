# MongoDB PHP 扩展

## Linux 上安装 MongoDB PHP 扩展

### 在终端上安装

你可以在 Linux 中执行以下命令来安装 MongoDB 的 PHP 扩展驱动

```javascript
$ sudo pecl install mongodb
```

使用php的pecl安装命令必须保证网络连接可用以及root权限。

### 安装手册

如果你想通过源码来编译扩展驱动。你必须手动编译源码包，这样做的好是最新修正的 bug 包含在源码包中。

你可以在 PHP 官网上下载 MongoDB PHP 驱动包,下载地址：http://pecl.php.net/package/mongodb。



![img](https://ask.qcloudimg.com/draft/5637037/8a6xnrsh1s.png)

完整安装命令如下：

```javascript
$ wget http://pecl.php.net/get/mongodb-1.5.2.tgz
$ cd /mongodb-1.5.2
$ phpize
$ ./configure
$ make && make install
```

如果你的 php 是自己编译的，则安装方法如下(假设是编译在 /usr/local/php目录中)：

```javascript
$ wget http://pecl.php.net/get/mongodb-1.5.2.tgz
$ cd /mongodb-1.5.2
$ /usr/local/php/bin/phpize
$ ./configure --with-php-config=/usr/local/php/bin/php-config
$ make && make install
```

安装成功后，会有类似以下安装目录信息输出：

```javascript
...
Installing shared extensions:     /usr/lib/php/extensions/debug-non-zts-20151012/
```

执行以上命令后，你需要修改php.ini文件，在 php.ini 文件中添加mongo配置，配置如下：

```javascript
extension_dir=/usr/lib/php/extensions/debug-non-zts-20151012/
extension=mongodb.so
```

> ***注意：**你需要指明 extension_dir 配置项的路径。* *可以通过以下命令查看目录地址：*

```js
$ php -i | grep extension_dir
  extension_dir => /usr/lib/php/extensions/debug-non-zts-20151012 =>
                   /usr/lib/php/extensions/debug-non-zts-20151012
```

## Window 上安装 MongoDB PHP扩展

PECL 上已经提供了用于 Window 平台的预编译 php mongodb 驱动二进制包(下载地址： https://pecl.php.net/package/mongodb)，你可以下载与你 php 对应的版本，但是你需要注意以下几点问题：

- VC6 是运行于 Apache 服务器
- Thread safe（线程安全）是以模块形式运行在 Apache 上，如果你以 CGI 的模式运行 PHP，请选择非线程安全模式（non-thread safe）。
- VC9 是运行于 IIS 服务器上。
- 下载完你需要的二进制包后，解压压缩包，将 php_mongodb.dll 文件添加到你的PHP扩展目录中（ext）。ext 目录通常在 PHP 安装目录下的 ext 目录。

打开 php 配置文件 php.ini 添加以下配置：

```js
extension=php_mongodb.dll
```

重启服务器。

通过浏览器访问phpinfo，如果安装成功，就会看到类型以下的信息：

![img](https://www.runoob.com/wp-content/uploads/2013/10/mongo-php-driver-installed-windows.png)

## MAC 中安装 MongoDB PHP扩展驱动

你可以使用 **autoconf** 安装 MongoDB PHP 扩展驱动。

你可以使用 **Xcode** 安装 MongoDB PHP 扩展驱动。

如果你使用 XAMPP，你可以使用以下命令安装 MongoDB PHP 扩展驱动：

```js
sudo /Applications/XAMPP/xamppfiles/bin/pecl install mongo
```

如果以上命令在XMPP或者MAMP中不起作用，你需要在 Github上下载兼容的预编译包。

然后添加 extension=mongodb.so 配置到你的 php.ini 文件中。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com