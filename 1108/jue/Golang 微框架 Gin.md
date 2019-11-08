# Golang 微框架 Gin

阅读 4329

收藏 72

2018-09-05

原文链接：[www.jianshu.com](https://www.jianshu.com/p/a31e4ee25305)

[做一个电商直播App，跟上这波双十一juejin.im](https://juejin.im/post/5dc275035188255faf60b5ec)

### 所谓框架

框架一直是敏捷开发中的利器，能让开发者很快的上手并做出应用，甚至有的时候，脱离了框架，一些开发者都不会写程序了。成长总不会一蹴而就，从写出程序获取成就感，再到精通框架，快速构造应用，当这些方面都得心应手的时候，可以尝试改造一些框架，或是自己创造一个。

曾经我以为Python世界里的框架已经够多了，后来发现相比golang简直小巫见大巫。golang提供的net/http库已经很好了，对于http的协议的实现非常好，基于此再造框架，也不会是难事，因此生态中出现了很多框架。既然构造框架的门槛变低了，那么低门槛同样也会带来质量参差不齐的框架。

考察了几个框架，通过其github的活跃度，维护的team，以及生产环境中的使用率。发现[Gin](https://link.jianshu.com/?t=https://gin-gonic.github.io/gin/)还是一个可以学习的轻巧框架。

### Gin

Gin是一个golang的微框架，封装比较优雅，API友好，源码注释比较明确，已经发布了1.0版本。具有快速灵活，容错方便等特点。其实对于golang而言，web框架的依赖要远比Python，Java之类的要小。自身的net/http足够简单，性能也非常不错。框架更像是一些常用函数或者工具的集合。借助框架开发，不仅可以省去很多常用的封装带来的时间，也有助于团队的编码风格和形成规范。

下面就Gin的用法做一个简单的介绍。

首先需要安装，安装比较简单，使用go get即可：

```
go get gopkg.in/gin-gonic/gin.v1
```

gin的版本托管再 gopkg的网站上。我在安装的过程中，gokpg卡住了，后来不得不根据gin里的godep的文件，把响应的源码从github上下载，然后copy到对应的目录。

关于golang的包管理和依赖，我们以后再讨论。

### Hello World

使用Gin实现`Hello world`非常简单，创建一个router，然后使用其Run的方法：

```
import (
    "gopkg.in/gin-gonic/gin.v1"
    "net/http"
)

func main(){
    
    router := gin.Default()

    router.GET("/", func(c *gin.Context) {
        c.String(http.StatusOK, "Hello World")
    })
    router.Run(":8000")
}
```

简单几行代码，就能实现一个web服务。使用gin的Default方法创建一个路由handler。然后通过HTTP方法绑定路由规则和路由函数。不同于net/http库的路由函数，gin进行了封装，把request和response都封装到gin.Context的上下文环境。最后是启动路由的Run方法监听端口。麻雀虽小，五脏俱全。当然，除了GET方法，gin也支持POST,PUT,DELETE,OPTION等常用的restful方法。

### restful路由

gin的路由来自httprouter库。因此httprouter具有的功能，gin也具有，不过gin不支持路由正则表达式：

```
func main(){
    router := gin.Default()
    
    router.GET("/user/:name", func(c *gin.Context) {
        name := c.Param("name")
        c.String(http.StatusOK, "Hello %s", name)
    })
}
```

冒号`:`加上一个参数名组成路由参数。可以使用c.Params的方法读取其值。当然这个值是字串string。诸如`/user/rsj217`，和`/user/hello`都可以匹配，而`/user/`和`/user/rsj217/`不会被匹配。

```
☁  ~  curl http://127.0.0.1:8000/user/rsj217
Hello rsj217%                                                                 ☁  ~  curl http://127.0.0.1:8000/user/rsj217/
404 page not found%                                                               ☁  ~  curl http://127.0.0.1:8000/user/
404 page not found%
```

除了`:`，gin还提供了`*`号处理参数，`*`号能匹配的规则就更多。

```
func main(){
    router := gin.Default()
    
    router.GET("/user/:name/*action", func(c *gin.Context) {
        name := c.Param("name")
        action := c.Param("action")
        message := name + " is " + action
        c.String(http.StatusOK, message)
    })
}
```

访问效果如下

```
☁  ~  curl http://127.0.0.1:8000/user/rsj217/
rsj217 is /%                                                                  ☁  ~  curl http://127.0.0.1:8000/user/rsj217/中国
rsj217 is /中国%
```

### query string参数与body参数

web提供的服务通常是client和server的交互。其中客户端向服务器发送请求，除了路由参数，其他的参数无非两种，查询字符串query string和报文体body参数。所谓query string，即路由用，用`?`以后连接的`key1=value2&key2=value2`的形式的参数。当然这个key-value是经过urlencode编码。

#### query string

对于参数的处理，经常会出现参数不存在的情况，对于是否提供默认值，gin也考虑了，并且给出了一个优雅的方案：

```
func main(){
    router := gin.Default()
    router.GET("/welcome", func(c *gin.Context) {
        firstname := c.DefaultQuery("firstname", "Guest")
        lastname := c.Query("lastname")

        c.String(http.StatusOK, "Hello %s %s", firstname, lastname)
    })
  router.Run()
}
```

使用c.DefaultQuery方法读取参数，其中当参数不存在的时候，提供一个默认值。使用Query方法读取正常参数，当参数不存在的时候，返回空字串：

```
☁  ~  curl http://127.0.0.1:8000/welcome
Hello Guest %                                                                 ☁  ~  curl http://127.0.0.1:8000/welcome\?firstname\=中国
Hello 中国 %                                                                  ☁  ~  curl http://127.0.0.1:8000/welcome\?firstname\=中国\&lastname\=天朝
Hello 中国 天朝%                                                              ☁  ~  curl http://127.0.0.1:8000/welcome\?firstname\=\&lastname\=天朝
Hello  天朝%
☁  ~  curl http://127.0.0.1:8000/welcome\?firstname\=%E4%B8%AD%E5%9B%BD
Hello 中国 %
```

之所以使用中文，是为了说明urlencode。注意，当firstname为空字串的时候，并不会使用默认的Guest值，空值也是值，DefaultQuery只作用于key不存在的时候，提供默认值。

#### body

http的报文体传输数据就比query string稍微复杂一点，常见的格式就有四种。例如`application/json`，`application/x-www-form-urlencoded`, `application/xml`和`multipart/form-data`。后面一个主要用于图片上传。json格式的很好理解，urlencode其实也不难，无非就是把query string的内容，放到了body体里，同样也需要urlencode。默认情况下，c.PostFROM解析的是`x-www-form-urlencoded`或`from-data`的参数。

```
func main(){
    router := gin.Default()
    router.POST("/form_post", func(c *gin.Context) {
        message := c.PostForm("message")
        nick := c.DefaultPostForm("nick", "anonymous")

        c.JSON(http.StatusOK, gin.H{
            "status":  gin.H{
                "status_code": http.StatusOK,
                "status":      "ok",
            },
            "message": message,
            "nick":    nick,
        })
    })
}
```

与get处理query参数一样，post方法也提供了处理默认参数的情况。同理，如果参数不存在，将会得到空字串。

```
☁  ~  curl -X POST http://127.0.0.1:8000/form_post -H "Content-Type:application/x-www-form-urlencoded" -d "message=hello&nick=rsj217" | python -m json.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   104  100    79  100    25  48555  15365 --:--:-- --:--:-- --:--:-- 79000
{
    "message": "hello",
    "nick": "rsj217",
    "status": {
        "status": "ok",
        "status_code": 200
    }
}
```

> 前面我们使用c.String返回响应，顾名思义则返回string类型。content-type是plain或者text。调用c.JSON则返回json数据。其中gin.H封装了生成json的方式，是一个强大的工具。使用golang可以像动态语言一样写字面量的json，对于嵌套json的实现，嵌套gin.H即可。

发送数据给服务端，并不是post方法才行，put方法一样也可以。同时querystring和body也不是分开的，两个同时发送也可以：

```
func main(){
    router := gin.Default()
    
    router.PUT("/post", func(c *gin.Context) {
        id := c.Query("id")
        page := c.DefaultQuery("page", "0")
        name := c.PostForm("name")
        message := c.PostForm("message")
        fmt.Printf("id: %s; page: %s; name: %s; message: %s \n", id, page, name, message)
        c.JSON(http.StatusOK, gin.H{
            "status_code": http.StatusOK,
        })
    })
}
```

上面的例子，展示了同时使用查询字串和body参数发送数据给服务器。

### 文件上传

#### 上传单个文件

前面介绍了基本的发送数据，其中`multipart/form-data`转用于文件上传。gin文件上传也很方便，和原生的net/http方法类似，不同在于gin把原生的request封装到c.Request中了。

```
func main(){
    router := gin.Default()
    
    router.POST("/upload", func(c *gin.Context) {
        name := c.PostForm("name")
        fmt.Println(name)
        file, header, err := c.Request.FormFile("upload")
        if err != nil {
            c.String(http.StatusBadRequest, "Bad request")
            return
        }
        filename := header.Filename

        fmt.Println(file, err, filename)

        out, err := os.Create(filename)
        if err != nil {
            log.Fatal(err)
        }
        defer out.Close()
        _, err = io.Copy(out, file)
        if err != nil {
            log.Fatal(err)
        }
        c.String(http.StatusCreated, "upload successful")
    })
    router.Run(":8000")
}
```

使用`c.Request.FormFile`解析客户端文件name属性。如果不传文件，则会抛错，因此需要处理这个错误。一种方式是直接返回。然后使用os的操作，把文件数据复制到硬盘上。

使用下面的命令可以测试上传，注意upload为`c.Request.FormFile`指定的参数，其值必须要是绝对路径：

```
curl -X POST http://127.0.0.1:8000/upload -F "upload=@/Users/ghost/Desktop/pic.jpg" -H "Content-Type: multipart/form-data"
```

#### 上传多个文件

单个文件上传很简单，别以为多个文件就会很麻烦。依葫芦画瓢，所谓多个文件，无非就是多一次遍历文件，然后一次copy数据存储即可。下面只写handler，省略main函数的初始化路由和开启服务器监听了：

```
router.POST("/multi/upload", func(c *gin.Context) {
        err := c.Request.ParseMultipartForm(200000)
        if err != nil {
            log.Fatal(err)
        }

        formdata := c.Request.MultipartForm 

        files := formdata.File["upload"] 
        for i, _ := range files { /
            file, err := files[i].Open()
            defer file.Close()
            if err != nil {
                log.Fatal(err)
            }

            out, err := os.Create(files[i].Filename)

            defer out.Close()

            if err != nil {
                log.Fatal(err)
            }

            _, err = io.Copy(out, file)

            if err != nil {
                log.Fatal(err)
            }

            c.String(http.StatusCreated, "upload successful")

        }

    })
```

与单个文件上传类似，只不过使用了`c.Request.MultipartForm`得到文件句柄，再获取文件数据，然后遍历读写。

使用curl上传

```
curl -X POST http://127.0.0.1:8000/multi/upload -F "upload=@/Users/ghost/Desktop/pic.jpg" -F "upload=@/Users/ghost/Desktop/journey.png" -H "Content-Type: multipart/form-data"
```

#### 表单上传

上面我们使用的都是curl上传，实际上，用户上传图片更多是通过表单，或者ajax和一些requests的请求完成。下面展示一下web的form表单如何上传。

我们先要写一个表单页面，因此需要引入gin如何render模板。前面我们见识了c.String和c.JSON。下面就来看看c.HTML方法。

首先需要定义一个模板的文件夹。然后调用c.HTML渲染模板，可以通过gin.H给模板传值。至此，无论是String，JSON还是HTML，以及后面的XML和YAML，都可以看到Gin封装的接口简明易用。

创建一个文件夹templates，然后再里面创建html文件upload.html:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>upload</title>
</head>
<body>
<h3>Single Upload</h3>
<form action="/upload", method="post" enctype="multipart/form-data">
    <input type="text" value="hello gin" />
    <input type="file" name="upload" />
    <input type="submit" value="upload" />
</form>


<h3>Multi Upload</h3>
<form action="/multi/upload", method="post" enctype="multipart/form-data">
    <input type="text" value="hello gin" />
    <input type="file" name="upload" />
    <input type="file" name="upload" />
    <input type="submit" value="upload" />
</form>

</body>
</html>
```

upload 很简单，没有参数。一个用于单个文件上传，一个用于多个文件上传。

```
    router.LoadHTMLGlob("templates/*")
    router.GET("/upload", func(c *gin.Context) {
        c.HTML(http.StatusOK, "upload.html", gin.H{})
    })
```

使用LoadHTMLGlob定义模板文件路径。

### 参数绑定

我们已经见识了`x-www-form-urlencoded`类型的参数处理，现在越来越多的应用习惯使用JSON来通信，也就是无论返回的response还是提交的request，其content-type类型都是`application/json`的格式。而对于一些旧的web表单页还是`x-www-form-urlencoded`的形式，这就需要我们的服务器能改hold住这多种content-type的参数了。

Python的世界里很好解决，毕竟动态语言不需要实现定义数据模型。因此可以写一个装饰器将两个格式的数据封装成一个数据模型。golang中要处理并非易事，好在有gin，他们的model bind功能非常强大。

```
type User struct {
    Username string `form:"username" json:"username" binding:"required"`
    Passwd   string `form:"passwd" json:"passwd" bdinding:"required"`
    Age      int    `form:"age" json:"age"`
}

func main(){
    router := gin.Default()
    
    router.POST("/login", func(c *gin.Context) {
        var user User
        var err error
        contentType := c.Request.Header.Get("Content-Type")

        switch contentType {
        case "application/json":
            err = c.BindJSON(&user)
        case "application/x-www-form-urlencoded":
            err = c.BindWith(&user, binding.Form)
        }

        if err != nil {
            fmt.Println(err)
            log.Fatal(err)
        }

        c.JSON(http.StatusOK, gin.H{
            "user":   user.Username,
            "passwd": user.Passwd,
            "age":    user.Age,
        })

    })

}
```

先定义一个`User`模型结构体，然后针对客户端的content-type，一次使`BindJSON`和`BindWith`方法。

```
☁  ~  curl -X POST http://127.0.0.1:8000/login -H "Content-Type:application/x-www-form-urlencoded" -d "username=rsj217&passwd=123&age=21" | python -m json.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    79  100    46  100    33  41181  29543 --:--:-- --:--:-- --:--:-- 46000
{
    "age": 21,
    "passwd": "123",
    "username": "rsj217"
}
☁  ~  curl -X POST http://127.0.0.1:8000/login -H "Content-Type:application/x-www-form-urlencoded" -d "username=rsj217&passwd=123&new=21" | python -m json.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    78  100    45  100    33  37751  27684 --:--:-- --:--:-- --:--:-- 45000
{
    "age": 0,
    "passwd": "123",
    "username": "rsj217"
}
☁  ~  curl -X POST http://127.0.0.1:8000/login -H "Content-Type:application/x-www-form-urlencoded" -d "username=rsj217&new=21" | python -m json.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0curl: (52) Empty reply from server
No JSON object could be decoded
```

可以看到，结构体中，设置了binding标签的字段（username和passwd），如果没传会抛错误。非banding的字段（age），对于客户端没有传，User结构会用零值填充。对于User结构没有的参数，会自动被忽略。

改成json的效果类似：

```
☁  ~  curl -X POST http://127.0.0.1:8000/login -H "Content-Type:application/json" -d '{"username": "rsj217", "passwd": "123", "age": 21}' | python -m json.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    96  100    46  100    50  32670  35511 --:--:-- --:--:-- --:--:-- 50000
{
    "age": 21,
    "passwd": "123",
    "username": "rsj217"
}
☁  ~  curl -X POST http://127.0.0.1:8000/login -H "Content-Type:application/json" -d '{"username": "rsj217", "passwd": "123", "new": 21}' | python -m json.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    95  100    45  100    50  49559  55066 --:--:-- --:--:-- --:--:-- 50000
{
    "age": 0,
    "passwd": "123",
    "username": "rsj217"
}
☁  ~  curl -X POST http://127.0.0.1:8000/login -H "Content-Type:application/json" -d '{"username": "rsj217", "new": 21}' | python -m json.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0curl: (52) Empty reply from server
No JSON object could be decoded
☁  ~  curl -X POST http://127.0.0.1:8000/login -H "Content-Type:application/json" -d '{"username": "rsj217", "passwd": 123, "new": 21}' | python -m json.tool
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0curl: (52) Empty reply from server
No JSON object could be decoded
```

> 使用json还需要注意一点，json是有数据类型的，因此对于 `{"passwd": "123"}` 和 `{"passwd": 123}`是不同的数据类型，解析需要符合对应的数据类型,否则会出错。

当然，gin还提供了更加高级方法，c.Bind，它会更加content-type自动推断是bind表单还是json的参数。

```
    router.POST("/login", func(c *gin.Context) {
        var user User
        
        err := c.Bind(&user)
        if err != nil {
            fmt.Println(err)
            log.Fatal(err)
        }

        c.JSON(http.StatusOK, gin.H{
            "username":   user.Username,
            "passwd":     user.Passwd,
            "age":        user.Age,
        })

    })
```

### 多格式渲染

既然请求可以使用不同的content-type，响应也如此。通常响应会有html，text，plain，json和xml等。 gin提供了很优雅的渲染方法。到目前为止，我们已经见识了c.String， c.JSON，c.HTML，下面介绍一下c.XML。

```
    router.GET("/render", func(c *gin.Context) {
        contentType := c.DefaultQuery("content_type", "json")
        if contentType == "json" {
            c.JSON(http.StatusOK, gin.H{
                "user":   "rsj217",
                "passwd": "123",
            })
        } else if contentType == "xml" {
            c.XML(http.StatusOK, gin.H{
                "user":   "rsj217",
                "passwd": "123",
            })
        }

    })
```

结果如下：

```
☁  ~  curl  http://127.0.0.1:8000/render\?content_type\=json
{"passwd":"123","user":"rsj217"}
☁  ~  curl  http://127.0.0.1:8000/render\?content_type\=xml
<map><user>rsj217</user><passwd>123</passwd></map>%
```

### 重定向

gin对于重定向的请求，相当简单。调用上下文的Redirect方法：

```
    router.GET("/redict/google", func(c *gin.Context) {
        c.Redirect(http.StatusMovedPermanently, "https://google.com")
    })
```

### 分组路由

熟悉Flask的同学应该很了解蓝图分组。Flask提供了蓝图用于管理组织分组api。gin也提供了这样的功能，让你的代码逻辑更加模块化，同时分组也易于定义中间件的使用范围。

```
    v1 := router.Group("/v1")

    v1.GET("/login", func(c *gin.Context) {
        c.String(http.StatusOK, "v1 login")
    })

    v2 := router.Group("/v2")

    v2.GET("/login", func(c *gin.Context) {
        c.String(http.StatusOK, "v2 login")
    })
```

访问效果如下：

```
☁  ~  curl http://127.0.0.1:8000/v1/login
v1 login%                                                                                 ☁  ~  curl http://127.0.0.1:8000/v2/login
v2 login%
```

### middleware中间件

golang的net/http设计的一大特点就是特别容易构建中间件。gin也提供了类似的中间件。需要注意的是中间件只对注册过的路由函数起作用。对于分组路由，嵌套使用中间件，可以限定中间件的作用范围。中间件分为全局中间件，单个路由中间件和群组中间件。

#### 全局中间件

先定义一个中间件函数：

```
func MiddleWare() gin.HandlerFunc {
    return func(c *gin.Context) {
        fmt.Println("before middleware")
        c.Set("request", "clinet_request")
        c.Next()
        fmt.Println("before middleware")
    }
}
```

该函数很简单，只会给c上下文添加一个属性，并赋值。**后面**的路由处理器，可以根据被中间件装饰后提取其值。需要注意，虽然名为全局中间件，只要注册中间件的过程之前设置的路由，将不会受注册的中间件所影响。只有注册了中间件一下代码的路由函数规则，才会被中间件装饰。

```
    router.Use(MiddleWare())
    {
        router.GET("/middleware", func(c *gin.Context) {
            request := c.MustGet("request").(string)
            req, _ := c.Get("request")
            c.JSON(http.StatusOK, gin.H{
                "middile_request": request,
                "request": req,
            })
        })
    }
```

使用router装饰中间件，然后在`/middlerware`即可读取request的值，注意在`router.Use(MiddleWare())`代码以上的路由函数，将不会有被中间件装饰的效果。

> 使用花括号包含被装饰的路由函数只是一个代码规范，即使没有被包含在内的路由函数，只要使用router进行路由，都等于被装饰了。想要区分权限范围，可以使用组返回的对象注册中间件。

```
☁  ~  curl  http://127.0.0.1:8000/middleware
{"middile_request":"clinet_request","request":"clinet_request"}
```

如果没有注册就使用`MustGet`方法读取c的值将会抛错，可以使用Get方法取而代之。

上面的注册装饰方式，会让所有下面所写的代码都默认使用了router的注册过的中间件。

#### 单个路由中间件

当然，gin也提供了针对指定的路由函数进行注册。

```
    router.GET("/before", MiddleWare(), func(c *gin.Context) {
        request := c.MustGet("request").(string)
        c.JSON(http.StatusOK, gin.H{
            "middile_request": request,
        })
    })
```

把上述代码写在 router.Use(Middleware())之前，同样也能看见`/before`被装饰了中间件。

#### 群组中间件

群组的中间件也类似，只要在对于的群组路由上注册中间件函数即可：

```
authorized := router.Group("/", MyMiddelware())
// 或者这样用：
authorized := router.Group("/")
authorized.Use(MyMiddelware())
{
    authorized.POST("/login", loginEndpoint)
}
```

群组可以嵌套，因为中间件也可以根据群组的嵌套规则嵌套。

### 中间件实践

中间件最大的作用，莫过于用于一些记录log，错误handler，还有就是对部分接口的鉴权。下面就实现一个简易的鉴权中间件。

```
    router.GET("/auth/signin", func(c *gin.Context) {
        cookie := &http.Cookie{
            Name:     "session_id",
            Value:    "123",
            Path:     "/",
            HttpOnly: true,
        }
        http.SetCookie(c.Writer, cookie)
        c.String(http.StatusOK, "Login successful")
    })

    router.GET("/home", AuthMiddleWare(), func(c *gin.Context) {
        c.JSON(http.StatusOK, gin.H{"data": "home"})
    })
```

登录函数会设置一个session_id的cookie，注意这里需要指定path为`/`，不然gin会自动设置cookie的path为`/auth`，一个特别奇怪的问题。`/homne`的逻辑很简单，使用中间件AuthMiddleWare注册之后，将会先执行AuthMiddleWare的逻辑，然后才到`/home`的逻辑。

AuthMiddleWare的代码如下：

```
func AuthMiddleWare() gin.HandlerFunc {
    return func(c *gin.Context) {
        if cookie, err := c.Request.Cookie("session_id"); err == nil {
            value := cookie.Value
            fmt.Println(value)
            if value == "123" {
                c.Next()
                return
            }
        }
        c.JSON(http.StatusUnauthorized, gin.H{
            "error": "Unauthorized",
        })
        c.Abort()
        return
    }
}
```

从上下文的请求中读取cookie，然后校对cookie，如果有问题，则终止请求，直接返回，这里使用了c.Abort()方法。

```
In [7]: resp = requests.get('http://127.0.0.1:8000/home')

In [8]: resp.json()
Out[8]: {u'error': u'Unauthorized'}

In [9]: login = requests.get('http://127.0.0.1:8000/auth/signin')

In [10]: login.cookies
Out[10]: <RequestsCookieJar[Cookie(version=0, name='session_id', value='123', port=None, port_specified=False, domain='127.0.0.1', domain_specified=False, domain_initial_dot=False, path='/', path_specified=True, secure=False, expires=None, discard=True, comment=None, comment_url=None, rest={'HttpOnly': None}, rfc2109=False)]>

In [11]: resp = requests.get('http://127.0.0.1:8000/home', cookies=login.cookies)

In [12]: resp.json()
Out[12]: {u'data': u'home'}
```

### 异步协程

golang的高并发一大利器就是协程。gin里可以借助协程实现异步任务。因为涉及异步过程，请求的上下文需要copy到异步的上下文，并且这个上下文是只读的。

```
    router.GET("/sync", func(c *gin.Context) {
        time.Sleep(5 * time.Second)
        log.Println("Done! in path" + c.Request.URL.Path)
    })

    router.GET("/async", func(c *gin.Context) {
        cCp := c.Copy()
        go func() {
            time.Sleep(5 * time.Second)
            log.Println("Done! in path" + cCp.Request.URL.Path)
        }()
    })
```

在请求的时候，sleep5秒钟，同步的逻辑可以看到，服务的进程睡眠了。异步的逻辑则看到响应返回了，然后程序还在**后台**的协程处理。

### 自定义router

gin不仅可以使用框架本身的router进行Run，也可以配合使用net/http本身的功能：

```
func main() {
    router := gin.Default()
    http.ListenAndServe(":8080", router)
}
```

或者

```
func main() {
    router := gin.Default()

    s := &http.Server{
        Addr:           ":8000",
        Handler:        router,
        ReadTimeout:    10 * time.Second,
        WriteTimeout:   10 * time.Second,
        MaxHeaderBytes: 1 << 20,
    }
    s.ListenAndServe()
}
```

当然还有一个优雅的重启和结束进程的方案。后面将会探索使用supervisor管理golang的进程。

### 总结

Gin是一个轻巧而强大的golang web框架。涉及常见开发的功能，我们都做了简单的介绍。关于服务的启动，请求参数的处理和响应格式的渲染，以及针对上传和中间件鉴权做了例子。更好的掌握来自实践，同时gin的源码注释很详细，可以阅读源码了解更多详细的功能和魔法特性。

文中的部分[代码](https://link.jianshu.com/?t=https://gist.github.com/rsj217/26492af115a083876570f003c64df118)