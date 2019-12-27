这一节看一下Java版本的redis客户端工具来操作redis。
jedis封装了丰富的api来操作redis，可以说命令行界面的命令它都有。
开始在 Java 中使用 Redis 前， 我们需要确保已经[安装](https://www.cnblogs.com/ibigboy/p/11143677.html)了 redis 服务，且你的机器上能正常使用 Java。

## 导入Java操作Redis的客户端工具--Jedis

```xml
<dependency>
     <groupId>redis.clients</groupId>
     <artifactId>jedis</artifactId>
     <version>3.1.0</version>
</dependency>
```
## Jedis操作Redis
### 操作string

```java
//string
//连接本地Redis服务
Jedis jedis = new Jedis("127.0.0.1",6379);

jedis.set("name","walking");
jedis.set("weixin","编程大道");

System.out.println(jedis.get("name"));//walking
System.out.println(jedis.get("weixin"));//编程大道
jedis.close();
```
### 操作list

```
//list
Jedis jedis = new Jedis("127.0.0.1",6379);

jedis.del("cityList");
jedis.lpush("cityList","北京","上海","重庆","深圳");

List<String> cityList1 = jedis.lrange("cityList", 0, -1);
System.out.println(cityList1);//[深圳, 重庆, 上海, 北京]
System.out.println(jedis.llen("cityList"));//4
System.out.println(jedis.lpop("cityList"));//深圳
System.out.println(jedis.rpop("cityList"));//北京
System.out.println(jedis.llen("cityList"));//2

jedis.close();
```
### 操作hash

```java
//hash
Jedis jedis = new Jedis("127.0.0.1",6379);

jedis.hset("user_0001","name","walking");
jedis.hset("user_0001","sex","1");
jedis.hset("user_0001","age","24");

System.out.println(jedis.hget("user_0001", "name"));
System.out.println(jedis.hget("user_0001", "sex"));
System.out.println(jedis.hget("user_0001", "age"));
Map<String, String> user_0001 = jedis.hgetAll("user_0001");
System.out.println(user_0001);//{name=walking, age=24, sex=1}
jedis.hdel("user_0001","age");
user_0001 = jedis.hgetAll("user_0001");
System.out.println(user_0001);//{name=walking, sex=1}

jedis.close();
```
### 操作set

```java
//set
Jedis jedis = new Jedis("127.0.0.1",6379);

jedis.sadd("articleSet","0001","0002","0003","0004");
//返回key集合所有的元素 
System.out.println(jedis.smembers("articleSet"));// [0004, 0001, 0002, 0003]
//移除并返回一个随机元素
System.out.println(jedis.spop("articleSet"));//0001
System.out.println(jedis.smembers("articleSet"));//[0004, 0002, 0003]
System.out.println(jedis.srandmember("articleSet"));//随机返回 0003
jedis.sadd("articleSet2","0022", "0004", "0021");
System.out.println(jedis.sinter("articleSet","articleSet2"));//交集 [0004]

jedis.close();
```
###  操作zset
```java
//zset
Jedis jedis = new Jedis("127.0.0.1",6379);

jedis.zadd("zset",3D,"0003");
jedis.zadd("zset",1D,"0001");
jedis.zadd("zset",4D,"0004");
jedis.zadd("zset",2.5D,"00025");
jedis.zadd("zset",2D,"0002");

System.out.println(jedis.zcard("zset"));//5
System.out.println(jedis.zcount("zset",2,3));//3
//通过分值区间查找成员
Set<String> zset = jedis.zrangeByScore("zset", 2D, 3D);
System.out.println(zset);////[0002, 00025, 0003]
//通过下标查找成员
System.out.println(jedis.zrange("zset", 0, -1));//[0001, 0002, 00025, 0003, 0004]
ScanResult<Tuple> zset1 = jedis.zscan("zset", ScanParams.SCAN_POINTER_START);
System.out.println(zset1.getCursor());//0
System.out.println(zset1.getResult());//[[0001,1.0], [0002,2.0], [00025,2.5], [0003,3.0], [0004,4.0]]
jedis.close();
```
### 系统命令

```java
//server
System.out.println("=========== server =============");
System.out.println(jedis.dbSize());//key的数量
System.out.println(jedis.time());//系统时间
System.out.println(jedis.clientList());//客户端连接列表
System.out.println(jedis.info());//redis信息
```
output
```bash
=========== server =============
//key的数量
7
//系统时间
[1575610535, 74018]
//客户端连接列表
id=42 addr=127.0.0.1:63466 fd=7 name= age=0 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 events=r cmd=client
//redis信息
# Server
redis_version:3.2.100
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:dd26f1f93c5130ee
redis_mode:standalone
os:Windows  
arch_bits:64
multiplexing_api:WinSock_IOCP
process_id:10960
run_id:1acb04abaca68c04e9e027017c301c04f9a402d2
tcp_port:6379
uptime_in_seconds:15572
uptime_in_days:0
hz:10
lru_clock:15329446
executable:D:\mysoft\redis-3.2.100\redis-server.exe
config_file:D:\mysoft\redis-3.2.100\redis.windows.conf

# Clients
connected_clients:1
client_longest_output_list:0
client_biggest_input_buf:0
blocked_clients:0

# Memory
used_memory:690720
used_memory_human:674.53K
used_memory_rss:689688
used_memory_rss_human:673.52K
used_memory_peak:690720
used_memory_peak_human:674.53K
total_system_memory:0
total_system_memory_human:0B
used_memory_lua:37888
used_memory_lua_human:37.00K
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
mem_fragmentation_ratio:1.00
mem_allocator:jemalloc-3.6.0

# Persistence
loading:0
rdb_changes_since_last_save:15
rdb_bgsave_in_progress:1
rdb_last_save_time:1575603910
rdb_last_bgsave_status:ok
rdb_last_bgsave_time_sec:0
rdb_current_bgsave_time_sec:0
aof_enabled:0
aof_rewrite_in_progress:0
aof_rewrite_scheduled:0
aof_last_rewrite_time_sec:-1
aof_current_rewrite_time_sec:-1
aof_last_bgrewrite_status:ok
aof_last_write_status:ok

# Stats
total_connections_received:41
total_commands_processed:894
instantaneous_ops_per_sec:19
total_net_input_bytes:34647
total_net_output_bytes:18067
instantaneous_input_kbps:0.76
instantaneous_output_kbps:0.25
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:0
evicted_keys:0
keyspace_hits:453
keyspace_misses:7
pubsub_channels:0
pubsub_patterns:0
latest_fork_usec:55286
migrate_cached_sockets:0

# Replication
role:master
connected_slaves:0
master_repl_offset:0
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

# CPU
used_cpu_sys:0.53
used_cpu_user:0.27
used_cpu_sys_children:0.00
used_cpu_user_children:0.00

# Cluster
cluster_enabled:0

# Keyspace
db0:keys=7,expires=0,avg_ttl=0
```
更多命令请参考：[Redis命令中心](http://www.redis.cn/commands.html)