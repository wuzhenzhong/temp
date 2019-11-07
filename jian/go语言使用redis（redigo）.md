# go语言使用redis（redigo）

[![img](https://upload.jianshu.io/users/upload_avatars/5022741/de6c8d6a-f9b3-4646-8244-20fc7be8a938.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/a0e226915ac1)

[laijh](https://www.jianshu.com/u/a0e226915ac1)关注

12018.12.25 14:10:28字数 833阅读 3,324

go的redis client用的比较多两个包是redix和redigo，因为beego cache模块里redis使用的是redigo，所以我也就使用这个包了。因为代码内容偏多，结构不清晰，不方便阅读，最后整理成一份思维导图，便于学习。当把整体分析，会发现提供给开发者使用的内容非常巧妙。





![img](https://upload-images.jianshu.io/upload_images/5022741-2f9c9daace6eb0a6.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image.png


[点击下载](https://github.com/laijinhang/go-personal-notes/tree/master/redigo包笔记)

Demo:

```go
package main

import (
    "fmt"
    "github.com/gomodule/redigo/redis"
    "log"
)

func main() {
    c1, err := redis.Dial("tcp", "127.0.0.1:6379")
    if err != nil {
        log.Fatalln(err)
    }
    defer c1.Close()
    c2, err := redis.DialURL("redis://127.0.0.1:6379")
    if err != nil {
        log.Fatalln(err)
    }
    defer c2.Close()


    rec1, err := c1.Do("Get", "name")
    fmt.Println(string(rec1.([]byte)))

    c2.Send("Get", "name")
    c2.Flush()
    rec2, err := c2.Receive()
    fmt.Println(string(rec2.([]byte)))
}
```

下面内容是更加详细的源码分析

提供给开发者使用的内容
（1）变量
（2）常量
（3）新类型
（4）接口
（5）结构体
（6）函数

1、变量
var ErrNil = errors.New("redigo: nil returned")
2、常量
3、新类型
（1）type Args []interface{}
（2）type Error string
4、接口
（1）Conn
（2）ConnWithTimeout
（3）Scanner
（4）Argument
5、结构体
（1）DialOption
（2）Pool
（3）PoolStats
（4）Subscription
（5）PubSubConn
（6）Message
（7）Script
（8）Pong
6、函数
（1）func NewPool(newFn func() (Conn, error), maxIdle int) *Pool
（2）func NewScript(keyCount int, src string) *Script
（3）func NewLoggingConn(conn Conn, logger *log.Logger, prefix string) Conn
（4）func NewLoggingConnFilter(conn Conn, logger *log.Logger, prefix string, skip func(cmdName string) bool) Conn（5）func DoWithTimeout(c Conn, timeout time.Duration, cmd string, args ...interface{}) (interface{}, error)（6）func DoWithTimeout(c Conn, timeout time.Duration, cmd string, args ...interface{}) (interface{}, error)（7）func Int(reply interface{}, err error) (int, error)（8）func Int64(reply interface{}, err error) (int64, error)（9）func Uint64(reply interface{}, err error) (uint64, error)（10）func Float64(reply interface{}, err error) (float64, error)（11）func String(reply interface{}, err error) (string, error)（12）func Bytes(reply interface{}, err error) ([]byte, error)（13）func Bool(reply interface{}, err error) (bool, error)（14）func MultiBulk(reply interface{}, err error) ([]interface{}, error)（15）func Values(reply interface{}, err error) ([]interface{}, error)（16）func Float64s(reply interface{}, err error) ([]float64, error)（17）func Strings(reply interface{}, err error) ([]string, error)（18）func ByteSlices(reply interface{}, err error) ([][]byte, error)（19）func Int64s(reply interface{}, err error) ([]int64, error)（20）func Ints(reply interface{}, err error) ([]int, error)（21）func StringMap(result interface{}, err error) (map[string]string, error)（22）func IntMap(result interface{}, err error) (map[string]int, error)（23）func Int64Map(result interface{}, err error) (map[string]int64, error)（24）func Positions(result interface{}, err error) ([]*[2]float64, error)
（25）func DialTimeout(network, address string, connectTimeout, readTimeout, writeTimeout time.Duration) (Conn, error)
（26）func DialReadTimeout(d time.Duration) DialOption
（27）func DialWriteTimeout(d time.Duration) DialOption
（28）func DialConnectTimeout(d time.Duration) DialOption
（29）func DialKeepAlive(d time.Duration) DialOption
（30）func DialNetDial(dial func(network, addr string) (net.Conn, error)) DialOption
（31）func DialDatabase(db int) DialOption
（32）func DialPassword(password string) DialOption
（33）func DialTLSConfig(c *tls.Config) DialOption
（34）func DialTLSSkipVerify(skip bool) DialOption

一、变量
var ErrNil = errors.New("redigo: nil returned")
暴露给开发者的目的是，因为从redis服务器获取数据的时候可能遇到值为空的情况。
二、常量
三、新类型
（1）type Args []interface{}

```swift
type Args []interface{}

func (args Args) Add(value ...interface{}) Args
// Add是直接将值追加到args后面的结果返回，但是并不会修改到args
func (args Args) AddFlat(v interface{}) Args 
// AddFlat根据值的类型存储，分为以下五种情况：Struct、Slice、Map、Ptr、其他类型
（1）Struct：
package main

import (
    "fmt"
    "github.com/gomodule/redigo/redis"
)

type Ints struct {
    A int
    b int
    C float32
}

func (Ints)Add() {
    fmt.Println(123)
}

func main() {
    i := Ints{1,2,1.2}
    args := redis.Args{1,2,3}
    args1 := args.AddFlat(i)
    fmt.Println(args, len(args))
    fmt.Println(args1, len(args1))
}
输出：
[1 2 3] 3
[1 2 3 A 1 C 1.2] 7
（2）Slice：
package main

import (
    "fmt"
    "github.com/gomodule/redigo/redis"
)

func main() {
    i := []int{1,2,12}
    args := redis.Args{1,2,3}
    args1 := args.AddFlat(i)
    fmt.Println(args, len(args))
    fmt.Println(args1, len(args1))
}
输出：
[1 2 3] 3
[1 2 3 1 2 12] 6
（3）Map：
package main

import (
    "fmt"
    "github.com/gomodule/redigo/redis"
)

func main() {
    i := map[int]string{1:"你好",2:"测试"}
    args := redis.Args{1,2,3}
    args1 := args.AddFlat(i)
    fmt.Println(args, len(args))
    fmt.Println(args1, len(args1))
}
输出：
[1 2 3] 3
[1 2 3 1 你好 2 测试] 7
（4）Prt
package main

import (
    "fmt"
    "github.com/gomodule/redigo/redis"
)

func main() {
    a := 123
    i := &a
    args := redis.Args{1,2,3}
    args1 := args.AddFlat(i)
    fmt.Println(args, len(args))
    fmt.Println(args1, len(args1))
}
输出：
[1 2 3] 3
[1 2 3 0xc00008a2d8] 4
（5）其他类型：
package main

import (
    "fmt"
    "github.com/gomodule/redigo/redis"
)

type Int int8

func main() {
    a := 123
    b := Int(8)
    args := redis.Args{1,2,3}
    args1 := args.AddFlat(a)
    args1 = args1.AddFlat(b)
    fmt.Println(args, len(args))
    fmt.Println(args1, len(args1))
}
输出：
[1 2 3] 3
[1 2 3 123 8] 5
```

（2）type Error string

```go
type Error string
func (err Error) Error() string { return string(err) }
```

四、接口
（1）Conn
（2）ConnWithTimeout
（3）Scanner
（4）Argument
五、结构体
（1）DialOption
源码：

```swift
type DialOption struct {
    f func(*dialOptions)
}
```

作用：该结构体是被设计成 当创建与redis服务器连接时，进行参数设置。
与该结构体有关的函数：

```swift
（1）func DialReadTimeout(d time.Duration) DialOption
设置读超时时间
（2）func DialWriteTimeout(d time.Duration) DialOption
设置读超时时间
（3）func DialConnectTimeout(d time.Duration) DialOption
设置连接超时时间
（4）func DialKeepAlive(d time.Duration) DialOption 
（5）func DialNetDial(dial func(network, addr string) (net.Conn, error)) DialOption
（6）func DialDatabase(db int) DialOption
设置连接数据库
（7）func DialPassword(password string) DialOption
设置连接redis密码
（8）func DialTLSConfig(c *tls.Config) DialOption
设置TLS配置信息
（9）func DialTLSSkipVerify(skip bool) DialOption
是否跳过TLS验证
（10）func DialUseTLS(useTLS bool) DialOption
是否使用TLS
```

（2）Pool
源码：

```swift
type Pool struct {
    Dial func() (Conn, error)
    TestOnBorrow func(c Conn, t time.Time) error
    MaxIdle int
    MaxActive int
    IdleTimeout time.Duration
    Wait bool
    MaxConnLifetime time.Duration
    chInitialized uint32
    mu     sync.Mutex 
    closed bool 
    active int 
    ch     chan struct{} 
    idle   idleList
}
```

作用：用来管理redis连接池
相关函数：

```swift
（1）func NewPool(newFn func() (Conn, error), maxIdle int) *Pool
            创建一个连接池对象
（2）func (p *Pool) Get() Conn
            获取一个连接 
（3）func (p *Pool) Stats() PoolStats
            获取池的状态
（4）func (p *Pool) ActiveCount() int
            获取存活数量
（5）func (p *Pool) IdleCount() int
            获取空闲数量
（6）func (p *Pool) Close() error
            关闭
```

（3）PoolStats
源码：

```cpp
type PoolStats struct {
    ActiveCount int 
    IdleCount int
}
```

作用：记录redis池中存活连接数量和空闲连接数
（4）Subscription
源码：

```cpp
type Subscription struct {
    Kind string          
    Channel string  
    Count int
}
```

作用：Subscription用于订阅或取消订阅通知。
相关函数：

```undefined

```

Demo：

```go
package main

import (
    "fmt"
    "github.com/gomodule/redigo/redis"
    "log"
    "reflect"
)

func main() {
    rc, err := redis.Dial("tcp", "127.0.0.1:6379")
    if err != nil {
        log.Fatalln(err)
    }
    defer rc.Close()

    rc.Send("SUBSCRIBE", "example")
    rc.Flush()
    for {
        reply, err := redis.Values(rc.Receive())
        if err != nil {
            fmt.Println(err)
        }
        if len(reply) == 3 {
            if reflect.TypeOf(reply[2]).String() == "[]uint8" {
                fmt.Println(string(reply[2].([]byte)))
            }
        }
    }
}
```

（5）PubSubConn
源码：

```rust
type PubSubConn struct {
    Conn Conn
}
```

相关函数：

```swift
（1）func (c PubSubConn) Subscribe(channel ...interface{}) error
            订阅给定的一个或多个频道的信息。
（2）func (c PubSubConn) PSubscribe(channel ...interface{}) error
            订阅一个或多个符合给定模式的频道。
（3）func (c PubSubConn) Unsubscribe(channel ...interface{}) error
            指退订给定的频道。
（4）func (c PubSubConn) PUnsubscribe(channel ...interface{}) error
            退订所有给定模式的频道。
（5）func (c PubSubConn) Ping(data string) error 
            测试客户端是否能够继续连通
（6）func (c PubSubConn) Receive() interface{}
             获取回复信息
（7）func (c PubSubConn) ReceiveWithTimeout(timeout time.Duration) interface{}
             在指定时间内获取时间
（8）func (c PubSubConn) Close() error 
              关闭
```

（6）Message
源码：

```go
type Message struct {
    Channel string    // 频道名称
    Pattern string      // 频道模式
    Data []byte            // 数据
}
```

（7）Script
源码：

```cpp
type Script struct {
    keyCount int
    src               string
    hash           string
}
```

相关函数：

```swift
（1）func NewScript(keyCount int, src string) *Script 
（2）func (s *Script) Hash() string
（3）func (s *Script) Do(c Conn, keysAndArgs ...interface{}) (interface{}, error) 
（4）func (s *Script) SendHash(c Conn, keysAndArgs ...interface{}) error 
（5）func (s *Script) Send(c Conn, keysAndArgs ...interface{}) error 
（6）func (s *Script) Load(c Conn) error 
```

（8）Pong
源码：

```rust
type Pong struct {
    Data string
}
```

(6)函数

```go
（1）func NewPool(newFn func() (Conn, error), maxIdle int) *Pool
（2）func NewScript(keyCount int, src string) *Script 
（3）func NewLoggingConn(conn Conn, logger *log.Logger, prefix string) Conn
（4）func NewLoggingConnFilter(conn Conn, logger *log.Logger, prefix string, skip func(cmdName string) bool) Conn
（5）func DoWithTimeout(c Conn, timeout time.Duration, cmd string, args ...interface{}) (interface{}, error)
（6）func DoWithTimeout(c Conn, timeout time.Duration, cmd string, args ...interface{}) (interface{}, error)
（7）func Int(reply interface{}, err error) (int, error)
（8）func Int64(reply interface{}, err error) (int64, error)
（9）func Uint64(reply interface{}, err error) (uint64, error)
（10）func Float64(reply interface{}, err error) (float64, error)
（11）func String(reply interface{}, err error) (string, error)
（12）func Bytes(reply interface{}, err error) ([]byte, error)
（13）func Bool(reply interface{}, err error) (bool, error) 
（14）func MultiBulk(reply interface{}, err error) ([]interface{}, error) 
（15）func Values(reply interface{}, err error) ([]interface{}, error)
（16）func Float64s(reply interface{}, err error) ([]float64, error)
（17）func Strings(reply interface{}, err error) ([]string, error) 
（18）func ByteSlices(reply interface{}, err error) ([][]byte, error)
（19）func Int64s(reply interface{}, err error) ([]int64, error)
（20）func Ints(reply interface{}, err error) ([]int, error)
（21）func StringMap(result interface{}, err error) (map[string]string, error) 
（22）func IntMap(result interface{}, err error) (map[string]int, error)
（23）func Int64Map(result interface{}, err error) (map[string]int64, error)
（24）func Positions(result interface{}, err error) ([]*[2]float64, error)
（25）func DialTimeout(network, address string, connectTimeout, readTimeout, writeTimeout time.Duration) (Conn, error) 
（26）func DialReadTimeout(d time.Duration) DialOption
（27）func DialWriteTimeout(d time.Duration) DialOption
（28）func DialConnectTimeout(d time.Duration) DialOption
（29）func DialKeepAlive(d time.Duration) DialOption 
（30）func DialNetDial(dial func(network, addr string) (net.Conn, error)) DialOption
（31）func DialDatabase(db int) DialOption
（32）func DialPassword(password string) DialOption
（33）func DialTLSConfig(c *tls.Config) DialOption
（34）func DialTLSSkipVerify(skip bool) DialOption
```

经过学习源码发现，这些顶尖的设计者与我们普通开发者的区别在于，他们包设计非常巧妙，以及只把有必要的内容提供给开发者。