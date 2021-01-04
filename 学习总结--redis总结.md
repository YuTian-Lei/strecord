## Redis

## Redis核心数据结构剖析

### zset

#### 底层设计

- 底层存储结构为skiplist
- 核心点为包括一个dict对象和skiplist对象
- dict保存key/value，key为元素，value为分值,保证set的元素唯一性
- skiplist保存的有序的元素列表，每个元素包括元素和分值

#### 常用命令

- zadd
- zrange

#### 应用场景

- 排行榜

### set

#### 底层设计

- intset
- hashtable

#### 常用命令

- sadd
- smembers 
- sdiff（差集）
- sinter（交集）
- sunion（并集）
- sismember(可以判断A是否是B的好友)
- scard(获取好友数量)
- srandmember(随机展示)

#### 应用场景

- 共同关注
- 共同喜好
- 二度好友
-  好友/关注/粉丝/感兴趣的人集合
- 随机展示
- 黑名单/白名单

### list

#### 底层设计

- quicklist
- ziplist

#### 常用命令

- rpush
- llen
- lpop/rpop
- lindex
- lrange
- ltrim

#### 应用场景

- 消息队列
- 排行榜(定时计算)
- 朋友圈的最新点赞列表、评论列表

### hash

#### 底层设计

- hashmap

#### 常用命令

- hset
- hgetall
- hlen
- hget
- hmset

#### 应用场景

- 购物车
- 存储对象

### string

#### 底层设计

- string

#### 常用命令

- set
- get
- exists

#### 应用场景

- 单值缓存
- 对象缓存
- 分布式锁

## Redis高并发分布式锁实战

```Java
//redis 2.8之前  本质是setknx与expire不是原子操作
public static  boolean acquire(Jedis jedis, String lockKey, int lockTimeout){
       log.info("分布式锁获取--开始");
       Long setnxResult = jedis.setnx(lockKey,String.valueOf(System.currentTimeMillis() + lockTimeout));
       if(setnxResult != null && setnxResult.intValue() == 1){
           log.info("加锁成功,threadName:{}",Thread.currentThread().getName());
           //设置失效时间
           jedis.expire(lockKey,lockTimeout/1000);
           return true;
       }else {
           //未获取锁,则继续判断,判断时间戳,看是否可以重置并获取到锁
           String lockValueStr = jedis.get(lockKey);
           if(lockValueStr == null || (lockValueStr != null && System.currentTimeMillis() > Long.valueOf(lockValueStr))){
               String getSetResult = jedis.getSet(lockKey,String.valueOf(System.currentTimeMillis() + lockTimeout));
               //乐观锁思想
               if(getSetResult == null || (getSetResult != null && StringUtils.equals(lockValueStr,getSetResult))){
                   log.info("加锁成功,threadName:{}",Thread.currentThread().getName());
                   //设置失效时间
                   jedis.expire(lockKey,lockTimeout/1000);
                   return true;
               }else{
                   log.info("加锁失败,threadName:{}",Thread.currentThread().getName());
                   return false;
               }
           }else {
               log.info("加锁失败,threadName:{}",Thread.currentThread().getName());
               return false;
           }
       }
    }
//redis 2.8之后
  private static final String LOCK_SUCCESS = "OK";
  private static final String SET_IF_NOT_EXIST = "NX";
  private static final String SET_WITH_EXPIRE_TIME = "PX";

  public  boolean acquire(Jedis jedis, String lockKey, int expireTime) {
        String result = jedis.set(lockKey, "lock", SET_IF_NOT_EXIST, SET_WITH_EXPIRE_TIME, expireTime);
        if (LOCK_SUCCESS.equals(result)) {
            return true;
        }
        return false;
    }

    /**
     * 获得锁
     */
    public boolean acquire(String lockKey, long expireTime) {
        Boolean success = redisTemplate.opsForValue().setIfAbsent(lockKey, "lock", expireTime, TimeUnit.MILLISECONDS);
        return success != null && success;
    }


//释放锁
 public static void release(Jedis jedis, String lockKey){
        jedis.del(lockKey);
 }

 public static boolean release(String lockId) {
        Boolean success = redisTemplate.delete(lockId);
        return success != null && success;
 }
```

## Redis缓存穿透、缓存击穿、缓存雪崩实战解析

### 缓存穿透（缓存没有,数据库也没有）

####  解决方案

- 缓存空对象
- 布隆过滤器（guava布隆过滤器、redis设计实现布隆过滤器）

### 缓存击穿（缓存失效后,缓存没有,数据库有）

#### 解决方案

- redis分布式锁,双重校验,减少打到数据库请求的次数

### 缓存雪崩(缓存服务异常、热点大量缓存过期)

#### 解决方案

- 避免缓存集中失效，不同的key设置不同的超时时间
- 构建redis集群
- redis分布式锁,双重校验(缓存雪崩)

## Redis布隆过滤器实现

### redis4.0版本之前

```Java
import com.google.common.hash.Funnels;
import com.google.common.hash.Hashing;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.Pipeline;

import javax.annotation.PostConstruct;
import javax.annotation.Resource;
import java.io.IOException;
import java.nio.charset.Charset;

public class BloomFilter {
    @Resource
    private JedisPool jedisPool;
    //预计插入量
    private long expectedInsertions = 1000;
    //可接受的错误率
    private double fpp = 0.001F;
    //布隆过滤器的键在Redis中的前缀 利用它可以统计过滤器对Redis的使用情况
    private String redisKeyPrefix = "bf:";
    private Jedis jedis;

    //利用该初始化方法从Redis连接池中获取一个Redis链接
    @PostConstruct
    public void init(){
        this.jedis = jedisPool.getResource();
    }

    public void setExpectedInsertions(long expectedInsertions) {
        this.expectedInsertions = expectedInsertions;
    }

    public void setFpp(double fpp) {
        this.fpp = fpp;
    }

    public void setRedisKeyPrefix(String redisKeyPrefix) {
        this.redisKeyPrefix = redisKeyPrefix;
    }

    //bit数组长度
    private long numBits = optimalNumOfBits(expectedInsertions, fpp);
    //hash函数数量
    private int numHashFunctions = optimalNumOfHashFunctions(expectedInsertions, numBits);

    //计算hash函数个数 方法来自guava
    private int optimalNumOfHashFunctions(long n, long m) {
        return Math.max(1, (int) Math.round((double) m / n * Math.log(2)));
    }

    //计算bit数组长度 方法来自guava
    private long optimalNumOfBits(long n, double p) {
        if (p == 0) {
            p = Double.MIN_VALUE;
        }
        return (long) (-n * Math.log(p) / (Math.log(2) * Math.log(2)));
    }

    /**
     * 判断keys是否存在于集合where中
     */
    public boolean isExist(String where, String key) {
        long[] indexs = getIndexs(key);
        boolean result;
        //这里使用了Redis管道来降低过滤器运行当中访问Redis次数 降低Redis并发量
        Pipeline pipeline = jedis.pipelined();
        try {
            for (long index : indexs) {
                pipeline.getbit(getRedisKey(where), index);
            }
            result = !pipeline.syncAndReturnAll().contains(false);
        } finally {
            try {
                pipeline.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if (!result) {
            put(where, key);
        }
        return result;
    }

    /**
     * 将key存入redis bitmap
     */
    private void put(String where, String key) {
        long[] indexs = getIndexs(key);
        //这里使用了Redis管道来降低过滤器运行当中访问Redis次数 降低Redis并发量
        Pipeline pipeline = jedis.pipelined();
        try {
            for (long index : indexs) {
                pipeline.setbit(getRedisKey(where), index, true);
            }
            pipeline.sync();
        } finally {
            try {
                pipeline.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 根据key获取bitmap下标 方法来自guava
     */
    private long[] getIndexs(String key) {
        long hash1 = hash(key);
        long hash2 = hash1 >>> 16;
        long[] result = new long[numHashFunctions];
        for (int i = 0; i < numHashFunctions; i++) {
            long combinedHash = hash1 + i * hash2;
            if (combinedHash < 0) {
                combinedHash = ~combinedHash;
            }
            result[i] = combinedHash % numBits;
        }
        return result;
    }

    /**
     * 获取一个hash值 方法来自guava
     */
    private long hash(String key) {
        Charset charset = Charset.forName("UTF-8");
        return Hashing.murmur3_128().hashObject(key, Funnels.stringFunnel(charset)).asLong();
    }

    private String getRedisKey(String where) {
        return redisKeyPrefix + where;
    }
}
```

### redis4.0版本后

```java
//redis官方布隆过滤器插件
public class RedisBloomDemo {
    public static void main(String[] args) {
        String userIdBloomKey = "userid";
        // 创建客户端，jedis实例
        Client client = new Client("localhost", 6378);
        // 创建一个有初始值和出错率的过滤器
        client.createFilter(userIdBloomKey,100000,0.01);
        // 新增一个<key,value>
        boolean userid1 = client.add(userIdBloomKey,"101310222");
        System.out.println("userid1 add " + userid1);

        // 批量新增values
        boolean[] booleans = client.addMulti(userIdBloomKey, "101310111", "101310222", "101310222");
        System.out.println("add multi result " + booleans);

        // 某个value是否存在
        boolean exists = client.exists(userIdBloomKey, "101310111");
        System.out.println("101310111 是否存在" + exists);

        //某批value是否存在
        boolean existsBoolean[] = client.existsMulti(userIdBloomKey, "101310111","101310222", "101310222","11111111");
        System.out.println("某批value是否存在 " + existsBoolean);
    }
}
```

## Redis实现消息队列

- 生产消费模式
- 发布/订阅模式

## redis过期策略有哪些

- 惰性删除(在进行get或setnx等操作时，先检查key是否过期)
- 定期删除

## redis淘汰机制有哪些

- volatile-lru：从已设置过期时间的数据集中挑选最近最少使用的数据淘汰。
- volatile-ttl：从已设置过期时间的数据集中挑选将要过期的数据淘汰。
- volatile-random：从已设置过期时间的数据集中任意选择数据淘汰。
- volatile-lfu：从已设置过期时间的数据集挑选使用频率最低的数据淘汰。
- allkeys-lru：从数据集中挑选最近最少使用的数据淘汰
- allkeys-lfu：从数据集中挑选使用频率最低的数据淘汰。
- allkeys-random：从数据集（server.db[i].dict）中任意选择数据淘汰
- no-enviction（驱逐）：禁止驱逐数据，这也是默认策略。意思是当内存不足以容纳新入数据时，新写入操作就会报错，请求可以继续进行，线上任务也不能持续进行，采用no-enviction策略可以保证数据不被丢失。

## 分布式寻址都有哪些算法

- hash算法（大量缓存重建）
- 一致性hash算法（自动缓存迁移）+虚拟节点（自动负载均衡）
- redis cluster的hash slot算法

## 一致性hash算法

- 首先求出memcached服务器（节点）的哈希值，并将其配置到0～232的圆（continuum）上。
- 然后采用同样的方法求出存储数据的键的哈希值，并映射到相同的圆上。
- 然后从数据映射到的位置开始顺时针查找，将数据保存到找到的第一个服务器上。如果超过2的32次方仍然找不到服务器，就会保存到第一台memcached服务器上。

## 分布式系统中的数据一致性模型

- 异步更新缓存(基于订阅binlog的同步机制)

- 延时双删策略

## 手写LRU

```java
package 作业;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Scanner;
import java.util.Set;


/*LRU是Least Recently Used 近期最少使用算法。
 *通过HashLiekedMap实现LRU的算法的关键是，如果map里面的元素个数大于了缓存最大容量，则删除链表头元素
 */

/*public LinkedHashMap(int initialCapacity,float loadFactor,boolean accessOrder)
 *LRU参数参数：
 *initialCapacity - 初始容量。
 *loadFactor - 加载因子（需要是按该因子扩充容量）。
 *accessOrder - 排序模式( true) - 对于访问顺序（get一个元素后，这个元素被加到最后，使用了LRU  最近最少被使用的调度算法），对于插入顺序，则为 false,可以不断加入元素。
 */

 /*相关思路介绍：
  * 当有一个新的元素加入到链表里面时，程序会调用LinkedHahMap类中Entry的addEntry方法，
  *而该方法又会 会调用removeEldestEntry方法，这里就是实现LRU元素过期机制的地方，
  * 默认的情况下removeEldestEntry方法只返回false，表示可以一直表链表里面增加元素，在这个里  *修改一下就好了。 
  *
  */
 
/*
测试数据：
5
11
4 7 0 7 1 0 1 2 1 2 6
*/

import java.util.*;
public class LRULinkedHashMap<K,V> extends LinkedHashMap<K,V>{     
    private int capacity;                     //初始内存容量
    
    LRULinkedHashMap(int capacity){          //构造方法，传入一个参数
        super(16,0.75f,true);               //调用LinkedHashMap，传入参数    
        this.capacity=capacity;             //传递指定的最大内存容量
    }
    @Override
    public boolean removeEldestEntry(Map.Entry<K, V> eldest){     
        //每加入一个元素，就判断是size是否超过了已定的容量
        System.out.println("此时的size大小="+size());
        if((size()>capacity))
        {
            System.out.println("超出已定的内存容量，把链表顶端元素移除："+eldest.getValue());
        }
        return size()>capacity;        
    }
    
    public static void main(String[] args) throws Exception{
        Scanner cin = new Scanner(System.in);
        
        System.out.println("请输入总共内存页面数： ");
        int n = cin.nextInt();
        Map<Integer,Integer> map=new LRULinkedHashMap<Integer, Integer>(n);
        
        System.out.println("请输入按顺序输入要访问内存的总共页面数： ");
        int y = cin.nextInt();
        
        System.out.println("请输入按顺序输入访问内存的页面序列： ");
        for(int i=1;i<=y;i++)
        {
            int x = cin.nextInt();
            map.put(x,  x);  
        }
        System.out.println("此时内存中包含的页面数是有:");
        //用for-each语句，遍历此时内存中的页面并输出
        for(java.util.Map.Entry<Integer, Integer> entry: map.entrySet()){
            System.out.println(entry.getValue());
        }
    }
}
```

## Redis持久机制

- RDB机制
- AOF机制

## 实战

## Redis集群

## Redis在微博、微信及电商场景典型应用实践

## Redis缓存设计与性能优化

## Redis+LUA实现秒杀与抢红包实例



