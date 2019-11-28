# setnx

```javascript
SETNX key value
```

**自1.0.0起可用。**

**时间复杂度：** O（1）

如果不存在，则设置`key`为保存字符串。在这种情况下，它等于SET。当已经保存一个值时，不执行任何操作。SETNX 是短期的“ **SET**如果**ñ** OTË **X**派”。`valuekeykey`

## 返回值

[整数回复](https://redis.io/topics/protocol#integer-reply)，具体为：

- `1` 如果密钥已设置

- `0` 如果密钥没有设置

## 例子

redis> SETNX mykey "Hello" `(integer) 1` redis> SETNX mykey "World" `(integer) 0` redis> GET mykey `"Hello"`

## 设计模式：锁定 `SETNX`

**请注意：**

1. 以下模式不利于[Redlock算法](http://redis.io/topics/distlock)，[该算法](http://redis.io/topics/distlock)实现起来稍微复杂一些，但提供了更好的保证并具有容错能力。

1. 无论如何，我们记录旧的模式，因为某些现有的实现链接到此页面作为参考。此外，Redis 命令如何用于装载编程原语是一个有趣的例子。

1. 无论如何，即使假定一个单实例锁定原语，从2.6.12开始，可以创建一个更简单的锁定原语，与此处讨论的等价，使用 SET 命令获取锁，以及一个简单的 Lua 脚本来释放锁。该模式记录在SET命令页面中。

也就是说，SETNX 可以被用作历史上被用作锁定原语。例如，要获取密钥的锁定`foo`，客户端可以尝试以下操作：

```javascript
SETNX lock.foo <current Unix time + lock timeout + 1>
```

如果 SETNX 返回`1`客户端获得的锁定，则将该`lock.foo`关键字设置为不再认为该锁定有效的 Unix 时间。客户端稍后将使用`DEL lock.foo`以释放锁。

如果 SETNX 返回`0`该键已被某个其他客户端锁定。如果它是非阻塞锁，我们可以返回给调用者，或者输入一个循环重试以保持锁，直到我们成功或某种超时过期。

### 处理死锁

在上述锁定算法中存在一个问题：如果客户端出现故障，崩溃或无法释放锁，会发生什么情况？可以检测到这种情况，因为锁定键包含 UNIX 时间戳。如果这样的时间戳等于当前的 Unix 时间，则该锁不再有效。

发生这种情况时，我们不能仅仅通过调用 DEL 来移除锁，然后尝试发出 SETNX，因为这里存在争用条件，当多个客户端检测到过期的锁并尝试释放它时。

- C1和C2读取`lock.foo`以检查时间戳，因为它们`0`在执行SETNX 之后都收到，因为锁仍然由持有锁之后崩溃的 C3 持有。

- C1发送 `DEL lock.foo`

- C1发送`SETNX lock.foo`并成功

- C2发送 `DEL lock.foo`

- C2发送`SETNX lock.foo`并成功

- **错误**：由于竞争条件，C1和C2都获得了锁定。

幸运的是，使用以下算法可以避免此问题。让我们看看我们的理智客户C4如何使用好的算法：

- C4发送`SETNX lock.foo`以获取锁

- 崩溃的客户端C3仍然拥有它，所以Redis将回复`0`给C4。

- C4发送`GET lock.foo`来检查锁是否过期。如果不是，它会睡眠一段时间并从一开始就重试。

- 相反，如果锁由于 Unix 时间`lock.foo`比当前 Unix 时间早而过期，C4会尝试执行：

GETSET lock.foo <current Unix timestamp + lock timeout + 1>

- 由于 GETSET 语义，C4可以检查存储的旧值`key`是否仍然是过期的时间戳。如果是这样，那就得到了锁。

- 如果另一个客户端（例如C5）比C4速度更快并通过 GETSET 操作获取了锁定，则C4 GETSET 操作将返回一个未过期的时间戳。C4将从第一步重新开始。请注意，即使C4在未来几秒钟内将键设置为几秒，这也不是问题。

为了使这种锁定算法更加健壮，持有锁的客户端应该始终检查超时在 DEL 用于解锁密钥之前没有过期，因为客户端失败可能很复杂，不仅会崩溃，而且会阻止大量时间对付某些操作并尝试在很长时间后发出 DEL（当 LOCK 已被另一客户端占用时）。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18