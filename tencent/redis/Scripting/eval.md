# eval

```javascript
EVAL script numkeys key [key ...] arg [arg ...]
```

**自2.6.0起可用。**

**时间复杂度：**取决于执行的脚本。

## EVAL简介

EVAL 和 EVALSHA 用于从版本2.6.0开始使用内置在 Redis 中的 Lua 解释器来评估脚本。

EVAL 的第一个参数是一个 Lua 5.1 脚本。脚本不需要定义一个 Lua 函数（不应该）。这只是一个 Lua 程序，它将在 Redis 服务器的上下文中运行。

EVAL 的第二个参数是脚本后面的参数个数（从第三个参数开始）代表 Redis 键名称。参数可以通过 Lua 中使用来访问`KEYS`全局变量在基于一个阵列（这样的形式`KEYS[1]`，`KEYS[2]`...）。

所有其他参数不应该代表键名称，可以通过 Lua 的使用来访问`ARGV`全局变量，非常相似与什么发生了键（所以`ARGV[1]`，`ARGV[2]`...）。

以下示例应该阐明上述内容：

```javascript
> eval "return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}" 2 key1 key2 first second
1) "key1"
2) "key2"
3) "first"
4) "second"
```

注意：正如你所看到的，Lua 数组是以 Redis 多重批量回复的形式返回的，这是一种 Redis 返回类型，客户端库可能会将其转换为编程语言中的 Array 类型。

可以使用两个不同的 Lua 函数从 Lua 脚本调用 Redis 命令：

- `redis.call()`

- `redis.pcall()`

`redis.call()`与`redis.pcall()`类似，唯一的区别是，如果 Redis 命令调用会导致错误，`redis.call()`将引发 Lua 错误，反过来会强制 EVAL 向命令调用方返回错误，同时`redis.pcall`将陷阱错误并返回 Lua 表代表错误。

函数`redis.call()`和`redis.pcall()`函数的参数都是格式良好的 Redis 命令的所有参数：

```javascript
> eval "return redis.call('set','foo','bar')" 0
OK
```

上面的脚本将键`foo`设置为字符串`bar`。然而，它违反了 EVAL 命令的语义，因为脚本使用的所有键都应该使用`KEYS`数组传递：

```javascript
> eval "return redis.call('set',KEYS[1],'bar')" 1 foo
OK
```

在执行之前，必须分析所有 Redis 命令，以确定命令将在哪些键上运行。为了使 EVAL 成为真，密钥必须明确传递。这在很多方面都很有用，但特别要确保 Redis 群集可以将您的请求转发到适当的群集节点。

请注意，此规则未实施，以便为用户提供滥用 Redis 单实例配置的机会，代价是编写与 Redis 群集不兼容的脚本。

Lua 脚本可以使用一组转换规则返回从 Lua 类型转换为 Redis 协议的值。

## Lua 和 Redis 数据类型之间的转换

当 Lua 使用`call()`或调用 Redis 命令时，Redis 返回值将转换为 Lua 数据类型`pcall()`。同样，在调用 Redis 命令和 Lua 脚本返回值时，Lua 数据类型将转换为 Redis 协议，以便脚本可以控制 EVAL 返回给客户端的内容。

数据类型之间的这种转换的设计方式是，如果将 Redis 类型转换为 Lua 类型，然后将结果转换回 Redis 类型，则结果与初始值相同。

换句话说，Lua 和 Redis 类型之间存在一对一的转换。下表显示了所有转换规则：

**Redis** 转换为 **Lua** 转换表。

- Redis 整数回复 -> Lua 编号

- Redis 批量回复 -> Lua 字符串

- Redis 多批量回复 -> Lua 表（可能嵌套有其他 Redis 数据类型）

- Redis 状态回复 -> `ok`包含状态的单个字段的 Lua 表

- Redis 错误回复 -> `err`包含错误的单个字段的 Lua 表

- Redis Nil 批量回复和 Nil 多批量回复 -> Lua false 布尔类型

**Lua** 到 **Redis** 转换表。

- Lua 数字 -> Redis 整数回复（将数字转换为整数）

- Lua 字符串 -> Redis 批量回复

- Lua 表（数组） -> Redis 多批量回复（如果有的话，截断到 Lua 数组中的第一个零）

- 具有单个`ok`字段的 Lua 表- > Redis 状态回复

- 具有单个`err`字段的 Lua 表 -> Redis 错误回复

- Lua boolean 错误 -> Redis 零批量回复

还有一个额外的 Lua-to-Redis 转换规则没有对应的 Redis 到 Lua 转换规则：

- Lua boolean true -> 值为1的 Redis 整数回复。还有两个重要的规则需要注意：

- Lua 有一个数字类型，Lua 数字。整数和浮点数之间没有区别。所以我们总是将 Lua 数字转换为整数回复，如果有的话删除数字的小数部分。**如果你想从 Lua 返回一个浮点数，你应该像字符串**一样**返回它**，就像 Redis 本身一样（参见例如 ZSCORE 命令）。

- 有[没有简单的方法有 lua 阵内尼尔斯](http://www.lua.org/pil/19.1.html)，这是的 Lua 表语义的结果，所以当 Redis 的一个 Lua 阵列转换成 Redis 的协议如果遇到零的转换停止。

以下是几个转换示例：

```javascript
> eval "return 10" 0
(integer) 10

> eval "return {1,2,{3,'Hello World!'}}" 0
1) (integer) 1
2) (integer) 2
3) 1) (integer) 3
   2) "Hello World!"

> eval "return redis.call('get','foo')" 0
"bar"
```

最后一个例子展示它是如何可能获得的确切的返回值`redis.call()`或者`redis.pcall()`从 Lua，如果命令直接调用将被退回。

在下面的例子中，我们可以看到如何处理带有 nils 的浮点数组和数组：

```javascript
> eval "return {1,2,3.3333,'foo',nil,'bar'}" 0
1) (integer) 1
2) (integer) 2
3) (integer) 3
4) "foo"
```

正如你可以看到3.333转化成3，永远不会返回的 bar 字符串，是因为以前是零。

## Helper 函数返回 Redis 类型

有两个帮助函数可以从 Lua 返回 Redis 类型。

- `redis.error_reply(error_string)`返回错误回复。这个函数只返回一个字段表，其中`err`字段为你设置的字段。

- `redis.status_reply(status_string)`返回状态回复。这个函数只返回一个字段表，其中`ok`字段为你设置的字段。

使用辅助函数或直接以指定格式返回表没有区别，所以以下两种形式是等价的：

```javascript
return {err="My Error"}
return redis.error_reply("My Error")
```

## 脚本的原子性

Redis 使用相同的 Lua 解释器来运行所有命令。另外，Redis 保证以原子方式执行脚本：执行脚本时不会执行其他脚本或 Redis 命令。这种语义与 MULTI / EXEC 相似。从所有其他客户端的角度来看，脚本的影响要么仍不可见，要么已经完成。

然而这也意味着执行缓慢的脚本不是一个好主意。创建快速脚本并不难，因为脚本开销非常低，但如果您要使用慢速脚本，您应该知道脚本运行时没有其他客户端可以执行命令。

## 错误处理

如前所述，`redis.call()`导致 Redis 命令错误的调用将停止脚本的执行并返回一个错误，这样就很明显错误是由脚本生成的：

```javascript
> del foo
(integer) 1
> lpush foo a
(integer) 1
> eval "return redis.call('get','foo')" 0
(error) ERR Error running script (call to f_6b1bf486c81ceb7edf3c093f4c48582e38c0e791): ERR Operation against a key holding the wrong kind of value
```

使用`redis.pcall()`不会引发错误，但错误对象是在上述规定的格式返回（作为一个Lua表与`err`字段）。该脚本可以通过返回由返回的错误对象将确切的错误传递给用户`redis.pcall()`。

## 带宽和EVALSHA

EVAL命令强制您一次又一次发送脚本正文。Redis不需要每次重新编译脚本，因为它使用内部缓存机制，但是在许多情况下，支付额外带宽的成本可能不是最佳的。

另一方面，使用特殊命令或通过定义命令`redis.conf`会是一个问题，原因如下：

- 不同的实例可能有不同的命令实现。

- 如果我们必须确保所有实例都包含给定命令，特别是在分布式环境中，则部署非常困难。

- 阅读应用程序代码，完整的语义可能不清楚，因为应用程序调用命令定义的服务器端。

为避免这些问题，同时避免带宽损失，Redis实施了EVALSHA命令。

EVALSHA的工作方式与EVAL完全相同，但不是将脚本作为第一个参数，而是使用脚本的SHA1摘要。行为如下：

- If the server still remembers a script with a matching SHA1 digest, the script is executed.

- 如果服务器不记得具有此SHA1摘要的脚本，则会返回一个特殊错误，告诉客户端使用EVAL。

例：

```javascript
> set foo bar
OK
> eval "return redis.call('get','foo')" 0
"bar"
> evalsha 6b1bf486c81ceb7edf3c093f4c48582e38c0e791 0
"bar"
> evalsha ffffffffffffffffffffffffffffffffffffffff 0
(error) `NOSCRIPT` No matching script. Please use [EVAL](/commands/eval).
```

即使客户端实际调用EVAL，客户端库实现始终可以乐观地发送EVALSHA，希望脚本已被服务器看到。如果`NOSCRIPT`错误返回，则将使用EVAL。

将键和参数作为附加EVAL参数传递在此上下文中也非常有用，因为脚本字符串保持不变并且可以由Redis高效缓存。

## 脚本缓存语义

执行的脚本保证永远在Redis实例的给定执行的脚本缓存中。这意味着如果对Redis实例执行EVAL，所有后续的EVALSHA调用都将成功。

脚本可以长时间缓存的原因是编写良好的应用程序不可能有足够的不同脚本来引起内存问题。每一个脚本在概念上都像是执行一个新的命令，甚至一个大型的应用程序可能只有几百个。即使应用程序被多次修改并且脚本会改变，所使用的内存也可以忽略不计。

刷新脚本缓存的唯一方法是明确调用SCRIPT FLUSH命令，该命令将*彻底刷新*脚本缓存，删除到目前为止执行的所有脚本。

这通常仅在实例将要为云环境中的另一个客户或应用程序实例化时才需要。

另外，如前所述，重新启动Redis实例会刷新脚本缓存，这不是持久性的。但是从客户端的角度来看，只有两种方法可以确保Redis实例在两个不同的命令之间没有重新启动。

- 我们与服务器的连接是持久的，并且从未关闭。

- 客户端显式检查`runid`INFO命令中的字段，以确保服务器未重新启动并且仍然是相同的进程。

实际上，对于客户端来说，简单地假定在给定连接的上下文中，保证缓存脚本在那里，除非管理员显式调用SCRIPT FLUSH命令。

用户可以指望Redis不删除脚本的事实在流水线上下文中在语义上很有用。

例如，与Redis持久连接的应用程序可以确保，如果脚本一旦仍在内存中就被发送，那么EVALSHA可以用于管道中的这些脚本，而不会由于未知脚本而产生错误（我们稍后会详细看到这个问题）。

一种常见的模式是调用SCRIPT LOAD加载将出现在管道中的所有脚本，然后直接在管道内部使用EVALSHA，而不需要检查由于未识别脚本哈希而导致的错误。

## SCRIPT命令

Redis提供了一个可用于控制脚本子系统的SCRIPT命令。SCRIPT目前接受三种不同的命令：

- SCRIPT FLUSH此命令是强制Redis刷新脚本缓存的唯一方法。在同一个实例可以重新分配给不同用户的云环境中，它非常有用。测试客户端库的脚本功能实现也很有用。

- `SCRIPT EXISTS sha1 sha2 ... shaN`

给定一个SHA1摘要列表作为参数，这个命令返回一个1或0的数组，其中1表示特定的SHA1被识别为已存在于脚本缓存中的脚本，而0表示具有该SHA1的脚本以前从未见过或者在最新的SCRIPT FLUSH命令之后至少从未见过）。

- `SCRIPT LOAD script` 该命令将指定的脚本注册到Redis脚本缓存中。该命令在我们希望确保EVALSHA不会失败的所有上下文中很有用（例如在管道或MULTI / EXEC操作期间），而无需实际执行脚本。

- 脚本杀手

此命令是中断长时间运行的脚本的唯一方法，该脚本可达到配置的脚本最大执行时间。SCRIPT KILL命令只能用于在执行期间不修改数据集的脚本（因为停止只读脚本不会违反脚本引擎的保证原子性）。有关长时间运行的脚本的更多信息，请参阅下一节。

## 脚本作为纯粹的功能

脚本的一个非常重要的部分是编写纯函数的脚本。默认情况下，在Redis实例中执行的脚本通过发送脚本本身而不是结果命令在副本上复制到AOF文件中。

原因是将脚本发送到另一个 Redis 实例通常比发送脚本生成的多个命令快得多，因此如果客户端将大量脚本发送给主设备，则将脚本转换为 slave / AOF 的单个命令会导致复制链接或仅追加文件的带宽太多（并且由于调度通过网络接收到的命令，因此与分派由 Lua 脚本调用的命令相比，Redis 需要更多的工作）。

通常情况下复制脚本而不是脚本的效果是合理的，但不是所有情况都是如此。因此，从 Redis 3.2 开始，脚本引擎能够复制脚本执行产生的写入命令序列，而不是复制脚本本身。有关更多信息，请参阅下一节。在本节中，我们假设通过发送整个脚本来复制脚本。我们称这种复制模式为**整个脚本复制**。

*整个脚本复制*方法的主要缺点是脚本需要具有以下属性：

- 脚本必须始终使用给定相同输入数据集的相同参数来评估相同的 Redis *写入*命令。脚本执行的操作不能依赖任何隐藏的（非显式的）信息或状态，这些信息或状态可能随着脚本执行的进行或脚本的不同执行而改变，也不依赖于来自 I / O 设备的任何外部输入。像使用系统时间，调用 Redis 随机命令（如 RANDOMKEY ）或使用 Lua 随机数生成器，可能会导致脚本不会总是以相同的方式评估。为了在脚本中强制执行此操作，Redis 将执行以下操作：

- Lua 不会导出命令来访问系统时间或其他外部状态。

- 如果脚本调用 Redis 命令，Redis 命令可以**在** Redis *随机*命令（如 RANDOMKEY ，SRANDMEMBER ，TIME ）**后**更改数据集**，**则 Redis 将阻止该脚本。这意味着如果脚本是只读的并且不修改数据集，则可以自由调用这些命令。请注意，*随机命令*不一定意味着使用随机数的命令：任何非确定性命令都被视为随机命令（这方面的最佳示例是 TIME 命令）。

- 可能以随机顺序返回元素的 Redis 命令（如 SMEMBERS（因为 Redis 集合是*无序的*））在从 Lua 调用时具有不同的行为，并在将数据返回到 Lua 脚本之前经历无声的词典排序过滤器。因此，`redis.call("smembers",KEYS[1])`将始终以相同的顺序返回 Set 元素，而从普通客户端调用的相同命令可能会返回不同的结果，即使该关键字包含完全相同的元素。

- Lua 伪随机数生成函数`math.random`并`math.randomseed`进行修改，以便每次执行新脚本时始终拥有相同的种子。这意味着`math.random`如果`math.randomseed`不使用脚本，每次执行脚本时，调用将始终生成相同的数字序列。

但是，用户仍然可以使用以下简单的技巧编写具有随机行为的命令。想象一下，我想写一个 Redis 脚本，它将用 N 个随机整数填充一个列表。

我可以从这个小小的 Ruby 程序开始：

```javascript
require 'rubygems'
require 'redis'

r = Redis.new

RandomPushScript = <<EOF
    local i = tonumber(ARGV[1])
    local res
    while (i > 0) do
        res = redis.call('lpush',KEYS[1],math.random())
        i = i-1
    end
    return res
EOF

r.del(:mylist)
puts r.eval(RandomPushScript,[:mylist],[10,rand(2**32)])
```

每次执行该脚本时，结果列表都将具有以下元素：

```javascript
> lrange mylist 0 -1
 1) "0.74509509873814"
 2) "0.87390407681181"
 3) "0.36876626981831"
 4) "0.6921941534114"
 5) "0.7857992587545"
 6) "0.57730350670279"
 7) "0.87046522734243"
 8) "0.09637165539729"
 9) "0.74990198051087"
10) "0.17082803611217"
```

为了使它成为一个纯函数，但仍然要确保每次调用脚本都会导致不同的随机元素，我们可以简单地向脚本添加一个额外的参数，这个参数将用于播种 Lua 伪随机数发电机。新脚本如下：

```javascript
RandomPushScript = <<EOF
    local i = tonumber(ARGV[1])
    local res
    math.randomseed(tonumber(ARGV[2]))
    while (i > 0) do
        res = redis.call('lpush',KEYS[1],math.random())
        i = i-1
    end
    return res
EOF

r.del(:mylist)
puts r.eval(RandomPushScript,1,:mylist,10,rand(2**32))
```

我们在这里做的是发送 PRNG 的种子作为论据之一。这样，给定相同参数的脚本输出将是相同的，但是我们正在改变每个调用中的一个参数，生成随机种子客户端。种子将作为复制链接和仅追加文件中的参数之一传播，以保证在重新加载 AOF 或从属进程处理脚本时将生成相同的更改。

注意：此行为的一个重要部分是 Redis 实现的 PRNG ，`math.random`和`math.randomseed`保证具有相同的输出，而不管运行 Redis 的系统的体系结构如何。32位，64位，大端和小端系统都会产生相同的输出。

## 复制命令而不是脚本

从 Redis 3.2 开始，可以选择另一种复制方法。我们可以复制脚本生成的单个写入命令，而不是复制整个脚本。我们称之为**脚本特效复制**。

在这种复制模式下，当执行 Lua 脚本时，Redis 将收集由 Lua 脚本引擎执行的所有实际修改数据集的命令。脚本执行完成后，脚本生成的命令序列将被包装到 MULTI / EXEC 事务中，并发送到从属和 AOF 。

根据用例，这在几个方面很有用：

- 当脚本计算速度慢时，但是这些效果可以通过几条写入命令进行汇总，但重新计算从属脚本或重新加载 AOF 时的脚本是很可惜的。在这种情况下，复制脚本的效果要好得多。

- 当启用脚本效果复制时，关于非确定性功能的控件被禁用。例如，您可以在任意位置随意使用脚本中的 TIME 或 SRANDMEMBER 命令。

- 在这种模式下的 Lua PRNG 在每次通话时随机播种。

为了启用脚本特效复制，您需要在脚本进行任何写操作之前发出以下 Lua 命令：

```javascript
redis.replicate_commands()
```

如果启用脚本效果复制，则该函数返回 true ；否则，如果在脚本已经调用某个写入命令后调用该函数，则返回 false ，并使用正常的整个脚本复制。

## 命令的选择性复制

选择脚本特技复制后（请参阅上一节），可以更多地控制命令复制到从属和 AOF 的方式。这是一个非常先进的功能，因为**滥用可以**通过违反主控，从属和 AOF 都必须包含相同逻辑内容的合同来**造成破坏**。

然而，这是一个有用的功能，因为有时候我们只需要在主服务器上执行某些命令来创建中间值。

在我们执行两套交集的 Lua 脚本中考虑一下。选取五个随机元素，并用这五个随机元素创建一个新集合。最后，我们删除表示两个原始集之间交集的临时密钥。我们想要复制的只是创建具有五个元素的新集合。复制创建临时密钥的命令也没有用。

因此，Redis 3.2 引入了一个新命令，该命令仅在脚本特效复制启用时才有效，并且能够控制脚本复制引擎。`redis.set_repl()`如果禁用脚本特技复制，则调用该命令并在调用时失败。

该命令可以用四个不同的参数调用：

```javascript
redis.set_repl(redis.REPL_ALL) -- Replicate to AOF and slaves.
redis.set_repl(redis.REPL_AOF) -- Replicate only to AOF.
redis.set_repl(redis.REPL_SLAVE) -- Replicate only to slaves.
redis.set_repl(redis.REPL_NONE) -- Don't replicate at all.
```

默认情况下，脚本引擎始终设置为`REPL_ALL`。通过调用此函数，用户可以打开/关闭AOF和/或从属复制，并稍后根据自己的意愿将其恢复。

一个简单的例子如下：

```javascript
redis.replicate_commands() -- Enable effects replication.
redis.call('set','A','1')
redis.set_repl(redis.REPL_NONE)
redis.call('set','B','2')
redis.set_repl(redis.REPL_ALL)
redis.call('set','C','3')
```

在运行上面的脚本之后，结果是只有A和C键将在从属和AOF上创建。

## 全局变量保护

Redis脚本不允许创建全局变量，以避免数据泄露到Lua状态。如果脚本需要在调用之间保持状态（非常罕见），应该使用Redis键。

当尝试全局变量访问时，脚本终止，EVAL返回错误：

```javascript
redis 127.0.0.1:6379> eval 'a=10' 0
(error) ERR Error running script (call to f_933044db579a2f8fd45d8065f04a8d0249383e57): user_script:1: Script attempted to create global variable 'a'
```

访问一个*不存在的*全局变量会产生类似的错误。

使用Lua调试功能或其他方法（例如修改用于实现全局保护的元表来避免全局保护并不难）。然而，意外地做到这一点很困难。如果用户使用Lua全局状态混淆，AOF和复制的一致性不能保证：不要这样做。

注意Lua新手：为了避免在脚本中使用全局变量，只需使用*local*关键字声明要使用的每个变量。

## 在脚本中使用SELECT

可以像使用普通客户端一样在Lua脚本中调用SELECT。但是，行为的一个细微方面在Redis 2.8.11和Redis 2.8.12之间发生了变化。在2.8.12发行版之前，由Lua脚本选择的数据库作为当前数据库*传输*到调用脚本。从Redis 2.8.12开始，由Lua脚本选择的数据库仅影响脚本本身的执行，但不会修改调用脚本的客户端选择的数据库。

修补程序级别版本之间的语义变化是必需的，因为旧的行为本身与Redis复制层不兼容，并且是错误的原因。

## 可用的库

Redis Lua解释器加载以下Lua库：

- `base` lib.

- `table` lib.

- `string` lib.

- `math` lib.

- `struct` lib.

- `cjson` lib.

- `cmsgpack` lib.

- `bitop` lib.

- `redis.sha1hex` function.

- `redis.breakpoint and redis.debug`在[Redis Lua调试器](https://redis.io/topics/ldb)的上下文中起作用。

每个Redis实例都*保证*具有上述所有库，因此您可以确保Redis脚本的环境始终如一。

struct，CJSON和cmsgpack是外部库，所有其他库都是标准的Lua库。

### struct

struct是一个用于在Lua中打包/解包结构的库。

```javascript
Valid formats:
> - big endian
< - little endian
![num] - alignment
x - pading
b/B - signed/unsigned byte
h/H - signed/unsigned short
l/L - signed/unsigned long
T   - size_t
i/In - signed/unsigned integer with size `n' (default is size of int)
cn - sequence of `n' chars (from/to a string); when packing, n==0 means
     the whole string; when unpacking, n==0 means use the previous
     read number as the string length
s - zero-terminated string
f - float
d - double
' ' - ignored
```

例：

```javascript
127.0.0.1:6379> eval 'return struct.pack("HH", 1, 2)' 0
"\x01\x00\x02\x00"
127.0.0.1:6379> eval 'return {struct.unpack("HH", ARGV[1])}' 0 "\x01\x00\x02\x00"
1) (integer) 1
2) (integer) 2
3) (integer) 5
127.0.0.1:6379> eval 'return struct.size("HH")' 0
(integer) 4
```

### CJSON

CJSON库在Lua中提供极快的JSON操作。

例：

```javascript
redis 127.0.0.1:6379> eval 'return cjson.encode({["foo"]= "bar"})' 0
"{\"foo\":\"bar\"}"
redis 127.0.0.1:6379> eval 'return cjson.decode(ARGV[1])["foo"]' 0 "{\"foo\":\"bar\"}"
"bar"
```

### cmsgpack

cmsgpack库在Lua中提供了简单快速的MessagePack操作。

例：

```javascript
127.0.0.1:6379> eval 'return cmsgpack.pack({"foo", "bar", "baz"})' 0
"\x93\xa3foo\xa3bar\xa3baz"
127.0.0.1:6379> eval 'return cmsgpack.unpack(ARGV[1])' 0 "\x93\xa3foo\xa3bar\xa3baz"
1) "foo"
2) "bar"
3) "baz"
```

### bitop

Lua位操作模块在数字上添加按位操作。自2.8.18版以来，它可用于Redis中的脚本。

例：

```javascript
127.0.0.1:6379> eval 'return bit.tobit(1)' 0
(integer) 1
127.0.0.1:6379> eval 'return bit.bor(1,2,4,8,16,32,64,128)' 0
(integer) 255
127.0.0.1:6379> eval 'return bit.tohex(422342)' 0
"000671c6"
```

它支持其他几个功能：`bit.tobit`，`bit.tohex`，`bit.bnot`，`bit.band`，`bit.bor`，`bit.bxor`，`bit.lshift`，`bit.rshift`，`bit.arshift`，`bit.rol`，`bit.ror`，`bit.bswap`。所有可用的功能都记录在[Lua BitOp文档中](http://bitop.luajit.org/api.html)

### `redis.sha1hex`

执行输入字符串的SHA1。

例：

```javascript
127.0.0.1:6379> eval 'return redis.sha1hex(ARGV[1])' 0 "foo"
"0beec7b5ea3f0fdbc95d0dd47f3c5bc275da8a33"
```

## 从脚本发出Redis日志

可以使用`redis.log`函数从Lua脚本写入Redis日志文件。

```javascript
redis.log(loglevel,message)
```

`loglevel` 是其中之一：

- `redis.LOG_DEBUG`

- `redis.LOG_VERBOSE`

- `redis.LOG_NOTICE`

- `redis.LOG_WARNING`

它们直接对应于正常的Redis日志级别。只有使用等于或大于当前配置的Redis实例日志级别的日志级别通过脚本发出的日志才会被发出。

该`message`参数是一个简单的字符串。例：

```javascript
redis.log(redis.LOG_WARNING,"Something is wrong with this script.")
```

将生成以下内容：

```javascript
[32343] 22 Mar 15:21:39 # Something is wrong with this script.
```

## 沙箱和最大执行时间

脚本不应尝试访问外部系统，如文件系统或任何其他系统调用。脚本只能在Redis数据上运行并传递参数。

脚本也受最大执行时间（默认为5秒）的限制。这个默认的超时时间很长，因为脚本通常应该在毫秒之内运行。限制主要是为了处理在开发过程中产生的意外无限循环。

可以通过`redis.conf`或使用CONFIG GET / CONFIG SET命令修改以毫秒级精度执行脚本的最长时间。影响最大执行时间的配置参数被调用`lua-time-limit`。

当脚本达到超时时，它不会由Redis自动终止，因为这违反了Redis与脚本引擎之间的合约，以确保脚本是原子性的。中断脚本意味着可能会使数据集保留半写数据。由于这个原因，当脚本执行超过指定时间时，会发生以下情况：

- Redis记录脚本运行时间过长。

- 它开始从其他客户端再次接受命令，但会向发送正常命令的所有客户端回复BUSY错误。在这种状态下唯一被允许的命令是SCRIPT KILL和`SHUTDOWN NOSAVE`。

- 可以使用SCRIPT KILL命令终止一个只执行只读命令的脚本。这不会违反脚本语义，因为脚本尚未将数据写入数据集。

- 如果脚本已经调用了写入命令，则唯一允许的命令变为`SHUTDOWN NOSAVE`停止服务器而不保存磁盘上的当前数据集（基本上服务器已中止）。

## EVALSHA在流水线上下文中

在流水线请求的上下文中执行EVALSHA时应该小心，因为即使在流水线中，命令的执行顺序也必须得到保证。如果EVALSHA将返回`NOSCRIPT`错误，则该命令不能在以后重新发布，否则违反了执行顺序。

客户端库实现应采用以下方法之一：

- 在管道环境中始终使用简单的EVAL。

- 累积所有要发送到管道中的命令，然后检查EVAL命令并使用SCRIPT EXISTS命令检查是否所有脚本都已定义。如果没有，请根据需要在管道顶部添加SCRIPT LOAD命令，并对所有EVAL呼叫使用EVALSHA。

## 调试Lua脚本

从Redis 3.2开始，Redis支持原生Lua调试。Redis Lua调试器是一个远程调试器，由一个服务器（Redis本身）和一个默认的客户端组成`redis-cli`。

Lua调试器在Redis文档的[Lua脚本调试](https://redis.io/topics/ldb)部分中进行了描述。

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18