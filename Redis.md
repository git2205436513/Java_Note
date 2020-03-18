# Redis ver 1.0
    fanfuni 2020/2/26 
## NoSQL
### 什么是NoSQL
NoSQL(NoSQL = Not Only SQL )，意即“不仅仅是SQL”，泛指非关系型的数据库。
### NoSQL为什么出现
由于数据量的逐渐增大、索引增大、访问量增大，单机MySQL逐渐的不给力。于是程序员选择使用缓存技术来缓解数据库压力，例如最开始使用的文件缓存。

但是随着访问量继续增大，大量的文件缓存也带来了比较大的IO压力，于是**Memcached**应运而生(Memcached是一个自由开源的，高性能，分布式内存对象缓存系统)。

但由于Memcached是能缓解读取压力，所以大型网站开始使用主从复制技术来达到读写分离来提高性能，即MySQL的master-slave模式（maseter写，slave读）。随着读写压力的进一步增加，由于MyISAM存在表锁的问题，MySQL数据库开始使用InnoDB来代替。同时业界开始使用分表分库来缓解，MySQL也提出了表分离和MySQL CluSter集群。

但MYSQL数据库本身性能也逐渐遇到了瓶颈，例如不容易快速恢复数据库，扩展性差，大数据下IO压力大，表结构更改困难等。

于是NoSQL出现了，它抛弃了关系型数据库的一些东西，例如NoSQL基于CAP模型，SQL基于ACID模型。解决了大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题，包括超大规模数据的存储。
### NoSQL特点
#### 易扩展：
NoSQL数据库种类繁多，但是一个共同的特点都是去掉关系数据库的关系型特性。数据之间无关系，这样就非常容易扩展。也无形之间，在架构的层面上带来了可扩展的能力。
#### 大数据量高性能:
NoSQL数据库都具有非常高的读写性能，尤其在大数据量下，同样表现优秀。这得益于它的无关系性，数据库的结构简单。
#### 多样灵活的数据模型：
NoSQL无需事先为要存储的数据建立字段，随时可以存储自定义的数据格式。
### 传统RDBMS VS NOSQL
#### RDBMS 
- 高度组织化结构化数据
- 结构化查询语言（SQL）
- 数据和关系都存储在单独的表中。
- 数据操纵语言，数据定义语言
- 严格的一致性
- 基础事务

#### NoSQL
- 代表着不仅仅是SQL
- 没有声明性查询语言
- 没有预定义的模式
- 键-值对存储，列存储，文档存储，图形数据库
- 最终一致性，而非ACID属性
- 非结构化和不可预知的数据
- CAP定理

### 3V与3高
大数据时代的3v：海量、多样、实时

互联网需求的3高：高并发、高可扩、高性能
### NoSQL数据模型
- KV键值对（redis）
- Bson （MongoDB）
- 列族（Cassandra, HBase）
- 图形（Neo4J, InfoGrid）

#### 优缺点比较
KV：查找速度快，但数据无法结构化

文档型：数据结构灵活多变，但查询性能不高，且缺乏统一的查询语法

列存储：查找速度快，可扩展性强、更容易分布式扩展，但功能相对局限

图形：设计灵活，构建简单，但不太好做分布式的集群方案

### CAP与ACID
#### CAP是什么
C（Consistency）强一致性 （所有节点访问同一份最新的数据副本）

A（Availability）可用性（在集群中一部分节点故障后，集群整体是否还能响应客户端的读写请求）

P（Partition tplerance）分区容错性（以实际效果而言，分区相当于对通信的时限要求。系统如果不能在时限内达成数据一致性，就意味着发生了分区的情况，必须就当前操作在C和A之间做出选择）

#### ACID是什么
A（Atomicity）原子性 （全部成功或全部失败，不允许结束在中间环节）

C（Consistency）一致性（事务开始与结束后，数据库完整性不可被破坏）

I（Isolation）独立性（并发的事务之间不会互相影响）

D（Durability）持久性（一旦事务提交后，修改将会永久的保存在数据库上）

#### 一个分布式系统不可能同时满足CAP三点
CA - 单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大。 传统Oracle数据库

CP - 满足一致性，分区容忍必的系统，通常性能不是特别高。Redis、Mongodb

AP - 满足可用性，分区容忍性的系统，通常可能对一致性要求低一些。大多数网站架构的选择

### BASE
BASE就是为了解决关系数据库强一致引起的问题而引起的可用性降低而提出的的解决方案。

基本可用（Basically Available）

软状态（Soft state）

最终一致（Eventually consistent）

思想就是通过让系统放松对某一时刻数据一致性的要求来换取系统整体伸缩性和性能上的改观


---

## Redis （REmote DIctionary Server） 远程服务字典
### 简单介绍
#### 是什么
开源免费，C语言编写，遵守BSD协议（开源协议），k/v分布式内存数据库，也被称为数据结构服务器。

#### 特点
- 支持数据持久化
- 不仅仅支持简单的K/v，还提供list/set/zset/hash等数据结构的存储
- redis支持数据备份，即master-slave模式的数据备份

#### 用于
- 内存存储与持久化
- 取最新N个数据操作
- 模拟类似于HttpSession需要设定过期时间的功能
- 发布、订阅消息系统
- 定时器、计数器

## 基本知识
- redis是单进程，用单进程模型来处理客户端的请求。对读写等事件的响应
是通过对epoll函数的包装来做到的。Redis的实际处理速度完全依靠主进程的执行效率。
- 默认16个数据库，类似数组下表从零开始，初始默认使用零号库
- Select命令切换数据库  例如 SELECT <dbid>
- Dbsize查看当前数据库的key的数量
- Flushdb：清空当前库
- Flushall；通杀全部库
- Redis索引都是从零开始
- 默认端口是6379 （6379 是 "MERZ " 九宫格输入法对应的数字，Alessia Merz 是一位意大利舞女、女演员）
- 统一密码管理，16个库都是同样密码，要么都OK要么一个也连接不上

## 五大数据类型
### String
string是redis最基本的类型，一个key对应一个value。string类型是二进制安全的。一个redis中字符串value最多可以是512M
### Hash
Redis hash 是一个键值对集合。hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。类似Java里面的Map<String,Object>
### list
Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素导列表的头部（左边）或者尾部（右边）。它的底层实际是个链表
### set
Redis的Set是string类型的无序集合。它是通过HashTable实现实现的.
### Zset
Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。zset的成员是唯一的,但分数(score)却可以重复。
## 常用命令
### Redis （key）
- keys * |查看当前库所有key
- exists key|判断某个key是否存在 1存在 0不存在
- move key db |将当前数据库的 key 移动到给定的数据库 db 当中。
- expire key |秒钟：为给定的key设置过期时间
- ttl key |查看还有多少秒过期，-1表示永不过期，-2表示已过期
- type key |查看你的key是什么类型

### String
- set/get/del key |存入/获取/删除值
- append/strlen key|附加字符/获取字符长度
- Incr/decr key |一定要是数字才能进行加减 +/- 1
- incrby/decrby key number| 条件同上 按设定的值（number）进行加减
- getrange/setrangev key start end| 从start到end获取/设置 key的值（包括start与end）
- setex(set with expire)key value键秒值/setnx key value|(set if not exist)
- mset/mget/msetnx [key value...]|批量设置/获取/去存在设置值（一错全错）
- getset key value|(先get再set)

### list
- lpush/rpush key values| （类似于栈）从左/右开始将元素压入栈
- lrange key start end |从start到end从key中取值
- lpop/rpop key |从key的左端（栈顶）/右端（栈底） 取值（取值后消失）
- lindex key number|按照索引（number）下标获得元素(从上到下)(不消失)
- lrem key count value| 删count个value（count>0 从表头开始|count<0从表尾开始|count=0移除所有）
- ltrim key start end |从start到end，截取指定范围的值后再赋值给key
- rpoplpush source destination|将列表source中的最后一个元素(尾元素)弹出，并返回给客户端。将 source 弹出的元素插入到列表 destination ，作为 destination 列表的的头元素。
- lset key index value|将列表 key 下标为 index 的元素的值设置为 value 
- linsert key  before/after value1 value2|在key中value前/后插入values

### set
- sadd key member [member …]|将一个或多个 member 元素加入到集合 key 当中，已经存在于集合的 member 元素将被忽略。
- sismember key member| 判断member是否为集合key中成员
- smembers key| 返回集合 key 中的所有成员。不存在的 key 被视为空集合。
- scard key|获取集合里面的元素个数
- srem key member [member …] |删除集合中(一个或多个)元素
- srandmember key count| 某个整数(随机出count个数)
- spop key count| 随机count个数出栈，出栈就消失
- smove key1 key2 在key1里某个值|作用是将key1里的某个值赋给key2（key1中值消失）
- sdiff/sinter/sunion key[key ...]|差集/交集/并集

### Hash
- hset/hget/hmset/hmget/hgetall|设值/取值/多重设值/多重取值/获得所有域和值
- hlen/hdel |返回key中域的数量/删除key中的一个或多个域，不存在的域将被忽略。
- hexists key field|检查给定域 field 是否存在于哈希表 hash 当中
- hkeys/hvals|返回hash表中所有的域/值
- hincrby/hincrbyfloat|为哈希表 key 中的域 field 加上增量/浮点数增量 increment 。
- hestnx|当且仅当域 field 尚未存在于哈希表的情况下， 将它的值设置为 value 

### Zset
- zadd key score member|将一个或多个 member 元素及其 score 值加入到有序集 key 当中
- zrange key start end|获取start到end的值
- zrangebyscore key  start end|获取start到end这个分数的值
- zrem key score|作用是删除score对应的元素
- zcard  key|返回有序集 key 的基数。
- count key min max|返回有序集 key 中， score 值在 min 和 max 之间(默认包括 score 值等于 min 或 max )的成员的数量。
- zrevrank key member|返回有序集 key 中成员 member 的排名。其中有序集成员按 score 值递减(从大到小)排序。
- zrevrange start stop [WITHSCORES]|逆序输出（与zrange输出相反）
- zrevrangebyscore  key start end|返回有序集key中，score值介于max和min之间(默认包括等于 max 或 min )的所有的成员。有序集成员按 score值递减(从大到小)的次序排列。

---

## redis.conf
- redis非大小写敏感
- 支持bytes不支持bit 1k = 1000bytes 1kb = 1024bytes 
- INCLUDES：文件配置可以通过includes包含，redis.conf作为总闸

### GENERAL
#### daemonize 
- **daemonize:yes**:
redis采用的是单进程多线程的模式。当redis.conf中选项daemonize设置成yes时，代表开启守护进程模式。在该模式下，redis会在后台运行，并将进程pid号写入至redis.conf选项pidfile设置的文件中，此时redis将一直运行，除非手动kill该进程。
- **daemonize:no**: 当daemonize选项设置成no时，当前界面将进入redis的命令行界面，exit强制退出或者关闭连接工具(putty,xshell等)都会导致redis进程退出。

---

- **pidfile**:
当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定
- **port**:
指定Redis监听端口，默认端口为6379
- **bind**:
绑定的主机地址
- **Tcp-backlog**:
设置tcp的backlog，backlog其实是一个连接队列，backlog队列总和=未完成三次握手队列 + 已经完成三次握手队列。在高并发环境下你需要一个高backlog值来避免慢客户端连接问题。
- **Timeout**:::::
空闲多长时间，redis连接关闭
- **Tcp-keepalive**::::
多少秒检测一次网络通信状态
- **Loglevel**:::
指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning
- **Logfile**
日志记录方式
- **Syslog-enabled**：
是否把日志输出到syslog中
- **Syslog-ident**:
指定syslog里的日志标志
- **Syslog-facility**:
指定syslog设备，值可以是USER或LOCAL0-LOCAL7
- **Databases**:
数据库数量

---
### SECURITY
config get requirepass 查看密码

config set requirepass "xxxxxx" 设置密码

auth xxxxxx 检测密码

---
### LIMITS
- **Maxclients**:设置redis同时可以与多少个客户端进行连接。默认情况下为10000个客户端
- **Maxmemory**:设置redis可以使用的内存量。一旦到达内存使用上限，redis将会试图移除内部数据，移除规则可以通过maxmemory-policy来指定
- **Maxmemory-policy**: 

（1）volatile-lru：使用LRU算法移除key，只对设置了过期时间的键

（2）allkeys-lru：使用LRU算法移除key

（3）volatile-random：在过期集合中移除随机的key，只对设置了过期时间的键

（4）allkeys-random：移除随机的key

（5）volatile-ttl：移除那些TTL值最小的key，即那些最近要过期的key

（6）noeviction：不进行移除。针对写操作，只是返回错误信息
- **Maxmemory-samples**:设置样本数量，LRU算法和最小TTL算法都并非是精确的算法，而是估算值，所以你可以设置样本的大小，redis默认会检查这么多个key并选择其中LRU的那个。

---
### SNAPSHOTTING（RDB）
- Save： 设置时间内操作多少次就生成.RDB文件保存数据
- Stop-writes-on-bgsave-error： 快照功能开启，后台持久化操作失败，Redis则会停止接受更新操作，否则没人注意到数据不会写磁盘持久化。
 如果后台持久化操作进程再次工作，Redis会自动允许更新操作（yes）；后台持久化操作出错,redis也仍然
可以继续像平常一样工作（no）
- rdbcompression： 对于存储到磁盘中的快照，可以设置是否进行压缩存储。
- rdbchecksum： 在存储快照后，是否让redis使用CRC64算法来进行数据校验
- dbfilename： 数据库文件的文件名
- dir： 数据目录
---

### APPEND ONLY MODE（AOF）
- appendonly： 是否打开AOF持久化，默认为NO
- appendfilename： 持久化文件名称 默认appendonly.aof
- Appendfsync： AOF模式{
    1.Always:同步持久化 每次发生数据变更会被立即记录到磁盘  性能较差但数据完整性比较好

    2.Everysec：出厂默认推荐，异步操作，每秒记录   如果一秒内宕机，有数据丢失
    
    3.n o： 不同步
    
}
- No-appendfsync-on-rewrite：重写时是否可以运用Appendfsync，用默认no即可，保证数据安全性。
- Auto-aof-rewrite-min-size：设置重写的基准值
- Auto-aof-rewrite-percentage：设置重写的基准值
---

## Redis 持久化
### RDB
#### 是什么？
在指定的时间间隔内将内存中的数据集快照写入磁盘。

Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到
一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。
整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能
如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方
式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。
#### 如何触发RDB快照
- 配置文件中默认的快照配置
- flushall，也会产生dump.rdb文件，但是是空文件
- save或bgsave save是只管保存，其它不管。bgsave则是异步快照，允许其它操作

#### 如何恢复
将备份文件 (dump.rdb) 移动到 redis 安装目录并启动服务即可

#### 优势与劣势
- 适合大规模的数据恢复
- 对数据完整性和一致性要求不高
- 在一定间隔时间做一次备份，所以如果redis意外down掉的话，就 - 会丢失最后一次快照后的所有修改
- Fork的时候，内存中的数据被克隆了一份，需要考虑性能

### AOF
#### 是什么？
以日志的形式来记录每个写操作，将Redis执行过的所有写指令记录下来(读操作不记录)，
只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis
重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作
#### AOF启动/修复/恢复
- 正常恢复
  启动： 修改默认的appendonly no，改为yes

  将有数据的aof文件复制一份保存到对应目录(config get dir)
  
  恢复：重启redis然后重新加载
- 异常恢复
  启动：修改默认的appendonly no，改为yes

  备份被写坏的AOF文件
  
  修复：Redis-check-aof --fix进行修复
  
  恢复：重启redis然后重新加载

---
#### Rewrite
##### 是什么
AOF采用文件追加方式，文件会越来越大为避免出现此种情况，新增了重写机制,
当AOF文件的大小超过所设定的阈值时，Redis就会启动AOF文件的内容压缩，
只保留可以恢复数据的最小指令集.可以使用命令bgrewriteaof
##### 重写原理
AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件最后再rename)。
##### 触发机制
Redis会记录上次重写时的AOF大小，默认配置是当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发
#### 优势与劣势
灵活的配置可以决定数据的同步方式。相同的数据集AOF远大于RDB，恢复速度慢于RDB。

---
## Redis事务
### 是什么
可以一次执行多个命令，本质是一组命令的集合。一个事务中的
所有命令都会序列化，按顺序地串行化执行而不会被其它命令插入，不许加塞。
### 能干嘛
一个队列中，一次性、顺序性、排他性的执行一系列命令
### 操作
#### 常用命令
- DISCARD 取消事务，放弃执行事务块里所有命令
- EXEC 执行所有事务块内的命令
- MULTI 标记一个事务块的开始
- UNWATCH 取消WATCH命令对所有key的监视
- WATCH key[key ...] 监视一个或多个key，如果事务执行之前key被改动，则事务被打断

#### 事务执行
事务执行中，如果存在语句错误，则全部事务不执行，若语句无错，数据错误，则只返回错误雨雾，其余指令不受影响。
#### WATCH监控
##### 乐观锁/悲观锁与CAS（Check And Set）
- **乐观锁**：每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。乐观锁适用于多读的应用类型，这样可以提高吞吐量，
 
乐观锁策略:提交版本必须大于记录当前版本才能执行更新
- **悲观锁**：每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会block直到它拿到锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁
- **CAS**： 命令用于执行一个"检查并设置"的操作
它仅在当前客户端最后一次取值后，该key 对应的值没有被其他客户端修改的情况下， 才能够将值写入。

##### exec
一旦执行exec，之前加的所有监控所都会被取消

##### 总结
通过WATCH命令在事务执行之前监控了多个Keys，倘若在WATCH之后有任何Key的值发生了变化，
EXEC命令执行的事务都将被放弃，同时返回Nullmulti-bulk应答以通知调用者事务执行失败

---

#### 事务执行的三阶段
- **开启**：以MULTI开始一个事务
- **入队**：将多个命令入队到事务中，接到这些命令并不会立即执行，而是放到等
执行的事务队列里面
- **执行**：由EXEC命令触发事务

#### 事务执行的三特性
 - **单独的隔离操作**：事务中的所有命令都会序列化、按顺序地执行。事务在执- 的过程中，不会被其他客户端发送来的命令请求所打断。
 - **没有隔离级别的概念**：队列中的命令没有提交之前都不会实际的被执行，因- 事务提交前任何指令都不会被实际执行，也就不存在”事务内的查询要看到事务里的更新，在事务外查询不能看到”这- 让人万分头痛的问题
 - **不保证原子性**：redis同一个事务中如果有一条命令执行失败，其后的命令仍会被执行，没有回滚

## redis的发布订阅
### 是什么？
进程间的一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。
### 命令
- PUBLISH : 将信息发送到指定频道
- SUBSCRIBE ：订阅给定的一个或多个频道信息
- PSUBSCRIBE ：订阅一个或多个符合给定模式的频道
- UNSUBSCRIBE：推定给定的频道
- PUNSUBSCRIBE：退订所有给定模式的频道
- PUBSUB：查看订阅与发布系统状态


## redis的主从复制
### 是什么？
也就是我们所说的主从复制，主机数据更新后根据配置和策略，
自动同步到备机的master/slaver机制，Master以写为主，Slave以读为主
### 做什么？
读写分离，容灾恢复
### 使用方法
#### （一主二从）
- 配从不配主
- 从库配置：slaveof 主库ip 主库端口（除非配置进config文件，否则每次断开都需要重配）
- 修改配置文件：
```
1.拷贝多个redis.conf文件
                
2.开启daemonize yes

3.Pid文件名字

4.指定端口

5.Log文件名字

6.Dump.rdb名字
```
- 使用slaveof 配置
- info replication 查看状态

#### 薪火相传
-上一个Slave可以是下一个slave的Master，Slave同样可以接收其他
slaves的连接和同步请求，那么该slave作为了链条中下一个的master,
可以有效减轻master的写压力
-Slaveof 新主库IP 新主库端口

#### 反客为主
-使当前数据库停止与其他数据库的同步，转成主数据库
-SLAVEOF no one
---
### 复制原理
- Slave启动成功连接到master后会发送一个sync命令
- Master接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令， - 在后台进程执行完毕之后，master将传送整个数据文件到slave,以完成一次完全同步
- 全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。
- 增量复制：Master继续将新的所有收集到的修改命令依次传给slave,完成同步
- 但是只要是重新连接master,一次完全同步（全量复制)将被自动执行

### 哨兵模式
#### 是什么？
反客为主的自动版，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库
#### 使用步骤
- 自定义的/myredis目录下新建sentinel.conf文件
- 配置哨兵,填写内容
```
sentinel monitor host6379(被监控主机名) 127.0.0.1 6379 1
一组sectinel可以监视多个主机
```
- 启动哨兵
```
edis-sentinel /redis/sentinel.conf 
```
- 原有的master挂了（原master复活后成为slave）
- 投票新选
- 重新主从继续开工,info replication查看信息

### 复制的缺点
复制延时