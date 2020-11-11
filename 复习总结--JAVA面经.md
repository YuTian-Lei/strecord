1.switch语句能否作用在byte上,能否作用在long上,能否作用在String上？
>
>- switch可作用于char byte short int
- switch可作用于char byte short int对应的包装类
- switch不可作用于long double float boolean，包括他们的包装类。

2.Spring IOC的的实现原理？
>工厂模式+反射

3.Spring AOP的实现原理？
>反射+动态代理

4.Redis注解的使用和事务实现的原理？
>运用了AOP

5.String,StringBuffer与StringBuilder的区别?
>StringBuilder > StringBuffer > String

6.ArrayList的扩容机制
>本质上就是计算出新的扩容数组的size后实例化，将原有数组内容复制到新数组中去。

7.char的长度是不可变的，而varchar的长度是可变的

8.为何索引的底层结构采用B+树而不是红黑树？
>
>- 文件很大，不可能全部存储在内存中，故要存储到磁盘上
- 索引的结构组织要尽量减少查找过程中磁盘I/O的存取次数（为什么使用B-/+Tree，还跟磁盘存取原理有关，B+树可以减少磁盘IO的存取次数）
- 局部性原理与磁盘预读，预读的长度一般为页（page）的整倍数
- 数据库系统巧妙利用了磁盘预读原理，将一个节点的大小设为等于一个页，这样每个节点只需要一次I/O就可以完全载入

9.MySQL索引的类型
>
>- 普通索引
- 唯一索引
- 主键索引
- 组合索引
- 全文索引

10.InnoDB索引和MyISAM索引的区别
>- 一是主索引的区别，InnoDB的数据文件本身就是索引文件(聚簇索引)。而MyISAM的索引和数据是分开的(非聚簇索引)。
- 二是辅助索引的区别：InnoDB的辅助索引data域存储相应记录主键的值而不是地址。而MyISAM的辅助索引和主索引没有多大区别。

11.什么是聚簇索引？什么是联合索引？
> - 聚簇索引：将数据存储与索引放到了一块，找到索引也就找到了数据
- 联合索引：两个或更多个列上的索引被称作联合索引，联合索引又叫复合索引。对于复合索引:Mysql从左到右的使用索引中的字段，一个查询可以只使用索引中的一部份，但只能是最左侧部分。

12.前缀索引
>对于列的值较长，比如BLOB、TEXT、VARCHAR，就必须建立前缀索引，即将值的前一部分作为索引。这样既可以节约空间，又可以提高查询效率。
但无法使用前缀索引做 ORDER BY 和 GROUP BY，也无法使用前缀索引做覆盖扫描。

13.覆盖索引
>跟联合索引有点类似，就是在查询的时候只用去读取索引而取得数据，无需进行二次查询相关表。这样的索引的叶子节点上面也包含了他们索引的数据。

14.什么情况下索引会失效？
>- 如果条件中有or，即使其中有条件带索引也不会使用(这也是为什么尽量少用or的原因,要想使用or，又想让索引生效，只能将or条件中的每个列都加上索引)
- 对于多列索引，不是使用的第一部分，则不会使用索引
- like查询是以%开头
- 如果列类型是字符串，那一定要在条件中将数据使用引号引用起来,否则不使用索引
- 如果mysql估计使用全表扫描要比使用索引快,则不使用索引

查看索引的使用情况
show status like ‘Handler_read%’;

15.MyISAM与InnoDB区别
>- InnoDB：提供事务支持事务，外部键等高级数据库功能；MyISAM不提供事务支持。
- InnoDB：支持事务和行级锁，是innodb的最大特色。MyISAM：只支持表级锁
- InnoDB：不支持FULLTEXT类型的全文索引

16.InnoDB什么时候使用行级锁？什么时候使用表级锁？
>在增删改查时匹配的条件字段不带有索引时，innodb使用的是表级锁

17.数据库常用的锁有哪些？
>表级锁定，行级锁定和页级锁定

18.表锁和行锁有什么区别
>- 表级所：锁定粒度大，锁冲突概率高、并发度低；好处是不会出现死锁、开销小、获取锁和释放锁的速度很快；适用于以查询为主，少量更新的应用
- 行级锁：好处是锁定对象的颗粒度很小，发生锁冲突的概率低、并发度高；缺点是开销大、加锁慢，行级锁容易发生死锁；适用于对事务完整性要求较高的系统

19.InnoDB行级锁类型
>- 共享锁：又称读锁，简单讲就是多个事务对同一数据进行共享一把锁，都能访问到数据，但是只能读不能修改。
- 排他锁：又称写锁，排他锁就是不能与其他所并存，如一个事务获取了一个数据行的排他锁，其他事务就不能再获取该行的其他锁，只有获取排他锁的事务可以对数据进行读取和修改。
- 意向锁是InnoDB自动加的，不需用户干预。意向锁不会与行级的共享 / 排他锁互斥！！！

20.mysql数据库cpu飙升到500%的话他怎么处理？
>- 多实例的服务器，先top查看是那一个进程，哪个端口占用CPU多；
- show processeslist查看是否由于大量并发，锁引起的负载问题；
- 否则，查看慢查询，找出执行时间长的sql；explain分析sql是否走索引，sql优化；
- 再查看是否缓存失效引起，需要查看buffer命中率；

22.并发和并行的区别
>- 并发的关键是你有处理多个任务的能力，不一定要同时。
- 并行的关键是你有同时处理多个任务的能力。

21.redis为什么是单线程？
>因为CPU不是Redis的瓶颈。Redis的瓶颈最有可能是机器内存或者网络带宽。

22.redis支持的数据类型有哪些？
>redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。


23.什么是缓存穿透?
>缓存穿透是指查询一个一定不存在的数据，由于缓存是不命中时需要从数据库查询，查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到数据库去查询，造成缓存穿透。
布隆过滤:  对所有可能查询的参数以hash形式存储，在控制层先进行校验，不符合则丢弃。
缓存空对象. 将 null 变成一个值.

24.什么是缓存雪崩？
>如果缓存集中在一段时间内失效，发生大量的缓存穿透，所有的查询都落在数据库上，造成了缓存雪崩。
- 加锁排队
- 数据预热
- 二级缓存，或者双缓存策略
- 缓存永远不过期

25.如何保证缓存和数据库的数据一致性？
>读的时候，先读缓存，缓存没有的话，就读数据库，然后取出数据后放入缓存，同时返回响应。
更新的时候，先更新数据库，然后再删除缓存。

26.redis持久化有几种方式？
>RDB持久化和AOF持久化配置

27.Redis缓存淘汰策略
>- noeviction
- allkeys-lru
- volatile-lru
- allkeys-random
- volatile-random
- volatile-ttl

28.Redis常见的性能问题有哪些？并且如何解决？
>- master写内存快照，seve命令调度rdbsave函数，会阻塞主线程的工程，当快照比较大的时候对性能的影响是非常大的，会间断性暂停服务 。所以master最好不要写内存快照。
- master AOF持久化，如果不重写AOF文件，这个持久化方式对性能的影响是最小的，但是AOF文件会不断增大，AOF文件过大会影响master重启时的恢复速度。
- master最好不要做任何持久化工作，包括内存快照和AOF日志文件，特别是不要启用内存快照做持久化，如果数据比较关键，某个slave开启AOF备份数据，策略每秒为同步一次。
- master调用BGREWRITEAOF重写AOF文件，AOF在重写的时候会占大量的CPU和内存资源，导致服务load过高，出现短暂的服务暂停现象。
- redis主从复制的性能问题，为了主从复制的速度和连接的稳定性，slave和master最好在同一个局域网内。

30.redis的使用
>对于查询，我们会考虑@Cacheable；对于插入和修改我们会考虑使用@CachePut；对于删除，我们会考虑@CacheEvict

31.死锁？死锁产生的原因？死锁的必要条件？怎么处理死锁？
> - 相互等待资源而产生的一种僵持状态，如果没有外力的干预将一直持续这个状态;
> - 系统资源不足、相互竞争资源、请求资源顺序不当
> - 互斥、不可抢占、循环等待、请求与保持
> - 因为互斥是不可改变的，所以只能破坏其他三个条件中的一个来解除死锁，方法：剥夺资源、杀死其中一个线程

31.操作系统之进程的状态和转换
>- 新建态
- 就绪态
- 运行态
- 等待态
- 终止态

![](https://img-blog.csdn.net/20170422190534211?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXdlNjExMjA3MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

32.Java线程的6种状态及切换
>- 初始(NEW)
- 运行(RUNNABLE)
- 阻塞(BLOCKED)
- 等待(WAITING)
- 超时等待(TIMED_WAITING)
- 终止(TERMINATED)

33.IPC通信方式？
>- 管道
- 消息队列
- 信号量
- 共享内存

34.常见的进程调度算法
>- 先来先服务和短作业(进程)优先调度算法
- 高优先权优先调度算法
- 基于时间片的轮转调度算法

35.同步和互斥的区别
>- 互斥是指某一资源同时只允许一个访问者对其进行访问，具有唯一性和排它性。但互斥无法限制访问者对资源的访问顺序，即访问是无序的。
- 同步是指在互斥的基础上（大多数情况），通过其它机制实现访问者对资源的有序访问。
- 同步其实已经实现了互斥，所以同步是一种更为复杂的互斥。
- 互斥是一种特殊的同步。

36.同步 异步 阻塞 非阻塞的区别
>同步/异步关注的是消息通知的机制，而阻塞/非阻塞关注的是程序（线程）等待消息通知时的状态
>- 同步阻塞
- 同步非阻塞
- 异步阻塞
- 异步非阻塞

37.线程/进程同步的方式
>- 临界区
- 互斥量
- 信号量
- 事件

38.什么是缓冲区溢出？缓冲区溢出的危害？
>copy数据进buffer时，数据长度超过buffer中的剩余空间;
>缓冲区溢出，结果随机，可能会导致程序功能不正常，也可能导致程序崩溃。

39.中断的类型？
>- I/O 中断
- 程序性中断
- 时钟中断
- 控制台中断
- 硬件故障

40.滑动窗口作用 
>- 滑动窗口机制是TCP用来控制发送数据包速率的
- 发送方每次只能发送滑动窗口内部的数据包

41.对tcp拥塞控制的理解
>- 慢开始
- 拥塞控制
- 快重传
- 快恢复

42.在浏览器中输入URL后，执行的全部过程
>- DNS域名解析
- 三次握手
- 建立TCP连接后发起http请求
- 服务器收到请求并响应HTTP请求
- 浏览器解析htm代码,并请求htm代码中的资源(如js、css图片等)
- 断开TCP连接
- 浏览器对页面进行渲染呈现给用户

43.http的长短连接
>- 在HTTP/1.0中默认使用短连接。也就是说，客户端和服务器每进行一次HTTP操作，就建立一次连接，任务结束就中断连接。当客户端浏览器访问的某个HTML或其他类型的Web页中包含有其他的Web资源（如JavaScript文件、图像文件、CSS文件等），每遇到这样一个Web资源，浏览器就会重新建立一个HTTP会话。
- 在使用长连接的情况下，当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，客户端再次访问这个服务器时，会继续使用这一条已经建立的连接。
- HTTP协议的长连接和短连接，实质上是TCP协议的长连接和短连接

44.HTTP1.0 HTTP 1.1主要区别
>- 长连接
- 节约带宽
- HOST域

45.HTTP1.1 HTTP 2.0主要区别
>- 多路复用
- 数据压缩
- 服务器推送

46.BIO与NIO、AIO的区别
>- 同步阻塞BIO(短连接)
- 同步非阻塞NIO（长连接、多路复用器）
- 异步非阻塞AIO

47.HTTPS工作过程
- 客户端发起请求
- 服务端返回证书
- 客户端从验证证书得到服务端的公钥
- 客户端生成随机数，并用公钥加密后发送给服务端
- 服务器根据随机数生成对称密钥
- 用对称密钥加密数据传输

48.HTTP与HTTPS有什么区别？
>- https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用
- http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议
- http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443
- http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全

49.如何预防SQL注入？
>- 严格检查输入变量的类型和格式
- 过滤和转义特殊字符
- 利用mysql的预编译机制

50.如何查看执行计划
>explain模拟优化器执行SQL语句，在5.6以及以后的版本中，除过select，其他比如insert，update和delete均可以使用explain查看执行计划，从而知道mysql是如何处理sql语句，分析查询语句或者表结构的性能瓶颈。
>explain select * from employees

51.Spring中的Bean是线程安全的吗
>- singleton:单例，默认作用域
- prototype:原型，每次创建一个新对象
- 原型Bean:对于原型Bean,每次创建一个新对象，也就是线程之间并不存在Bean共享，自然是不会有线程安全的问题
- 单例Bean:对于单例Bean,所有线程都共享一个单例实例Bean,因此是存在资源的竞争;如果单例Bean,是一个无状态Bean，也就是线程中的操作不会对Bean的成员执行查询以外的操作，那么这个单例Bean是线程安全的。比如Spring mvc 的 Controller、Service、Dao等，这些Bean大多是无状态的，只关注于方法本身;对于有状态的bean，Spring官方提供的bean，一般提供了通过ThreadLocal去解决线程安全的方法，比如RequestContextHolder、TransactionSynchronizationManager、LocaleContextHolder等。

52.使用ThreadLocal的好处
>使得多线程场景下，多个线程对这个单例Bean的成员变量并不存在资源的竞争，因为ThreadLocal为每个线程保存线程私有的数据。这是一种以空间换时间的方式。

53.Spring事务的隔离级别
>- ISOLATION_DEFAULT
- ISOLATION_READ_UNCOMMITTED 
- ISOLATION_READ_COMMITTED  
- ISOLATION_REPEATABLE_READ  
- ISOLATION_SERIALIZABLE

54.Spring中七种事务传播行为

事务传播行为类型|说明
-|-
PROPAGATION_REQUIRED|	如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。这是最常见的选择。
PROPAGATION_SUPPORTS|	支持当前事务，如果当前没有事务，就以非事务方式执行。
PROPAGATION_MANDATORY|	使用当前的事务，如果当前没有事务，就抛出异常。
PROPAGATION_REQUIRES_NEW|	新建事务，如果当前存在事务，把当前事务挂起。
PROPAGATION_NOT_SUPPORTED|	以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
PROPAGATION_NEVER|	以非事务方式执行，如果当前存在事务，则抛出异常。
PROPAGATION_NESTED|	如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类似的操作。

55.Spring常用的三种注入方式
>- 构造方法注入
- setter注入
- 基于注解的注入

56.Spring 框架中用到了哪些设计模式
>- 工厂设计模式 : Spring使用工厂模式通过 BeanFactory、ApplicationContext 创建 bean 对象。
- 代理设计模式 : Spring AOP 功能的实现。
- 单例设计模式 : Spring 中的 Bean 默认都是单例的。
- 模板方法模式 : Spring 中 jdbcTemplate、hibernateTemplate 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- 包装器设计模式 : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- 观察者模式: Spring 事件驱动模型就是观察者模式很经典的一个应用。
- 适配器模式 :Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配Controller。

57.SpringMVC重要组件
>- DispatchServlet
- HandlerMapping
- HandlerAdapter
- ViewResovler

58.SpringMVC常用注解
>- @Controller
- @RequestMapping
- @Resource和@Autowired
- @ModelAttribute和 @SessionAttributes
- @PathVariable
- @requestParam
- @ResponseBody
- @Component
- @Repository

59.SpringMVC怎样设定重定向和转发？
>- 在返回值的前面加”forward”,就可以实现让结果转发；
- 在返回值的前面加上”redirect”，就可以让返回值重定向。

60.SpringMVC的控制器是不是单例模式，如果是，有什么问题，怎么解决？
>是单例模式，所以在多线程访问的时候有线程安全问题，不要用同步，会影响性能，解决方案是在控制器里面不能写字段。(即不要设置变量)

61.Spring MVC如何解决中文乱码问题 ？
>- 可以使用SpringMVC提供的过滤器（CharacterEncodingFilter）来解决；只需要配置该过滤器就可以，需要注意的是：
- 过滤器的编码设置应该与jsp页面保存一致
- 表单的提交方式设置为post

62.SpringMVC 工作原理？
>- 1）客户端发送请求到 DispatcherServlet
- 2）DispatcherServlet 查询 handlerMapping 找到处理请求的 Controller
- 3）Controller 调用业务逻辑后，返回 ModelAndView
- 4）DispatcherServlet 查询 ModelAndView，找到指定视图
- 5）视图将结果返回到客户端

63.如果在拦截请求中,我想拦截 get 方式提交的方法,怎么配置？
>可以在@RequestMapping 注解里面加上 method=RequestMethod.GET

64.我想在拦截的方法里面得到从前台传入的参数,怎么得到？
直接在形参里面声明这个参数就可以,但必须名字和传过来的参数一样

65.如果前台有很多个参数传入,并且这些参数都是一个对象的,那么怎么样快速得到这象？
>直接在方法中声明这个对象,SpringMvc 就自动会把属性赋值到这个对象里面。

66.SpringMvc 用什么对象从后台向前台传递数据的？
>通过 ModelMap 对象,可以在这个对象里面用 put 方法,把对象加到里面,前台就可以过 el 表达式拿到。

67.SpringMvc 怎么和 AJAX 相互调用的？
>通过 Jackson 框架就可以把 Java 里面的对象直接转化成 Js 可以识别的 Json 对象

68.mybatis中的#和$的区别
>- 使用${}方式传入的参数，mybatis不会对它进行特殊处理，而使用#{}传进来的参数，mybatis默认会将其当成字符串。
- #和$在预编译处理中是不一样的。#类似jdbc中的PreparedStatement，对于传入的参数，在预处理阶段会使用?代替;而${}则是简单的替换
- 能使用#{}的地方应尽量使用#{}
- 像PreparedStatement ，#{}可以有效防止sql注入，${}则可能导致sql注入成功

69.Mybatis四种分页方式
>- 数组分页
- sql分页
- 拦截器分页(pageHelper)
- RowBounds分页(逻辑分页)

70.物理分页和逻辑分页的区别
>- 物理分页就是数据库本身提供了分页方式，如MySQL的limit，oracle的rownum ，好处是效率高，不好的地方就是不同数据库有不同的搞法。
- 逻辑分页利用游标分页，好处是所有数据库都统一，坏处就是效率低。

71.MyBatis中的延迟加载
>举个例子：如果查询订单并且关联查询用户信息。如果先查询订单信息即可满足要求，当我们需要查询用户信息时再查询用户信息。把对用户信息的按需去查询就是延迟加载。 所以延迟加载即先从单表查询、需要时再从关联表去关联查询，大大提高数据库性能，因为查询单表要比关联查询多张表速度要快。 

72.MyBatis一级缓存和二级缓存?
>- 一级缓存是SqlSession级别的缓存。在操作数据库时需要构造 sqlSession对象，在对象中有一个(内存区域)数据结构（HashMap）用于存储缓存数据。不同的sqlSession之间的缓存数据区域（HashMap）是互相不影响的。
- 一级缓存的作用域是同一个SqlSession，在同一个sqlSession中两次执行相同的sql语句，第一次执行完毕会将数据库中查询的数据写到缓存（内存），第二次会从缓存中获取数据将不再从数据库查询，从而提高查询效率。当一个sqlSession结束后该sqlSession中的一级缓存也就不存在了。Mybatis默认开启一级缓存。
- 二级缓存是mapper级别的缓存，多个SqlSession去操作同一个Mapper的sql语句，多个SqlSession去操作数据库得到数据会存在二级缓存区域，多个SqlSession可以共用二级缓存，二级缓存是跨SqlSession的。]
- 二级缓存是多个SqlSession共享的，其作用域是mapper的同一个namespace，不同的sqlSession两次执行相同namespace下的sql语句且向sql中传递参数也相同即最终执行相同的sql语句，第一次执行完毕会将数据库中查询的数据写到缓存（内存），第二次会从缓存中获取数据将不再从数据库查询，从而提高查询效率。Mybatis默认没有开启二级缓存需要在setting全局参数中配置开启二级缓存。


73.Mybatis都有哪些Executor执行器？它们之间的区别是什么？
>Mybatis有三种基本的Executor执行器，SimpleExecutor、ReuseExecutor、BatchExecutor。

>- SimpleExecutor：每执行一次update或select，就开启一个Statement对象，用完立刻关闭Statement对象。
- ReuseExecutor：执行update或select，以sql作为key查找Statement对象，存在就使用，不存在就创建，用完后，不关闭Statement对象，而是放置于Map<String, Statement>内，供下一次使用。简言之，就是重复使用Statement对象。
- BatchExecutor：执行update（没有select，JDBC批处理不支持select），将所有sql都添加到批处理中（addBatch()），等待统一执行（executeBatch()），它缓存了多个Statement对象，每个Statement对象都是addBatch()完毕后，等待逐一执行executeBatch()批处理。与JDBC批处理相同。

74.什么是springboot
>用来简化spring应用的初始搭建以及开发过程 使用特定的方式来进行配置

75.springboot常用的starter有哪些
>- spring-boot-starter-web 嵌入tomcat和web开发需要servlet与jsp支持
- spring-boot-starter-data-jpa 数据库支持
- spring-boot-starter-data-redis redis数据库支持
- spring-boot-starter-data-solr solr支持
- mybatis-spring-boot-starter 第三方的mybatis集成starter

76.springboot自动配置的原理
>在spring程序main方法中 添加@SpringBootApplication或者@EnableAutoConfiguration会自动去maven中读取每个starter中的spring.factories文件  该文件里配置了所有需要被创建spring容器中的bean

77.springboot读取配置文件的方式
>springboot默认读取配置文件为application.properties或者是application.yml

78.springboot如何添加【修改代码】自动重启功能
>添加开发者工具集=====spring-boot-devtools

79.Spring Boot 的核心注解是哪个？它主要由哪几个注解组成的？
>- @SpringBootConfiguration：组合了 @Configuration 注解，实现配置文件的功能。
- @EnableAutoConfiguration：打开自动配置的功能，也可以关闭某个自动配置的选项，如关闭数据源自动配置功能：@SpringBootApplication(exclude = { DataSourceAutoConfiguration.class })。
- @ComponentScan：Spring组件扫描。

80.Spring Boot 推荐和默认的日志框架是哪个？
>Spring Boot 将使用 Logback 作为默认日志框架

81.SpringBoot 实现热部署有哪几种方式？
>- Spring Loaded
- Spring-boot-devtools

82.单例模式的双重检测

	public class Singleton{   
    	// 静态属性，volatile保证可见性和禁止指令重排序
    	private volatile static Singleton instance = null; 
    	// 私有化构造器  
    	private Singleton(){}   

    	public static  Singleton getInstance(){   
       	 // 第一重检查锁定
       	 if(instance==null){  
       	     // 同步锁定代码块 
       	     synchronized(Singleton.class){
         	       // 第二重检查锁定
        	        if(instance==null){
          	          // 注意：非原子操作
           	         instance=new Singleton(); 
          	      }
         	   }              
       	 }   
      	  return instance;   
  	  }   
	}

83.包访问权限

权限|	类内|	同包	|不同包子类	|不同包非子类
-|-|-|-|-
private|√	|×	|×|	×
default	|√	|√|	×|	×
protected|	√|	√	|√|	×
public	|√|	√	|√|	√|

84.JAVA反射
>- 获取类对象三种方式
- 类的成员方法
- 类的成员变量
- 类的构造函数
- 类实现的接口
- 类的父类

85.JAVA创建对象的五种方式
>- new对象
- 反射的newinstance方法
- 反射获取构造器的newinstance方法
- 拷贝
- 反序列化

86.Java中String类为什么要设计成不可变的？
>- 常量池的需要
- hashcode 缓存的需要
- 多线程安全

87.String直接赋值和使用new的区别
>- 直接赋值，可能创建一个或者不创建对象
- 使用new,至少创建一个对象，也可能两个；使用new，在heap中肯定会创建一个，同时如果常量池没有，也会在常量池中创建一个string对象

88.为什么在重写equals方法时都要重写hashCode方法？
>- 如果两个对象相同（即用equals比较返回true），那么它们的hashCode值一定要相同；
- 如果两个对象的hashCode相同，它们并不一定相同(即用equals比较返回false)  

89.Exception和Error有什么区别
>- Exception是java程序运行中可预料的异常情况，咱们可以获取到这种异常，并且对这种异常进行业务外的处理。
- Error是java程序运行中不可预料的异常情况，这种异常发生以后，会直接导致JVM不可处理或者不可恢复的情况。

90.编译时异常与运行时异常的区别
>- 运行时异常：都是RuntimeException类及其子类异常，如NullPointerException(空指针异常)、IndexOutOfBoundsException(下标越界异常)等，这些异常是不检查异常，程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生。
运行时异常的特点是Java编译器不会检查它，也就是说，当程序中可能出现这类异常，即使没有用try-catch语句捕获它，也没有用throws子句声明抛出它，也会编译通过。
- 非运行时异常 （编译异常）：是RuntimeException以外的异常，类型上都属于Exception类及其子类。从程序语法角度讲是必须进行处理的异常，如果不处理，程序就不能编译通过。如IOException、SQLException等以及用户自定义的Exception异常，一般情况下不自定义检查异常。

91.如何实现对象克隆？
>- 实现 Cloneable 接口，重写 clone() 方法。
- 实现 Cloneable 接口，会报 CloneNotSupportedException 异常。

92.如何实现深拷贝
>- Object 的 clone() 方法是浅拷贝，即如果类中属性有自定义引用类型，只拷贝引用，不拷贝引用指向的对象
- 对象的属性的Class 也实现 Cloneable 接口，在克隆对象时也手动克隆属性
- 结合序列化(JDK java.io.Serializable 接口、JSON格式、XML格式等)，完成深拷贝

93.什么是Java序列化，如何实现java序列化
>- 序列化：把Java对象转换为字节序列的过程。
- 反序列化：把字节序列恢复为Java对象的过程。
- 只有实现了Serializable和Externalizable接口的类的对象才能被序列化

94.关于线程同步（7种同步方式）
>- synchronized关键字修饰的方法
- synchronized关键字修饰的语句块
- 特殊域变量(volatile)实现线程同步
- 使用重入锁实现线程同步
- 使用局部变量实现线程同步 
- 使用阻塞队列实现线程同步
- 使用原子变量实现线程同步

95.乐观锁和悲观锁的详细理解
>- 乐观锁说白了并不是锁，而只是版本号检查而已
- 乐观锁是一种思想，具体实现是，表中有一个版本字段，第一次读的时候，获取到这个字段。处理完业务逻辑开始更新的时候，需要再次查看该字段的值是否和第一次的一样。如果一样更新，反之拒绝。之所以叫乐观，因为这个模式没有从数据库加锁。
-  悲观锁是读取的时候为后面的更新加锁，之后再来的读操作都会等待。这种是数据库锁

96.乐观锁常见的两种实现方式？
>- 使用版本标识来确定读到的数据与提交时的数据是否一致。提交后修改版本标识，不一致时可以采取丢弃和再次尝试的策略。
- java中的compareandswap即cas，解决多线程并行情况下使用锁造成性能损耗的一种机制

97.什么是CAS？ABA问题如何解决？
>- 比较和交换（Conmpare And Swap）是用于实现多线程同步的原子指令。
- JAVA中提供了AtomicStampedReference/AtomicMarkableReference来处理会发生ABA问题的场景，主要是在对象中额外再增加一个标记来标识对象是否有过变更。

98.atomic的实现原理
>自旋 + CAS（乐观锁）

99.队列同步器（以下简称AQS）
>队列同步器，是用来构建锁或者其他同步组件的基础框架，它使用了一个int成员变量来表示同步状态，通过内置的FIFO队列来完成状态抢占线程的排队工作。ReenTrantLock实现了AQS

100.Semaphore实现原理
>Semaphore是用来保护一个或者多个共享资源的访问，Semaphore内部维护了一个计数器，其值为可以访问的共享资源的个数。一个线程要访问共享资源，先获得信号量，如果信号量的计数器值大于1，意味着有共享资源可以访问，则使其计数器值减去1，再访问共享资源;如果计数器值为0,线程进入休眠。当某个线程使用完共享资源后，释放信号量，并将信号量内部的计数器加1，之前进入休眠的线程将被唤醒并再次试图获得信号量。

101.为什么使用线程池？
>为了避免重复的创建线程，线程池的出现可以让线程进行复用

102.Executors创建线程池的四种方法

- newSingleThreadExecutor：创建一个单线程的线程池。这个线程池只有一个线程在工作，也就是相当于单线程串行执行所有任务。如果这个唯一的线程因为异常结束，那么会有一个新的线程来替代它。此线程池保证所有任务的执行顺序按照任务的提交顺序执行。
- newFixedThreadPool：创建固定大小的线程池。每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。线程池的大小一旦达到最大值就会保持不变，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程。
- newCachedThreadPool：创建一个可缓存的线程池。如果线程池的大小超过了处理任务所需要的线程，那么就会回收部分空闲（60秒不执行任务）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。
- newScheduledThreadPool：创建一个大小无限的线程池。此线程池支持定时以及周期性执行任务的需求。
- 推荐使用ThreadPoolExecutor自定义线程池

103.创建线程池的参数？

- corePollSize：核心线程数。
- maximumPoolSize：最大线程数。表明线程中最多能够创建的线程数量。
- keepAliveTime：空闲的线程保留的时间。
- TimeUnit：空闲线程的保留时间单位。
- BlockingQueue<Runnable>：阻塞队列，存储等待执行的任务。
- ThreadFactory：线程工厂，用来创建线程
- RejectedExecutionHandler：队列已满，而且任务量大于最大线程的异常处理策略。

104.线程池的submit和execute方法区别

- 接收的参数不一样
- submit有返回值，而execute没有
- submit方便Exception处理

105.JAVA中的Fork/Join框架
>fork/join框架是ExecutorService接口的一个实现，可以帮助开发人员充分利用多核处理器的优势，编写出并行执行的程序，提高应用程序的性能；设计的目的是为了处理那些可以被递归拆分的任务。fork/join框架与其它ExecutorService的实现类相似，会给线程池中的线程分发任务，不同之处在于它使用了工作窃取算法，所谓工作窃取，指的是对那些处理完自身任务的线程，会从其它线程窃取任务执行。fork/join框架的核心是ForkJoinPool类，该类继承了AbstractExecutorService类。ForkJoinPool实现了工作窃取算法并且能够执行 ForkJoinTask任务。

106.同步容器

- Vector
- Stack
- HashTable
- Collections.synchronized

107.并发容器

- ConcurrentHashMap
- CopyOnWriteArrayList
- CopyOnWriteArraySet
- ArrayBlockingQueue
- LinkedBlockingQueue

108.ArrayList的扩容机制
>创建一个新的数组对象，将原数组的数据复制到新数组中，并使用原来的引用

109.Array和ArrayList之间的区别
>- array中存放的引用，需要在此new对象
- ArrayList可以只是先声明
- 初始化大小
- Array不能够随意添加和删除其中的项，而ArrayList可以在任意位置插入和删除项

110.对象的访问定位的两种方式？
>- 句柄访问
- 直接指针访问

111.对象是否可回收?
>- 引用计数算法
- 可达性分析算法
- 对象从生存到死亡
- 回收方法区

112.Java内存泄露的理解与解决
>GC线程会从代码栈中的引用变量开始跟踪，从而判定哪些内存是正在使用的。如果GC线程通过这种方式，无法跟踪到某一块堆内存，那么GC就认为这块内存将不再使用了（即代码中已经无法访问这块内存了）。

113.Java内存的内存泄漏案例
>- 集合类，集合类仅仅有添加元素的方法，而没有相应的删除机制，导致内存被占用
- 单例模式

114.为什么要进行分代回收?
>JVM使用分代回收测试，是因为:不同的对象，生命周期是不一样的。因此不同生命周期的对象采用不同的收集方式,可以提高垃圾回收的效率。

115.垃圾回收原理
>- CMS垃圾回收器 Concurent Marked Sweep :并行的标记垃圾回收器,获取最短停顿的回收器， 标记清除算法实现
- 缺点是：对cpu资源敏感,无法处理浮动垃圾,有大量碎片产生

116.CMS垃圾回收器步骤
>- 初始标记
- 并行标记
- 并发预清理
- 重新标记
- 并发清理
- 并发重置

117.什么时候触发GC？
>- 执行 system.gc()的时候
- 老年代空间不足，一次Full GC 之后，然后不足 会触发 java.outofmemoryError:java heap space
- 永久代空间不足    永生代或者永久代， java.outofMemory  PerGen Space
- minor之后 survior放不下，放入老年代，老年代也放不下，触发FullGC, 或者新生代有对象放入老年代，老年代放不下，触发FullGC
- 新生代晋升为老年代时候，老年代剩余空间低于新生代晋升为老年代的速率，会触发老年代回收
- new 一个大对象，新生代放不下，直接到老年代，空间不够，触发FullGC

118.怎么避免频繁GC
>- 不要频繁的new 对象
- 不要显示的调研system.gc()
- 不要用String+ 使用StringBuilder'
- 不要使用Long Integer 尽量使用基本类型
- 少用静态变量 不会回收
- 可以使用null 进行回收