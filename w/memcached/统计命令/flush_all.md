# Memcached flush_all 命令

Memcached flush_all 命令用于用于清理缓存中的所有 **key=>value(键=>值)** 对。

该命令提供了一个可选参数 **time**，用于在制定的时间后执行清理缓存操作。

### 语法：

flush_all 命令的基本语法格式如下：

```
flush_all [time] [noreply]
```

### 实例

清理缓存:

```
set w3cschool 0 900 9
memcached
STORED
get w3cschool
VALUE w3cschool 0 9
memcached
END
flush_all
OK
get w3cschool
END
```