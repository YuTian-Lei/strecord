# JVM(HotSpot虚拟机)

##  类加载

### 类加载时机

> Java虚拟机规范并没有严格规定什么时候进行加载阶段,但是严格规定了有且只有6种情况必须立即对类进行"初始化"(而加载、验证、准备自然需要在此之前开始)
>
> - new对象、调用静态方法
> - 反射包中,class.forName
> - 初始化类时，其父类还没有进行初始化,则需要进行父类初始化
> - 虚拟机启动时,用户需要指定主类,虚拟机会先初始化这个主类
> - JDK1.7四种类型方法句柄
> - JDK8后的默认方法,接口实现类发生初始化时,接口要在其之前被初始化

### 类加载过程

- 加载阶段
- 连接阶段(验证、准备、解析)
- 初始化阶段

###  类加载器

- 类与类加载器唯一确定一个类对象
- 启动类加载器(Java_Home/lib)
- 扩展类加载器(Java_Home/lib/ext)
- 应用程序类加载器(ClassPath及当前程序类路径)
- 双亲委派模型
- 数组类的特殊性：数组类本身不通过类加载器创建，它是由 Java 虚拟机直接创建的。

### 虚拟机参数

- 验证类的加载情况呢，可以通过在启动的时候配置虚拟机参数：`-XX:+TraceClassLoading`查看

## 运行时数据区域

- 程序计数器(线程私有)
- java虚拟机栈(线程私有,存放局部变量表)
- 本地方法栈(线程私有,为native方法服务)
- Java堆(线程共享,对象实例及数组)
- 方法区(线程共享,类型信息、常量、静态变、运行时常量池)
- JDK1.8后运行时常量池逻辑上在方法区,物理上在堆
- 直接内存

## 对象创建

### 过程

- 类加载检查
- 对象内存分配
- 将出对象头外的内存空间都初始化为零值
- 设置对象头
- 执行构造函数

### 对象内存布局

![](.\img\watermark)

![](.\img\70)



#### 对象头

##### markword(8个字节,存储对象自身运行时数据)

- 哈希码
- GC分代年龄
- 锁状态标志
- 线程持有锁
- 偏向线程ID
- 偏向时间戳

![](.\img\71)

##### 类型指针(开启压缩指针4个字节,不压缩指针8个字节)

> 对象指向它的类型元数据的指针,Java虚拟机通过这个指针来确定该对象是那个类的实例

#### 实例数据

- 各个类型的字段内容(引用指针)

#### 对齐填充

- 起占位符的作用,对齐填充,保证能够被8个字节整除(64位虚拟机)

#### 对象访问定位

- 使用句柄访问(相当于二级指针)
- 直接指针访问(相当于一级指针,hotspot采用此种方式)

### 四大引用

- 强引用：永远不会被回收
- 软引用：内存不足时,进行二次垃圾回收,才会被回收
- 弱引用：下一次垃圾回收时被回收
- 虚引用：设置的唯一目的就是为了能在这个对象被回收时收到一个系统通知,无法通过虚引用获取对象实例

## 垃圾回收

### 垃圾回收算法

#### 对象是否存活

- 引用计数法
- 可达性分析算法

#### GCROOT有哪些

- 虚拟机栈中的引用对象
- 方法区中类静态变量引用对象
- 方法区中常量引用对象
- 本地方法栈中引用的对象
- 虚拟机内部的引用,如基本类型的class对象,常驻的异常对象

#### 分代收集理论

- 新生代（新生代收集/Minor GC）
- 老年代（老年代收集/Major GC）

#### 垃圾收集算法

- 标记-清除算法(基础算法)
- 标记-复制算法(新生代垃圾收集算法，Appel式回收,新生代分成一块较大的Eden空间和两块较小的survivor空间)
- 标记-整理算法(老年代垃圾收集算法)

### 垃圾收集器

- Serial收集器(串行垃圾收集器,新生代垃圾收集器)
- ParNew收集器(并行垃圾收集器,新生代垃圾收集器)
- Parallel Scavenge收集器(并行垃圾收集器,新生代垃圾收集器,侧重点于吞吐量)
- Serial Old收集器(串行垃圾收集器,Serial收集器老年代版本,老年代垃圾收集器)
- Parallel Old收集器(并行垃圾收集器,Pa·rallel Scavenge收集器老年代版本,侧重点于吞吐量)
- CMS收集器(并发垃圾收集器,老年代垃圾收集器,"标记-清除"算法,会引起三色标记问题)
- G1收集器(并发垃圾收集器,混合收集器,采用了内存分区(Region)的思路,不同于之前的收集算法,会引起三色标记问题)

#### 垃圾收集器组合(hotspot)

![](.\img\72)

- Serial (DefNew) + Serial Old（Serial Mark Sweep Compact） ,-XX:+UseSerialGC
- Parallel (ParNew) + Serial Old（Serial Mark Sweep Compact）,-XX:+UseParNewGC
- Parallel Scavenge (PSYoungGen) + Serial Old（Serial Mark Sweep Compact (PSOldGen)）,-XX:+UseParallelGC
- Parallel Scavenge (PSYoungGen) + Parallel Old (ParOldGen),-XX:+UseParallelOldGC
- Serial (DefNew) + CMS（Concurrent Mark Sweep）,-XX:-UseParNewGC -XX:+UseConcMarkSweepGC
- Parallel (ParNew) + CMS（Concurrent Mark Sweep） + Serial Old（Serial Mark Sweep Compact）,-XX:+UseParNewGC -XX:+UseConcMarkSweepGC
- G1（Garbage First），不需要搭配其他垃圾收集器,-XX:+UseG1GC

### 内存分配策略

![](.\img\73)

- 对象优先在Eden分配
- 大对象直接进入老年代
- 长期存活对象进入老年代
- 动态对象年龄判定
- 空间分配担保

## Java内存模型

- 线程并发访问下的内存模型
- 所有的变量都存储在主内存(共享变量)
- 各个线程的工作内存保存了变量的主内存副本
- 线程对变量的所有操作(读取、赋值等)都必须在工作内存中进行
- volatile、synchronized变量及原子性、可见性、有序性
- 先行发生(Happen-Before)原则





