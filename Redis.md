## Redis

### Redis VS MemCached

相同点：都是基于内存的缓存数据库，都有过期策略，性能都很高

不同点：

- 数据结构：Redis支持更丰富的数据结构
- MemCached为多线程非阻塞I/O复用，Redis单线程阻塞I/O多路复用
- Redis支持事务、Lua脚本、订阅脚本
- Redis支持持久化，可以将数据写入磁盘；MemCached不支持。（内存满了？灾难恢复）
- Redis支持原生集群

### 为什么用Redis?

- 高性能
- 高并发

### Redis的5种数据类型？

- String:最基本的数据类型，可以存放字符串、整数、浮点数、图片路径等

  - SDS实现
  - 适用于token、Session
  - 分布式锁

  ```java
  需要存储常规数据的场景举例：缓存 Session、Token、图片地址、序列化后的对象(相比较于 Hash 存储更节省内存)。相关命令：SET、GET。
  需要计数的场景举例：用户单位时间的请求数（简单限流可以用到）、页面单位时间的访问数。相关命令：SET、GET、 INCR、DECR 。
  ```

- List:用于返回信息流(最新动态、最新新闻)
  - ziplist/quciklist
  - 可以通过**RPOP-LPUSH**和**RPUSH-LPOP**实现队列，通过**RPOP-RPUSH**实现栈
- hash:用于存储对象
  - 实现与HashMap类似，**Dict,Ziplist**
  - 可以方便的修改某一信息,存储用户信息、商品信息等
- set:用于存储不重复的数据
  - **Dict,Intset**
  - 提供了并差交等API，可用来实现**共同好友/共同关注/推荐音乐/推荐好友/共同点赞**等
  - 抽奖系统：随机获取数据
- zset：set中的每个元素都有score这一属性
  - **ZipList,SkipList**
  - 可用来实现排行榜：**微信运动/游戏段位**等

### Redis的3中延伸数据类型？

- BitMap:每个bit位存放1 or 0
  - 用于完成与"0/1"状态相关的业务，如商品上下架状态等
- HyperLogLog
  - 数据量巨大的计数场景
- GeoSpatial
  - 附近的人

### Redis 的持久化策略？

- RDB

  在某一时间点生成快照

- AOF（Append File Only)

  1. Append:在Redis每一次执行写操作后都会将此操作写到AOF缓冲区中

  2. Write:调用系统调用的write方法将AOF缓冲区中的内容写到AOF文件中，但此步并没有同步磁盘

  3. Fsync:将AOF文件同步到磁盘中，同步策略有3种

     ![image-20240302172806252](C:\Users\35114\AppData\Roaming\Typora\typora-user-images\image-20240302172806252.png)

  4. Rewrite:AOF越来越大，对文件进行重写

  5. load:重启后加载AOF文件

- RDB混AOF

  AOF中写的内容是RDB，兼顾了速度与安全；RDB更快，AOF更安全

### Redis的应用

- 分布式锁：依靠setnx来实现互斥，集群下使用Redission
- 限流:Redis+Lua脚本实现限流
- 消息队列：可以但不建议

### Redis缓存

- 缓存雪崩：大量key在同一时间段过期，造成访问大量打在数据库上，数据库承受不了这样的并发压力
  - key随机过期
  - key不过期，后台异步更新
  - 服务不可用：采用Redis集群/限流/多级缓存
- 缓存击穿：热点key失效后，大量访问打到数据库上
  - 互斥锁
  - 不过期
- 缓存穿透：所要查询的值不仅Redis中不存在，数据库中也不存在
  - 业务误操作（不小心把缓存和数据库删了、黑客恶意攻击）
  - 首先就是要参数格式校验
    - 缓存空数据
    - 布隆过滤器
    - 接口限流