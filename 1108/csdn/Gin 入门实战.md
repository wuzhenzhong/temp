# Gin 入门实战

2019-06-14 18:26:09  更多



版权声明：本文为博主原创文章，遵循[ CC 4.0 BY-SA ](http://creativecommons.org/licenses/by-sa/4.0/)版权协议，转载请附上原文出处链接和本声明。本文链接：https://blog.csdn.net/e421083458/article/details/91994788



### 目录

- [Gin 入门实战](https://blog.csdn.net/e421083458/article/details/91994788#Gin__1)

- - [Agenda](https://blog.csdn.net/e421083458/article/details/91994788#Agenda_2)

- [拨开云雾见天日](https://blog.csdn.net/e421083458/article/details/91994788#_7)

- - [1-1 前置知识](https://blog.csdn.net/e421083458/article/details/91994788#11__9)
  - [1-2 golang开发环境准备](https://blog.csdn.net/e421083458/article/details/91994788#12_golang_38)
  - [1-2 go mod 包管理工具](https://blog.csdn.net/e421083458/article/details/91994788#12_go_mod__57)

- [万丈高楼平地起](https://blog.csdn.net/e421083458/article/details/91994788#_61)

- - [2-1 安装gin及快速开始](https://blog.csdn.net/e421083458/article/details/91994788#21_gin_63)

  - [2-2 请求路由](https://blog.csdn.net/e421083458/article/details/91994788#22__85)

  - [2-3 获取请求参数](https://blog.csdn.net/e421083458/article/details/91994788#23__172)

  - [2-4 验证请求参数](https://blog.csdn.net/e421083458/article/details/91994788#24__300)

  - [2-5 中间件](https://blog.csdn.net/e421083458/article/details/91994788#25__511)

  - [2-6 其他补充](https://blog.csdn.net/e421083458/article/details/91994788#26__592)

  - [2-7 构建企业级脚手架](https://blog.csdn.net/e421083458/article/details/91994788#27__723)

  - - [现在开始](https://blog.csdn.net/e421083458/article/details/91994788#_730)
    - [文件分层](https://blog.csdn.net/e421083458/article/details/91994788#_798)
    - [引入轻量级golang类库，支持 mysql、redis、http.client、log、支持多级环境配置、支持链路日志打印](https://blog.csdn.net/e421083458/article/details/91994788#golang_mysqlredishttpclientlog_831)
    - [输出格式统一封装](https://blog.csdn.net/e421083458/article/details/91994788#_859)
    - [定义中间件链路日志打印](https://blog.csdn.net/e421083458/article/details/91994788#_888)
    - [请求数据绑定到结构体与校验](https://blog.csdn.net/e421083458/article/details/91994788#_959)

- [秤砣虽小压千斤](https://blog.csdn.net/e421083458/article/details/91994788#_1013)

- - [3-1 用户管理系统设计](https://blog.csdn.net/e421083458/article/details/91994788#31__1015)

  - - [功能点](https://blog.csdn.net/e421083458/article/details/91994788#_1018)
    - [数据库](https://blog.csdn.net/e421083458/article/details/91994788#_1022)
    - [后端准备](https://blog.csdn.net/e421083458/article/details/91994788#_1037)
    - [前端准备](https://blog.csdn.net/e421083458/article/details/91994788#_1040)

  - [3-2 实战开发](https://blog.csdn.net/e421083458/article/details/91994788#32__1048)

- [课程总结](https://blog.csdn.net/e421083458/article/details/91994788#_1104)



# Gin 入门实战

## Agenda

- 拨开云雾见天日：前置知识讲解
- 万丈高楼平地起：基础中的精髓 及 搭建企业级golang脚手架
- 秤砣虽小压千斤：实战学习开发用户管理系统

# 拨开云雾见天日

## 1-1 前置知识

- Go开发web的优势
  在 Go 语言出现之前，开发者们总是面临非常艰难的抉择，
  究竟是使用执行速度快但是编译速度并不理想的语言（如：C++），还是
  使用编译速度较快但执行效率不佳的语言（如：.NET、Java），
  开发难度较低但执行速度一般的动态语言呢？(如：Python)
  显然，Go 语言在这 3 个条件之间做到了最佳的平衡：快速编译，高效执行，易于开发。
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/2019063020580180.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2U0MjEwODM0NTg=,size_16,color_FFFFFF,t_70)
- gin是什么？
  Gin 框架 现在是 github 上 start 最多 Go 语言编写的 Web 框架，相比其他它的几个 start 数量差不多的框架，它更加的轻量，有更好的性能。
- 性能对比

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190614183218411.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2U0MjEwODM0NTg=,size_16,color_FFFFFF,t_70)

1. 在固定时间内重复完成的总数，数值越高的是越好的结果
2. 单次重复持续时间（ns /op），越低越好
3. 堆内存（B /op），越低越好
4. 每次重复的平均分配（allocs /op），越低越好

从内存分配、单词相应时间、Qps三个纬度分析。gin基本是碾压其他对手的。

- httprouter 为gin插上了翅膀

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190614183240998.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2U0MjEwODM0NTg=,size_16,color_FFFFFF,t_70)

https://chai2010.gitbooks.io/advanced-go-programming-book/ch5-web/ch5-02-router.html
https://www.cnblogs.com/foxy/p/9469401.html

## 1-2 golang开发环境准备

```
下载与解压
wget https://dl.google.com/go/go1.11.4.linux-amd64.tar.gz
tar -xvf go1.11.4.linux-amd64.tar.gz

修改环境变量
vim ~/.bash_profile

export GOROOT=/root/go
export GOPATH=/root/go_path
export GOBIN=$GOROOT/bin
export PATH=$GOBIN:$GOROOT/bin:$GOPATH/bin:$PATH
source ~/.bash_profile

查看go的版本号
go version
123456789101112131415
```

## 1-2 go mod 包管理工具

https://blog.csdn.net/e421083458/article/details/89762113

# 万丈高楼平地起

## 2-1 安装gin及快速开始

测试使用版本：
[github.com/gin-gonic/gin](http://github.com/gin-gonic/gin) v1.4.0

```
go get -v github.com/gin-gonic/gin

package main

import "github.com/gin-gonic/gin"

func main() {
	r := gin.Default()
	r.GET("/ping", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "pong",
		})
	})
	r.Run() // listen and serve on 0.0.0.0:8080
}
123456789101112131415
```

## 2-2 请求路由

- 设置多种请求类型

```
func main() {
	// Creates a gin router with default middleware:
	// logger and recovery (crash-free) middleware
	router := gin.Default()

	router.GET("/someGet", getting)
	router.POST("/somePost", posting)
	router.PUT("/somePut", putting)
	router.DELETE("/someDelete", deleting)
	router.PATCH("/somePatch", patching)
	router.HEAD("/someHead", head)
	router.OPTIONS("/someOptions", options)

	// By default it serves on :8080 unless a
	// PORT environment variable was defined.
	router.Run()
	// router.Run(":3000") for a hard coded port
}

12345678910111213141516171819
```

- 绑定静态文件夹

```
func main() {
	router := gin.Default()
	router.Static("/assets", "./assets")
	router.StaticFS("/more_static", http.Dir("my_file_system"))
	router.StaticFile("/favicon.ico", "./resources/favicon.ico")

	// Listen and serve on 0.0.0.0:8080
	router.Run(":8080")
}
123456789
```

- 参数作为URL

```
package main

import "github.com/gin-gonic/gin"

type Person struct {
	ID string `uri:"id" binding:"required,uuid"`
	Name string `uri:"name" binding:"required"`
}

func main() {
	route := gin.Default()
	route.GET("/:name/:id", func(c *gin.Context) {
		var person Person
		if err := c.ShouldBindUri(&person); err != nil {
			c.JSON(400, gin.H{"msg": err})
			return
		}
		c.JSON(200, gin.H{"name": person.Name, "uuid": person.ID})
	})
	route.Run(":8088")
}
123456789101112131415161718192021
```

- 正则绑定

```
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	router := gin.Default()

	router.GET("/user/*action", func(c *gin.Context) {
		c.String(http.StatusOK, "Hello world")
	})

	router.Run(":8080")
}
12345678910111213141516
```

## 2-3 获取请求参数

- 获取GET请求参数

```
func main() {
	router := gin.Default()

	// Query string parameters are parsed using the existing underlying request object.
	// The request responds to a url matching:  /welcome?firstname=Jane&lastname=Doe
	router.GET("/welcome", func(c *gin.Context) {
		firstname := c.DefaultQuery("firstname", "Guest")
		lastname := c.Query("lastname") // shortcut for c.Request.URL.Query().Get("lastname")

		c.String(http.StatusOK, "Hello %s %s", firstname, lastname)
	})
	router.Run(":8080")
}
12345678910111213
```

- 获取POST请求参数

```
func main() {
	router := gin.Default()

	router.POST("/form_post", func(c *gin.Context) {
		message := c.PostForm("message")
		nick := c.DefaultPostForm("nick", "anonymous")

		c.JSON(200, gin.H{
			"status":  "posted",
			"message": message,
			"nick":    nick,
		})
	})
	router.Run(":8080")
}
123456789101112131415
```

- 获取Body值

```
package main

import (
	"github.com/gin-gonic/gin"
	"io/ioutil"
	"net/http"
)

func main() {
	router := gin.Default()
	
	router.POST("/user/*action", func(c *gin.Context) {
		bodyBytes, _ := ioutil.ReadAll(c.Request.Body)
		c.String(http.StatusOK, string(bodyBytes))
	})
	router.Run(":8080")
}
1234567891011121314151617
```

测试效果：

```
#form-urlencode
curl 'http://127.0.0.1:8080/user/adsadsad' -F "key=value"
--------------------------88baf4eac8fd4ba1
Content-Disposition: form-data; name="key"

value
--------------------------88baf4eac8fd4ba1--

#form-data
curl 'http://127.0.0.1:8080/user/adsadsad' -d "key=value"
key=value
1234567891011
```

- bind绑定结构体获取参数（Get&POST&POST_BODY）

> 只需要在结构体上设置form标签即可

```
package main

import (
	"fmt"
	"log"
	"time"

	"github.com/gin-gonic/gin"
)

type Person struct {
	Name     string    `form:"name"`
	Address  string    `form:"address"`
	Birthday time.Time `form:"birthday" time_format:"2006-01-02" time_utc:"1"`
}

func main() {
	route := gin.Default()
	route.GET("/testing", startPage)
	route.POST("/testing", startPage)
	route.Run(":8085")
}

func startPage(c *gin.Context) {
	var person Person
	if c.ShouldBind(&person) == nil {
		log.Println(person.Name)
		log.Println(person.Address)
		log.Println(person.Birthday)
	}
	c.String(200, fmt.Sprintf("%#v",person))
}
1234567891011121314151617181920212223242526272829303132
```

测试

```
curl 'http://127.0.0.1:8085/testing?name=1&address=2&birthday=2006-01-02'

main.Person{Name:"1", Address:"2", Birthday:time.Time{wall:0x0, ext:63271756800, loc:(*time.Location)(nil)}}

curl 'http://127.0.0.1:8085/testing' -d 'name=5566&address=3344&birthday=2006-01-02'
main.Person{Name:"5566", Address:"3344", Birthday:time.Time{wall:0x0, ext:63271756800, loc:(*time.Location)(nil)}}%
123456
```

## 2-4 验证请求参数

- 结构体binding验证

gin默认使用validator.v8作为验证器。

多种验证规则请参见：

https://github.com/go-playground/validator/tree/v8

https://godoc.org/gopkg.in/go-playground/validator.v8#hdr-Using_Validator_Tags

> 注意点：gin必须使用 binding tag 来设置校验规则，而不是 validate。

```
package main

import (
	"fmt"
	"time"

	"github.com/gin-gonic/gin"
)

type Person struct {
	Name     string    `form:"name" binding:"required"`
	Address  string    `form:"address"`
	Birthday time.Time `form:"birthday" time_format:"2006-01-02" time_utc:"1"`
}

func main() {
	route := gin.Default()
	route.GET("/testing", startPage)
	route.Run(":8085")
}

func startPage(c *gin.Context) {
	var person Person
	if err:=c.ShouldBind(&person);err!=nil {
		c.String(500,fmt.Sprint(err))
	}
	c.String(200, fmt.Sprintf("%#v",person))
}
12345678910111213141516171819202122232425262728
```

测试

```
curl 'http://127.0.0.1:8085/testing' -d 'name=&address=3344&birthday=2006-01-02&age=1'
Key: 'Person.Age' Error:Field validation for 'Age' failed on the 'gt' tag
Key: 'Person.Name' Error:Field validation for 'Name' failed on the 'required' tagmain.Person{Age:1, Name:"", Address:"3344", Birthday:time.Time{wall:0x0, ext:63271756800, loc:(*time.Location)(nil)}}

curl 'http://127.0.0.1:8085/testing' -d 'name=5566&address=3344&birthday=2006-01-02&age=1'
Key: 'Person.Age' Error:Field validation for 'Age' failed on the 'gt' tagmain.Person{Age:1, Name:"5566", Address:"3344", Birthday:time.Time{wall:0x0, ext:63271756800, loc:(*time.Location)(nil)}}
123456
```

- 自定义验证

```
package main

import (
	"net/http"
	"reflect"
	"time"

	"github.com/gin-gonic/gin"
	"github.com/gin-gonic/gin/binding"
	"gopkg.in/go-playground/validator.v8"
)

// Booking contains binded and validated data.
type Booking struct {
	CheckIn  time.Time `form:"check_in" binding:"required,bookabledate" time_format:"2006-01-02"`
	CheckOut time.Time `form:"check_out" binding:"required,gtfield=CheckIn" time_format:"2006-01-02"`
}

func bookableDate(
	v *validator.Validate, topStruct reflect.Value, currentStructOrField reflect.Value,
	field reflect.Value, fieldType reflect.Type, fieldKind reflect.Kind, param string,
) bool {
	if date, ok := field.Interface().(time.Time); ok {
		today := time.Now()
		if today.Year() > date.Year() || today.YearDay() > date.YearDay() {
			return false
		}
	}
	return true
}

func main() {
	route := gin.Default()

	if v, ok := binding.Validator.Engine().(*validator.Validate); ok {
		v.RegisterValidation("bookabledate", bookableDate)
	}

	route.GET("/bookable", getBookable)
	route.Run(":8085")
}

func getBookable(c *gin.Context) {
	var b Booking
	if err := c.ShouldBindWith(&b, binding.Query); err == nil {
		c.JSON(http.StatusOK, gin.H{"message": "Booking dates are valid!"})
	} else {
		c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
	}
}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950
$ curl "localhost:8085/bookable?check_in=2018-04-16&check_out=2018-04-17"
{"message":"Booking dates are valid!"}

$ curl "localhost:8085/bookable?check_in=2018-03-08&check_out=2018-03-09"
{"error":"Key: 'Booking.CheckIn' Error:Field validation for 'CheckIn' failed on the 'bookabledate' tag"}
12345
```

- 升级v9验证，支持多语言错误信息

> 注意v9验证的话，标签要使用 validate ，也是为了与v8的标签分开。

```
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
	"github.com/go-playground/locales/en"
	"github.com/go-playground/locales/zh"
	"github.com/go-playground/locales/zh_Hant_TW"
	"github.com/go-playground/universal-translator"
	"gopkg.in/go-playground/validator.v9"
	zh_translations "gopkg.in/go-playground/validator.v9/translations/zh"
	en_translations "gopkg.in/go-playground/validator.v9/translations/en"
	zh_tw_translations "gopkg.in/go-playground/validator.v9/translations/zh_tw"
)

var (
	Uni      *ut.UniversalTranslator
	Validate *validator.Validate
)

type User struct {
	Username string `form:"user_name" validate:"required"`
	Tagline  string `form:"tag_line" validate:"required,lt=10"`
	Tagline2 string `form:"tag_line2" validate:"required,gt=1"`
}

func main() {
	en := en.New()
	zh := zh.New()
	zh_tw := zh_Hant_TW.New()
	Uni = ut.New(en, zh, zh_tw)
	Validate = validator.New()

	route := gin.Default()
	route.GET("/testing", startPage)
	route.POST("/testing", startPage)
	route.Run(":8085")
}

func startPage(c *gin.Context) {
	//这部分应该放到中间件中
	locale:=c.DefaultQuery("locale","zh")
	trans, _ := Uni.GetTranslator(locale)
	switch locale {
	case "zh":
		zh_translations.RegisterDefaultTranslations(Validate, trans)
		break
	case "en":
		en_translations.RegisterDefaultTranslations(Validate, trans)
		break
	case "zh_tw":
		zh_tw_translations.RegisterDefaultTranslations(Validate, trans)
		break
	default:
		zh_translations.RegisterDefaultTranslations(Validate, trans)
		break
	}

	//自定义错误内容
	Validate.RegisterTranslation("required", trans, func(ut ut.Translator) error {
		return ut.Add("required", "{0} must have a value!", true) // see universal-translator for details
	}, func(ut ut.Translator, fe validator.FieldError) string {
		t, _ := ut.T("required", fe.Field())
		return t
	})

	//这块应该放到公共验证方法中
	user := User{}
	c.ShouldBind(&user)
	fmt.Println(user)
	err := Validate.Struct(user)
	if err != nil {
		errs := err.(validator.ValidationErrors)
		sliceErrs:=[]string{}
		for _, e := range errs {
			sliceErrs=append(sliceErrs,e.Translate(trans))
		}
		c.String(200,fmt.Sprintf("%#v",sliceErrs))
	}
	c.String(200, fmt.Sprintf("%#v",""))
}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081
```

测试

```
http://127.0.0.1:8085/testing?user_name=1111111&tag_line=1111122222222222222222&tag_line=111&locale=zh
[]string{"Tagline长度必须小于10个字符", "Tagline2 must have a value!"}""

http://127.0.0.1:8085/testing?user_name=1111111&tag_line=1111122222222222222222&tag_line=111&locale=zh
[]string{"Tagline must be less than 10 characters in length", "Tagline2 must have a value!"}""
12345
```

## 2-5 中间件

- 使用gin中间件

```
func Logger() gin.HandlerFunc {
	return func(c *gin.Context) {
		t := time.Now()

		// Set example variable
		c.Set("example", "12345")

		// before request

		c.Next()

		// after request
		latency := time.Since(t)
		log.Print(latency)

		// access the status we are sending
		status := c.Writer.Status()
		log.Println(status)
	}
}

func main() {
	r := gin.New()
	r.Use(Logger())

	r.GET("/test", func(c *gin.Context) {
		example := c.MustGet("example").(string)

		// it would print: "12345"
		log.Println(example)
	})

	// Listen and serve on 0.0.0.0:8080
	r.Run(":8080")
}
1234567891011121314151617181920212223242526272829303132333435
```

- 自定义ip白名单中间件

```
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
	"log"
)

func IPAuthMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		ipList:=[]string{
			"192.168.1.1",
		}
		isMatched:=false
		for _, host := range ipList {
			if c.ClientIP() == host {
				isMatched = true
			}
		}
		if !isMatched{
			c.String(200,fmt.Sprintf("%v, not in iplist", c.ClientIP()))
			c.Abort()
		}
	}
}

func main() {
	r := gin.New()
	r.Use(IPAuthMiddleware())

	r.GET("/test", func(c *gin.Context) {
		example := c.MustGet("example").(string)
		log.Println(example)
	})

	r.Run(":8080")
}
12345678910111213141516171819202122232425262728293031323334353637
```

## 2-6 其他补充

- 优雅关停
  确保在 go1.8+ 下编译

```
// +build go1.8
package main
import (
	"context"
	"log"
	"net/http"
	"os"
	"os/signal"
	"syscall"
	"time"
	"github.com/gin-gonic/gin"
)

func main() {
	router := gin.Default()
	router.GET("/", func(c *gin.Context) {
		time.Sleep(5 * time.Second)
		c.String(http.StatusOK, "Welcome Gin Server")
	})

	srv := &http.Server{
		Addr:    ":8080",
		Handler: router,
	}

	go func() {
		// service connections
		if err := srv.ListenAndServe(); err != nil && err != http.ErrServerClosed {
			log.Fatalf("listen: %s\n", err)
		}
	}()

	signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
	<-quit
	log.Println("Shutdown Server ...")

	ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
	defer cancel()
	if err := srv.Shutdown(ctx); err != nil {
		log.Fatal("Server Shutdown:", err)
	}
	
	select {
	case <-ctx.Done():
		log.Println("timeout of 5 seconds.")
	}
	log.Println("Server exiting")
}
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748
```

Linux Signal及Golang中的信号处理
https://blog.csdn.net/leonpengweicn/article/details/52131313

- 模板渲染
  gin在html/template基础之上，进行的文件解析预处理。模块文件使用方法与之前相同。
  **LoadHTMLGlob** 可以解析一个文件夹的模板。

```
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)
func main() {
	router := gin.Default()
	//router.LoadHTMLFiles("templates/index.html",)
	router.LoadHTMLGlob("templates/*")
	router.GET("/index", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", gin.H{
			"title": "Main website",
		})
	})
	router.Run(":8080")
}
1234567891011121314151617
```

index.html

```
<html>
<h1>
    {{ .title }}
</h1>
</html>
12345
```

template使用样例
https://www.do1618.com/archives/893/go-template学习样例/
https://www.calhoun.io/intro-to-templates-p3-functions/

- Let’s Encrypt 自动证书下载
  无需配置任何证书即可自动下载证书，过期后自动续订。

> Let’s Encrypt原理
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190614183310725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2U0MjEwODM0NTg=,size_16,color_FFFFFF,t_70)
> 更多原理细节：
> https://www.cnblogs.com/esofar/p/9291685.html
> https://blog.csdn.net/canghaiguzhou/article/details/79945001

gin使用方法超级简单：

```
package main

import (
	"log"

	"github.com/gin-gonic/autotls"
	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()
	r.GET("/ping", func(c *gin.Context) {
		c.String(200, "pong")
	})
	log.Fatal(autotls.Run(r, "www.itpp.tk"))
}
12345678910111213141516
```

测试

```
https://www.itpp.tk/ping
pong
12
```

查看证书缓存

```
[root@izrj998xcgvke9018fkvbvz autotls]# ls ~/.cache/golang-autocert/
acme_account+key  www.itpp.tk
12
```

福利彩蛋，免费域名注册：
https://www.freenom.com/zh/index.html?lang=zh

## 2-7 构建企业级脚手架

代码地址：https://github.com/e421083458/gin_scaffold
使用gin构建了企业级脚手架，代码简洁易读，可快速进行高效web开发。
主要功能有：
1、请求链路日志打印，涵盖mysql/redis/request_in/request_out
2、接入validator.v9，支持多语言错误信息提示及自定义错误提示。
3、借助golang_common，支持了多配置环境及log/redis/mysql/http.client

### 现在开始

- 安装软件依赖
  go mod使用请查阅：
  https://blog.csdn.net/e421083458/article/details/89762113

```
go mod tidy
1
```

- 运行脚本

```
go run main.go

➜  gin_scaffold git:(master) ✗ go run main.go
------------------------------------------------------------------------
[INFO]  config=./conf/dev/
[INFO]  start loading resources.
[INFO]  success loading resources.
------------------------------------------------------------------------
[GIN-debug] [WARNING] Now Gin requires Go 1.6 or later and Go 1.7 will be required soon.

[GIN-debug] [WARNING] Creating an Engine instance with the Logger and Recovery middleware already attached.

[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:	export GIN_MODE=release
 - using code:	gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /demo/index               --> github.com/e421083458/gin_scaffold/controller.(*Demo).Index-fm (6 handlers)
[GIN-debug] GET    /demo/bind                --> github.com/e421083458/gin_scaffold/controller.(*Demo).Bind-fm (6 handlers)
[GIN-debug] GET    /demo/dao                 --> github.com/e421083458/gin_scaffold/controller.(*Demo).Dao-fm (6 handlers)
[GIN-debug] GET    /demo/redis               --> github.com/e421083458/gin_scaffold/controller.(*Demo).Redis-fm (6 handlers)
 [INFO] HttpServerRun::8880
123456789101112131415161718192021
```

- 测试mysql与请求链路

```
curl 'http://127.0.0.1:8880/demo/dao?id=1'
{
    "errno": 0,
    "errmsg": "",
    "data": "[{\"id\":1,\"area_name\":\"area_name\",\"city_id\":1,\"user_id\":2,\"update_at\":\"2019-06-15T00:00:00+08:00\",\"create_at\":\"2019-06-15T00:00:00+08:00\",\"delete_at\":\"2019-06-15T00:00:00+08:00\"}]",
    "trace_id": "c0a8fe445d05b9eeee780f9f5a8581b0"
}

查看链路日志（确认是不是一次请求查询，都带有相同trace_id）：
tail -f gin_scaffold.inf.log

[INFO][2019-06-16T11:39:26.802][log.go:58] _com_request_in||method=GET||from=127.0.0.1||traceid=c0a8fe445d05b9eeee780f9f5a8581b0||cspanid=||uri=/demo/dao?id=1||args=map[]||body=||spanid=9dad47aa57e9d186
[INFO][2019-06-16T11:39:26.802][log.go:58] _com_mysql_success||affected_row=1||traceid=c0a8fe445d05b9ee07b80f9f66cb39b0||spanid=9dad47aa1408d2ac||source=/Users/niuyufu/go/src/github.com/e421083458/gin_scaffold/dao/demo.go:24||proc_time=0.000000000||sql=SELECT * FROM `area`  WHERE (id = '1')||level=sql||current_time=2019-06-16 11:39:26||cspanid=
[INFO][2019-06-16T11:39:26.802][log.go:58] _com_request_out||method=GET||args=map[]||proc_time=0.025019164||traceid=c0a8fe445d05b9eeee780f9f5a8581b0||spanid=9dad47aa57e9d186||uri=/demo/dao?id=1||from=127.0.0.1||response={\"errno\":0,\"errmsg\":\"\",\"data\":\"[{\\\"id\\\":1,\\\"area_name\\\":\\\"area_name\\\",\\\"city_id\\\":1,\\\"user_id\\\":2,\\\"update_at\\\":\\\"2019-06-15T00:00:00+08:00\\\",\\\"create_at\\\":\\\"2019-06-15T00:00:00+08:00\\\",\\\"delete_at\\\":\\\"2019-06-15T00:00:00+08:00\\\"}]\",\"trace_id\":\"c0a8fe445d05b9eeee780f9f5a8581b0\"}||cspanid=
1234567891011121314
```

- 测试参数绑定与多语言验证

```
curl 'http://127.0.0.1:8880/demo/bind?name=name&locale=zh'
{
    "errno": 500,
    "errmsg": "Age为必填字段,Passwd为必填字段",
    "data": "",
    "trace_id": "c0a8fe445d05badae8c00f9fb62158b0"
}

curl 'http://127.0.0.1:8880/demo/bind?name=name&locale=en'
{
    "errno": 500,
    "errmsg": "Age is a required field,Passwd is a required field",
    "data": "",
    "trace_id": "c0a8fe445d05bb4cd3b00f9f3a768bb0"
}
123456789101112131415
```

### 文件分层

```
├── README.md
├── conf   配置文件夹
│   └── dev
│       ├── base.toml
│       ├── mysql_map.toml
│       └── redis_map.toml
├── controller 控制器
│   └── demo.go
├── dao DB数据访问层
│   └── demo.go
├── dto  Bind结构体层
│   └── demo.go
├── gin_scaffold.inf.log  info日志
├── gin_scaffold.wf.log warning日志
├── go.mod go module管理文件
├── go.sum
├── main.go
├── middleware 中间件层
│   ├── panic.go
│   ├── response.go
│   ├── token_auth.go
│   └── translation.go
├── public  公共文件
│   ├── log.go
│   ├── mysql.go
│   └── validate.go
├── router  路由层
│   ├── httpserver.go
│   └── route.go
└── tmpl
123456789101112131415161718192021222324252627282930
```

### 引入轻量级golang类库，支持 mysql、redis、http.client、log、支持多级环境配置、支持链路日志打印

```
go get -v github.com/e421083458/golang_common
1
```

> 测试日志打印功能：

```
package main

import (
	"github.com/e421083458/golang_common/lib"
	"log"
	"time"
)

func main() {
	if err := lib.InitModule("./conf/dev/",[]string{"base","mysql","redis",}); err != nil {
		log.Fatal(err)
	}
	defer lib.Destroy()

	//todo sth
	lib.Log.TagInfo(lib.NewTrace(), lib.DLTagUndefind, map[string]interface{}{"message": "todo sth"})
	time.Sleep(time.Second)
}
123456789101112131415161718
```

类库更多功能细节请查看：
https://github.com/e421083458/golang_common

### 输出格式统一封装

```
func ResponseError(c *gin.Context, code ResponseCode, err error) {
	trace, _ := c.Get("trace")
	traceContext, _ := trace.(*lib.TraceContext)
	traceId := ""
	if traceContext != nil {
		traceId = traceContext.TraceId
	}
	resp := &Response{ErrorCode: code, ErrorMsg: err.Error(), Data: "", TraceId: traceId}
	c.JSON(200, resp)
	response, _ := json.Marshal(resp)
	c.Set("response", string(response))
	c.AbortWithError(200, err)
}

func ResponseSuccess(c *gin.Context, data interface{}) {
	trace, _ := c.Get("trace")
	traceContext, _ := trace.(*lib.TraceContext)
	traceId := ""
	if traceContext != nil {
		traceId = traceContext.TraceId
	}
	resp := &Response{ErrorCode: SuccessCode, ErrorMsg: "", Data: data, TraceId: traceId}
	c.JSON(200, resp)
	response, _ := json.Marshal(resp)
	c.Set("response", string(response))
}
1234567891011121314151617181920212223242526
```

### 定义中间件链路日志打印

```
package middleware

import (
	"bytes"
	"errors"
	"fmt"
	"github.com/e421083458/gin_scaffold/public"
	"github.com/e421083458/golang_common/lib"
	"github.com/gin-gonic/gin"
	"io/ioutil"
	"time"
)
//链路请求日志
func RequestInLog(c *gin.Context) {
	traceContext := lib.NewTrace()
	if traceId := c.Request.Header.Get("com-header-rid"); traceId != "" {
		traceContext.TraceId = traceId
	}
	if spanId := c.Request.Header.Get("com-header-spanid"); spanId != "" {
		traceContext.SpanId = spanId
	}
	c.Set("startExecTime", time.Now())
	c.Set("trace", traceContext)
	bodyBytes, _ := ioutil.ReadAll(c.Request.Body)
	c.Request.Body = ioutil.NopCloser(bytes.NewBuffer(bodyBytes)) // Write body back

	lib.Log.TagInfo(traceContext, "_com_request_in", map[string]interface{}{
		"uri":    c.Request.RequestURI,
		"method": c.Request.Method,
		"args":   c.Request.PostForm,
		"body":   string(bodyBytes),
		"from":   c.ClientIP(),
	})
}
//链路输出日志
func RequestOutLog(c *gin.Context) {
	endExecTime := time.Now()
	response, _ := c.Get("response")
	st, _ := c.Get("startExecTime")
	startExecTime, _ := st.(time.Time)
	public.ComLogNotice(c, "_com_request_out", map[string]interface{}{
		"uri":       c.Request.RequestURI,
		"method":    c.Request.Method,
		"args":      c.Request.PostForm,
		"from":      c.ClientIP(),
		"response":  response,
		"proc_time": endExecTime.Sub(startExecTime).Seconds(),
	})
}

func TokenAuthMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		RequestInLog(c)
		defer RequestOutLog(c)
		isMatched := false
		for _, host := range lib.GetStringSliceConf("base.http.allow_ip") {
			if c.ClientIP() == host {
				isMatched = true
			}
		}
		if !isMatched{
			ResponseError(c, InternalErrorCode, errors.New(fmt.Sprintf("%v, not in iplist", c.ClientIP())))
			c.Abort()
			return
		}
		c.Next()
	}
}
1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465666768
```

### 请求数据绑定到结构体与校验

dto/demo.go

```
package dto

import (
	"errors"
	"github.com/e421083458/gin_scaffold/public"
	"github.com/gin-gonic/gin"
	"github.com/go-playground/universal-translator"
	"gopkg.in/go-playground/validator.v9"
	"strings"
)

type InStruct struct {
	Name   string `form:"name" validate:"required"`
	Age    int64  `form:"age" validate:"required"`
	Passwd string `form:"passwd" validate:"required"`
}

func (o *InStruct) BindingValidParams(c *gin.Context)  error{
	if err:=c.ShouldBind(o);err!=nil{
		return err
	}
	v:=c.Value("trans")
	trans,ok := v.(ut.Translator)
	if !ok{
		trans, _ = public.Uni.GetTranslator("zh")
	}
	err := public.Validate.Struct(o)
	if err != nil {
		errs := err.(validator.ValidationErrors)
		sliceErrs:=[]string{}
		for _, e := range errs {
			sliceErrs=append(sliceErrs,e.Translate(trans))
		}
		return errors.New(strings.Join(sliceErrs,","))
	}
	return nil
}
12345678910111213141516171819202122232425262728293031323334353637
```

controller/demo.go

```
func (demo *Demo) Bind(c *gin.Context) {
	st:=&dto.InStruct{}
	if err:=st.BindingValidParams(c);err!=nil{
		middleware.ResponseError(c,500,err)
		return
	}
	middleware.ResponseSuccess(c, "")
	return
}
123456789
```

# 秤砣虽小压千斤

## 3-1 用户管理系统设计

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019061418333776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2U0MjEwODM0NTg=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190614183349328.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2U0MjEwODM0NTg=,size_16,color_FFFFFF,t_70)

### 功能点

- 管理员登陆及退出
- 用户列表 翻页
- 增、删、改

### 数据库

- 用户表设计

```
CREATE TABLE `user` (
 `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '自增id',
 `name` varchar(255) NOT NULL DEFAULT '' COMMENT '姓名',
 `addr` varchar(255) NOT NULL DEFAULT '' COMMENT '住址',
 `age` smallint(4) NOT NULL DEFAULT '0' COMMENT '年龄',
 `birth` varchar(100) NOT NULL DEFAULT '2000-01-01 00:00:00' COMMENT '生日',
 `sex` smallint(4) NOT NULL DEFAULT '0' COMMENT '性别',
 `update_at` datetime NOT NULL DEFAULT '1970-01-01 00:00:00' COMMENT '更新时间',
 `create_at` datetime NOT NULL DEFAULT '1970-01-01 00:00:00' COMMENT '创建时间',
 PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=31 DEFAULT CHARSET=utf8 COMMENT='用户表'
1234567891011
```

### 后端准备

- fork一份gin脚手架程序
  https://github.com/e421083458/gin_scaffold

### 前端准备

- 下载vue-admin
  https://github.com/taylorchen709/vue-admin
- 安装及设置webstorm
  调整webstorm的vue.js插件
  https://www.cnblogs.com/ssrsblogs/p/6231981.html
  调整webstorm的es6
  https://blog.csdn.net/qijuju/article/details/79015073

## 3-2 实战开发

- 开发接口

```
[GIN-debug] POST   /api/login                --> github.com/e421083458/gin_scaffold/controller.(*Api).Login-fm (7 handlers)
[GIN-debug] GET    /api/loginout             --> github.com/e421083458/gin_scaffold/controller.(*Api).LoginOut-fm (7 handlers)
[GIN-debug] GET    /api/user/listpage        --> github.com/e421083458/gin_scaffold/controller.(*Api).ListPage-fm (8 handlers)
[GIN-debug] GET    /api/user/add             --> github.com/e421083458/gin_scaffold/controller.(*Api).AddUser-fm (8 handlers)
[GIN-debug] GET    /api/user/edit            --> github.com/e421083458/gin_scaffold/controller.(*Api).EditUser-fm (8 handlers)
[GIN-debug] GET    /api/user/remove          --> github.com/e421083458/gin_scaffold/controller.(*Api).RemoveUser-fm (8 handlers)
[GIN-debug] GET    /api/user/batchremove     --> github.com/e421083458/gin_scaffold/controller.(*Api).RemoveUser-fm (8 handlers)
1234567
```

- 整合前端调试

```
npm run dev
1
```

修改 config/index.js文件，作接口代理转发

```
dev: {
        env: require('./dev.env'),
        port: 8080,
        autoOpenBrowser: true,
        assetsSubDirectory: 'static',
        assetsPublicPath: '/',
        proxyTable: {
            "/api/": {
                target:"http://127.0.0.1:8880/",
                changeOrigin: true,
                pathRewrite: {
                }
            }
        },
        cssSourceMap: false
    }
12345678910111213141516
```

- 前后端部署与整合
  正式部署时我们一般考虑使用nginx 作转发。

```
    server {
        listen       8882;
        server_name  localhost;
        root /Users/niuyufu/WebstormProjects/vue-admin/dist;
        index  index.html index.htm index.php;
        location / {
            try_files $uri $uri/ /index.html?$args;
        }
        location /api/ {
            proxy_pass http://127.0.0.1:8880/api/;
        }
    }
123456789101112
```

# 课程总结

随堂笔记
https://blog.csdn.net/e421083458/article/details/91994788
gin入门实战 - 基础精髓 随堂代码
https://github.com/e421083458/hello_gin
golang基础代码类库
https://github.com/e421083458/golang_common
gin开发脚手架
https://github.com/e421083458/gin_scaffold
整合vue.js的admin后台
https://github.com/e421083458/vue-admin