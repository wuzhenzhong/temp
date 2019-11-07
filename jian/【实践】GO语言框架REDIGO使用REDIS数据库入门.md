# 【实践】GO语言框架REDIGO使用REDIS数据库入门

[![img](https://upload.jianshu.io/users/upload_avatars/1190574/93697e8d8af5.jpeg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/347463663a86)

[笔名辉哥](https://www.jianshu.com/u/347463663a86)关注

0.8592019.09.29 15:57:53字数 376阅读 131



![img](https://upload-images.jianshu.io/upload_images/1190574-53c0a242a2b94ee4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1000/format/webp)

# 1. 摘要

基于GO的REDIOS调用框架有开源库redigo。本文主要讲解redigo的框架和调用样例。

# 2. 使用实践

## 2.1 前置条件

（1）GO环境已经搭建
（2）REDIS已经搭建
没有搭建的，参考[《【实践】REDIS缓存数据库从安装到入门》](https://www.jianshu.com/p/825d257595a2)。
（3）下载redigo库

> go get github.com/gomodule/redigo/redis

## 2.2 测试实践

### 2.2.1 建立工程

在GO的源目录下建立rediogoDemo工程，包含main.go文件。

### 2.2.2 测试读写和有效时间

```swift
func testSetGet() {
    c, err := redis.Dial("tcp", "52.82.14.217:6379")
    if err != nil {
        fmt.Println("Connect to redis error", err)
        return
    }
    defer c.Close()

    _, err = c.Do("SET", "name", "duncanwang", "EX", "5")
    if err != nil {
        fmt.Println("redis set failed:", err)
    }

    username, err := redis.String(c.Do("GET", "name"))
    if err != nil {
        fmt.Println("redis get failed:", err)
    } else {
        fmt.Printf("Get mykey: %v \n", username)
    }

    time.Sleep(8 * time.Second)

    username, err = redis.String(c.Do("GET", "name"))
    if err != nil {
        fmt.Println("redis get failed:", err)
    } else {
        fmt.Printf("Get name: %v \n", username)
    }

}
```

### 2.2.3 批量写入读取

```go
func testMSetGet() {
    c, err := redis.Dial("tcp", "52.82.14.217:6379")
    if err != nil {
        fmt.Println("Connect to redis error", err)
        return
    }
    defer c.Close()

    _, err = c.Do("MSET", "name", "duncanwang","sex","male")
    if err != nil {
        fmt.Println("redis set failed:", err)
    }

    is_key_exit, err := redis.Bool(c.Do("EXISTS", "name"))
    if err != nil {
        fmt.Println("error:", err)
    } else {
        fmt.Printf("Is name key exists ? : %v \n", is_key_exit)
    }

    reply, err := redis.Values(c.Do("MGET", "name", "sex"))
    if err != nil {
        fmt.Printf("patch get name & sex error \n。")
    } else {
        var name string
        var sex string
        _, err := redis.Scan(reply, &name, &sex);
        if err != nil {
            fmt.Printf("Scan error \n。")
        } else {
            fmt.Printf("The name is %v, sex is %v \n", name, sex)
        }
    }

}
```

### 2.2.4 读写json到redis转换

```go
func testJsonConvert() {
    c, err := redis.Dial("tcp", "52.82.14.217:6379")
    if err != nil {
        fmt.Println("Connect to redis error", err)
        return
    }
    defer c.Close()

    key := "profile"
    imap := map[string]string{"name": "duncanwang", "sex": "male","mobile":"13671927788"}
    value, _ := json.Marshal(imap)

    n, err := c.Do("SETNX", key, value)
    if err != nil {
        fmt.Println(err)
    }
    if n == int64(1) {
        fmt.Println("set Json key success。")
    }

    var imapGet map[string]string

    valueGet, err := redis.Bytes(c.Do("GET", key))
    if err != nil {
        fmt.Println(err)
    }

    errShal := json.Unmarshal(valueGet, &imapGet)
    if errShal != nil {
        fmt.Println(err)
    }
    fmt.Println(imapGet["name"])
    fmt.Println(imapGet["sex"])
    fmt.Println(imapGet["mobile"])
}
```

### 2.2.5 主函数和引用

```go
package main
import (
    "fmt"
    "time"
    "encoding/json"
    "github.com/gomodule/redigo/redis"
)

func main() {
    /*测试读写和有效时间*/
    testSetGet();

    /*批量写入读取*/
    testMSetGet();

    /*读写json到redis*/
    testJsonConvert();
}

// ... 加入之前的函数
```

运行结果如下，达到了预期目的。

```swift
 D:\jusanban\doc\50-编码实现\GO\src\rediogoDemo> go build
PS D:\jusanban\doc\50-编码实现\GO\src\rediogoDemo> ./rediogoDemo.exe
Get mykey: duncanwang
redis get failed: redigo: nil returned
Is name key exists ? : true
The name is duncanwang, sex is male
duncanwang
male
13671927788
```

如果需要完整工程源码可加入辉哥的知识星球获取。
[https://t.zsxq.com/EiyNbqB](https://links.jianshu.com/go?to=https%3A%2F%2Ft.zsxq.com%2FEiyNbqB)

REDIGO的完整帮助文档参考：
[https://godoc.org/github.com/gomodule/redigo/redis](https://links.jianshu.com/go?to=https%3A%2F%2Fgodoc.org%2Fgithub.com%2Fgomodule%2Fredigo%2Fredis)

# 3. 框架整体介绍



![img](https://upload-images.jianshu.io/upload_images/1190574-26699cffbd443b0f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

# 4. 参考

（1）Go实战--golang中使用redis(redigo和go-redis/redis)
[https://blog.csdn.net/wangshubo1989/article/details/75050024](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fwangshubo1989%2Farticle%2Fdetails%2F75050024)

（2）开源库redigo和文档[欧阳采用]
github地址：
[https://github.com/gomodule/redigo](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fgomodule%2Fredigo)
文档地址：
[https://godoc.org/github.com/gomodule/redigo/redis](https://links.jianshu.com/go?to=https%3A%2F%2Fgodoc.org%2Fgithub.com%2Fgomodule%2Fredigo%2Fredis)
go语言使用redis（redigo）
https://www.jianshu.com/p/62f0b9ce7584

（3）开源库go-redis/redis的使用
github地址：
[https://github.com/go-redis/redis](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fgo-redis%2Fredis)
文档地址：
[https://godoc.org/github.com/go-redis/redis](https://links.jianshu.com/go?to=https%3A%2F%2Fgodoc.org%2Fgithub.com%2Fgo-redis%2Fredis)

（4）Redis 教程
[https://www.runoob.com/redis/redis-tutorial.html](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.runoob.com%2Fredis%2Fredis-tutorial.html)

（5）redis命令列表
[https://redis.io/commands](https://links.jianshu.com/go?to=https%3A%2F%2Fredis.io%2Fcommands)