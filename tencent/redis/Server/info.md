# info

```javascript
INFO [section]
```

**自1.0.0起可用。**

INFO 命令返回有关服务器的信息和统计信息，格式简单易懂，易于人工阅读。

可选参数可用于选择特定部分的信息：

- `server`：有关 Redis 服务器的一般信息

- `clients`：客户端连接部分

- `memory`：内存消耗相关信息

- `persistence`：RDB 和 AOF 相关信息

- `stats`：一般统计资料

- `replication`：主/从复制信息

- `cpu`：CPU消耗统计

- `commandstats`：Redis 命令统计

- `cluster`：Redis 群集部分

- `keyspace`：数据库相关统计

它也可以采用以下值：

- `all`：返回所有部分

- `default`：仅返回默认的一组部分

当没有提供参数时，假定该`default`选项。

## 返回值

[批量字符串回复](https://redis.io/topics/protocol#bulk-string-reply)：作为文本行的集合。

行可以包含部分名称（以＃字符开头）或属性。所有的`field:value`属性都以终止`\r\n`的形式。

redis> INFO `# Server redis_version:999.999.999 redis_git_sha1:e09e31b1 redis_git_dirty:0 redis_build_id:f05c0e7d7d91e005 redis_mode:standalone os:Linux 4.8.0-1-amd64 x86_64 arch_bits:64 multiplexing_api:epoll gcc_version:6.2.0 process_id:934 run_id:feb550f7d0ffb809357adf071709bb037e8e8969 tcp_port:6379 uptime_in_seconds:2791032 uptime_in_days:32 hz:10 lru_clock:13100366 executable:/usr/local/bin/redis-server config_file: # Clients connected_clients:4 client_longest_output_list:0 client_biggest_input_buf:0 blocked_clients:0 # Memory used_memory:251242072 used_memory_human:239.60M used_memory_rss:259956736 used_memory_rss_human:247.91M used_memory_peak:251282528 used_memory_peak_human:239.64M used_memory_peak_perc:99.98% used_memory_overhead:74887972 used_memory_startup:487168 used_memory_dataset:176354100 used_memory_dataset_perc:70.33% total_system_memory:1044770816 total_system_memory_human:996.37M used_memory_lua:37888 used_memory_lua_human:37.00K maxmemory:0 maxmemory_human:0B maxmemory_policy:noeviction mem_fragmentation_ratio:1.03 mem_allocator:jemalloc-4.0.3 lazyfree_pending_objects:0 # Persistence loading:0 rdb_changes_since_last_save:6876033 rdb_bgsave_in_progress:0 rdb_last_save_time:1503481558 rdb_last_bgsave_status:ok rdb_last_bgsave_time_sec:-1 rdb_current_bgsave_time_sec:-1 rdb_last_cow_size:0 aof_enabled:0 aof_rewrite_in_progress:0 aof_rewrite_scheduled:0 aof_last_rewrite_time_sec:-1 aof_current_rewrite_time_sec:-1 aof_last_bgrewrite_status:ok aof_last_write_status:ok aof_last_cow_size:0 # Stats total_connections_received:23 total_commands_processed:16508817 instantaneous_ops_per_sec:513 total_net_input_bytes:1358627279 total_net_output_bytes:2356537899 instantaneous_input_kbps:41.00 instantaneous_output_kbps:552.63 rejected_connections:0 sync_full:0 sync_partial_ok:0 sync_partial_err:0 expired_keys:20321 evicted_keys:0 keyspace_hits:3796517 keyspace_misses:1588914 pubsub_channels:0 pubsub_patterns:0 latest_fork_usec:0 migrate_cached_sockets:0 # Replication role:master connected_slaves:0 master_replid:9ba40bba4cef6177aa5cff0c69964cdee6a6ca21 master_replid2:0000000000000000000000000000000000000000 master_repl_offset:0 second_repl_offset:-1 repl_backlog_active:0 repl_backlog_size:1048576 repl_backlog_first_byte_offset:0 repl_backlog_histlen:0 # CPU used_cpu_sys:3633.24 used_cpu_user:18088.07 used_cpu_sys_children:0.00 used_cpu_user_children:0.00 # Cluster cluster_enabled:0 # Keyspace db0:keys=1435809,expires=723,avg_ttl=49231687326`

## 注意

请注意，根据 Redis 的版本，某些字段已添加或删除。因此，健壮的客户端应用程序应该通过跳过未知属性来解析此命令的结果，并优雅地处理缺失的字段。

以下是 Redis> = 2.4的字段说明。

以下是**服务器**部分中所有字段的含义：

- `redis_version`：Redis 服务器的版本

- `redis_git_sha1`: Git SHA1

- `redis_git_dirty`: Git dirty flag

- `os`：托管 Redis 服务器的操作系统

- `arch_bits`：体系结构（32或64位）

- `multiplexing_api`：Redis 使用的事件循环机制

- `gcc_version`：用于编译 Redis 服务器的 GCC 编译器的版本

- `process_id`：服务器进程的 PID

- `run_id`：标识 Redis 服务器的随机值（将由 Sentinel 和 Cluster 使用）

- `tcp_port`：TCP / IP 侦听端口

- `uptime_in_seconds`：自 Redis 服务器启动以来的秒数

- `uptime_in_days`：以天为单位表示相同的值

- `lru_clock`：每分钟递增一次，用于 LRU 管理

以下是**客户**部分中所有字段的含义：

- `connected_clients`：客户端连接数（不包括从站的连接数）

- `client_longest_output_list`：当前客户端连接中最长的输出列表

- `client_biggest_input_buf`：当前客户端连接中最大的输入缓冲区

- `blocked_clients`：阻塞呼叫中挂起的客户端数量（BLPOP，BRPOP，BRPOPLPUSH）

以下是**内存**部分中所有字段的含义：

- `used_memory`：Redis 使用其分配程序分配的字节总数（标准 **libc**，**jemalloc** 或替代分配程序，如 [**tcmalloc**](http://code.google.com/p/google-perftools/)

- `used_memory_human`：以前值的人类可读表示

- `used_memory_rss`：操作系统看到的 Redis 分配的字节数（又称常驻集大小）。这是由诸如`top(1)`and`ps(1)`之类的工具报告的数字

- `used_memory_peak`：Redis 消耗的最高内存（以字节为单位）

- `used_memory_peak_human`：以前值的人类可读表示

- `used_memory_lua`：Lua 引擎使用的字节数

- `mem_fragmentation_ratio`：`used_memory_rss`和`used_memory` 之间的比率

- `mem_allocator`：内存分配器，在编译时选择

理想情况下，`used_memory_rss`价值应该只比略高`used_memory`。当使用rss >>时，差异很大意味着有内存碎片（内部或外部），可以通过检查来评估`mem_fragmentation_ratio`。当使用>> rss时，表示 Redis 内存的一部分已被操作系统换掉：期望有一些显着的延迟。

由于 Redis 无法控制其分配映射到内存页面的方式，高`used_memory_rss`是内存使用率通常会增加的结果。

当 Redis 释放内存时，内存将返回给分配器，并且分配器可能会或可能不会将内存释放回系统。`used_memory`操作系统报告的值和内存消耗之间可能存在差异。这可能是由于 Redis 已经使用和释放内存的事实，但并未回馈给系统。该`used_memory_peak`值通常用于检查这一点。

以下是**持久性**部分中所有字段的含义：

- `loading`：表示转储文件的负载是否正在进行的标志

- `rdb_changes_since_last_save`：自上次转储以来的更改次数

- `rdb_bgsave_in_progress`：表示 RDB 保存的标志正在进行中

- `rdb_last_save_time`：上次成功 RDB 保存的基于时代的时间戳

- `rdb_last_bgsave_status`：最后一次 RDB 保存操作的状态

- `rdb_last_bgsave_time_sec`：以秒为单位的最后一次 RDB 保存操作的持续时间

- `rdb_current_bgsave_time_sec`：正在进行的 RDB 保存操作的持续时间（如果有）

- `aof_enabled`：指示 AOF 记录的标志被激活

- `aof_rewrite_in_progress`：表示正在进行 AOF 重写操作的标志

- `aof_rewrite_scheduled`：一旦正在进行的 RDB 保存完成，将会计划指示 AOF 重写操作的标志。

- `aof_last_rewrite_time_sec`：最后一次 AOF 重写操作的持续时间，以秒为单位

- `aof_current_rewrite_time_sec`：正在进行的 AOF 重写操作的持续时间（如果有）

- `aof_last_bgrewrite_status`：最后一次 AOF 重写操作的状态

`changes_since_last_save` 指的是自上次调用 SAVE 或 BGSAVE 以来在数据集中产生某种更改的操作数。

如果激活了 AOF，则会添加这些附加字段：

- `aof_current_size`：AOF 当前文件大小

- `aof_base_size`：最近启动或重写时的 AOF 文件大小

- `aof_pending_rewrite`：一旦正在进行的 RDB 保存完成，将会计划指示 AOF 重写操作的标志。

- `aof_buffer_length`：AOF 缓冲区的大小

- `aof_rewrite_buffer_length`：AOF 重写缓冲区的大小

- `aof_pending_bio_fsync`：后台 I / O 队列中 fsync 挂起作业的数量

- `aof_delayed_fsync`：延迟 fsync 计数器

如果正在进行加载操作，则会添加这些附加字段：

- `loading_start_time`：加载操作开始的基于时间点的时间戳

- `loading_total_bytes`：文件总大小

- `loading_loaded_bytes`：已加载的字节数

- `loading_loaded_perc`：以百分比表示的相同数值

- `loading_eta_seconds`：ETA 以秒为单位，负载完成

以下是**统计**部分中所有字段的含义：

- `total_connections_received`：服务器接受的连接总数

- `total_commands_processed`：服务器处理的命令总数

- `instantaneous_ops_per_sec`：每秒处理的命令数

- `rejected_connections`：由于`maxclients`限制而被拒绝的连接数

- `expired_keys`：密钥到期事件的总数

- `evicted_keys`：由于`maxmemory`限制而被驱逐的密钥数量

- `keyspace_hits`：在主词典中成功查找键的次数

- `keyspace_misses`：主字典中键的失败查找次数

- `pubsub_channels`：客户订阅的发布/订阅频道的全部数量

- `pubsub_patterns`：客户订阅的发布/订阅模式的全部数量

- `latest_fork_usec`：以微秒为单位的最新分叉操作的持续时间

以下是**复制**部分中所有字段的含义：

- `role`：如果实例是没有人的奴隶，则值为“主人”;如果实例是奴隶主人，则值为“奴隶”。请注意，从站可以是另一个从站的主站（雏菊链）。如果实例是从站，则提供以下附加字段：

- `master_host`：主机的主机或IP地址

- `master_port`：主控侦听 TCP 端口

- `master_link_status`：链接的状态（向上/向下）

- `master_last_io_seconds_ago`：自上次与主交互之后的秒数

- `master_sync_in_progress`：指示主站正在同步到从站

如果正在进行 SYNC 操作，则会提供以下附加字段：

- `master_sync_left_bytes`：同步完成之前剩余的字节数

- `master_sync_last_io_seconds_ago`：SYNC操作期间自上次传输 I / O 以来的秒数

如果主站和从站之间的链路断开，则会提供附加字段：

- `master_link_down_since_seconds`：自链路断开以来的秒数总是提供以下字段：

- `connected_slaves`：连接的从站数量

对于每个从站，添加以下行：

- `slaveXXX`：id，IP 地址，端口，stateHere 是 **cpu** 部分中所有字段的含义：

- `used_cpu_sys`：Redis 服务器使用的系统 CPU

- `used_cpu_user`：Redis 服务器使用的用户 CPU

- `used_cpu_sys_children`：后台进程使用的系统 CPU

- `used_cpu_user_children`: User CPU consumed by the background processes

所述 **commandstats** 部分提供基于该命令类型的统计信息，包括呼叫的数量，这些命令所消耗的总的 CPU 时间，和每命令执行所消耗的平均 CPU。

对于每个命令类型，添加以下行：

- `cmdstat_XXX`：`calls=XXX,usec=XXX,usec_per_call=XXX`该**集群**节目前只包含一个独特的领域：

- `cluster_enabled`：表示启用了 Redis 群集

该**密钥空间**部分提供了每个数据库的主词典的统计数据。统计信息是密钥的数量，以及过期密钥的数量。

对于每个数据库，添加以下行：

- `dbXXX`: `keys=XXX,expires=XXX`

本文档系腾讯云云+社区成员共同维护，如有问题请联系 yunjia_community@tencent.com

最后更新于：2017-12-18