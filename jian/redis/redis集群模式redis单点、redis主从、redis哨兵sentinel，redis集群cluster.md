# redis集群模式:redis单点、redis主从、redis哨兵sentinel，redis集群cluster

22019.07.18 10:17:03字数 3322阅读 963

目录

##### redis单点、redis主从、redis哨兵 sentinel，redis集群cluster配置搭建与使用

```css
1 .redis 安装及配置
  1.1 redis 单点
    1.1.2 在命令窗口操作redis
    1.1.3 使用jedis客户端操作redis
    1.1.4 使用spring-redis操作
    1.1.5 使用Lettuce操作redis
  1.2 redis 主从
  1.3 哨兵sentinel
    1.3.2 哨兵sentinel配置
    1.3.3 启动哨兵，使用jedis连接哨兵操作redis
    1.3.4 编写程序&运行
    1.3.5模拟主节点宕机情况
  1.4 redis cluster
    1.4.1 配置 redis cluster 集群
    1.4.2启动redis集群
    1.4.3 使用jedis连接redis cluster 集群
  1.5 总结
```

redis单点、redis主从、redis哨兵 sentinel，redis集群cluster配置搭建与使用
redis是如今被互联网公司使用最广泛的一个中间件，我们打开GitHub搜索redis，边可以看到，该项目的介绍是这样的：

> Redis is an in-memory database that persists on disk. The data model is key-value, but many different kind of values are supported: Strings, Lists, Sets, Sorted Sets, Hashes, HyperLogLogs, Bitmaps.

从这句话中，我们可以提取其特性的关键字：
in-memory database ，内存数据库
support：Strings , lists, sets ,hashes ,hyperloglogs, bitmaps
也就是高性能，支持数据类型多。本文假设你已经了解redis的基本使用，进而讨论redis的单点，高可用，集群。

## 1 .redis 安装及配置

redis的安装十分简单，打开redis的官网 [http://redis.io](https://links.jianshu.com/go?to=http%3A%2F%2Fredis.io%2F) 。

1. 下载一个最新版本的安装包，如 redis-version.tar.gz
2. 解压 `tar zxvf redis-version.tar.gz`
3. 执行 make (执行此命令可能会报错，例如确实gcc，一个个解决即可)

如果是 mac 电脑，安装redis将十分简单执行`brew install redis`即可。

安装好redis之后，我们先不慌使用，先进行一些配置。打开`redis.conf`文件，我们主要关注以下配置：

```bash
port 6379             # 指定端口为 6379，也可自行修改 
daemonize yes         # 指定后台运行
```

## 1.1 redis 单点

安装好redis之后，我们来运行一下。启动redis的命令为 :

```
redishome/bin/redis-server path/to/redis.config
```

假设我们没有配置后台运行（即：daemonize no）,那么我们会看到如下启动日志：

```go
93825:C 20 Jan 2019 11:43:22.640 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
93825:C 20 Jan 2019 11:43:22.640 # Redis version=5.0.3, bits=64, commit=00000000, modified=0, pid=93825, just started
93825:C 20 Jan 2019 11:43:22.640 # Configuration loaded
93825:S 20 Jan 2019 11:43:22.641 * Increased maximum number of open files to 10032 (it was originally set to 256).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 5.0.3 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6380
 |    `-._   `._    /     _.-'    |     PID: 93825
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'   
```

无论是否配置了后台运行，启动成功之后，我们可以新开一个命令行窗口来操作试试。

### 1.1.2 在命令窗口操作redis

使用命令：`telnet localhost 6379` 来连接redis，或者你可以直接使用代码来连接测试。连接之后，看到如下信息：

```csharp
Connected to localhost.
Escape character is '^]'.
```

我们输入几个命令试试：

```csharp
set hello world     设置key-value
get hello           获取key值
expire hello 10     设置10秒过期
ttl hello           查看过期时间
del hello           删除key
```

如此，我们便体验了一把redis，可以说是非常简单了。刚才我们是使用命令行来操作redis的，下面我们来使用代码操作一下redis,以Java为例，我们使用一个开源的 java - redis客户端。

### 1.1.3 使用jedis客户端操作redis

打开GitHub，搜索redis,进入到项目主页之后，我们可以看到使用方法：

1. 加入jedis依赖

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.0.0</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```

1. 编写代码如下

```csharp
Jedis jedis = new Jedis("localhost",6379);

jedis.set("hello", "world");

String value = jedis.get("hello");

System.out.println(value); // get world

jedis.del("hello");

System.out.println(jedis.get("hello"));// get null
```

### 1.1.4 使用spring-redis操作

上面jedis操作redis的例子很简单，除了使用jedis之外，还可以使用spring-redis。步骤如下

1. 配置redis

```xml
<bean id="jedisConnFactory"
    class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory"
    p:use-pool="true"/>

<!-- redis template definition -->
<bean id="redisTemplate"
    class="org.springframework.data.redis.core.RedisTemplate"
    p:connection-factory-ref="jedisConnFactory"/>
```

1. 编写代码

```tsx
public class Example {

    // inject the actual template
    @Autowired
    private RedisTemplate<String, String> template;

    // inject the template as ListOperations
    // can also inject as Value, Set, ZSet, and HashOperations
    @Resource(name="redisTemplate")
    private ListOperations<String, String> listOps;

    public void addLink(String userId, URL url) {
        listOps.leftPush(userId, url.toExternalForm());
        // or use template directly
        redisTemplate.boundListOps(userId).leftPush(url.toExternalForm());
    }
}
```

### 1.1.5 使用Lettuce操作redis

Lettuce是一个基于netty的 **非阻塞**的 redis客户端。支持Java8以及响应式。其官网为 [https://lettuce.io/](https://links.jianshu.com/go?to=https%3A%2F%2Flettuce.io%2F)。Lettuce也可以和spring搭配使用。
使用Lettuce需要加入如下maven依赖：

```xml
<dependency>
    <groupId>io.lettuce</groupId>
    <artifactId>lettuce-core</artifactId>
    <version>5.1.5.RELEASE</version>
</dependency>
```

基本的 get,set示例代码如下：

```csharp
public class LettuceTest {

    public static void main(String[] args) {
        RedisURI uri = new RedisURI();
        uri.setHost("myredishost");
        uri.setPort(6379);
        uri.setDatabase(0);
        RedisClient redisClient = RedisClient.create(uri);
        StatefulRedisConnection<String, String> connection = redisClient.connect();
        RedisCommands<String, String> syncCommands = connection.sync();

        syncCommands.set("testKey", "Hello, Redis!");
        System.out.println(syncCommands.get("testKey"));
        
        connection.close();
        redisClient.shutdown();
    }
}
```

## 1.2 redis 主从

上面我们启动了一台redis，并对其进行操作。当然这只是实验性的玩玩。假设我们生产环境使用了一台redis，redis挂了怎么办？如果等到运维重启redis，并恢复好数据，可能需要花费很长时间。那么在这期间，我们的服务是不可用的，这应该是不能容忍的。假设我们做了主从，主库挂了之后，运维让从库接管，那么服务可以继续运行，正所谓有备无患。

redis主从配置非常简单,过程如下(ps 演示情况下主从配置在一台电脑上)：

1. 复制两个redis配置文件（启动两个redis，只需要一份redis程序，两个不同的redis配置文件即可）

```undefined
mkdir redis-master-slave
cp path/to/redis/conf/redis.conf path/to/redis-master-slave master.conf
cp path/to/redis/conf/redis.conf path/to/redis-master-slave slave.conf
```

1. 修改配置

```css
## master.conf
port 6379

## master.conf
port 6380
slaveof 127.0.0.1 6379
```

1. 分别启动两个redis

```undefined
redis-server path/to/redis-master-slave/master.conf
redis-server path/to/redis-master-slave/slave.conf
```

启动之后，打开两个命令行窗口，分别执行telnet localhost 6379 telnet localhost 6380
然后分别在两个窗口中执行 info 命令,可以看到

```css
# Replication
role:master

# Replication
role:slave
master_host:127.0.0.1
master_port:6379
```

主从配置没问题。

然后在master 窗口执行 set 之后，到slave窗口执行get，可以get到，说明主从同步成功。

这时，我们如果在slave窗口执行 set ，会报错:

```undefined
-READONLY You can't write against a read only replica.
```

因为从节点是只读的。

## 1.3 哨兵sentinel

上面我们介绍了主从，从库作为一个“傀儡”，可以在需要的时候“顶上来”，”接盘“。我们配置的主从是为了”有备无患“，在主redis挂了之后，可以立马切换到从redis上，可能只需要花几分钟的时间，但是仍然是需要人为操作。假设主redis在晚上23点挂了，10分钟之后你接到电话，老板让你赶紧修复，于是你从被窝爬起来整，岂不是很头疼。假如你关机了，又其他人知道服务器密码，那系统岂不是要停机一晚上？太可怕了。

这个时候redis sentinel 就派上用场了。sentinel 通常翻译成哨兵，就是放哨的，这里它就是用来监控主从节点的健康情况。客户端连接redis主从的时候，先连接 sentinel，sentinel会告诉客户端主redis的地址是多少，然后客户端连接上redis并进行后续的操作。当主节点挂掉的时候，客户端就得不到连接了因而报错了，客户端重新想sentinel询问主master的地址，然后客户端得到了[新选举出来的主redis]，然后又可以愉快的操作了。

### 1.3.2 哨兵sentinel配置

为了说明sentinel的用处，我们做个试验。配置3个redis（1主2从），1个哨兵。步骤如下：

```bash
mkdir redis-sentinel
cd redis-sentinel
cp redis/path/conf/redis.conf path/to/redis-sentinel/redis01.conf
cp redis/path/conf/redis.conf path/to/redis-sentinel/redis02.conf
cp redis/path/conf/redis.conf path/to/redis-sentinel/redis03.conf
touch sentinel.conf
```

上我们创建了 3个redis配置文件，1个哨兵配置文件。我们将 redis01设置为master,将redis02，redis03设置为slave。

```css
vim redis01.conf
port 63791

vim redis02.conf
port 63792
slaveof 127.0.0.1 63791

vim redis03.conf
port 63793
slaveof 127.0.0.1 63791

vim sentinel.conf
daemonize yes
port 26379
sentinel monitor mymaster 127.0.0.1 63793 1   # 下面解释含义
```

上面的主从配置都熟悉，只有哨兵配置 sentinel.conf，需要解释一下：

```css
mymaster        为主节点名字，可以随便取，后面程序里边连接的时候要用到
127.0.0.1 63793 为主节点的 ip,port
1               后面的数字 1 表示选举主节点的时候，投票数。1表示有一个sentinel同意即可升级为master
```

### 1.3.3 启动哨兵，使用jedis连接哨兵操作redis

上面我们配置好了redis主从，1主2从，以及1个哨兵。下面我们分别启动redis，并启动哨兵

```undefined
redis-server path/to/redis-sentinel/redis01.conf
redis-server path/to/redis-sentinel/redis02.conf
redis-server path/to/redis-sentinel/redis03.conf

redis-server path/to/redis-sentinel/sentinel.conf --sentinel
```

启动之后，可以分别连接到 3个redis上，执行info查看主从信息。

### 1.3.4 编写程序&运行

下面使用程序来连接哨兵，并操作redis。

```csharp
   public static void main(String[] args) throws Exception{

        Set<String> hosts = new HashSet<>();
        hosts.add("127.0.0.1:26379");
        //hosts.add("127.0.0.1:36379"); 配置多个哨兵
       
        JedisSentinelPool pool = new JedisSentinelPool("mymaster",hosts);
        Jedis jedis = null;

        for(int i=0 ;i<20;i++){
            Thread.sleep(2000);

            try{

                jedis = pool.getResource();
                String v = randomString();
                jedis.set("hello",v);

                System.out.println(v+"-->"+jedis.get("hello").equals(v));

            }catch (Exception e){
                System.out.println(" [ exception happened]" + e);
            }
        }
    }
```

程序非常简单，循环运行20次，连接哨兵，将随机字符串 set到redis，get结果。打印信息，异常捕获。

### 1.3.5模拟主节点宕机情况

运行上面的程序（**注意，在实验这个效果的时候，可以将sleep时间加长或者for循环增多，以防程序提前停止，不便看整体效果**），然后将主redis关掉，模拟redis挂掉的情况。现在主redis为redis01,端口为63791

```undefined
redis-cli -p 63791 shutdown
```

这个时候如果sentinel没有设置后台运行，可以在命令行窗口看到 master切换的情况日志。

```bash
# Sentinel ID is fd0634dc9876ec60da65db5ff1e50ebbeefdf5ce
# +monitor master mymaster 127.0.0.1 63791 quorum 1
* +slave slave 127.0.0.1:63792 127.0.0.1 63792 @ mymaster 127.0.0.1 63791
* +slave slave 127.0.0.1:63793 127.0.0.1 63793 @ mymaster 127.0.0.1 63791
# +sdown master mymaster 127.0.0.1 63791
# +odown master mymaster 127.0.0.1 63791 #quorum 1/1
# +new-epoch 1
# +try-failover master mymaster 127.0.0.1 63791
# +vote-for-leader fd0634dc9876ec60da65db5ff1e50ebbeefdf5ce 1
# +elected-leader master mymaster 127.0.0.1 63791
# +failover-state-select-slave master mymaster 127.0.0.1 63791
# +selected-slave slave 127.0.0.1:63793 127.0.0.1 63793 @ mymaster 127.0.0.1 63791
* +failover-state-send-slaveof-noone slave 127.0.0.1:63793 127.0.0.1 63793 @ mymaster 127.0.0.1 63791
* +failover-state-wait-promotion slave 127.0.0.1:63793 127.0.0.1 63793 @ mymaster 127.0.0.1 63791
# +promoted-slave slave 127.0.0.1:63793 127.0.0.1 63793 @ mymaster 127.0.0.1 63791
# +failover-state-reconf-slaves master mymaster 127.0.0.1 63791
* +slave-reconf-sent slave 127.0.0.1:63792 127.0.0.1 63792 @ mymaster 127.0.0.1 63791
* +slave-reconf-inprog slave 127.0.0.1:63792 127.0.0.1 63792 @ mymaster 127.0.0.1 63791
* +slave-reconf-done slave 127.0.0.1:63792 127.0.0.1 63792 @ mymaster 127.0.0.1 63791
# +failover-end master mymaster 127.0.0.1 63791
# +switch-master mymaster 127.0.0.1 63791 127.0.0.1 63793
* +slave slave 127.0.0.1:63792 127.0.0.1 63792 @ mymaster 127.0.0.1 63793
* +slave slave 127.0.0.1:63791 127.0.0.1 63791 @ mymaster 127.0.0.1 63793
# +sdown slave 127.0.0.1:63791 127.0.0.1 63791 @ mymaster 127.0.0.1 63793
# -sdown slave 127.0.0.1:63791 127.0.0.1 63791 @ mymaster 127.0.0.1 63793
* +convert-to-slave slave 127.0.0.1:63791 127.0.0.1 63791 @ mymaster 127.0.0.1 63793
```

上面的日志较多，仔细找找可以看到下面几行主要的：

```css
初始情况下，1主2从
# +monitor master mymaster 127.0.0.1 63791 quorum 1
* +slave slave 127.0.0.1:63792 127.0.0.1 63792 @ mymaster 127.0.0.1 63791
* +slave slave 127.0.0.1:63793 127.0.0.1 63793 @ mymaster 127.0.0.1 63791
发现主挂了，准备 故障转移
# +try-failover master mymaster 127.0.0.1 63791
将主切换到了 63793 即redis03 
# +switch-master mymaster 127.0.0.1 63791 127.0.0.1 63793
```

这个日志比较晦涩，从代码运行效果看，如下：

```css
14:45:20.675 [main] INFO redis.clients.jedis.JedisSentinelPool - Trying to find master from available Sentinels...
14:45:25.731 [main] DEBUG redis.clients.jedis.JedisSentinelPool - Connecting to Sentinel 192.168.1.106:26379
14:45:25.770 [main] DEBUG redis.clients.jedis.JedisSentinelPool - Found Redis master at 127.0.0.1:63792
14:45:25.771 [main] INFO redis.clients.jedis.JedisSentinelPool - Redis master running at 127.0.0.1:63792, starting Sentinel listeners...
14:45:25.871 [main] INFO redis.clients.jedis.JedisSentinelPool - Created JedisPool to master at 127.0.0.1:63792
ejahaeegig-->true
deeeadejjf-->true
 [ exception happened]redis.clients.jedis.exceptions.JedisConnectionException: Could not get a resource from the pool
 [ exception happened]........
 [ exception happened]........
 [ exception happened]........
 [ exception happened]........
 [ exception happened]........
 [ exception happened]........
 [ exception happened]........
 [ exception happened]........
 [ exception happened]........
 14:46:02.737 [MasterListener-mymaster-[192.168.1.106:26379]] DEBUG redis.clients.jedis.JedisSentinelPool - Sentinel 192.168.1.106:26379 published: mymaster 127.0.0.1 63792 127.0.0.1 63793.
14:46:02.738 [MasterListener-mymaster-[192.168.1.106:26379]] INFO redis.clients.jedis.JedisSentinelPool - Created JedisPool to master at 127.0.0.1:63793
haiihiihbb-->true
ifgebdcicd-->true
aajhbjagag-->true

Process finished with exit code 0
```

从结果看出

开始正常操作redis，并设置了两次。
主redis挂了，jedis得不到连接，报错了JedisConnectionException：Could not get a resource from the pool
主redis没选好之前，程序持续报错。
主redis选好了，程序正常运行，最后结束。
我们看到最后一次运行设置的值是aajhbjagag,我们可以连接剩下的2台redis中的任意一台，get hello,结果肯定是一致的。

## 1.4 redis cluster

上面的章节中，我们分别学习了redis 单点，redis主从，并增加了高可用的 sentinel 哨兵模式。我们所做的这些工作只是保证了数据备份以及高可用，目前为止我们的程序一直都是向1台redis写数据，其他的redis只是备份而已。实际场景中，单个redis节点可能不满足要求，因为：

- 单个redis并发有限
- 单个redis接收所有的数据，最终回导致内存太大，内存太大回导致rdb文件过大，从很大的rdb文件中同步恢复数据会很慢。

所有，我们需要redis cluster 即redis集群。

Redis 集群是一个提供在**多个Redis间节点间共享数据**的程序集。

Redis集群并不支持处理多个keys的命令,因为这需要在不同的节点间移动数据,从而达不到像Redis那样的性能,在高负载的情况下可能会导致不可预料的错误.

Redis 集群通过分区来提供**一定程度的可用性**,在实际环境中当某个节点宕机或者不可达的情况下继续处理命令. Redis 集群的优势:

- 自动分割数据到不同的节点上。
- 整个集群的部分节点失败或者不可达的情况下能够继续处理命令。

为了配置一个redis cluster,我们需要准备至少6台redis，为啥至少6台呢？我们可以在redis的官方文档中找到如下一句话：

```csharp
Note that the minimal cluster that works as expected requires to contain at least three master nodes. 
```

因为最小的redis集群，需要至少3个主节点，既然有3个主节点，而一个主节点搭配至少一个从节点，因此至少得6台redis。然而对我来说，就是复制6个redis配置文件。本实验的redis集群搭建依然在一台电脑上模拟。

### 1.4.1 配置 redis cluster 集群

上面提到，配置redis集群需要至少6个redis节点。因此我们需要准备及配置的节点如下：

```undefined
主：redis01  从 redis02    slaveof redis01
主：redis03  从 redis04    slaveof redis03
主：redis05  从 redis06    slaveof redis05
mkdir redis-cluster
cd redis-cluster
mkdir redis01 到 redis06 6个文件夹
cp redis.conf 到 redis01 ... redis06
修改端口
分别配置3组主从关系
```

### 1.4.2启动redis集群

上面的配置完成之后，分别启动6个redis实例。配置正确的情况下，都可以启动成功。然后运行如下命令创建集群：

```undefined
redis-5.0.3/src/redis-cli --cluster create 127.0.0.1:6371 127.0.0.1:6372 127.0.0.1:6373 127.0.0.1:6374 127.0.0.1:6375 127.0.0.1:6376 --cluster-replicas 1
```

注意，这里使用的是ip:port，而不是 domain:port ，因为我在使用 localhost:6371 之类的写法执行的时候碰到错误：

```css
ERR Invalid node address specified: localhost:6371
```

执行成功之后，连接一台redis，执行 cluster info 会看到类似如下信息：

```css
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
cluster_current_epoch:6
cluster_my_epoch:1
cluster_stats_messages_ping_sent:1515
cluster_stats_messages_pong_sent:1506
cluster_stats_messages_sent:3021
cluster_stats_messages_ping_received:1501
cluster_stats_messages_pong_received:1515
cluster_stats_messages_meet_received:5
cluster_stats_messages_received:3021
```

我们可以看到`cluster_state:ok`,`cluster_slots_ok:16384`,`cluster_size:3`。

### 1.4.3 使用jedis连接redis cluster 集群

上面我们配置了一个redis集群，包含6个redis节点，3主3从。下面我们来使用jedis来连接redis集群。代码如下：

```csharp
    public static void main(String[] args) {

        Set<HostAndPort> jedisClusterNodes = new HashSet<HostAndPort>();
        //Jedis Cluster will attempt to discover cluster nodes automatically
        jedisClusterNodes.add(new HostAndPort("127.0.0.1", 6371));
        jedisClusterNodes.add(new HostAndPort("127.0.0.1", 6372));
        jedisClusterNodes.add(new HostAndPort("127.0.0.1", 6373));
        jedisClusterNodes.add(new HostAndPort("127.0.0.1", 6374));
        jedisClusterNodes.add(new HostAndPort("127.0.0.1", 6375));
        jedisClusterNodes.add(new HostAndPort("127.0.0.1", 6376));
        JedisCluster jc = new JedisCluster(jedisClusterNodes);
        jc.set("foo", "bar");
        String value = jc.get("foo");
        System.out.println(" ===> " + value);
    }
```

上面我们设置了信息`set foo bar`，但是不知道被设置到那一台redis上去了。请读者思考一下，我们是集群模式，所以数据被分散放到不同的槽中了，Redis 集群有16384个哈希槽,每个key通过CRC16校验后对16384取模来决定放置哪个槽.集群的每个节点负责一部分hash槽,举个例子,比如当前集群有3个节点,那么:

- 节点 A 包含 0 到 5500号哈希槽.
- 节点 B 包含5501 到 11000 号哈希槽.
- 节点 C 包含11001 到 16384号哈希槽.

看到这里你应该还是不知道`set foo bar` 放到哪台redis上去了，不妨尝试连接任意一台redis探索一下，你会知道的。

## 总结

至此，我们了解并动手实践了redis的安装，redis单点，redis主从，redis 哨兵 sentinel，redis 集群cluster。
我们来梳理一下redis主从，redis哨兵，redis机器的区别和关系。

redis主从：是备份关系， 我们操作主库，数据也会同步到从库。 如果主库机器坏了，从库可以上。就好比你 D盘的片丢了，但是你移动硬盘里边备份有。
redis哨兵：哨兵保证的是HA，保证特殊情况故障自动切换，哨兵盯着你的“redis主从集群”，如果主库死了，它会告诉你新的老大是谁。
redis集群：集群保证的是高并发，因为多了一些兄弟帮忙一起扛。同时集群会导致数据的分散，整个redis集群会分成一堆数据槽，即不同的key会放到不不同的槽中。

主从保证了数据备份，哨兵保证了HA 即故障时切换，集群保证了高并发性。

一切动手做了才会熟悉。