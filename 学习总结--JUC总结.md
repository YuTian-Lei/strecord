# Java并发

##  Java并发基础

### 如何实现线程

1. 继承Thread类创建线程类
2. 通过Runnable接口创建线程类
3. 通过Callable和Future创建线程

```java
public class HelloWorld {
public static void main(String[] args) throws InterruptedException, ExecutionException {    
	//thread类方式创建
     new Thread(){
    	 public void run(){
    		 System.out.println("Thread方式创建");
    	 }
     }.start();
	 //Runnable接口
     new Thread(new Runnable(){
    	 public void run(){
    		 System.out.println("runnable接口");
    	 }
     }).start();
	 //Callable接口和Future创建
     FutureTask<Integer> ft = new FutureTask<Integer>(new Callable<Integer>(){
    	 public Integer call(){
    		 System.out.println("callable接口");
    		 return 1994;
    	 }
     });
     new Thread(ft).start();
     System.out.println("线程的返回值："+ft.get());
	}
}
```

### 如何停止线程

1. 正常运行结束
2. 使用退出标志退出线程(**volatile变量**)
3. Interrupt方法结束线程
4. stop方法终止线程（线程不安全）

### 线程的6种状态

![](C:\Users\lenovo\Desktop\学习总结\img\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Ztd2luZA==,size_16,color_FFFFFF,t_70)

- 新建(new)
- 运行(runnable)
- 无限期等待(waiting)
- 限期等待(timed waiting)
- 阻塞(blocked)
- 结束(terminated)

### 线程同步

- synchronized(可重入锁)
- wait
- notify
- notifyAll

### ThreadLocal

- 每个thread有个ThreadLocalMap 
- ThreadLocalMap 以threadlocal为key，value为值
- 每个线程独立维护ThreadLocalMap 
- ThreadLocalMap中key为弱引用,可能存在内存泄漏问题,可以手动调用ThreadLocal的remove()方法

```Java
private static ThreadLocal<List<String>> myThreadLocal =
    new ThreadLocal<List<String>>() {
        @Override public List<String> initialValue() {
            return new ArrayList<String>();
        }
};
```



### 线程安全问题

- 多个线程存在共享数据访问
- 存在竞态条件
- "先检查后执行"
- "读取-修改-写入"
- 死锁问题

### 线程安全的实现方法

- 互斥同步(加锁)
- 非阻塞同步(CAS)
- 无同步方案(无状态的对象一定是线程安全的、线程本地存储ThreadLocal)

## JMM

### 线程通信机制

#### 进程间通信五种方式

- 管道
- 命名管道
- 消息队列
- 信号量
- 共享内存

#### 线程间通信的方式

- 内存共享(Java采用)
- 消息传递

### 内存模型

- 工作内存和主内存
- volatile变量
- 原子性、可见性和有序性(volatile/synchronized)
- 先行发生原则

### synchronized

#### 实现机制

- java对象头中的markword
- monitorenter
- monitorexit

#### 锁优化

- 锁消除
- 锁粗化
- 锁升级: 无锁->偏向锁->自旋锁(轻量级锁)->重量级锁(申请mutex,进行内核态和用户态切换)

### volatile

- 禁止指令重排序
- 保证变量的可见性

### DCL(Double Check Lock)

## JUC

![](.\img\406312-20160614144055573-1639231351.png)

### AQS

>所谓AQS，指的是AbstractQueuedSynchronizer，它提供了一种实现阻塞锁和一系列依赖FIFO等待队列的同步器的框架，ReentrantLock、Semaphore、CountDownLatch、CyclicBarrier等并发类均是基于AQS来实现的，具体用法是通过继承AQS实现其模板方法，然后将子类作为同步组件的内部类。主要应用在JUC的Lock和tools包中。

### CAS

> AVA中的CAS操作都是通过sun包下Unsafe类实现，而Unsafe类中的方法都是native方法，由JVM本地实现.主要应用在JUC的Atomic包中

### Atomic

> 其中java.util.concurrent.atomic包，**方便在无锁的情况下，进行原子操作**。

![](C:\Users\lenovo\Desktop\学习总结\img\35063859.jpg)

```Java
public final int getAndIncrement() {
        for (;;) {
            int current = get();
            int next = current + 1;
            if (compareAndSet(current, next))
                return current;
        }
}
```

### Excutor

![img](http://www.blogjava.net/images/blogjava_net/xylz/Windows-Live-Writer/-Java-Concurrency-28--part-1-_1302E/Executor-class_2.png)

```Java
public class AsyncProcessor {

  /**
   * 线程池核心池的大小<corePoolSize>  -- 计算密集型
   * 线程数 = CPU核数+1，也可以设置成CPU核数*2，但还要看JDK的版本以及CPU配置(服务器的CPU有超线程)
   */
  /*private static final int COREPOOLSIZE = Runtime.getRuntime().availableProcessors() * 2;*/

  /**
   * 线程池核心池的大小<corePoolSize>  -- IO密集型 计算公式：线程数 = CPU核心数/(1-阻塞系数) 这个阻塞系数一般为0.8~0.9之间，也可以取0.8或者0.9
   */
  private static final int COREPOOLSIZE = Runtime.getRuntime().availableProcessors() * 10;

  /**
   * 线程池的最大线程数<maximumPoolSize>
   */
  private static final int MAXIMUMPOOLSIZE = COREPOOLSIZE * 2;

  /**
   * 默认线程存活时间
   */
  private static final long DEFAULT_KEEP_ALIVE = 60L;

  /**
   * 默认队列大小
   */
  private static final int DEFAULT_SIZE = 500;

  /**
   * 线程池名称格式
   */
  private static final String THREAD_POOL_NAME = "SchedulePoolThreadExecutor-%d";

  /**
   * 捕获线程异常
   * */
  private static Thread.UncaughtExceptionHandler uncaughtExceptionHandler = new Thread.UncaughtExceptionHandler(){
    @Override public void uncaughtException(Thread t, Throwable e) {
      if (e != null) {
        log.error(e.getMessage(), e);
      }
    }
  };

  /**
   * 线程工厂
   */
  private static final ThreadFactory FACTORY =
      new BasicThreadFactory.Builder().namingPattern(THREAD_POOL_NAME).uncaughtExceptionHandler(uncaughtExceptionHandler).build();

  /**
   * 执行队列
   */
  private static BlockingQueue<Runnable> executeQueue = new ArrayBlockingQueue<>(DEFAULT_SIZE);

  /**
   * 自定义拒绝策略
   */
  private static RejectedExecutionHandler rejectedExecutionHandler = new RejectedExecutionHandler(){
    @Override public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
      // todo 记录异常
      // todo 报警处理等
      log.error("线程池拒绝策略触发!当前执行任务数:{}" , executor.getActiveCount());
    }
  };

  /**
   * 单例模式 -- 饿汉
   */
  private static AsyncProcessor instance = new AsyncProcessor();

  private AsyncProcessor() {
  }

  public static AsyncProcessor instance() {
    return instance;
  }

  /**
   * 异步操作任务调度线程池
   */
  private ExecutorService executor = new ThreadPoolExecutor(
      COREPOOLSIZE,
      MAXIMUMPOOLSIZE,
      DEFAULT_KEEP_ALIVE,
      TimeUnit.SECONDS,
      executeQueue,
      FACTORY,
      rejectedExecutionHandler);

  /**
   * 执行任务
   */
  public void execute(Runnable task) {
    executor.execute(task);
  }

  /**
   * 提交任务
   */
  public <T> Future<T> submit(Callable<T> task) {
    return executor.submit(task);
  }

  /**
   * 停止线程池
   */
  public void shutdown() {
    if(executor != null)
    {
      executor.shutdown();
    }
  }
}
//关于corePoolSize、maxPoolSize、queueCapacity之间的关系：
//corePoolSize为初始线程个数，当corePoolSize的线程都在执行中时，则将Runnable临时放入queueCapacity的缓冲队列中等待，当queueCapacity满了时，才会将线程个数从corePoolSize扩展至maxPoolSize，如果此时queueCapacity缓存队列任然是满的，则后续Runnable对象加入其中时就会被abort抛弃。
1.线程数量 < corePoolSize  corePoolSize运行
2.corePoolSize < 线程数量 < queueCapacity   corePoolSize全部运行,多出的放入缓冲队列中
3.corePoolSize + queueCapacity   < 线程数量  < maxPoolSize + queueCapacity  最大线程开启,超出核心线程数和队列容量和的线程,放入最大线程中运行
4.线程数量 > maxPoolSize + queueCapacity     触发拒绝策略
    
    
1.线程池不添加任务,则poolSize为0;之后添加任务数 < corePoolSize,则poolSize = 添加的任务数; 线程开启后一直存在,不会超时销毁
2.allowCoreThreadTimeOut = false 核心线程一直存在,最大线程超时(空闲超时)自动销毁
3.allowCoreThreadTimeOut = true  核心线程超时(空闲超时)自动销毁,最大线程超时(空闲超时)自动销毁
    
    
    
1.当线程池小于corePoolSize时，新提交任务将创建一个新线程执行任务，即使此时线程池中存在空闲线程。
2.当线程池达到corePoolSize时，新提交任务将被放入workQueue中，等待线程池中任务调度执行
3.当workQueue已满，且maximumPoolSize>corePoolSize时，新提交任务会创建新线程执行任务
4.当提交任务数超过maximumPoolSize时，新提交任务由RejectedExecutionHandler处理
5.当线程池中超过corePoolSize线程，空闲时间达到keepAliveTime时，关闭空闲线程
6.当设置allowCoreThreadTimeOut(true)时，线程池中corePoolSize线程空闲时间达到keepAliveTime也将关闭    
    
//allowCoreThreadTimeOut这个方法就像其字面的意思一样，允许Core Thread超时后可以关闭
```

#### 拒绝策略

- **AbortPolicy**:丢弃任务并抛出RejectedExecutionException异常
- **DiscardPolicy**:丢弃任务，但是不抛出异常
- **DiscardOldestPolicy**:丢弃队列最前面的任务，然后重新提交被拒绝的任务
- **CallerRunsPolicy**:由调用线程处理该任务
- 自定义拒绝策略

### Collections

![](.\img\webp)

### locks

- lock接口
- Condition接口
- ReadWriteLock接口

```Java
//Lock和Condition
public class ThreadLock {
	private int age = 20;
	private Lock lock = new ReentrantLock();
	private Condition condition = lock.newCondition();

	public void setAge(int age) throws InterruptedException {
		synchronized (this) {
			this.age = age;
			this.wait();
			TimeUnit.SECONDS.sleep(10);
			System.out.println("释放锁");
		}
	}

	public synchronized int getAge() {
		this.notify();
		return age;
	}

	public void setAge1(int age) {
		lock.lock();
		try {
			this.age = age;
			condition.await();
			TimeUnit.SECONDS.sleep(10);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} finally {
			lock.unlock();
			System.out.println("释放锁");
		}
	}

	public int getAge1() {
		lock.lock();
		try {
			condition.signal();
			return age;
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			lock.unlock();
		}
		return 0;
	}

	public static void main(String[] args) throws InterruptedException {
		ThreadLock t1 = new ThreadLock();
		ThreadLock t2 = new ThreadLock();
		CountDownLatch count = new CountDownLatch(1);
		new Thread(new Runnable() {
			public void run() {
				t1.setAge1(18);
			}
		}).start();
		new Thread(new Runnable() {
			public void run() {
				System.out.println(t1.getAge1());
			}
		}).start();
		count.await();
	}
}

//ReadWriteLock
public class Test {

    static HashMap<Integer, Integer> hashMap = new HashMap<>();
    static ReadWriteLock readWriteLock = new ReentrantReadWriteLock();

    static void put(Integer key, Integer value) {

        readWriteLock.writeLock().lock();
        try {
            System.out.println(Thread.currentThread().getName()+ "  ==开始写入" + value);
            TimeUnit.MILLISECONDS.sleep(300);
            hashMap.put(key, value);
            System.out.println("写入完成");

        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            readWriteLock.writeLock().unlock();
        }

    }

    static void get(Integer key) {
        readWriteLock.readLock().lock();
        try {
            System.out.println(Thread.currentThread().getName() + "  读取数据");
            TimeUnit.MILLISECONDS.sleep(300);
            Integer integer = hashMap.get(key);
            System.out.println("读取完成" + integer);

        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            readWriteLock.readLock().unlock();
        }

    }

    public static void main(String[] args) {

        for (int i = 1; i <= 5; i++) {
            int finalI = i;
            new Thread(new Runnable() {
                @Override
                public void run() {
                    Test.put(finalI, finalI);
                }
            }, String.valueOf(i)).start();
        }


        for (int i = 1; i <= 5; i++) {
            int finalI = i;
            new Thread(new Runnable() {
                @Override
                public void run() {
                    Test.get(finalI);
                }
            }, String.valueOf(i)).start();
        }

    }
}
```



### tools

- **Executors**:创建线程池
- **Semaphor**:`Semaphore`能够实现的功能是允许多个线程同时获取共享资源，实际是共享锁（基于AQS的共享实现模式）的实现
- **CyclicBarrier**:CyclicBarrier允许一组线程相互等待，直到到达某个公共的屏障点这组线程才继续执行
- **CountDownLatch**:CountDownLatch所描述的是"在完成一组正在其他线程中执行的操作之前，它允许一个或多个线程一直等待"
- **Exchanger**:用于两个工作线程之间交换数据的封装工具类

```Java
//Executors ->线程池执行FutureTask
public class HelloWorld {
	public static void main(String[] args) throws InterruptedException,ExecutionException {
		ExecutorService pool =  Executors.newCachedThreadPool();//创建线程池
	
	    FutureTask<Integer> ft = new FutureTask<Integer>(new Callable<Integer>(){
	   	 public Integer call(){
	   		 System.out.println("callable接口");
	   		 return 1994;
	   	 }
	    });
	    Thread thread = new Thread(ft);//执行线程
	    pool.execute(thread);
		pool.execute(new Thread(){     //执行匿名内部类的线程
	   	 public void run(){
			 System.out.println("Thread方式创建");
		 }
	    });
		pool.execute(new Thread(new Runnable(){//执行匿名内部类的线程
	   	 public void run(){
		 System.out.println("runnable接口");
		 }
		}));
	     System.out.println("线程的返回值："+ft.get());
		}
	}
//Semaphor
public static void main(String[] args) throws InterruptedException  {
		AtomicInteger incr = new AtomicInteger(1);
		CountDownLatch countDownLatch = new CountDownLatch(200);
		Semaphore semp = new Semaphore(5);
		for(int i = 0; i < 200; i++) {
			new Thread() {
				public void run() {
					try {
						semp.acquire();
						TimeUnit.MILLISECONDS.sleep(1000);
						System.out.println("获取执行"+incr.getAndIncrement()+"线程");
						countDownLatch.countDown();
					} catch (InterruptedException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}finally {
						semp.release();
					}
				}
			}.start();
		}
		countDownLatch.await();
	}
//CyclicBarrier
public class CyclicBarrierTest {
    private static CyclicBarrier cyclicBarrier = new CyclicBarrier(5, () -> System.out.println("all thread complete"));
 
    public static void main(String[] args) {
        for (int i = 0; i <5 ; i++) {
            new Thread(() -> {
                try {
                    System.out.println(Thread.currentThread().getId()+" running!");
                    cyclicBarrier.await();
                    System.out.println(Thread.currentThread().getId()+" running again!");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
            }).start();
        }
        System.out.println("main begin");
    }
}
//CountDownLatch
public static void main(String[] args) throws InterruptedException  {
		 CountDownLatch countDownLatch = new CountDownLatch(5);
		 new Thread(new Runnable() {
			 public void run() {
				 for(int i = 0 ; i < 4; i++) {
				     System.out.println("第"+i+"次释放");
					 countDownLatch.countDown();
					 }
			 }
		 }).start();
		 countDownLatch.await();
		 System.out.println("程序终止");
	}
//Exchanger
public class ExchangerTest {

    public static void main(String[] args) {
        final Exchanger<String> exchanger = new Exchanger<>();


        new Thread(()->{
            System.out.println(Thread.currentThread().getName() + " start . ");
            try {
                String exchange = exchanger.exchange("I am come from T-A");
                System.out.println(Thread.currentThread().getName() + " get value : " + exchange);
                System.out.println(Thread.currentThread().getName() + " end . ");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        },"A").start();


        new Thread(()->{
            System.out.println(Thread.currentThread().getName() + " start . ");
            try {
                String exchange = exchanger.exchange("I am come from T-B");
                System.out.println(Thread.currentThread().getName() + " get value : " + exchange);
                System.out.println(Thread.currentThread().getName() + " end . ");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        },"B").start();
    }
}
```

### Fork/join

> Java.util.concurrent.ForkJoinPool由Java大师Doug Lea主持编写，它可以将一个大的任务拆分成多个子任务进行并行处理，最后将子任务结果合并成最J后的计算结果，并进行输出。



