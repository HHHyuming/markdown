#### redis 分布式锁实现

使用 setnx 进行加锁的操作，当key存在时说明该锁未被释放。为了避免设置了锁但是因为程序的某种原因，可能会导致这个key没有删除，从而这个锁没有被释放。我们可以使用expire对这个key进行过期处理。当然这个时候还是有问题的，假如expire某种原因下未执行，setnx之后宕机了。我们就需要setnx和expire最好同时进行，就有了新的 set a b ex 20  设置key的同时设置expire

#### redis 单线程的体现

单线程的优点：避免了上下文的切换，这之间浪费的时间，主要体现在处理请求上达到了 10w+的QPS

#### redis 实现延时队列

体现：在用户提交订单后30分钟未支付，则取消订单

实现：目的：取出任务队列中的任务，判断该任务是否过期(即达到延时时间)，然后去执行这个任务

个人认为要不这个当个服务起来吧

```python
from threading import Condition
# 使用condition来控制延时的wait时间，以及解决锁的问题
# 伪代码，多线程调用

1.首先用redis作用延时队列
key的特点如订单，那么就是 key = order:user
添加延时任务到队列 zadd(key, delay_time, value_user_id)
取的时候 zreverange(key, 0, time.time() ,withscore=True)
这个时候返回的是在当前时间范围内的任务也就是可能有多个任务

如果zreverange没有返回data 按CSDN参考博客sleep几秒，可行但是感觉太low了，其实这里的任务和定是任务的区别不大，定时任务经常是会需要使用的，我们可以搞一个定时任务的系统，根据实际情况，和项目的环境依赖等问题，看看能否独立，如果可以独立可以专门搞个服务，这里采用 API的形式调用服务(因为这里的定时任务可能需要依赖于加载运行时的上下文)





```



#### redis  持久化，主从数据交互

持久化有两种：

RDB：全量备份 通过redis.conf 配置save参数会将内存中的数据刷到文件中，保存了某个时间点的数据集，，

RDB：有点，rdb保存了某个时间点的数据集，rdb方式在保存RDB文件时，父进程将FORK一个子进程，事情将有子进程做，父进程没有其他IO操作，在回复数据时RDB更快

RDB：缺点会丢失数据

AOF：增量备份 通过配置文件中的 appendonly yes 开启， appendonly fsync 设置每隔几秒进行一次增量备份，记录服务器每次写的操作

AOF：优点数据容易回复，最多丢失1s数据

AOF：缺点AOF文件体积通常比RDB大，AOF速度慢与RDB

这两种持久化的方式是可以并存的，当两者并存的时候AOF会优先载入，因为AOF保存的数据集
要比RDB的要完整

#### redis数据备份

可以在服务器上创建一个 crontab 任务，根据业务以及实际场景取设定该任务复制RDB的时间



#### redis pipline好处

好处就是可以减少网络IO这种往返浪费不必要的时间造成的这种损失，可一次性执行一批redis指令，而不用一个个给到redis

#### redis 集群高可用保证，集群原理

采用哈希槽（这应该是核心中的核心），为每个key进行CRC16位编码算出该 key应落在哪个哈希槽上，取的时候也CRC一下看看根据cluster的 哈希槽位的节点取值就 OK了，这种结构很方便扩容和缩容，用户主从复制和sentinel的功能，  一个集群的构建核心竞争力之一就是其扩容和缩容能力，的简单与否

#### redis 主从复制原理

两个主要的东西：psync 和偏移量offset
第一次复制时，slave会发送一个psync的命令给master进行一个全量复制，master和slave个自会维护一个offset偏移量，来保证以后的每次数据复制的完整性



#### redis 雪崩，以及缓存穿透，缓存击穿，以及对应的解决办法

缓存雪崩：是指在某一时刻，一批key在同一时间都过期了，所有请求都绕过了redis，打到了DB上面，导致DB也扛不住这么大的压力。

> 解决办法，给这些key的过期时间设置成随机的值，避免造成某一时刻批量的key过期出现的问题

缓存穿透：指的是用户会去访问一些不存在的key，这个key在后面的DB中实际也是不存在的，导致这种请求每次都精准的绕过redis打到了DB上面，而DB会进行这种无用的查询，消耗性能，

> 解决办法：我们可以使用布隆过滤器取有效的解决这个问题，布隆过滤器可以判断出这个key是否存在，如果不存在，就直接return 2.先去DB查，没有这个key，就直接将这个key设置成null就成了，

缓存击穿：指的是有一个key一直被访问，一个很热的key，当这个key过期的时候，这时就会有大并发的请求打到DB上

> 解决办法：这种热点的key就可以设置永久不过期了



#### 内存淘汰机制，LRU的实现（如果没有清楚key将会调用淘汰机制，回收最少使用的key）

惰性删除：就是当用户访问的时候才会去看判断这个key是否有过期，不必浪费redis的额外开销

定期删除：redis会定期的随机挑选一批key进行过期校验，

LRU:

```python
from collcetions import OrderedDict

class LRU:
	def __init__(self, capacity):
        self.capacity = capacity
        self.cache = OrderedDict()
     
    def get(self, key):
        if key in self.cache:
            value = self.cache[key]
            self.cache.move_to_end(key)
            return value
       
    def set(self, key, value):
        if key in self.cache:
            self.cache[key] = value
            self.cache.move_to_end(key)
            return value
        else:
            self.cache[key] = value
       	if len(self.cache) > self.capacity:
            self.cache.popitem(last=False)
        
```



```
不管是本地缓存还是分布式缓存，为了保证较高性能，都是使用内存来保存数据，由于成本和内存限制，当存储的数据超过缓存容量时，需要对缓存的数据进行剔除。
一般的剔除策略有 FIFO 淘汰最早数据、LRU 剔除最近最少使用、和 LFU 剔除最近使用频率最低的数据几种策略。

noeviction:返回错误当内存限制达到并且客户端尝试执行会让更多内存被使用的命令（大部分的写入指令，但DEL和几个例外）
allkeys-lru: 尝试回收最少使用的键（LRU），使得新添加的数据有空间存放。
volatile-lru: 尝试回收最少使用的键（LRU），但仅限于在过期集合的键,使得新添加的数据有空间存放。
allkeys-random: 回收随机的键使得新添加的数据有空间存放。
volatile-random: 回收随机的键使得新添加的数据有空间存放，但仅限于在过期集合的键。
volatile-ttl: 回收在过期集合的键，并且优先回收存活时间（TTL）较短的键,使得新添加的数据有空间存放。
如果没有键满足回收的前提条件的话，策略volatile-lru, volatile-random以及volatile-ttl就和noeviction 差不多了。

作者：敖丙
链接：https://juejin.im/post/5dcaebea518825571f5c4ab0
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```











#### 一致性哈希校验（这个是分布式相关，现阶段了解即可）

```
参考博客：https://www.cnblogs.com/study-everyday/p/8629100.html

重要概念---》集群共享数据集，核心问题是数据的落地以及数据的一致性
# 哈希算法
hash(a.png) % 4 = 2
这里以redis replication为例探讨一下哈希分布式的缺点，
情景复现：
	1.假如我们有4台redis示例，对应4个slave，这里slave我们就忽略不管了，用上面的公式，我们要计算图片所以这个图片的哈希结果只会在 1,2,3,4 里面选，这个数据对应落在1,2,3,4 实例上的redis上
	2.此刻假设redis cluster 需要增加1个节点，那么取模就要相应改成5，不然第5台redis实例落不到数据，而且加入1,2,3,4中有一台宕机了，那么那台的数据将会是不可用的，然后你懂的，这回造成缓存雪崩，DB很危险为了解决 （1,2）的问题 一致性哈希校验的算法诞生了
	3.出现的两个问题 1---》缓存雪崩，2.数据无法落地 3.扩容问题
	
# 一致性哈希
hash（服务器A的IP地址） %  2^32  其中2^32 为哈希圆环
将以上的主机映射到哈希圆环上，对每个key进行hash算出hash之，然后在圆环上顺时针走，遇到的第一个节点，就把数据射进去就行了，
当圆环上的一个节点宕机了，受影响的数据是该节点的逆时间方向的前一个节点之间的数据，这时候加入进来的数据就会到该几点顺时针后的一个节点里面射，至于扩容就很好理解了，对节点hash然后映射到哈希圆环上去就行了，数据自己会射过去

2.到这里我感觉只解决了第二个数据无法落地的情况，以及第三个扩容问题，第一个问题缓存雪崩(如果这个集群的数据是共享的，就不会出现这种情况，比如redis是共享数据集的)，在这里我暂时也当做是共享数据集的把，以上3个问题全部解决
	

```

#### 秒杀系统

```
1.redis 集群，url 动态化，NGINX负载均衡
2. 限流 熔断 ，降级，隔离，库存预热，削峰填谷(消息队列，消费)

```

