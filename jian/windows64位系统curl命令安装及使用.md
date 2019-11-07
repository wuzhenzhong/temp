# windows64位系统curl命令安装及使用

[![img](https://upload.jianshu.io/users/upload_avatars/2976869/bf9925a0-8b3a-404c-9296-5c4b1fbdb3ab.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/ff2903c0af37)

[趁你还年轻233](https://www.jianshu.com/u/ff2903c0af37)关注

2017.11.28 12:47:58字数 1,064阅读 3,867

在学习《深入浅出nodejs》Cookie章节的时候，有一个客户端发送cookie的终端命令。
`curl -v -H "Cookie:foo=bar;baz=val" "http://127.0.0.1:1337/path?foo=bar&amp;foo=baz`
可以看出，curl命令可以通过命令行的方式，执行Http请求。
但是我打开cmd后运行上述命令，没有生效。

所以我将来探索下windows（64位）下安装并使用curl的方式，捎带会有一些有趣的思考。
PS：我的系统环境是 windows10（64位），因此凡是64位的windows系统，此方法均适用。

在官网处下载工具包：[http://curl.haxx.se/download.html](https://link.jianshu.com/?t=http%3A%2F%2Fcurl.haxx.se%2Fdownload.html)



![img](https://upload-images.jianshu.io/upload_images/2976869-9d03ac62272e1fda.png?imageMogr2/auto-orient/strip|imageView2/2/w/643/format/webp)



此处下载的是CAB后缀的，后续会有版本选择说明。

##### 使用方式1：在curl.exe目录中使用（非常不推荐）

解压下载后的压缩文件，通过cmd命令进入到curl.exe所在的目录。
　　由于我使用的是windows 64位 的系统，因此可以使用I386或AMD64下的curl.exe工具。

##### 使用方式2：放置在system32中（不推荐）

解压下载好的文件，拷贝curl.exe（I386和AMD64文件下的curl.exe均可）文件到C:\Windows\System32

##### 使用方式3：配置用户变量（推荐）

直接编辑用户变量的Path，为其新增"你的curl目录位置\curl-7.56.1\I386"或"你的curl目录位置\curl-7.56.1\AMD64"

##### 使用方式4：配置系统变量（非常推荐）

在系统变量中，配置
　　CURL_HOME ----- "你的curl目录位置\curl-7.56.1"
　　path ---- 末尾添加 “;%CURL_HOME%\I386”或者“;%CURL_HOME%\AMD64”

测试方法：
cmd或者ps窗口键入`curl -h`，返回下面的界面，表示curl安装成功。



![img](https://upload-images.jianshu.io/upload_images/2976869-8ba5fd5937444740.png?imageMogr2/auto-orient/strip|imageView2/2/w/617/format/webp)



说明：

1.方式2中的拷贝文件，必须是单个的curl.exe文件，直接存放在system32目录下
2.方式4中高级系统变量的设置，只能以目录作为最小单元

思考：

**1.AMD64与I386的区别是什么，为什么都能用？**
AMD64是64位系统，I386是32位系统，其实就是X64和X86的区别。
都能用的原因是，32位系统下的程序兼容64位系统。

**2.环境变量分为：用户变量和系统变量，分别在什么场景下设置更好？**
用户变量仅作用于当前用户。
系统变量可作用于所有用户。
系统变量优先级更高。例如用户变量和系统变量中同时设置了curl命令，会优先执行系统变量中的。
个人认为，常用系统工具，例如curl，npm这样的，可以设置到系统变量中；如果像chrome，evernote这种取决于用户习惯的命令，设置到用户变量中较好。

**3.curl安装包版本选择？**
大多数情况选择CAB版本，其他情况较少。



![img](https://upload-images.jianshu.io/upload_images/2976869-ff60129aedf9fe0d.png?imageMogr2/auto-orient/strip|imageView2/2/w/636/format/webp)


**Win64 x86_64 7zip**→curl_7_53_1_openssl_nghttp2_x64



![img](https://upload-images.jianshu.io/upload_images/2976869-df744e7a896468b2.png?imageMogr2/auto-orient/strip|imageView2/2/w/371/format/webp)



这里是用OpenSSL，ngttp2，zlib和IPv6支持构建的Windows预编译的curl版本。
不过还是不明觉厉，我只觉得多了一个CA证书。

**Win64 x86_64 zip** curl源代码



![img](https://upload-images.jianshu.io/upload_images/2976869-b2e134f31969a6eb.png?imageMogr2/auto-orient/strip|imageView2/2/w/380/format/webp)



在github上已开源，地址为[https://github.com/curl/curl](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2Fcurl%2Fcurl)。

**Win64 x86_64 zip CAB** 包含curl和libcurl



![img](https://upload-images.jianshu.io/upload_images/2976869-7c0dce942ce8ccb1.png?imageMogr2/auto-orient/strip|imageView2/2/w/386/format/webp)



libcurl是curl正在使用的库。可以在自己开发的软件中使用。

**Win64 x86_64 7zip**→curl-7.56.1-win64-mingw



![img](https://upload-images.jianshu.io/upload_images/2976869-a1ff987213d51938.png?imageMogr2/auto-orient/strip|imageView2/2/w/371/format/webp)



和Win64 x86_64 zip类似，具体功能未知。

参考：
[https://www.cnblogs.com/xing901022/p/4652624.html](https://link.jianshu.com/?t=https%3A%2F%2Fwww.cnblogs.com%2Fxing901022%2Fp%2F4652624.html)

That it !

> 期待和大家交流，共同进步，欢迎大家加入我创建的与前端开发密切相关的技术讨论小组：
>
> - SegmentFault技术圈:[ES新规范语法糖](https://link.jianshu.com/?t=https%3A%2F%2Fsegmentfault.com%2Fg%2F1570000010695363)
> - SegmentFault专栏：[趁你还年轻，做个优秀的前端工程师](https://link.jianshu.com/?t=https%3A%2F%2Fsegmentfault.com%2Fblog%2Fchennihainianqing)
> - 知乎专栏：[趁你还年轻，做个优秀的前端工程师](https://link.jianshu.com/?t=https%3A%2F%2Fzhuanlan.zhihu.com%2Fwyasy)
> - Github博客: [趁你还年轻233的个人博客
>   ](https://link.jianshu.com/?t=https%3A%2F%2Fgithub.com%2FFrankKai%2FFrankKai.github.io)
> - 前端开发交流群：660634678

> 努力成为优秀前端工程师！