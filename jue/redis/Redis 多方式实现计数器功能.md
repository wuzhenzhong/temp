# Redis 多方式实现计数器功能（附代码）

> 本文由 [yanglbme](https://github.com/yanglbme) 原创，首发于掘金，禁止未授权转载。

计数器在很多网站中都进行了广泛的应用，比如文章的点赞数、页面的浏览数、网站的访客数、视频的播放数等等。在这篇文章里，我会使用 Redis 的三种数据类型，来分别实现计数器的功能。

请跟随我一起来看看吧。

## 使用字符串键

下面代码演示了如何利用 Redis 中的字符串键来实现计数器功能。其中，`incr()` 方法用于累加计数，`get_cnt()` 方法用于获取当前的计数值。

```
from redis import Redis

class Counter:
    def __init__(self, client: Redis, key: str):
        self.client = client
        self.key = key

    def incr(self, amount=1):
        """计数累加"""
        self.client.incr(self.key, amount=amount)

    def decr(self, amount=1):
        """计数累减"""
        self.client.decr(self.key, amount=amount)

    def get_cnt(self):
        """获取当前计数的值"""
        return self.client.get(self.key)


if __name__ == '__main__':
    client = Redis(decode_responses=True)
    counter = Counter(client, 'page_view:12')
    counter.incr()
    counter.incr()
    print(counter.get_cnt())  # 2

复制代码
```

假设我们要统计 page_id 为 12 的页面的浏览数，那么我们可以设定 key 为 `page_view:12`，用户每一次浏览，就调用一次 counter 的 `incr()` 方法进行计数。

## 使用哈希键

在上面的代码中，我们需要针对每个统计项，都单独设置一个字符串键。那么，下面我们来看看如何通过 Redis 的哈希键，来对关联的统计项进行统一管理。

```
from redis import Redis

class Counter:
    def __init__(self, client: Redis, key: str, counter: str):
        self.client = client
        self.key = key
        self.counter = counter

    def incr(self, amount=1):
        """计数累加"""
        self.client.hincrby(self.key, self.counter, amount=amount)

    def decr(self, amount=1):
        """计数累减"""
        self.client.hincrby(self.key, self.counter, amount=-amount)

    def get_cnt(self):
        """获取当前计数的值"""
        return self.client.hget(self.key, self.counter)


if __name__ == '__main__':
    client = Redis(decode_responses=True)
    counter = Counter(client, 'page_view', '66')
    counter.incr()
    counter.incr()
    print(counter.get_cnt())  # 2

复制代码
```

如果采用哈希键，那么，我们对于同一类型的计数，可以使用一个相同的 key 来进行存储。比如，在上面例子中，我们使用 `page_view` 来统计页面的浏览数，对于 page_id 为 66 的页面，直接添加到 page_view 对应的字段中即可。

## 使用集合键

在上面两个例子中，当动作被执行时，程序可以调用一次 `incr()` 累加计数的方法。某些场景下，我们可能需要对特定的动作，仅仅计数一次。什么叫“仅仅计数一次”？就是说，同一个用户/IP，多次访问某个页面，计数器只会将计数值增加 1。来看看以下代码：

```
from redis import Redis

class Counter:
    def __init__(self, client: Redis, key: str):
        self.client = client
        self.key = key

    def add(self, item: str) -> bool:
        """计数累加，若计数之前item已存在，放回False；否则返回True"""
        return self.client.sadd(self.key, item) == 1

    def get_cnt(self):
        """获取当前计数的值"""
        return self.client.scard(self.key)


if __name__ == '__main__':
    client = Redis(decode_responses=True)
    counter = Counter(client, 'uv')
    counter.add('user1')
    counter.add('user2')
    counter.add('user1')  # 重复放入
    print(counter.get_cnt())  # 2

复制代码
```

在实际应用中，以上代码需要稍作改动，但基本的思路不变。怎么样，你学会了吗？

欢迎扫码关注我的公众号“**Doocs开源社区**”。