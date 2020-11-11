# Redis

## 定义

> Redis（Remote Dictionary Server )，即远程字典服务，是一个开源的使用ANSI [C语言]编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。

## Redis数据结构

> Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)

### String（字符串）

- string 是 redis 最基本的类型，你可以理解成与 Memcached 一模一样的类型，一个 key 对应一个 value
- string 类型是二进制安全的。意思是 redis 的 string 可以包含任何数据。比如jpg图片或者序列化的对象
- string 类型是 Redis 最基本的数据类型，string 类型的值最大能存储 512MB

```
redis 127.0.0.1:6379> SET runoob "菜鸟教程"
OK
redis 127.0.0.1:6379> GET runoob
"菜鸟教程"
```

### Hash（哈希）

- Redis hash 是一个键值(key=>value)对集合
- Redis hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象

```
redis 127.0.0.1:6379> DEL runoob
redis 127.0.0.1:6379> HMSET runoob field1 "Hello" field2 "World"
"OK"
redis 127.0.0.1:6379> HGET runoob field1
"Hello"
redis 127.0.0.1:6379> HGET runoob field2
"World"
```

### List（列表）

- Redis 列表是简单的字符串列表，按插入顺序排序。你可以添加元素到列表的头部（左边）或尾部（右边）

```
redis 127.0.0.1:6379> DEL runoob
redis 127.0.0.1:6379> lpush runoob redis
(integer) 1
redis 127.0.0.1:6379> lpush runoob mongodb
(integer) 2
redis 127.0.0.1:6379> lpush runoob rabitmq
(integer) 3
redis 127.0.0.1:6379> lrange runoob 0 10
1) "rabitmq"
2) "mongodb"
3) "redis"
redis 127.0.0.1:6379>
```

### Set（集合）

- Redis 的 Set 是 string 类型的无序集合
- 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)

```
redis 127.0.0.1:6379> DEL runoob
redis 127.0.0.1:6379> sadd runoob redis
(integer) 1
redis 127.0.0.1:6379> sadd runoob mongodb
(integer) 1
redis 127.0.0.1:6379> sadd runoob rabitmq
(integer) 1
redis 127.0.0.1:6379> sadd runoob rabitmq
(integer) 0
redis 127.0.0.1:6379> smembers runoob

1) "redis"
2) "rabitmq"
3) "mongodb"
```

### zset(sorted set：有序集合)

- Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员
- zset每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序
- zset的成员是唯一的,但分数(score)却可以重复

```
redis 127.0.0.1:6379> DEL runoob
redis 127.0.0.1:6379> zadd runoob 0 redis
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 mongodb
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 rabitmq
(integer) 1
redis 127.0.0.1:6379> zadd runoob 0 rabitmq
(integer) 0
redis 127.0.0.1:6379> > ZRANGEBYSCORE runoob 0 1000
1) "mongodb"
2) "rabitmq"
3) "redis"
```

## Redis常用命令

- [Redis命令](https://www.runoob.com/redis/redis-commands.html)

- [Redis keys 命令](https://www.runoob.com/redis/redis-keys.html)
- [Redis 字符串命令](https://www.runoob.com/redis/redis-strings.html)
- [Redis hash 命令](https://www.runoob.com/redis/redis-hashes.html)
- [Redis 列表(List)命令](https://www.runoob.com/redis/redis-lists.html)
- [Redis 集合(Set)命令](https://www.runoob.com/redis/redis-sets.html)

- [Redis 有序集合命令](https://www.runoob.com/redis/redis-sorted-sets.html)
- [Redis 事务命令](https://www.runoob.com/redis/redis-transactions.html)

## Redis 发布订阅 

>Redis 发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。Redis 客户端可以订阅任意数量的频道。

```
//订阅
redis 127.0.0.1:6379> SUBSCRIBE redisChat

Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "redisChat"
3) (integer) 1

//发布
redis 127.0.0.1:6379> PUBLISH redisChat "Redis is a great caching technique"

(integer) 1

redis 127.0.0.1:6379> PUBLISH redisChat "Learn redis by runoob.com"

(integer) 1

# 订阅者的客户端会显示如下消息
1) "message"
2) "redisChat"
3) "Redis is a great caching technique"
1) "message"
2) "redisChat"
3) "Learn redis by runoob.com"
```

## Redis事务

>Redis 事务可以一次执行多个命令，单个 Redis 命令的执行是原子性的，但 Redis 没有在事务上增加任何维持原子性的机制，所以 Redis 事务的执行并不是原子性的。 并且带有以下三个重要的保证：
>
>- 批量操作在发送 EXEC 命令前被放入队列缓存
>- 收到 EXEC 命令后进入事务执行，事务中任意命令执行失败，其余的命令依然被执行
>- 在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中
>
>一个事务从开始到执行会经历以下三个阶段：
>
>- 开始事务。
>- 命令入队
>- 执行事务

```
redis 127.0.0.1:6379> MULTI
OK

redis 127.0.0.1:6379> SET book-name "Mastering C++ in 21 days"
QUEUED

redis 127.0.0.1:6379> GET book-name
QUEUED

redis 127.0.0.1:6379> SADD tag "C++" "Programming" "Mastering Series"
QUEUED

redis 127.0.0.1:6379> SMEMBERS tag
QUEUED

redis 127.0.0.1:6379> EXEC
1) OK
2) "Mastering C++ in 21 days"
3) (integer) 3
4) 1) "Mastering Series"
   2) "C++"
   3) "Programming"
```

## Redis持久化

> redis是一个内存数据库，数据保存在内存中，但是我们都知道内存的数据变化是很快的，也容易发生丢失。幸好Redis还为我们提供了持久化的机制，分别是RDB(Redis DataBase)和AOF(Append Only File)。

### **RDB机制**

> RDB其实就是把数据以快照的形式保存在磁盘上;RDB持久化是指在指定的时间间隔内将内存中的数据集快照写入磁盘。**也是默认的持久化方式**，这种方式是就是将内存中数据以快照的方式写入到二进制文件中,默认的文件名为dump.rdb。
>
> 三种触发机制：
>
> - save：执行该命令时，会阻塞当前Redis服务器，Redis不能处理其他命令，直到RDB**过程完成为止**
> - bgsave：执行该命令时，Redis会在后台异步进行快照操作，快照同时还可以响应客户端请求。
> - **自动触发**：自动触发是由我们的配置文件来完成的。在redis.conf配置文件中

####  优点

- **RDB 非常适用于灾难恢复**
- 性能最大化
- 相比于AOF机制，如果数据集很大，RDB的启动效率会更高

#### 缺点

- **如果你需要尽量避免在服务器故障时丢失数据**，**那么 RDB 不适合你**

### **AOF机制**

> 全量备份总是耗时的，有时候我们提供一种更加高效的方式AOF，工作机制很简单，redis会将每一个收到的写命令都通过write函数追加到文件中。通俗的理解就是日志记录。
>
> - 每修改同步always：同步持久化 每次发生数据变更会被立即记录到磁盘 性能较差但数据完整性比较好
>
> - 每秒同步everysec：异步操作，每秒记录 如果一秒内宕机，有数据丢失
> - 不同no：从不同步

#### 优点

- 该机制可以带来更高的数据安全性，即数据持久性

#### 缺点

- 对于相同的数据集来说，AOF 文件的体积通常要大于 RDB 文件的体积
- 根据同步策略的不同，AOF在运行效率上往往会慢于RDB

## Redis命名空间分组

> 那么如何以命名空间分组呢？其实很简单，只用在存储数据时，键值对中的键命名以冒号分开即可；
>
> 例如：
>
> - vehicle:car1
> - vehicle:car2

## Jedis使用

### 单机模式

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {"classpath:spring-service.xml"})
public class JedisTest {
    private static Jedis jedis = new Jedis("182.92.9.232", 6379);
    static {
        jedis.auth("lpf@vivo321");
    }

    @Autowired
    private JedisUtil jedisUtil;

    @Test
    public void JedisUtilTest(){
        jedisUtil.setVByList("testList","lpf1");
        jedisUtil.setVByList("testList","lpf2");
        jedisUtil.setVByList("testList","lpf3");
        jedisUtil.setVByList("testList","lpf4");
        List<String> list = jedisUtil.getVByList("testList",0,3,String.class);
        list.forEach(str->{
            System.out.println(str);
        });
    }


    @Test
    public void keyTest()  {
        System.out.println(jedis.flushDB());// 清空数据
        System.out.println(jedis.echo("hello"));

        // 判断key否存在
        System.out.println(jedis.exists("foo"));

        jedis.set("key", "values");
        jedis.set("key1", "values");
        jedis.set("key2", "values");
        System.out.println(jedis.exists("key"));// 判断是否存在

        // 如果数据库没有任何key，返回nil，否则返回数据库中一个随机的key。
        String randomKey = jedis.randomKey();
        System.out.println("randomKey: " + randomKey);

        // 设置60秒后该key过期
        jedis.expire("key", 60);

        // key有效毫秒数
        System.out.println(jedis.pttl("key"));

        // 移除key的过期时间
        jedis.persist("key");

        // 获取key的类型, "string", "list", "set". "none" none表示key不存在
        System.out.println("type: " + jedis.type("key"));

        // 导出key的值
        byte[] bytes = jedis.dump("key");
        System.out.println(new String(bytes));

        // 将key重命名
        jedis.renamenx("key", "keytest");
        System.out.println("key是否存在: " + jedis.exists("key"));// 判断是否存在
        System.out.println("keytest是否存在: " + jedis.exists("keytest"));// 判断是否存在

        // 查询匹配的key
        // KEYS       * 匹配数据库中所有 key 。
        // KEYS       h?llo 匹配 hello ， hallo 和 hxllo 等。
        // KEYS       h*llo 匹配 hllo 和 heeeeello 等。
        // KEYS       h[ae]llo 匹配 hello 和 hallo ，但不匹配 hillo 。
        // 特殊符号用 \ 隔开。
        Set<String> set = jedis.keys("*");

        Iterator<String> keys = set.iterator();
        while (keys.hasNext()){
            System.out.println(keys.next());
        }

        // 删除key
        jedis.del("key");
        System.out.println(jedis.exists("key"));
    }

    @Test
    public void stringTest() {

        jedis.set("hello", "hello");
        System.out.println(jedis.get("hello"));

        // 使用append 向字符串后面添加
        jedis.append("hello", " world");
        System.out.println(jedis.get("hello"));

        // set覆盖字符串
        jedis.set("hello", "123");
        System.out.println(jedis.get("hello"));

        // 设置过期时间
        jedis.setex("hello2", 2, "world2");
        System.out.println(jedis.get("hello2"));
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
        }
        System.out.println(jedis.get("hello2"));

        // 一次添加多个key-value对
        jedis.mset("a", "1", "b", "2");
        // 获取a和b的value
        List<String> valus = jedis.mget("a", "b");
        System.out.println(valus);

        // 批量删除
        jedis.del("a", "b");
        System.out.println(jedis.exists("a"));
        System.out.println(jedis.exists("b"));
    }

    @Test
    public void listTest() {
        String key = "namespace:mylist";
        jedis.del(key);

        // 队列添加元素
        jedis.rpush(key, "aaaa");
        jedis.rpush(key, "aaaa");
        jedis.rpush(key, "bbbb");
        jedis.rpush(key, "cccc");
        jedis.rpush(key, "cccc");

        // 队列长度
        System.out.println("lenth: " + jedis.llen(key));

        // 打印队列，从索引0开始，到倒数第1个（全部元素）
        System.out.println("all elements: " + jedis.lrange(key, 0, -1));

        // 索引为1的元素
        System.out.println("index of 1: " + jedis.lindex(key, 1));

        // 设置队列里面一个元素的值，当index超出范围时会返回一个error。
        jedis.lset(key, 1, "aa22");
        System.out.println("index of 1: " + jedis.lindex(key, 1));

        // 从队列的右边入队一个元素
        jedis.rpush(key, "-2", "-1");// 先-2，后-1入队列
        System.out.println("all elements: " + jedis.lrange(key, 0, -1));

        // 从队列的左边入队一个或多个元素
        jedis.lpush(key, "second element", "first element");// 先second
        // element，后first
        // elementF入队列
        System.out.println("all elements: " + jedis.lrange(key, 0, -1));

        // 从队列的右边出队一个元素
        System.out.println(jedis.rpop(key));
        // 从队列的左边出队一个元素
        System.out.println(jedis.lpop(key));
        System.out.println("all elements: " + jedis.lrange(key, 0, -1));

        // count > 0: 从头往尾移除值为 value 的元素，count为移除的个数。
        // count < 0: 从尾往头移除值为 value 的元素，count为移除的个数。
        // count = 0: 移除所有值为 value 的元素。
        jedis.lrem(key, 1, "cccc");
        System.out.println("all elements: " + jedis.lrange(key, 0, -1));

        // 即最右边的那个元素也会被包含在内。 如果start比list的尾部下标大的时候，会返回一个空列表。
        // 如果stop比list的实际尾部大的时候，Redis会当它是最后一个元素的下标。
        System.out.println(jedis.lrange(key, 0, 2));
        System.out.println("all elements: " + jedis.lrange(key, 0, -1));

        // 删除区间以外的元素
        System.out.println(jedis.ltrim(key, 0, 2));
        System.out.println("all elements: " + jedis.lrange(key, 0, -1));
    }

    @Test
    public void setTest() {
        // 清空数据
        System.out.println(jedis.flushDB());
        String key = "myset";
        String key2 = "myset2";

        // 集合添加元素
        jedis.sadd(key, "aaa", "bbb", "ccc");
        jedis.sadd(key2, "bbb", "ccc", "ddd");

        // 获取集合里面的元素数量
        System.out.println(jedis.scard(key));

        // 获得两个集合的交集，并存储在一个关键的结果集
        jedis.sinterstore("destination", key, key2);
        System.out.println(jedis.smembers("destination"));

        // 获得两个集合的并集，并存储在一个关键的结果集
        jedis.sunionstore("destination", key, key2);
        System.out.println(jedis.smembers("destination"));

        // key集合中，key2集合没有的元素，并存储在一个关键的结果集
        jedis.sdiffstore("destination", key, key2);
        System.out.println(jedis.smembers("destination"));

        // 确定某个元素是一个集合的成员
        System.out.println(jedis.sismember(key, "aaa"));

        // 从key集合里面随机获取一个元素
        System.out.println(jedis.srandmember(key));

        // aaa从key移动到key2集合
        jedis.smove(key, key2, "aaa");
        System.out.println(jedis.smembers(key));
        System.out.println(jedis.smembers(key2));

        // 删除并获取一个集合里面的元素
        System.out.println(jedis.spop(key));

        // 从集合里删除一个或多个元素
        jedis.srem(key2, "ccc", "ddd");
        System.out.println(jedis.smembers(key2));
    }

    @Test
    public void sortSetTest() {
        // 清空数据
        System.out.println(jedis.flushDB());
        String key = "namespace:mysortset";

        Map<String, Double> scoreMembers = new HashMap<String, Double>();
        scoreMembers.put("aaa", 1001.0);
        scoreMembers.put("bbb", 1002.0);
        scoreMembers.put("ccc", 1003.0);

        // 添加数据
        jedis.zadd(key, 1004.0, "ddd");
        jedis.zadd(key, scoreMembers);

        // 获取一个排序的集合中的成员数量
        System.out.println(jedis.zcard(key));

        // 返回的成员在指定范围内的有序集合，以0表示有序集第一个成员，以1表示有序集第二个成员，以此类推。
        // 负数下标，以-1表示最后一个成员，-2表示倒数第二个成员
        Set<String> coll = jedis.zrange(key, 0, -1);
        System.out.println(coll);

        // 返回的成员在指定范围内的逆序集合
        coll = jedis.zrevrange(key, 0, -1);
        System.out.println(coll);

        // 元素下标
        System.out.println(jedis.zscore(key, "bbb"));

        // 删除元素
        System.out.println(jedis.zrem(key, "aaa"));
        System.out.println(jedis.zrange(key, 0, -1));

        // 给定值范围内的成员数
        System.out.println(jedis.zcount(key, 1002.0, 1003.0));
    }

    @Test
    public void hashTest() {
        // 清空数据
        System.out.println(jedis.flushDB());
        String key = "myhash";
        Map<String, String> hash = new HashMap<String, String>();
        hash.put("aaa", "11");
        hash.put("bbb", "22");
        hash.put("ccc", "33");

        // 添加数据
        jedis.hmset(key, hash);
        jedis.hset(key, "ddd", "44");

        // 获取hash的所有元素(key值)
        System.out.println(jedis.hkeys(key));

        // 获取hash中所有的key对应的value值
        System.out.println(jedis.hvals(key));

        // 获取hash里所有元素的数量
        System.out.println(jedis.hlen(key));

        // 获取hash中全部的域和值,以Map<String, String> 的形式返回
        Map<String, String> elements = jedis.hgetAll(key);
        System.out.println(elements);

        // 判断给定key值是否存在于哈希集中
        System.out.println(jedis.hexists(key, "bbb"));

        // 获取hash里面指定字段对应的值
        System.out.println(jedis.hmget(key, "aaa", "bbb"));

        // 获取指定的值
        System.out.println(jedis.hget(key, "aaa"));

        // 删除指定的值
        System.out.println(jedis.hdel(key, "aaa"));
        System.out.println(jedis.hgetAll(key));

        // 为key中的域 field 的值加上增量 increment
        System.out.println(jedis.hincrBy(key, "bbb", 100));
        System.out.println(jedis.hgetAll(key));
    }

    @Test
    public void transactionTest() {
        Transaction t = jedis.multi();
        t.set("hello", "world");
        Response<String> response = t.get("hello");

        t.zadd("foo", 1, "barowitch");
        t.zadd("foo", 0, "barinsky");
        t.zadd("foo", 0, "barikoviev");
        Response<Set<String>> sose = t.zrange("foo", 0, -1); //  返回全部相应并以有序集合的方式返回
        System.out.println(response);
        System.out.println(sose);
        t.exec(); // 此行注意，不能缺少

        String foolbar = response.get(); // Response.get() 可以从响应中获取数据

        int soseSize = sose.get().size(); // sose.get() 会立即调用set方法
        System.out.println(foolbar);
        System.out.println(sose.get());
    }
}
```



### 集群模式

## redisTemplate使用

### 单机模式

```Java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {"classpath:spring-service.xml"})
public class RedisTemplateTest {

    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    public void RedisUtilTest() {
        User user = new User();
        user.setUsername("lpf");
        RedisUtils.setValue("test", user);
        User user1 = (User) RedisUtils.getValue("test");
        System.out.println(user1.getUsername());
    }

    @Test
    public void stringTest() {
        //redisTemplate.expire("keyInt",1,TimeUnit.SECONDS);
        redisTemplate.opsForValue().increment("namespace:test:keyInt",1);
        System.out.println(redisTemplate.opsForValue().get("keyInt"));


        redisTemplate.opsForValue().set("num", "123");
        System.out.println(redisTemplate.opsForValue().get("num"));

        redisTemplate.opsForValue().set("num", "123", 10, TimeUnit.SECONDS);
        System.out.println(redisTemplate.opsForValue().get("num"));

        redisTemplate.opsForValue().set("key", "hello world");
        redisTemplate.opsForValue().set("key", "redis", 6);
        System.out.println("***************" + redisTemplate.opsForValue().get("key"));

        redisTemplate.opsForValue().set("getSetTest", "test");
        System.out.println(redisTemplate.opsForValue().getAndSet("getSetTest", "test2"));
        System.out.println(redisTemplate.opsForValue().get("getSetTest"));

        redisTemplate.opsForValue().set("test", "Hello");
        System.out.println(redisTemplate.opsForValue().get("test"));
        redisTemplate.opsForValue().append("test", "world");
        System.out.println(redisTemplate.opsForValue().get("test"));

        redisTemplate.opsForValue().set("key", "hello world");
        System.out.println("***************" + redisTemplate.opsForValue().size("key"));
    }

    @Test
    public void listTest() {
        System.out.println(redisTemplate.opsForList().size("list"));

        redisTemplate.opsForList().leftPush("list", "java");
        redisTemplate.opsForList().leftPush("list", "python");
        redisTemplate.opsForList().leftPush("list", "c++");
        System.out.println(redisTemplate.opsForList().range("list", 0, -1));

        redisTemplate.delete("list");
        String[] strs = new String[] {"1", "2", "3"};
        redisTemplate.opsForList().leftPushAll("list", strs);
        System.out.println(redisTemplate.opsForList().range("list", 0, -1));

        redisTemplate.delete("list");
        redisTemplate.opsForList().rightPush("listRight", "java");
        redisTemplate.opsForList().rightPush("listRight", "python");
        redisTemplate.opsForList().rightPush("listRight", "c++");
        System.out.println(redisTemplate.opsForList().range("list", 0, -1));

        redisTemplate.delete("list");
        redisTemplate.opsForList().rightPushAll("list", strs);
        System.out.println(redisTemplate.opsForList().range("list", 0, -1));

        System.out.println(redisTemplate.opsForList().range("listRight", 0, -1));
        redisTemplate.opsForList().set("listRight", 1, "setValue");
        System.out.println(redisTemplate.opsForList().range("listRight", 0, -1));

        //Long remove(K key, long count, Object value);
        //从存储在键中的列表中删除等于值的元素的第一个计数事件。
        //计数参数以下列方式影响操作：
        //count> 0：删除等于从头到尾移动的值的元素。
        //count <0：删除等于从尾到头移动的值的元素。
        //count = 0：删除等于value的所有元素。
        System.out.println(redisTemplate.opsForList().range("listRight", 0, -1));
        //将删除列表中存储的列表中第一次次出现的“setValue”
        redisTemplate.opsForList().remove("listRight", 1, "setValue");
        System.out.println(redisTemplate.opsForList().range("listRight", 0, -1));

        System.out.println(redisTemplate.opsForList().range("listRight", 0, -1));
        System.out.println(redisTemplate.opsForList().index("listRight", 2));

        System.out.println(redisTemplate.opsForList().range("list", 0, -1));
        System.out.println(redisTemplate.opsForList().leftPop("list"));
        System.out.println(redisTemplate.opsForList().range("list", 0, -1));

        System.out.println(redisTemplate.opsForList().range("listRight", 0, -1));
        System.out.println(redisTemplate.opsForList().rightPop("listRight"));
        System.out.println(redisTemplate.opsForList().range("listRight", 0, -1));
    }

    @Test
    public void hashTest() {
        Map<String, Object> testMap = new HashMap();
        testMap.put("name", "666");
        testMap.put("age", 27);
        testMap.put("class", "1");
        redisTemplate.opsForHash().putAll("redisHash", testMap);

        System.out.println(redisTemplate.opsForHash().get("redisHash","age"));
        System.out.println(redisTemplate.opsForHash().entries("redisHash"));

        System.out.println(redisTemplate.opsForHash().hasKey("redisHash","666"));
        System.out.println(redisTemplate.opsForHash().hasKey("redisHash","777"));

        redisTemplate.opsForHash().put("redisHash", "name", "667");
        redisTemplate.opsForHash().put("redisHash", "age", 36);
        redisTemplate.opsForHash().put("redisHash", "class", "66");
        System.out.println(redisTemplate.opsForHash().entries("redisHash"));
        System.out.println(redisTemplate.opsForHash().keys("redisHash"));
        System.out.println(redisTemplate.opsForHash().values("redisHash"));



        System.out.println(redisTemplate.opsForHash().size("redisHash"));
        Cursor<Map.Entry<Object, Object>> curosr = redisTemplate.opsForHash().scan("redisHash", ScanOptions.NONE);
        while (curosr.hasNext()) {
            Map.Entry<Object, Object> entry = curosr.next();
            System.out.println(entry.getKey() + ":" + entry.getValue());
        }

        System.out.println(redisTemplate.opsForHash().delete("redisHash", "name"));
        System.out.println(redisTemplate.opsForHash().entries("redisHash"));
    }

    @Test
    public void setTest(){
        String[] strs= new String[]{"str1","str2","str3","lpf","aaa"};
        System.out.println(redisTemplate.opsForSet().add("setTest", strs));

        String[] strs2 = new String[]{"str1","str2"};
        System.out.println(redisTemplate.opsForSet().pop("setTest"));
        System.out.println(redisTemplate.opsForSet().members("setTest"));
        System.out.println(redisTemplate.opsForSet().remove("setTest",strs2));
        System.out.println(redisTemplate.opsForSet().members("setTest"));

        redisTemplate.opsForSet().move("setTest","aaa","setTest2");
        System.out.println(redisTemplate.opsForSet().members("setTest"));
        System.out.println(redisTemplate.opsForSet().members("setTest2"));

        System.out.println(redisTemplate.opsForSet().size("setTest"));

        Cursor<Object> curosr = redisTemplate.opsForSet().scan("setTest", ScanOptions.NONE);
        while (curosr.hasNext()) {
            System.out.println(curosr.next());
        }
    }

    @Test
    public void sortSetTest(){
        System.out.println(redisTemplate.opsForZSet().add("zset1","zset-1",2.0));
        redisTemplate.opsForZSet().add("zset1","zset-2",1.2);
        redisTemplate.opsForZSet().add("zset1","zset-3",3.0);
        ZSetOperations.TypedTuple<Object> objectTypedTuple1 = new DefaultTypedTuple<>("zset-5",9.6);
        ZSetOperations.TypedTuple<Object> objectTypedTuple2 = new DefaultTypedTuple<>("zset-6",9.9);
        Set<ZSetOperations.TypedTuple<Object>> tuples = new HashSet<ZSetOperations.TypedTuple<Object>>();
        tuples.add(objectTypedTuple1);
        tuples.add(objectTypedTuple2);
        System.out.println(redisTemplate.opsForZSet().add("zset1",tuples));
        System.out.println(redisTemplate.opsForZSet().range("zset1",0,-1));

        System.out.println(redisTemplate.opsForZSet().range("zset1",0,-1));
        System.out.println(redisTemplate.opsForZSet().remove("zset1","zset-6"));
        System.out.println(redisTemplate.opsForZSet().range("zset1",0,-1));

        //Set<V> range(K key, long start, long end);
        //通过索引区间返回有序集合成指定区间内的成员，其中有序集成员按分数值递增(从小到大)顺序排列
        System.out.println(redisTemplate.opsForZSet().range("zset1",0,-1));
        //Long rank(K key, Object o);
        //返回有序集中指定成员的排名，其中有序集成员按分数值递增(从小到大)顺序排列
        System.out.println(redisTemplate.opsForZSet().rank("zset1","zset-5"));

        System.out.println(redisTemplate.opsForZSet().rangeByScore("zset1",0,5));
        //Long count(K key, double min, double max);
        //通过分数返回有序集合指定区间内的成员个数
        System.out.println(redisTemplate.opsForZSet().count("zset1",0,5));
        System.out.println(redisTemplate.opsForZSet().size("zset1"));

        //Double score(K key, Object o);
        //获取指定成员的score值
        System.out.println(redisTemplate.opsForZSet().score("zset1","zset-1"));

        System.out.println(redisTemplate.opsForZSet().range("zset1",0,-1));
        System.out.println(redisTemplate.opsForZSet().removeRange("zset1",1,2));
        System.out.println(redisTemplate.opsForZSet().range("zset1",0,-1));

        Cursor<ZSetOperations.TypedTuple<Object>> cursor = redisTemplate.opsForZSet().scan("zset1", ScanOptions.NONE);
        while (cursor.hasNext()) {
            ZSetOperations.TypedTuple<Object> item = cursor.next();
            System.out.println(item.getValue() + ":" + item.getScore());
        }
    }


    @Test
    public void redisTemplateTest(){
        //向redis里存入数据和设置缓存时间
        redisTemplate.opsForValue().set("baike", "100", 60 * 10, TimeUnit.SECONDS);
        //val做-1操作
        redisTemplate.boundValueOps("baike").increment(-1);
        //根据key获取缓存中的val
        redisTemplate.opsForValue().get("baike");
        //val +1
        redisTemplate.boundValueOps("baike").increment(1);
        //根据key获取过期时间
        redisTemplate.getExpire("baike");
        //根据key获取过期时间并换算成指定单位
        redisTemplate.getExpire("baike",TimeUnit.SECONDS);
        //根据key删除缓存
        redisTemplate.delete("baike");
        //检查key是否存在，返回boolean值
        redisTemplate.hasKey("baike");
        //向指定key中存放set集合
        redisTemplate.opsForSet().add("baike", "1","2","3");
        //设置过期时间
        redisTemplate.expire("baike",1000 , TimeUnit.MILLISECONDS);
        //根据key查看集合中是否存在指定数据
        redisTemplate.opsForSet().isMember("baike", "1");
        //根据key获取set集合
        redisTemplate.opsForSet().members("baike");
        //验证有效时间
        Long expire = redisTemplate.boundHashOps("baike").getExpire();
        System.out.println("redis有效时间："+expire+"S");
    }
    /**
     * 根据前缀删除key
     */
    public void deleteByPrex(String prex) {
        prex = prex + "**";
        Set<String> keys = redisTemplate.keys(prex);
        if (CollectionUtils.isNotEmpty(keys)) {
            redisTemplate.delete(keys);
        }
    }
}

```

### 集群模式

## redis高可用分布式集群

- **主从复制**

- **哨兵模式**

- **Redis-Cluster集群**：redis3.0推出的服务器端集群，服务器Sharding技术。
- **Redis Sharding集群**：轻量级的客户端Redis Sharding技术，采用一致性哈希算法(consistent hashing)

## redis分布式事务

## redis分布式锁

## **redis一致性hash算法**

>将用户和redis节点的hash值对应到一个32位的环形数据结构上，环形结构首尾封闭，用户通过hash算法来定位在环形结构上，redis节点也通过hash算法来定位到环形结构上，此时的命中问题就变成了，用户节点通过顺时针旋转，在旋转的过程中若碰到redis节点，就在该节点上读取数据，若此时在环形结构上增加新的redis节点，由于是顺时针寻找对应的redis节点，所以用户此时的redis命中率还是很高的，不会因为增加了一台redis节点就导致大量的用户命中失败的情况出现。(**Redis Sharding集群**)

##  分布式寻址算法

- hash算法（大量缓存重建）
- 一致性hash算法（自动缓存迁移）+虚拟节点（自动负载均衡）
- redis cluster的hash slot算法

## redis面试常问问题

- redis过期策略有哪些？
- redis淘汰机制有哪些？
- 手写LRU代码实现
- 如何保证redis的高并发和高可用？
- redis主从复制原理
- redis哨兵原理
- redis持久化方式？底层如何实现？
- redis集群模式工作原理？
- 集群模式redis的key是如何寻址的？
- 分布式寻址都有哪些算法？
- 一致性hash算法
- redis的雪崩、穿透和击穿
- redis崩溃如何处理
- 如何处理redis穿透
- 如何保证缓存和数据库的双写一致性
- redis并发竞争问题是什么？如何解决
- redis事务的CAS方案
- 生产环境redis是怎么部署的

## Redis可以用来做什么

## Redis使用的业务场景

### Redis

#### Redis 三种集群模式

- 主从模式
- 哨兵模式
- 集群

#### redis支持的数据结构  

string list hash set zset

#### redis线程模型 

redis是单线程实现。

#### redis 提供的持久机制

redis 支持rdb和afo两种持久机制。rdb是定时的持久机制，宕机有可能会存在数据丢失。aof是基于操作日志的持久机制。

#### redis事务

redis提供了一些在一定程度上支持线程安全和事务的命令。例如：multi/exec watch inc等。但是redis的事务并不支持回滚，即可以两个命令可以同时提交执行，但是如果有失败，成功的也不会回滚

#### redis数据淘汰策略

在 redis 中，允许用户设置最大使用内存大小通过配置redis.conf中的maxmemory这个值来开启内存淘汰功能，在内存限定的情况下是很有用的。

#### redis lru数据淘汰机制

再数据集合中随机挑选几个键值对，取出其中lru最大的键值对淘汰。所以redis并不是保证淘汰的是最近最少使用的，而是随机挑选的几个键值对中的最近最少使用的

#### 过期策略的实现

常见的过期策略有三种，定时，惰性，定期。redis使用了惰性和定期策略，memcached只使用了惰性策略

- 定时策略：指定在多少时间之后过期。
- 惰性策略：key过期的时候不删除，每次从数据库获取key的时候去检查是否过期，若过期，则删除，返回null
- 定期策略：每隔一段时间执行一次删除(在redis.conf配置文件设置hz，1s刷新的频率)过期key操作。java实现的话就是起个定时任务而已

#### 缓存穿透

缓存穿透指的是使用不存在的key进行大量的高并发查询，这导致缓存无法命中，每次请求都要穿透到后端数据库系统进行查询，数据库压力过大。

常用解决方案：将空值缓存起来。

其他解决方案：使用布隆过滤器（guava 19开始已支持布隆过滤器），布隆过滤器原理与应用

#### 缓存雪崩

缓存雪崩指缓存服务器重启或者大量缓存集中在某一个时间段内失效

常用解决办法：对不同的数据使用不同的失效时间，甚至对相同的数据、不同的请求使用不同的失效时间。

#### 缓存优秀实践

      1）使用前对数据大小进行评估，包括缓存的数据结构、大小、数量、失效时间
    
       2）根据业务进行隔离，尽量不要多个业务共用一个缓存实例
    
       3）缓存的key尽可能的设定缓存失效时间，且失效时间不能集中在某一点
    
       4）缓存尽量不要存大对象
    
       5）写缓存时一定要写入完全正确的数据。如果缓存数据部分有效，部分无效，宁可放弃缓存
    
       6）一定要对操作超时时间进行设置。一般我们设计缓存作为加速数据库读取的手段，也会对缓存操作做降级处理，因此推荐使用更短的缓存超时时间，如果一定要给出一个数字，则希望是100毫秒以内

#### redis相比memcached有哪些优势？

(1) memcached所有的值均是简单的字符串，redis作为其替代者，支持更为丰富的数据类型

(2) redis的速度比memcached快很多

(3) redis可以持久化其数据

#### redis 最适合的场景

- 会话缓存（Session Cache） 最常用的一种使用Redis的情景是会话缓存（session cache）
- 全页缓存（FPC） 除基本的会话token之外，Redis还提供很简便的FPC平台
- 队列 Reids在内存存储引擎领域的一大优点是提供 list 和 set 操作，这使得Redis能作为一个很好的消息队列平台来使用
- 排行榜/计数器 Redis在内存中对数字进行递增或递减的操作实现的非常好
- 发布/订阅 最后（但肯定不是最不重要的）是Redis的发布/订阅功能

#### 使用Redis有哪些好处？

(1) 速度快，因为数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)

(2) 支持丰富数据类型，支持string，list，set，sorted set，hash

(3) 支持事务，操作都是原子性，所谓的原子性就是对数据的更改要么全部执行，要么全部不执行

(4) 丰富的特性：可用于缓存，消息，按key设置过期时间，过期后将会自动删除

### MySQL的数据类型

>主要包括以下五大类：

- 整数类型：BIT、BOOL、TINY INT、SMALL INT、MEDIUM INT、 INT、 BIG INT
- 浮点数类型：FLOAT、DOUBLE、DECIMAL
- 字符串类型：CHAR、VARCHAR、TINY TEXT、TEXT、MEDIUM TEXT、LONGTEXT、TINY BLOB、BLOB、MEDIUM BLOB、LONG BLOB
- 日期类型：Date、DateTime、TimeStamp、Time、Year
- 其他数据类型：BINARY、VARBINARY、ENUM、SET、Geometry、Point、MultiPoint、LineString、MultiLineString、Polygon、GeometryCollection等