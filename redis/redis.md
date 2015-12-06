#  *redis*
1. NoSql入门和概述
2. Redis入门介绍
3. Redis数据类型
4. 解析配置文件
5. Redis的持久化
6. Redis的事务
7. Redis的发布订阅
8. Redis的复制
9. Redis的java客户端Jedis




# *Redis数据类型*  
  Redis五大数据类型:
  1. String
  2. Hash
  3. List
  4. Set
  5. Zset  
![1.bmp](redis/1.bmp "")
## keys
>*  keys *   
*  exists key   
*  move key db --->当前库就没有了，被移除了   
*  expire key 秒钟：为给定的key设置过期时间   
*  ttl key 查看还有多少秒过期，-1表示永不过期，-2表示已过期   
*  type key 查看你的key是什么类型   
*  Redis字符串(String)
>

## String
>*  set/get/del/append/strlen   
*  Incr/decr/incrby/decrby,一定要是数字才能进行加减   
*  getrange/setrange   
*  setex(set with expire)/setnx(set if not exist)  
*  mset/mget/msetnx

## List  
>*  lpush/rpush/lrange     
*  lpop/rpop  
*  lindex，按照索引下标获得元素(从上到下)   
*  llen   
*  lrem key 删N个value   
    \*从left往right删除2个值等于v1的元素，返回的值为实际删除的数量  
    \*LREM list3 0值，表示删除全部给定的值。零个就是全部值  
*  ltrim key 开始index 结束index，截取指定范围的值   
*  ltrim：截取指定索引区间的元素，格式是ltrimlist的key 起始索引结束索引  
*  rpoplpush 源列表 目的列表 :移除列表的最后一个元素，并将该元素添加到另一个列表并返回  
*  lset key index value  
*  linsert key before/after 值1 值2     
*  性能总结  
1.它是一个字符串链表，left、right都可以插入添加；  
2.如果键不存在，创建新的链表；  
3.如果键已存在，覆盖；  
4.如果值全移除，对应的键也就消失了。  
5.链表的操作无论是头和尾效率都极高，但假如是对中间元素进行操作，效率就很惨淡了。*  

## Set
>*  sadd/smembers/sismember  
*  scard，获取集合里面的元素个数  
*  srem key value 删除集合中元素   
*  srandmember 数字   
1.从set集合里面随机取出2个（不会和上一次的重复），
2.如果超过最大数量就全部取出，  
3.如果写的值是负数，比如-3，表示需要取出3个，但是可能会有重复值.    
*  spop 随机出栈  
smove key1 key2 在key1里某个值  
数学集合类   
* 差集：sdiff .
在第一个set里面而不在后面任何一个set里面的项  
* 交集：sinter
* 并集：sunion  

## Hash  
>*  KV模式不变，但V是一个键值对   
*  hset/hget/hmset/hmget/hgetall/hdel  
*  hlen   
*  hexists key 在key里面的某个值的key   
*  hkeys/hvals   
*  hincrby/hincrbyfloat   
*  hsetnx   

## Zset(sorted set)  
>*  在set基础上，加一个score值。 之前set是k1 v1 v2 v3， 现在zset是k1 score1 v1 score2 v2  
*  zadd/zrange    
*  zrangebyscore key 开始score 结束score   
*  withscores
( 不包含
limit 0-区间值 返回限制 )  
*  zrem key score对应值的某个值。删除元素，格式是zremzset的key项的值，项的值可以是多个
zremkeyscore某个对应值，可以是多个值  
*  zcard/zcount key score区间/zrank/zscore key 分数对应值   
*  zcard：获取集合中元素个数  
*  zcount：获取分数区间内元素个数，zcountkey 开始分数区间结束分数区间  
*  zrank：获取value在zset中的下标位置  
*  zscore：按照值获得对应的分数
*  zrevrank :正序、逆序获得下标索引值
*  	zrevrange  
*  zrevrangebyscore 结束score 开始score   
zrevrangebyscore zset1 90 60 withscores  分数是反着来的  

# *解析配置文件 redis.conf *  
## 它在哪里
## 为什么我将它拷贝出来单独执行？
## Units单位  
>　　1 配置大小单位,开头定义了一些基本的度量单位，只支持bytes，不支持bit  
　　2 对大小写不敏感


## 	INCLUDES包含    
>和我们的Struts2配置文件类似，可以通过includes包含，redis.conf可以作为总闸，包含其他
1. GENERAL :通用
2. Daemonize
3. Pidfile
4. Port
5. Tcp-backlog :
tcp-backlog
设置tcp的backlog，backlog其实是一个连接队列，backlog队列总和=未完成三次握手队列+已经完成三次握手队列。
在高并发环境下你需要一个高backlog值来避免慢客户端连接问题。注意Linux内核会将这个值减小到/proc/sys/net/core/somaxconn的值，所以需要确认增大somaxconn和tcp_max_syn_backlog两个值
来达到想要的效果  
6. Timeout
7. Bind
8. Tcp-keepalive :
单位为秒，如果设置为0，则不会进行Keepalive检测，建议设置成60
9. Loglevel
10. Logfile
11. Syslog-enabled :
是否把日志输出到syslog中
12. Syslog-ident :
指定syslog里的日志标志
13. Syslog-facility :
指定syslog设备，值可以是USER或LOCAL0-LOCAL7
14. Databases
15. SNAPSHOTTING :快照
16. Save :
save 秒钟 写操作次数  

## 参数说明
>参数说明
redis.conf配置项说明如下：
1. Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程
  daemonizeno
2. 当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定
  pidfile/var/run/redis.pid
3. 指定Redis监听端口，默认端口为6379，作者在自己的一篇博文中解释了为什么选用6379作为默认端口，因为6379在手机按键上MERZ对应的号码，而MERZ取自意大利歌女AlessiaMerz的名字
  port6379
4. 绑定的主机地址
  bind127.0.0.1
5. 当客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能
  timeout300
6. 指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose
  loglevelverbose
7. 日志记录方式，默认为标准输出，如果配置Redis为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给/dev/null
  logfilestdout
8. 设置数据库的数量，默认数据库为0，可以使用SELECT<dbid>命令在连接上指定数据库id
  databases16
9. 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合
  save<seconds> <changes>
 Redis默认配置文件中提供了三个条件：
  save 9001
  save 30010
  save 6010000
 分别表示900秒（15分钟）内有1个更改，300秒（5分钟）内有10个更改以及60秒内有10000个更改。
10. 指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大
 rdbcompression yes
11. 指定本地数据库文件名，默认值为dump.rdb
  dbfilenamedump.rdb
12. 指定本地数据库存放目录
  dir./
13. 设置当本机为slav服务时，设置master服务的IP地址及端口，在Redis启动时，它会自动从master进行数据同步
  slaveof<masterip> <masterport>
14. 当master服务设置了密码保护时，slav服务连接master的密码
  masterauth<master-password>
15. 设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过AUTH<password>命令提供密码，默认关闭
  requirepassfoobared
16. 设置同一时间最大客户端连接数，默认无限制，Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，如果设置maxclients0，表示不作限制。当客户端连接数到达限制时，Redis会关闭新的连接并向客户端返回maxnumber of clientsreached错误信息
  maxclients128
17. 指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，当此方法处理后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis新的vm机制，会把Key存放内存，Value会存放在swap区
  maxmemory<bytes>
18. 指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为redis本身同步数据文件是按上面save条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为no
  appendonlyno
19. 指定更新日志文件名，默认为appendonly.aof
  appendfilename appendonly.aof
20. 指定更新日志条件，共有3个可选值：
 no：表示等操作系统进行数据缓存同步到磁盘（快）
 always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）
 everysec：表示每秒同步一次（折衷，默认值）
appendfsynceverysec
21. 指定是否启用虚拟内存机制，默认值为no，简单的介绍一下，VM机制将数据分页存放，由Redis将访问量较少的页即冷数据swap到磁盘上，访问多的页面由磁盘自动换出到内存中（在后面的文章我会仔细分析Redis的VM机制）
  vm-enabled no
22. 虚拟内存文件路径，默认值为/tmp/redis.swap，不可多个Redis实例共享
  vm-swap-file /tmp/redis.swap
23. 将所有大于vm-max-memory的数据存入虚拟内存,无论vm-max-memory设置多小,所有索引数据都是内存存储的(Redis的索引数据就是keys),也就是说,当vm-max-memory设置为0的时候,其实是所有value都存在于磁盘。默认值为0
  vm-max-memory 0
24. Redisswap文件分成了很多的page，一个对象可以保存在多个page上面，但一个page上不能被多个对象共享，vm-page-size是要根据存储的数据大小来设定的，作者建议如果存储很多小对象，page大小最好设置为32或者64bytes；如果存储很大大对象，则可以使用更大的page，如果不确定，就使用默认值
  vm-page-size 32
25. 设置swap文件中的page数量，由于页表（一种表示页面空闲或使用的bitmap）是在放在内存中的，，在磁盘上每8个pages将消耗1byte的内存。
  vm-pages 134217728
26. 设置访问swap文件的线程数,最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。默认值为4
  vm-max-threads 4
27. 设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启
 glueoutputbuf yes
28. 指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法
 hash-max-zipmap-entries 64
 hash-max-zipmap-value 512
29. 指定是否激活重置哈希，默认为开启（后面在介绍Redis的哈希算法时具体介绍）
 activerehashing yes
30. 指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件
  include/path/to/local.conf

## Redis的持久化
>
### RDB（Redis DataBase）
* 官网介绍
graphic
是什么：   
在指定的时间间隔内将内存中的数据集快照写入磁盘， 也就是行话讲的Snapshot快照，它恢复时是将快照文件直接读到内存里
Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到 一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。 整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能 如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方 式要比AOF方式更加的高效。RDB的缺点是最后一次持久化后的数据可能丢失。   
* Fork  
Fork的作用是复制一个与当前进程一样的进程。新进程的所有数据（变量、环境变量、程序计数器等） 数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程
Rdb 保存的是dump.rdb文件

>配置位置  
如何触发RDB快照   
配置文件中默认的快照配置  
冷拷贝后重新使用
* 可以cp dump.rdb dump_new.rdb
命令save或者是bgsave
* Save：save时只管保存，其它不管，全部阻塞
BGSAVE：Redis会在后台异步进行快照操作， 快照同时还可以响应客户端请求。可以通过lastsave 命令获取最后一次成功执行快照的时间
* 执行flushall命令，也会产生dump.rdb文件，但里面是空的，无意义
如何恢复
将备份文件 (dump.rdb) 移动到 redis 安装目录并启动服务即可
CONFIG GET dir获取目录
优势
适合大规模的数据恢复
对数据完整性和一致性要求不高
劣势
在一定间隔时间做一次备份，所以如果redis意外down掉的话，就 会丢失最后一次快照后的所有修改
Fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑
如何停止
动态所有停止RDB保存规则的方法：redis-cli config set save ""   

>### AOF（Append Only File）
*
以日志的形式来记录每个写操作，将Redis执行过的所有写指令记录下来(读操作不记录)， 只许追加文件但不可以改写文件，redis启动之初会读取该文件重新构建数据，换言之，redis 重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作
*  Aof保存的是appendonly.aof文件   
* 配置位置
*
AOF启动/修复/恢复   
正常恢复
启动：设置Yes   
修改默认的appendonly no，改为yes   
将有数据的aof文件复制一份保存到对应目录(config get dir)   
恢复：重启redis然后重新加载   
异常恢复   
启动：  设置Yes   
修改默认的appendonly no，改为yes   
备份被写坏的AOF文件   
修复：  
Redis-check-aof --fix进行修复
恢复：重启redis然后重新加载
Rewrite
* AOF采用文件追加方式，文件会越来越大为避免出现此种情况，新增了重写机制, 当AOF文件的大小超过所设定的阈值时，Redis就会启动AOF文件的内容压缩， 只保留可以恢复数据的最小指令集.可以使用命令bgrewriteaof
重写原理
AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件最后再rename)， 遍历新进程的内存中数据，每条记录有一条的Set语句。重写aof文件的操作，并没有读取旧的aof文件， 而是将整个内存中的数据库内容用命令的方式重写了一个新的aof文件，这点和快照有点类似
触发机制
Redis会记录上次重写时的AOF大小，默认配置是当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发
优势
每秒同步：appendfsync always 同步持久化 每次发生数据变更会被立即记录到磁盘 性能较差但数据完整性比较好
每修改同步：appendfsync everysec 异步操作，每秒记录 如果一秒内宕机，有数据丢失
不同步：appendfsync no 从不同步
劣势
相同数据集的数据而言aof文件要远大于rdb文件，恢复速度鳗鱼rdb
Aof运行效率要慢于rdb,每秒同步策略效率较好，不同步效率和rdb相同

>### 总结  
官网建议   
* RDB持久化方式能够在指定的时间间隔能对你的数据进行快照存储
AOF持久化方式记录每次对服务器写的操作,当服务器重启的时候会重新执行这些 命令来恢复原始的数据,AOF命令以redis协议追加保存每次写的操作到文件末尾. Redis还能对AOF文件进行后台重写,使得AOF文件的体积不至于过大
只做缓存：如果你只希望你的数据在服务器运行的时候存在,你也可以不使用任何持久化方式.
同时开启两种持久化方式
在这种情况下,当redis重启的时候会优先载入AOF文件来恢复原始的数据, 因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整.
RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件。那要不要只使用AOF呢？ 作者建议不要，因为RDB更适合用于备份数据库(AOF在不断变化不好备份)， 快速重启，而且不会有AOF可能潜在的bug，留着作为一个万一的手段。  


>性能建议   
* 因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留save900 1这条规则。
如果EnalbeAOF，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了。代价一是带来了持续的IO，二是AOFrewrite的最后将rewrite过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOFrewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上。默认超过原大小100%大小时重写可以改到适当的数值。
如果不Enable AOF，仅靠Master-Slave Replication实现高可用性也可以。能省掉一大笔IO也减少了rewrite时带来的系统波动。代价是如果Master/Slave同时倒掉，会丢失十几分钟的数据，启动脚本也要比较两个Master/Slave中的RDB文件，载入较新的那个。新浪微博就选用了这种架构


## Redis的事务   
>### 是什么  
* 可以一次执行多个命令，本质是一组命令的集合。一个事务中的 所有命令都会序列化，按顺序地串行化执行执行而不会被其它命令插入，不许加塞   

>### 能干嘛
* 一个队列中，一次性、顺序性、排他性的执行一系列命令

>### 事务命令  
* MULTI 标记一个事务块的开始
* EXEC  执行所有事物块内的命令
* DISCARD 取消事务，放弃执行事物块内的命令
* UNWATCH 取消watch命令对所有key的监视
* WATCH kye[key ...]  监视一个（多个）key
>### 小结
* Watch指令，类似乐观锁，事务提交时，如果Key的值已被别的客户端改变，比如某个list已被别的客户端push/pop过了，整个事务队列都不会被执行
>### 3阶段
* 开启：以MULTI开始一个事务
* 入队：将多个命令入队到事务中，接到这些命令并不会立即执行，而是放到等待执行的事务队列里面
* 执行：由EXEC命令触发事务  
>### 3特性
* 单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行。事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。
* 没有隔离级别的概念：队列中的命令没有提交之前都不会实际的被执行，因为事务提交前任何指令都不会被实际执行， 也就不存在”事务内的查询要看到事务里的更新，在事务外查询不能看到”这个让人万分头痛的问题
* 不保证原子性：redis同一个事务中如果有一条命令执行失败，其后的命令仍然会被执行，没有回滚

## Redis的发布订阅
>### 是什么
* 进程间的一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。
 >### 命令
 1. PSUBSCRIBE [pattern ...]订阅一个或多个符合指定模式的频道  
 * PUBSUB subcommand[argument[argument ...]]查看订阅与发布系统状态  
 * PUBLISH channel message 将信息发送到指定的频道  
 * PUNSUBSCRIBE [pattern [pattern...]] 退订所有给定模式的频道    
 * SUBSCRIBE channel[channel...] 订阅一个或多个给定频道的信息  
 * UNSUBSCRIBE[chanel[chanel ...]] 退订给定的频道  

## Redis的复制(Master/Slave)
>### 行话：也就是我们所说的主从复制，主机数据更新后根据配置和策略， 自动同步到备机的master/slaver机制，Master以写为主，Slave以读为主   
>### 能干嘛
* 读写分离
* 容灾恢复

>### 怎么玩
* 主库不配从库配 :  
从库配置：slaveof 主库IP 主库端口   
每次与master断开之后，都需要重新连接，除非你配置进redis.conf文件
修改配置文件细节操作   
　　１.　拷贝多个redis.conf文件   
　　2.　 开启daemonize yes   
　　3.Pid文件名字   指定端口   Log文件名字  Dump.rdb名字
* 常用3招   info replication查看状态  
一主二仆 ：一个Master两个Slave 。
主机正常配置，从机执行 slaveof 127.0.0.1 6379 。日志查看，

>* 薪火相传   
上一个Slave可以是下一个slave的Master，Slave同样可以接收其他 slaves的连接和同步请求，那么该slave作为了链条中下一个的master, 可以有效减轻master的写压力
中途变更转向:会清除之前的数据，重新建立拷贝最新的
Slaveof 新主库IP 新主库端口
* 反客为主  
SLAVEOF no one :
使当前数据库停止与其他数据库的同步，转成主数据库
* 复制原理   
Slave启动成功连接到master后会发送一个sync命令
Master接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令， 在后台进程执行完毕之后，master将传送整个数据文件到slave,以完成一次完全同步   
全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。   
增量复制：Master继续将新的所有收集到的修改命令依次传给slave,完成同步
但是只要是重新连接master,一次完全同步（全量复制)将被自动执行
* 哨兵模式(sentinel)
是什么   
反客为主的自动版，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库
怎么玩(使用步骤：)   
调整结构，6379带着80、81   
/usr/common目录下新建sentinel.conf文件，名字绝不能错   
填写内容   
sentinel monitor 被监控数据库名字(自己起名字) 127.0.0.1 6379 1
