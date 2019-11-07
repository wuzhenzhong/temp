# gorm

[![img](https://upload.jianshu.io/users/upload_avatars/18675188/ee5413bd-aa14-4905-afec-185b0c94dc48.jpeg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/dc9c2f40bd74)

[JarAlreadyTaken](https://www.jianshu.com/u/dc9c2f40bd74)关注

0.0722019.07.10 17:01:44字数 1,018阅读 2,596

# orm

orm 全称 object relation mapping 对象映射关系，目的是解决面向对象和关系数据库之间存在的互不匹配的现象。
sql注入曾经是一种常见的网络攻击方式，针对程序编写疏忽而产生的问题比如：通过sql语句实现无账号登录、删除甚至篡改数据库。这是由于以前sql语句是拼接后执行的，因此在动态参数完成拼接时若有带有sql操作的关键字的动态参数参与拼接，则整体结果会向恶意注入者期望的方向执行。
orm就很好的解决了这些问题，在其底层逻辑中会带有转义操作，不担心注入问题，而且对于我们的`struct`结构体而言，也提供了对应关系操作，对于编程者来说是极大地便利：将注意力从数据库的细节转移到业务逻辑上。orm作为中间层，可以简化数据库的迁移操作。
orm的缺点：不能够生成所有的sql业务语句，有些复杂的还是需要sql原生语句操作；性能低于直接使用sql语句的。

# gorm

golang的一个orm框架。

## 使用示例

### 创建表

#### sql

CREATE TABLE users( name VARCHAR(50), age INT);

#### gorm

```go
type User struct{
  name stringg
  age int
}
db.Table(“users”).CreateTable(&User{})
```

创建users表，内容由结构体来定义的。

### 插入数据

#### sql

INSERT INTO users (name, age) VALUES ('ee'，'18')

#### gorm

```go
user := User{
  name : "ee",
  age : 18,
}
db.Create(&user)
```

先定义一个User类的结构体user，然后调用gorm的Create函数参数为user，插入到数据库中。

### 查询

#### sql

SELECT * FROM users WHERE name="ee"；

#### gorm

```go
var users [ ]User//定义一个User类型的数组名字叫做users

db.Where(“name=?”,”ee”).Find(&users)  //方法1:查询所有结果放入users数组
db.Where(&User{name:”ee”,age:18}).Find(&users)
db.Where(map[string] interface{}{"name":"ee","age":18}).Find(&users)
```

这是三种gorm中的查询方式分别是通过sql、struct、map来查询数据。

### 获取第一条匹配记录

#### sql

SELECT * FROM users WHERE name="ee" limit 1;

#### gorm

```go
var user User 
db.Where("name=?","ee").First(&user)
```

在gorm有一个单独的函数来获取第一条数据
批量查询需要创建一个数组来接受数据，这里需要创建一个单个变量来接受查询出来的数据。

### LIMIT多条数据

#### sql

SELECT * FROM users LIMIT 3;

#### gorm

```go
var users [ ]User
db.Limit(3).Find(&users)
```

### LIKE AND IN

#### sql

SELECT * FROM users WHERE role='admin' OR role ='super_admin';

#### gorm

```go
db.Where("role=?","admin”).Or("ole=?",super_admin").Find(&users)
db.Where("name LIKE ?","%ee%").Find(&user)
db.Where("name=? AND age>=?”, "ee", "18").Find(&users)
db.Where("name in (?)",[ ]string{"ee","sh"}).Find(&usere)
```

### 非选项

#### sql

SELECT * FROM users WHERE name <> "ee";

#### gorm

```go
db.Not("name","ee").First(&users) 
```

### gorm支持查询链

```go
db.Where("name=?","ee").Or("name=?","sh").Not("age=?",18).Find(&users)
```

### 部分字段查询

#### sql

SELECT name, age FROM users;

#### gorm

```go
db.Select(“name, age”).Find(&users)
db.Select([ ]string{“name”,”age”}).Find(&users)
```

可以选择指定的字段来进行获取。

### 数据排序

#### sql

SELECT * FROM users ORDER BY age DESC name;

#### gorm

```go
db.Order("age desc").Order("name").Find(&users)
```

### count函数获取记录条数

#### sql

SELECT count(*) FROM users WHERE name ='ee';

#### gorm

```go
var count int
db.Where("name=?","ee").Count(&count) 
```

将创建的int类型以指针的方式传入（对指针操作使修改生效）

### 数据更新

#### sql

UPDATE users SET name='hello' WHERE id=111;

#### gorm

```go
var user User

db.Where("id=?", "111").First(&user)
Db.Model(&user).Update("name","hello")
```

更新时先取出来再进行修改

### 多个属性修改

#### gorm

```go
db.Model(&user).Updates(map[string]interface{}{“name”:”hello”,”age”:18})

db.Model(&user).Update(User{name:"hello",age:18})
```

### 删除

#### sql

DELETE FROM users WHERE id=10;

#### gorm

```go
var user User 
db.Where("id=?","10").First(&user)
db.Delete(&user)
```

先找id为10的数据，保存到user中，将数据传入到删除函数中，批量删除和查询是对应的， 先批量查询出来然后批量传入到删除数据中。

```go
db.Where("name=?","ee").Delete(User{})
```

## 软删除

很多模型中有`DeletedAt`字段，那么就自动获取到了软删除的功能，那么在调用`Delete`的时候 数据并不会从数据库中永久删除而是将字段DeletedAt的值设置成当前的时间。

## gorm支持事务

数据库中的事务是很必要的一个事情，在grom中是支持事务的，不过是需要我们自己来简单的实现一些，假如说我们的事务是三步，我们要做的操作是：
执行操作前

```go
tx  :=db.Begin() //代表事务开始
tx.Create()
```

之后对操作进行判断，如果成功执行了的话就执行2，继续3。
如果在这之间有一个操作失败了（需要我们通过`if`来进行判断），就调用

```go
tx.Rollback
```

进行回滚操作
如果整个事务执行完了就要进行提交

```go
tx.commit()
```

将整个事务过程进行提交