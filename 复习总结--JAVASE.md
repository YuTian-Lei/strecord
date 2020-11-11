## 一、JAVA部分
### 1、JAVA基础
***
#### 1.1 Java 常用包总结

**java常用包**：   
java.lang--语言包：Java语言的基础类，包括Object类、Thread类、String、Math、System、Runtime、Class、Exception、Process等，是Java的核心类库

java.util--实用工具包：Scanner、Date、Calendar、LinkedList、AarryList、concurrent、Hashtable、Stack、TreeSet等；


java.NET--网络功能包：URL、Socket、ServerSocket等；

java.sql--数据库连接包：实现JDBC的类库；

java.io--输入输出包：提供与流相关的各种包；

**Java 常用第三方jar包:**

log4j：一个非常常用的log日志jar包。

JUnit：java单元测试；

maven：项目管理的；

gson：Google 的Json解析库；

jsoup：html解析；

apache commons：包含了大量组件，很多实用小工具。

**Java常用接口：**

Comparable ,Collection,Set, List, Map, Runnable Iterable Iterator 等等
***
#### 1.2 Java标识符
**标识符是什么？**

 > 标识符就是用于Java程序中变量，类，方法等命名的符号。

**使用标识符时，需要遵守几条规则：**

-  标识符可以由字母，数字，下划线（——），美元（$）组成，但是不能包含@，%，空格等其他的特殊符号，不能以数字开头。例如 123name 就是不合法的

- 标识符不能是Java关键字和保留字（Java预留的关键字，或者以后升级版本中有可能作为关键字），但可以包含关键字和保留字~例如：不可以使用void 作为标识符，但是Myvoid 可以

- 标识符是严格却分大小写的，所以一定要分清alibaba和ALIbaba是两个不同的标识符哦

- 标识符的命名最好能反应出其作用，做到见名知意
***
####  1.3 Java特性
>封装、抽象、继承、多态
***
#### 1.4 匿名对象
**匿名对象使用方法**

当对象对方法仅进行一次调用的时候，就可以简化成匿名对象。
- 	
    new Car().run()

    new Car().run()
匿名对象可以作为实际参数进行传递。
-
    public static void show(Car c)
    	{
	//......
	}
	show(new Car());
 匿名对象要注意的事项：

 > 我们一般不会给匿名对象赋予属性值，因为永远无法获取到。
 > 
 > 两个匿名对象永远都不可能是同一个对象。
***
### 1.5 继承
 >Java之中只允许多层继承，不允许多重继承，Java存在单继承局限。
 >
 >父类私有属性发现无法直接进行访问，但是却发现可以通过setter、getter方法间接的进行操作
 >
 >现在默认调用的是无参构造，而如果这个时候父类没有无参构造，则子类必须通过super()调用指定参数的构造方法.
***
### 1.6 抽象类和接口
#### 抽象类

  **定义**：抽象方法必须用abstract关键字进行修饰。如果一个类含有抽象方法，则称这个类为抽象类，抽象类必须在类前用abstract关键字修饰。因为抽象类中含有无具体实现的方法，所以不能用抽象类创建对象。

  **说明**：包含抽象方法的类称为抽象类，但并不意味着抽象类中只能有抽象方法，它和普通类一样，同样可以拥有成员变量和普通的成员方法。注意，抽象类和普通类的主要有三点区别：

1. 抽象方法必须为public或者protected（因为如果为private，则不能被子类继承，子类便无法实现该方法），缺省情况下默认为public。

2. 抽象类不能用来创建对象；

3. 如果一个类继承于一个抽象类，则子类必须实现父类的抽象方法。如果子类没有实现父类的抽象方法，则必须将子类也定义为为abstract类。
接口

#### 一、基本概念

接口（Interface），**在JAVA编程语言中是一个抽象类型，是抽象方法的集合**。接口通常以interface来声明。一个类通过继承接口的方式，从而来继承接口的抽象方法。

**如果一个类只由抽象方法和全局常量组成，那么这种情况下不会将其定义为一个抽象类，只会定义为一个接口。所以接口严格的来讲属于一个特殊的类，而这个类里面只有抽象方法和全局常量**，就连构造方法也没有。

范例：定义一个接口

	interface A{//定义一个接口
	
		public static final String MSG = "hello";//全局常量
		public abstract void print();//抽象方法
		}
		class B implements A{
			public void print(){...};
		} 
	public class TestDemo {
		public static void main(String[] args){
			A test = new B();
			A.print();
			}
		}
#### 二、接口的使用

1.由于接口里面存在抽象方法，所以接口对象不能直接使用关键字new进行实例化。接口的使用原则如下： 

- （1）接口必须要有子类，但此时一个子类可以使用implements关键字实现多个接口； 
- （2）接口的子类（如果不是抽象类），那么必须要覆写接口中的全部抽象方法； 
- （3）**接口的对象可以利用子类对象的向上转型进行实例化**。

2.对于子类而言，除了实现接口外，还可以继承抽象类。若既要继承抽象类，同时还要实现接口的话，使用一下语法格式：

	class 子类 [extends 父类] [implemetns 接口1,接口2,...] {}
	class X extends C implements A,B {}
3.在Java中，一个抽象类只能继承一个抽象类，但一个接口却可以使用extends关键字同时继承多个接口（但接口不能继承抽象类）。
范例：

```Java
interface A{
	public void funA();
}

interface B{
    public void funB();
}

//C接口同时继承了A和B两个接口
interface C extends A,B{//使用的是extends
	public void funC();
}
//接口中定义的变量默认(缺省)为public static final修饰
//接口中定义的方法默认(缺省)为public abstract修饰
```

#### 抽象类和接口的异同

由此可见，从继承关系来说接口的限制比抽象类少： 

(1)一个抽象类只能继承一个抽象父类，而接口可以继承多个接口； 

(2)一个子类只能继承一个抽象类，却可以实现多个接口（在Java中，接口的主要功能是解决单继承局限问题）

设计层面上的区别:

(1）抽象类是对一种事物的抽象，即对类抽象，而接口是对行为的抽象。抽象类是对整个类整体进行抽象，包括属性、行为，但是接口却是对类局部（行为）进行抽象。

(2)设计层面不同，抽象类作为很多子类的父类，它是一种模板式设计。而接口是一种行为规范，它是一种辐射式设计。
***
### 1.7 内部类

> 在Java中，可以将一个类定义在另一个类里面或者一个方法里面，这样的类称为内部类。广泛意义上的内部类一般来说包括这四种：**成员内部类、局部内部类、匿名内部类和静态内部类**。下面就先来了解一下这四种内部类的用法。

匿名内部类创建线程的两种方式：

	public class ThreadDemo {
	 	public static void main(String[] args) {
	     // 继承thread类实现多线程
	     new Thread() {
	         public void run() {
	             for (int x = 0; x < 100; x++) {
	                 System.out.println(Thread.currentThread().getName() + "--"
	                         + x);
	             }
	         }
	     }.start();
	     ;
	 
	     // 实现runnable借口，创建多线程并启动
	     new Thread(new Runnable() {
	         @Override
	         public void run() {
	             for (int x = 0; x < 100; x++) {
	                 System.out.println(Thread.currentThread().getName() + "--"
	                         + x);
	             }
	         }
	     }) {
	     }.start();
		}
	}

**当匿名内部类要使用外部定义的变量时，这个变量必须为final.**
***
### 1.8 Static方法和非Static方法
>Static方法可以访问静态方法和静态变量，不可以访问非静态方法和变量；非Static方法可以访问静态方法和变量；
***
### 1.9 JAVA创建对象的五种方式
- new对象
- Class 类的 newInstance() 方法    --->Person p2 = (Person) Class.forName("com.ys.test.Person").newInstance();
- Constructor 类的 newInstance 方法 --->Person p3 = (Person) Person.class.getConstructors()[0].newInstance();
- 利用 Clone 方法 --->Person p4 = (Person) p3.clone();
- 反序列化
***
### 1.10 final、finally、finalize的区别
1. final修饰符（关键字）
2. finally是在异常处理时提供finally块来执行任何清除操作。
3. finalize是方法名。java技术允许使用finalize（）方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。
### 1.11 JAVA异常
#### 1.11.1 概念
>如果某个方法不能按照正常的途径完成任务，就可以通过另一种路径退出方法。在这种情况下会抛出一个封装了错误信息的对象。此时，这个方法会立刻退出同时不返回任何值。另外，调用这个方法的其他代码也无法继续执行，异常处理机制会将代码执行交给异常处理器。

![](https://images2015.cnblogs.com/blog/1119636/201704/1119636-20170425221211553-683693443.png)

#### 1.11.2 处理机制
>在 Java 应用程序中，异常处理机制为：抛出异常，捕捉异常。throw/throws就是一个甩手掌柜，它只会把异常一层层地往上抛，直到有人去处理它。而try...catch就是那个劳苦工人，负责获取相应的异常并对它进行处理。

#### 1.11.3 throw和throws的区别
1. throws
	- 用在方法声明后面，跟的是异常类名
	- 可以跟多个异常类名，用逗号隔开
	- 表示抛出异常，由该方法的调用者来处理
	- throws表示出现异常的一种可能性，并不一定会发生这些异常
2. throw
	- 用在方法体内，跟的是异常对象名
	- 只能抛出一个异常对象名
	- 表示抛出异常，由方法体内的语句处理
	- throw则是抛出了异常，执行throw则一定抛出了某种异常
#### 1.11.4 try catch finally
>finally总会被执行；常见的问题return，在try和catch代码块中有return时，在执行return之前，会先执行finally代码块，然后在return；finally中改变变量的值，不会影响try和catch的值；
### 1.12 JAVA反射和动态代理
1. 反射
>反射机制是 Java 语言提供的一种基础功能，赋予程序在运行时自省（introspect，官方用语）的能力。通过反射我们可以直接操作类或者对象，比如获取某个对象的类定义，获取类声明的属性和方法，调用方法或者构造对象，甚至可以运行时修改类定义。

2. 动态代理

JDK Proxy 是通过实现 InvocationHandler 接口来实现的，代码如下:

```Java
interface Animal {
    void eat();
}
class Dog implements Animal {
    @Override
    public void eat() {
        System.out.println("The dog is eating");
    }
}
class Cat implements Animal {
    @Override
    public void eat() {
        System.out.println("The cat is eating");
    }
}

// JDK 代理类
class AnimalProxy implements InvocationHandler {
    private Object target; // 代理对象
    public Object getInstance(Object target) {
        this.target = target;
        // 取得代理对象
        return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), this);
    }
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("调用前");
        Object result = method.invoke(target, args); // 方法调用
        System.out.println("调用后");
        return result;
    }
}

public static void main(String[] args) {
    // JDK 动态代理调用
    AnimalProxy proxy = new AnimalProxy();
    Animal dogProxy = (Animal) proxy.getInstance(new Dog());
    dogProxy.eat();
}
```

### 1.13 JAVA注解
#### 概念
>定义：注解（Annotation），也叫元数据。一种代码级别的说明。它是JDK1.5及以后版本引入的一个特性，与类、接口、枚举是在同一个层次。它可以声明在包、类、字段、方法、局部变量、方法参数等的前面，用来对这些元素进行说明，注释。
#### 作用

- 编写文档：通过代码里标识的元数据生成文档【生成文档doc文档】
- 代码分析：通过代码里标识的元数据对代码进行分析【使用反射】
- 编译检查：通过代码里标识的元数据让编译器能够实现基本的编译检查【Override】
### 1.14 JAVA泛型
- 泛型方法
- 泛型接口
- 泛型类
- 泛型通配符
- 泛型上下界
- 类型擦除
### 1.15 JAVA序列化和反序列化
>把对象转换为字节序列的过程称为对象的序列化。把字节序列恢复为对象的过程称为对象的反序列化。

对象的序列化主要有两种用途：

1. 把对象的字节序列永久地保存到硬盘上，通常存放在一个文件中；(web服务器session对象)
2. 在网络上传送对象的字节序列(远程通信)
### 1.16 浅拷贝和深拷贝
- 浅拷贝：创建一个新对象，然后将当前对象的非静态字段复制到该新对象，如果字段是值类型的，那么对该字段执行复制；如果该字段是引用类型的话，则复制引用但不复制引用的对象。因此，原始对象及其副本引用同一个对象。注意：调用对象的 clone 方法，必须要让类实现 Cloneable 接口，并且覆写 clone 方法。
- 深拷贝：创建一个新对象，然后将当前对象的非静态字段复制到该新对象，无论该字段是值类型的还是引用类型，都复制独立的一份。当你修改其中一个对象的任何内容时，都不会影响另一个对象的内容。可以利用序列化实现深拷贝。
### 1.17 Class对象的生成方式
- Class.forName("类名字符串")  （注意：类名字符串必须是全称，包名+类名）
- 类名.class
- 实例对象.getClass()
###  1.18 JAVA基本类型的默认值和取值范围

|         | 默认值    | 存储需求（字节） | 取值范围     | 示例               |
| ------- | --------- | ---------------- | ------------ | ------------------ |
| byte    | 0         | 1                | -2^7—2^7-1   | byte b=10;         |
| short   | 0         | 2                | -2^15—2^15-1 | short s=10;        |
| int     | 0         | 4                | -2^31—2^31-1 | int i=10;          |
| long    | 0         | 8                | -2^63—2^63-1 | long o=10L;        |
| float   | 0.0f      | 4                | IEEE754      | float f=10.0F      |
| double  | 0.0d      | 8                | IEEE754      | double d=10.0;     |
| boolean | false     | 1                | true\false   | boolean flag=true; |
| char    | ‘ \u0000′ | 2                | 0—2^16-1     | char c=’c’ ;       |

### 1.19 泛型通配符

- <? extends T>:上界通配符,此写法的泛型集合，可以使用get接收返回数据,接收类型为上界T;不可使用add方法添加数据。
- <? super T>:下界通配符,此写法的泛型集合,可以使用add添加数据；不宜使用get接收数据，get接收数据均为object。

### 1.20 Java对象初始化顺序

- 父类中static代码块及static变量
- 子类中static代码块及static变量
- 父类普通代码块及字段变量
- 父类构造函数
- 子类普通代码块及字段变量
- 子类构造函数

## 2、JAVA集合

### 2.1 总体框架

1. Java集合是java提供的工具包，包含了常用的数据结构：集合、链表、队列、栈、数组、映射等。 Java集合工具包位置是java.util.* 。
2. Java集合主要可以划分为4个部分：List列表、Set集合、Map映射、工具类(Iterator迭代器、Enumeration枚举类、Arrays和Collections)。
3. Java集合工具包框架图(如下)：

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWFnZXMwLmNuYmxvZ3MuY29tL2Jsb2cvNDk3NjM0LzIwMTMwOS8wODE3MTAyOC1hNWUzNzI3NDFiMTg0MzE1OTFiYjU3N2IxZTFjOTVlNi5qcGc)

### 2.2 总体介绍
1. Collection是一个接口，是高度抽象出来的集合，它包含了集合的基本操作和属性。Collection包含了List和Set两大分支。

  - List是一个有序的队列，每一个元素都有它的索引。第一个元素的索引值是0。List的实现类有LinkedList, ArrayList, Vector, Stack。

 - Set是一个不允许有重复元素的集合。Set的实现类有HastSet和TreeSet。HashSet依赖于HashMap，它实际上是通过HashMap实现的；TreeSet依赖于TreeMap，它实际上是通过TreeMap实现的。

2. Map是一个映射接口，即key-value键值对。Map中的每一个元素包含一个key和key对应的value,每一个key是唯一的。

   - AbstractMap是个抽象类，它实现了Map接口中的大部分API。而HashMap，TreeMap，WeakHashMap都是继承于AbstractMap。
  - Hashtable虽然继承于Dictionary，但它实现了Map接口。

3. 接下来，再看Iterator。它是遍历集合的工具，即我们通常通过Iterator迭代器来遍历集合。我们说Collection依赖于Iterator，是因为Collection的实现类都要实现iterator()函数，返回一个Iterator对象。其中，ListIterator是专门为遍历List而存在的。

4. 再看Enumeration，它是JDK 1.0引入的抽象类。作用和Iterator一样，也是遍历集合；但是Enumeration的功能要比Iterator少。在上面的框图中，Enumeration只能在Hashtable, Vector, Stack中使用。

5. 最后，看Arrays和Collections。它们是操作数组、集合的两个工具类。

 ![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy80MzEwODc5LTI1NDc0ZGVjYzU3ZWNhMDcucG5n)
### 2.3 List
>Java的List是非常常用的数据类型。List是有序的Collection。Java List一共三个实现类：分别是ArrayList、Vector和LinkedList。
#### 2.3.1 ArrayList（数组）
>ArrayList是最常用的List实现类，内部是通过数组实现的，它允许对元素进行快速随机访问。数组的缺点是每个元素之间不能有间隔，当数组大小不满足时需要增加存储能力，就要将已经有数组的数据复制到新的存储空间中。当从ArrayList的中间位置插入或者删除元素时，需要对数组进行复制、移动、代价比较高。因此，它适合随机查找和遍历，不适合插入和删除。
**Arraylist是如何实现动态扩容的**
#### 2.3.2 Vector（数组实现、线程同步）
Vector与ArrayList一样，也是通过数组实现的，不同的是它支持线程的同步，即某一时刻只有一个线程能够写Vector，避免多线程同时写而引起的不一致性，但实现同步需要很高的花费，因此，访问它比访问ArrayList慢。
#### 2.3.3 LinkList（链表）
LinkedList是用链表结构存储数据的，很适合数据的动态插入和删除，随机访问和遍历速度比较慢。另外，他还提供了List接口中没有定义的方法，专门用于操作表头和表尾元素，可以当作堆栈、队列和双向队列使用。
### 2.4 Set
>Set注重独一无二的性质,该体系集合用于存储无序(存入和取出的顺序不一定相同)元素，值不能重复。对象的相等性本质是对象hashCode值（java是依据对象的内存地址计算出的此序号）判断的，如果想要让两个不同的对象视为相等的，就必须覆盖Object的hashCode方法和equals方法。要计算set集合中的元素对象是否相等，就必须重写Object的hashcode方法和equals方法。插入元素对象时先比较hashcode方法，在比较equals方法.

**说明**：        

	1. equals()相等的两个对象他们的hashCode()肯定相等，也就是用equals()对比是绝对可靠的。
	2. hashCode()相等的两个对象他们的equals()不一定相等，也就是hashCode()不是绝对可靠的。
	3. 重写了equals方法，必须重写hashcode方法
	4. set和map接口存储的的对象一般需要重写euqals方法和hashcode方法
#### 2.4.1 HashSet（Hash表）
哈希表边存放的是哈希值。HashSet存储元素的顺序并不是按照存入时的顺序（和List显然不同） 而是按照哈希值来存的所以取数据也是按照哈希值取得。元素的哈希值是通过元素的hashcode方法来获取的, HashSet首先判断两个元素的哈希值，如果哈希值一样，接着会比较equals方法 如果 equls结果为true ，HashSet就视为同一个元素。如果equals 为false就不是同一个元素。

哈希值相同equals为false的元素是怎么存储呢,就是在同样的哈希值下顺延（可以认为哈希值相同的元素放在一个哈希桶中）。也就是哈希一样的存一列。
#### 2.4.2 TreeSet（二叉树）
1. TreeSet()是使用二叉树的原理对新add()的对象按照指定的顺序排序（升序、降序），每增加一个对象都会进行排序，将对象插入的二叉树指定的位置。
2.  Integer和String对象都可以进行默认的TreeSet排序，而自定义类的对象是不可以的，自己定义的类必须实现Comparable接口，并且覆写相应的compareTo()函数，才可以正常使用。 
3.  在覆写compare()函数时，要返回相应的值才能使TreeSet按照一定的规则来排序 
4.  比较此对象与指定对象的顺序。如果该对象小于、等于或大于指定对象，则分别返回负整数、零或正整数。
#### 2.4.3 LinkHashSet（HashSet+LinkedHashMap）
对于LinkedHashSet而言，它继承与HashSet、又基于LinkedHashMap来实现的。LinkedHashSet底层使用LinkedHashMap来保存所有元素，它继承与HashSet，其所有的方法操作上又与HashSet相同，因此LinkedHashSet 的实现上非常简单，只提供了四个构造方法，并通过传递一个标识参数，调用父类的构造器，底层构造一个LinkedHashMap来实现，在相关操作上与父类HashSet的操作相同，直接调用父类HashSet的方法即可。
### 2.5 Map
#### 2.5.1. HashMap（数组+链表+红黑树）
HashMap根据键的hashCode值存储数据，大多数情况下可以直接定位到它的值，因而具有很快的访问速度，但遍历顺序却是不确定的。 HashMap最多只允许一条记录的键为null，允许多条记录的值为null。HashMap非线程安全，即任一时刻可以有多个线程同时写HashMap，可能会导致数据的不一致。如果需要满足线程安全，可以用 Collections的synchronizedMap方法使HashMap具有线程安全的能力，或者使用ConcurrentHashMap。
#### 2.5.2 HashTable（线程安全）
Hashtable是遗留类，很多映射的常用功能与HashMap类似，不同的是它承自Dictionary类，并且是线程安全的，任一时间只有一个线程能写Hashtable，并发性不如ConcurrentHashMap，因为ConcurrentHashMap引入了分段锁。Hashtable不建议在新代码中使用，不需要线程安全的场合可以用HashMap替换，需要线程安全的场合可以用ConcurrentHashMap替换。
#### 2.5.3 TreeMap（可排序）
TreeMap实现SortedMap接口，能够把它保存的记录根据键排序，默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator遍历TreeMap时，得到的记录是排过序的。 如果使用排序的映射，建议使用TreeMap。 在使用TreeMap时，key必须实现Comparable接口或者在构造TreeMap传入自定义的Comparator，否则会在运行时抛出java.lang.ClassCastException类型的异常。
#### 2.5.4 LinkHashMap（记录插入顺序）
LinkedHashMap是HashMap的一个子类，保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的，也可以在构造时带参数，按照访问次序排序。
#### 2.5.5 ConcurrentHashMap
ConcurrentHashMap 和 HashMap 思路是差不多的，但是因为它支持并发操作，所以要复杂一些。整个 ConcurrentHashMap 由一个个 Segment 组成，Segment 代表”部分“或”一段“的意思，所以很多地方都会将其描述为分段锁。注意，行文中，我很多地方用了“槽”来代表一个 segment。简单理解就是，ConcurrentHashMap 是一个 Segment 数组，Segment 通过继承 ReentrantLock 来进行加锁，所以每次需要加锁的操作锁住的是一个 segment，这样只要保证每个 Segment 是线程安全的，也就实现了全局的线程安全。
### 2.6 集合遍历方法总结
#### 2.6.1 Java中提供的遍历方式
1. 传统的for循环遍历，基于计数器的:
>遍历者自己在集合外部维护一个计数器，然后依次读取每一个位置的元素，当读取到最后一个元素后，停止。主要就是需要按元素的位置来读取元素。这也是最原始的集合遍历方法。写法为：

		for (int i = 0; i < list.size(); i++) {
  		  list.get(i);
		}
2. 迭代器遍历，Iterator:
 >Iterator本来是OO的一个设计模式，主要目的就是屏蔽不同数据集合的特点，统一遍历集合的接口。Java作为一个OO语言，自然也在Collections中支持了Iterator模式。写法为：

	Iterator iterator = list.iterator();
		while (iterator.hasNext()) {
 	    iterator.next();
	}

3. foreach循环遍历：
> 屏蔽了显式声明的Iterator和计数器。优点：代码简洁，不易出错。缺点：只能做简单的遍历，不能在遍历过程中操作（删除、替换）数据集合。写法为：

	for (ElementType element : list) {
	}
#### 2.6.2 每个遍历方法的实现原理是什么？


1. 传统的for循环遍历，基于计数器的：
 > 遍历者自己在集合外部维护一个计数器，然后依次读取每一个位置的元素，当读取到最后一个元素后，停止。主要就是需要按元素的位置来读取元素。

2. 迭代器遍历，Iterator：
 >每一个具体实现的数据集合，一般都需要提供相应的Iterator。相比于传统for循环，Iterator取缔了显式的遍历计数器。所以基于顺序存储集合的Iterator可以直接按位置访问数据。而基于链式存储集合的Iterator，正常的实现，都是需要保存当前遍历的位置。然后根据当前位置来向前或者向后移动指针。

3. foreach循环遍历：
>根据反编译的字节码可以发现，foreach内部也是采用了Iterator的方式实现，只不过Java编译器帮我们生成了这些代码。
### 2.6.3 各遍历方式的适用于什么场合？


1、传统的for循环遍历，基于计数器的：
>顺序存储：读取性能比较高。适用于遍历顺序存储集合。
>
>链式存储：时间复杂度太大，不适用于遍历链式存储的集合。

2、迭代器遍历，Iterator：
>顺序存储：如果不是太在意时间，推荐选择此方式，毕竟代码更加简洁，也防止了Off-By-One的问题。
>
>链式存储：意义就重大了，平均时间复杂度降为O(n)，还是挺诱人的，所以推荐此种遍历方式。

3、foreach循环遍历：
>foreach只是让代码更加简洁了，但是他有一些缺点，就是遍历过程中不能操作数据集合（删除等），所以有些场合不使用。而且它本身就是基于Iterator实现的，但是由于类型转换的问题，所以会比直接使用Iterator慢一点，但是还好，时间复杂度都是一样的。所以怎么选择，参考上面两种方式，做一个折中的选择。
### 2.7 各个集合的使用
#### 2.7.1 ArrayList
	import java.lang.String;
	import java.util.ArrayList;
	import java.util.Iterator;
	
	public class HelloWorld {
	public static void main(String[] args) {
	  ArrayList<String> list = new ArrayList<String>();
	  list.add("Hello");
	  list.add("world");
	  Iterator<String> it = list.iterator();
	  while(it.hasNext()){
		  System.out.println(it.next());
	  }
	}
#### 2.7.2 Vector
	import java.lang.String;
	import java.util.Vector;
	import java.util.Iterator;
	import java.util.List;
	
	public class HelloWorld {
		public static void main(String[] args) {
		  List<String> list = new Vector<String>();
	 	 list.add("Hello");
	 	 list.add("world");
	 	 Iterator<String> it = list.iterator();
	 	 while(it.hasNext()){
		 System.out.println(it.next());
	 	 }
		}
	}
#### 2.7.3 LinkedList
	import java.lang.String;
	import java.util.LinkedList;
	import java.util.Iterator;
	import java.util.List;
	
	public class HelloWorld {
		public static void main(String[] args) {
	 	 List<String> list = new LinkedList<String>();
	 	 list.add("Hello");
	 	 list.add("world");
	 	 Iterator<String> it = list.iterator();
	 	 while(it.hasNext()){
		  	System.out.println(it.next());
	  	}
	   }
	}
#### 2.7.4 HashSet
	import java.lang.String;
	import java.util.Iterator;
	import java.util.Set;
	import java.util.HashSet;
	
	public class HelloWorld {
		public static void main(String[] args) {
			Set<String> st = new HashSet<String>();
			st.add("Hello");
			st.add("world");
			Iterator<String> it = st.iterator();
			while(it.hasNext()){
				System.out.println(it.next());
			}
	   }	
	}
#### 2.7.5 TreeSet
	import java.lang.String;
	import java.util.Iterator;
	import java.util.Set;
	import java.util.TreeSet;


	public class HelloWorld {
		public static void main(String[] args) {
			Set<Integer> st = new TreeSet<Integer>();
			st.add(100);
			st.add(600);
			st.add(400);
			st.add(200);
			st.add(300);
			Iterator<Integer> it = st.iterator();
			while(it.hasNext()){
				System.out.println(it.next());
			}
		}
	}
#### 2.7.6 LinkedHashSet
	import java.lang.String;
	import java.util.Iterator;
	import java.util.LinkedHashSet;
	import java.util.Set;


	public class HelloWorld {
		public static void main(String[] args) {
			Set<Integer> st = new LinkedHashSet<Integer>();
			st.add(100);
			st.add(600);
			st.add(400);
			st.add(200);
			st.add(300);
			Iterator<Integer> it = st.iterator();
			while(it.hasNext()){
				System.out.println(it.next());
			}
		}
	}
#### 2.7.7 HashMap
	import java.lang.String;
	import java.util.HashMap;
	import java.util.Iterator;
	import java.util.Map;
	
	public class HelloWorld {
		public static void main(String[] args) {
	        Map<String,String> mmp = new HashMap<String,String>();
	        mmp.put("b", "hello");
	        if(!mmp.containsKey("a")){
	        	mmp.put("a", "world");
	        }
	        mmp.put("b", null);  //会覆盖之前的值
			Iterator<String> it = mmp.keySet().iterator();
			while(it.hasNext()){
				String key = it.next();
				System.out.println(mmp.get(key));
			}
		}
	}
#### 2.7.8 HashTable
	import java.lang.String;
	import java.util.Hashtable;
	import java.util.Iterator;
	import java.util.Map;
	
	public class HelloWorld {
		public static void main(String[] args) {
	        Map<String,String> mmp = new Hashtable<String,String>();
	        mmp.put("b", "hello");
	        mmp.put("a", "world");
			Iterator<Map.Entry<String,String>> it = mmp.entrySet().iterator();
			while(it.hasNext()){
				Map.Entry<String,String> entry = it.next();
				System.out.println(entry.getValue());
			}
		}
	}
#### 2.7.9 TreeMap
	import java.lang.String;
	import java.util.Iterator;
	import java.util.Map;
	import java.util.TreeMap;
	
	public class HelloWorld {
		public static void main(String[] args) {
		    Map<Integer,String> mmp = new TreeMap<Integer,String>();
		    mmp.put(new Integer(10), "hello");
		    mmp.put(new Integer(2), "world");
		    mmp.put(new Integer(5), "world");
		    mmp.put(new Integer(9), "world");
		    mmp.put(new Integer(3), "world");
			for(Entry<Integer, String> entry:mmp.entrySet()){
				System.out.println(entry.getKey());
			}
		}
	}
#### 2.7.10 ConcurrentHashMap

**addThread类**

	public class addThread extends Thread{
	 private Map<Integer,String> mmp;	 
	 addThread(Map<Integer,String> mmp){
			this.mmp = mmp;
	 }	
	 public void run(){
		 while(true){
			 Random random = new Random();
			 mmp.put(random.nextInt(), "Hello world");
		 }
	 }
   }

**removeThread类**

	public class testThread extends Thread {
		private Map<Integer,String> mmp;
	
		testThread(Map<Integer,String> mmp){
			this.mmp = mmp;
		}	
		public void run(){
			try {
				sleep(1000);
			} catch (InterruptedException e1) {
				// TODO Auto-generated catch block
				e1.printStackTrace();
			}
			Iterator<Map.Entry<Integer, String>> it = mmp.entrySet().iterator();
			while(it.hasNext()){
				Map.Entry<Integer, String> entry = it.next();
				System.out.println("remove key:"+entry.getKey());
			    mmp.remove(entry.getKey());
			    try {
					sleep(1000);
				} catch (InterruptedException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
	}

**Main方法**

	public class HelloWorld {
		public static void main(String[] args) {
		     Map<Integer,String> mmp = new ConcurrentHashMap<Integer,String>();
		     
		     Thread threadone = new addThread(mmp);
		     Thread threadtwo = new testThread(mmp);
		     
		     threadone.start();
		     threadtwo.start();
		}
	}
***
## 3、JAVA并发
### 3.1 concurrent包下的工具类
![](C:\Users\lenovo\Desktop\学习总结\img\406312-20160614144055573-1639231351.png)
### 3.2 创建线程的三种方式

1. 继承Thread类创建线程类
2. 通过Runnable接口创建线程类
3. 通过Callable和Future创建线程
***
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
### 3.3 线程池的四种使用方法
> Java里面线程池的顶级接口是Executor，但是严格意义上讲Executor并不是一个线程池，而只是一个执行线程的工具。真正的线程池接口是ExecutorService。下面这张图完整描述了线程池的类体系结构。

![](http://www.blogjava.net/images/blogjava_net/xylz/Windows-Live-Writer/-Java-Concurrency-28--part-1-_1302E/Executor-class_2.png)

- newSingleThreadExecutor：创建一个单线程的线程池。这个线程池只有一个线程在工作，也就是相当于单线程串行执行所有任务。如果这个唯一的线程因为异常结束，那么会有一个新的线程来替代它。此线程池保证所有任务的执行顺序按照任务的提交顺序执行。
- newFixedThreadPool：创建固定大小的线程池。每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。线程池的大小一旦达到最大值就会保持不变，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程。
- newCachedThreadPool：创建一个可缓存的线程池。如果线程池的大小超过了处理任务所需要的线程，那么就会回收部分空闲（60秒不执行任务）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。
- newScheduledThreadPool：创建一个大小无限的线程池。此线程池支持定时以及周期性执行任务的需求。
***
```java
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
```
#### 线程池执行流程图

![](https://img-blog.csdnimg.cn/2018120317293464.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Z6eTYyOTQ0MjQ2Ng==,size_16,color_FFFFFF,t_70)

### 3.4 线程的生命周期
当线程被创建并启动以后，它既不是一启动就进入了执行状态，也不是一直处于执行状态。在线程的生命周期中，它要经过新建(New)、就绪（Runnable）、运行（Running）、阻塞(Blocked)和死亡(Dead)5种状态。尤其是当线程启动以后，它不可能一直"霸占"着CPU独自运行，所以CPU需要在多条线程之间切换，于是线程状态也会多次在运行、阻塞之间切换。

![](https://img2018.cnblogs.com/blog/1436671/201908/1436671-20190805220044649-1708749533.png)
#### 3.4.1 新建状态（NEW）
>当程序使用new关键字创建了一个线程之后，该线程就处于新建状态，此时仅由JVM为其分配内存，并初始化其成员变量的值
#### 3.4.2 就绪状态（RUNNABLE）
>当线程对象调用了start()方法之后，该线程处于就绪状态。Java虚拟机会为其创建方法调用栈和程序计数器，等待调度运行。
#### 3.4.3运行状态（RUNNING）
>如果处于就绪状态的线程获得了CPU，开始执行run()方法的线程执行体，则该线程处于运行状态。 
#### 3.4.4 阻塞状态（BLOCKED）
>阻塞状态是指线程因为某种原因放弃了cpu 使用权，也即让出了cpu timeslice，暂时停止运行。直到线程进入可运行(runnable)状态，才有机会再次获得cpu timeslice 转到运行(running)状态。阻塞的情况分三种：
>
>-  等待阻塞（o.wait->等待对列）： 运行(running)的线程执行o.wait()方法，JVM会把该线程放入等待队列(waitting queue)中。 
>- 同步阻塞(lock->锁池) 运行(running)的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入锁池(lock pool)中。 
>- 其他阻塞(sleep/join) 运行(running)的线程执行Thread.sleep(long ms)或t.join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入可运行(runnable)状态。
#### 3.4.5 线程死亡（DEAD）
>线程会以下面三种方式结束，结束后就是死亡状态。 正常结束 
>
>1. run()或call()方法执行完成，线程正常结束。 异常结束 
>2. 线程抛出一个未捕获的Exception或Error。 调用stop 
>3. 直接调用该线程的stop()方法来结束该线程—该方法通常容易导致死锁，不推荐使用。

### 3.5 终止线程4种方式
1. 正常运行结束
2. 使用退出标志退出线程
3. Interrupt方法结束线程
4. stop方法终止线程（线程不安全）
### 3.6 JAVA锁
1. 乐观锁
2. 悲观锁
3. 自旋锁
4. Synchronized同步锁
5. ReentrantLock
6. AtomicInteger
7. 可重入锁（递归锁）
8. ReadWriteLock读写锁
9. 共享锁和独占锁
10. 重量级锁（Mutex Lock）
11. 轻量级锁
12. 偏向锁
13. 分段锁
14. 公平锁与非公平锁

#### 3.6.1 synchronized关键字
1. Java中每个对象都有一个锁或者称为监视器，当访问某个对象的synchronized方法时，表示将该对象上锁，而不仅仅是为该方法上锁。
2. 这样如果一个对象的synchronized方法被某个线程执行时，其他线程无法访问该对象的任何synchronized方法（但是可以调用其他非synchronized的方法）。直至该synchronized方法执行完。
3. 当调用一个对象的静态synchronized方法时，它锁定的并不是synchronized方法所在的对象，而是synchronized方法所在对象对应的Class对象。这样，其他线程就不能调用该类的其他静态synchronized方法了，但是可以调用非静态的synchronized方法。

总结：**对象锁和对象锁互斥，类锁和类锁互斥，类锁和对象锁互相独立。对象锁是指加了synchornized修饰的非静态方法和synchornized(this)的代码块；类锁是加了synchornized修饰的静态方法。没有加synchornized修饰的就是没有进行同步的代码块，任何线程可以访问**。

##### synchornized锁的特性：
1. Synchronized是非公平锁
2. synchronized是一个重量级操作，需要调用操作系统相关接口，性能是低效的，有可能给线程加锁消耗的时间比有用操作消耗的时间更多
3. 每个对象都有个monitor对象，加锁就是在竞争monitor对象
#### ReentrantLock
	public class testSynchornized implements Runnable {
		private Lock lock = new ReentrantLock();
		private int count = 10;
	
		public void run(){
			System.out.println("ThreadName=" + Thread.currentThread().getName());
			try {
				lock.lock();
				for(int i =0;i<3;i++){
					count--;
					System.out.println(count);
					Thread.sleep(500);
				}
			} catch (Exception e) {
				// TODO: handle exception
			}finally{
				lock.unlock();
			}		
		}
	}
#### synchornized和ReentrantLock区别

1. ReentrantLock显示的获得、释放锁，synchronized隐式获得释放锁 
2. ReentrantLock可响应中断、可轮回，synchronized是不可以响应中断的，为处理锁的不可用性提供了更高的灵活性 
3. ReentrantLock是API级别的，synchronized是JVM级别的 
4. ReentrantLock可以实现公平锁 
5. ReentrantLock通过Condition可以绑定多个条件 
6. 底层实现不一样， synchronized是同步阻塞，使用的是悲观并发策略，lock是同步非阻塞，采用的是乐观并发策略 
7. Lock是一个接口，而synchronized是Java中的关键字，synchronized是内置的语言实现。 
8. synchronized在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；而Lock在发生异常时，如果没有主动通过unLock()去释放锁，则很可能造成死锁现象，因此使用Lock时需要在finally块中释放锁。 
9. Lock可以让等待锁的线程响应中断，而synchronized却不行，使用synchronized时，等待的线程会一直等待下去，不能够响应中断。 
10. 通过Lock可以知道有没有成功获取锁，而synchronized却无法办到。 
11. Lock可以提高多个线程进行读操作的效率，既就是实现读写锁等。

#### Condition类和Object类锁方法区别区别 
1. Condition类的awiat方法和Object类的wait方法等效 
2. Condition类的signal方法和Object类的notify方法等效 
3. Condition类的signalAll方法和Object类的notifyAll方法等效
4. ReentrantLock类可以唤醒指定条件的线程，而object的唤醒是随机的
#### Semaphore信号量
>Semaphore是一种基于计数的信号量。它可以设定一个阈值，基于此，多个线程竞争获取许可信号，做完自己的申请后归还，超过阈值后，线程申请许可信号将会被阻塞。Semaphore可以用来构建一些对象池，资源池之类的，比如数据库连接池

*实现互斥锁（计数器为1）*
>我们也可以创建计数为1的Semaphore，将其作为一种类似互斥锁的机制，这也叫二元信号量，表示两种互斥状态。

	// 创建一个计数阈值为5的信号量对象 
	// 只能5个线程同时访问 
	Semaphore semp = new Semaphore(5);
	try { 
	 // 申请许可
	 semp.acquire(); 
		try { 
		// 业务逻辑
		} catch (Exception e) {
		  } finally {
			// 释放许可 
			semp.release(); 
			} 
	} catch (InterruptedException e){
	  }
#### AtomicInteger
>首先说明，此处AtomicInteger，一个提供原子操作的Integer的类，常见的还有AtomicBoolean、AtomicInteger、AtomicLong、AtomicReference等，他们的实现原理相同，区别在与运算对象类型的不同。令人兴奋地，还可以通过AtomicReference<V>将一个对象的所有操作转化成原子操作。
#### 可重入锁
>线程可以进入任何一个它已经拥有的锁所同步着的代码块。ReentrantLock和synchronized都是可重入锁
#### ReadWriteLock读写锁
>
>- 读锁 如果你的代码只读数据，可以很多人同时读，但不能同时写，那就上读锁 
>- 写锁 如果你的代码修改数据，只能有一个人在写，且不能同时读取，那就上写锁。总之，读的时候上读锁，写的时候上写锁！
>- Java中读写锁有个接口java.util.concurrent.locks.ReadWriteLock，也有具体的实现ReentrantReadWriteLock。
#### 锁的状态
>锁的状态总共有四种：无锁状态、偏向锁、轻量级锁和重量级锁。

- 锁升级
 >随着锁的竞争，锁可以从偏向锁升级到轻量级锁，再升级的重量级锁（但是锁的升级是单向的，也就是说只能从低到高升级，不会出现锁的降级）。

- 轻量级锁
>“轻量级”是相对于使用操作系统互斥量来实现的传统锁而言的。但是，首先需要强调一点的是，轻量级锁并不是用来代替重量级锁的，它的本意是在没有多线程竞争的前提下，减少传统的重量级锁使用产生的性能消耗。在解释轻量级锁的执行过程之前，先明白一点，轻量级锁所适应的场景是线程交替执行同步块的情况，如果存在同一时间访问同一锁的情况，就会导致轻量级锁膨胀为重量级锁。

- 偏向锁
>Hotspot的作者经过以往的研究发现大多数情况下锁不仅不存在多线程竞争，而且总是由同一线程多次获得。偏向锁的目的是在某个线程获得锁之后，消除这个线程锁重入（CAS）的开销，看起来让这个线程得到了偏护。引入偏向锁是为了在无多线程竞争的情况下尽量减少不必要的轻量级锁执行路径，因为轻量级锁的获取及释放依赖多次CAS原子指令，而偏向锁只需要在置换ThreadID的时候依赖一次CAS原子指令（由于一旦出现多线程竞争的情况就必须撤销偏向锁，所以偏向锁的撤销操作的性能损耗必须小于节省下来的CAS原子指令的性能消耗）。上面说过，轻量级锁是为了在线程交替执行同步块时提高性能，而偏向锁则是在只有一个线程执行同步块时进一步提高性能。

- 重量级锁（Mutex Lock）
>Synchronized是通过对象内部的一个叫做监视器锁（monitor）来实现的。但是监视器锁本质又是依赖于底层的操作系统的Mutex Lock来实现的。而操作系统实现线程之间的切换这就需要从用户态转换到核心态，这个成本非常高，状态之间的转换需要相对比较长的时间，这就是为什么Synchronized效率低的原因。因此，这种依赖于操作系统Mutex Lock所实现的锁我们称之为“重量级锁”。JDK中对Synchronized做的种种优化，其核心都是为了减少这种重量级锁的使用。JDK1.6以后，为了减少获得锁和释放锁所带来的性能消耗，提高性能，引入了“轻量级锁”和“偏向锁”。

#### 锁优化

- 减少锁持有时间
- 减小锁粒度
- 锁分离
- 锁粗化
- 锁消除
#### 线程常用方法
1. sleep()：强迫一个线程睡眠Ｎ毫秒。 
2. isAlive()： 判断一个线程是否存活。 
3. join()： 等待线程终止。 
4. activeCount()： 程序中活跃的线程数。 
5. enumerate()： 枚举程序中的线程。 
6. currentThread()： 得到当前线程。 
7. isDaemon()： 一个线程是否为守护线程。 
8. setDaemon()： 设置一个线程为守护线程。(用户线程和守护线程的区别在于，是否等待主线程依赖于主线程结束而结束) 
9. setName()： 为线程设置一个名称。 
10. wait()： 强迫一个线程等待。
11. notify()： 通知一个线程继续运行。 
12. setPriority()： 设置一个线程的优先级。 
13. getPriority():：获得一个线程的优先级。
#### volatile关键字的作用
- 变量可见性
- 禁止重排序
#### ThreadLocal
>ThreadLocal，很多地方叫做线程本地变量，也有些地方叫做线程本地存储，ThreadLocal的作用是提供线程内的局部变量，**这种变量在线程的生命周期内起作用，减少同一个线程内多个函数或者组件之间一些公共变量的传递的复杂度**。

####  CAS
>CAS（Compare-and-Swap），即比较并替换，是一种实现并发算法时常用到的技术，Java并发包中的很多类都使用了CAS技术。但是，CAS存在ABA问题，解决方案是：可以对每个值添加一个版本号来判断，CAS只是一种思想

####  StringBuffer\StringBuilder\String

- 执行速度：StringBuilder>StringBuffer>String
- StringBuffer是线程安全的、StringBuilder是非线程安全的

对于三者的总结：

1. 如果操作少量的数据用String
2. 单线程下操作大量的数据用StringBuilder
3. 多线程下操作大量的数据用StringBuffer
####  HashMap\HashTable\concurrentHashMap
1. 线程安全

>HashMap是线程不安全的；HashTable是线程安全的concurrentHashMap是线程安全的

2. 结构
>concurrentHashMap底层采用分段的数组+链表(红黑树)实现，线程安全；HashTable和HashMap底层采用数组+链表(红黑树)实现

3. 异同
>HashMap可以接受为null的键值(key)和值(value)，而Hashtable则不行
####  ConcurrentHashMap\HashTable\synchronized Map
>三者均可以实现线程安全的HashMap，实现方法不同
####  线程安全的集合类
>Vector\Stack\Hashtable\ConcurrentHashMap\enumeration
***
## 4、JVM
![](https://images0.cnblogs.com/blog/161247/201311/28155725-80e4545786814522a7a3026c13df63b8.png)

### 4.1 JVM内存结构(运行时数据区)
> JVM内存结构:：堆、虚拟机栈、方法区、程序计数器和本地方法栈

- 堆： 对于大多数应用来说，Java堆（Java Heap）是Java虚拟机所管理的内存中最大的一块。Java堆是被所有线程共享的一块内存区域，在虚拟机启动时创建。此内存区域的唯一目的就是存放对象实例，**几乎所有的对象实例都在这里分配内存**。

-  虚拟机栈：Java虚拟机栈（Java Virtual Machine Stacks）是线程私有的，它的生命周期与线程相同。虚拟机栈描述的是Java方法执行的内存模型：每个方法被执行的时候都会同时创建一个栈帧（Stack Frame）用于存储**局部变量表、操作栈、动态链接、方法出口等信息**。每一个方法被调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中从入栈到出栈的过程。 局部变量表存放了编译期可知的各种**基本数据类型对象（boolean、byte、char、short、int、float、long、double）、对象引用**。

- 方法区：方法区（Method Area）与Java堆一样，是各个线程共享的内存区域，**它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据**。虽然Java虚拟机规范把方法区描述为堆的一个逻辑部分，但是它却有一个别名叫做Non-Heap（非堆），目的应该是与Java堆区分开来。

- 程序计数器：此内存区域是唯一一个在Java虚拟机规范中没有规定任何OutOfMemoryError情况的区域。

- 本地方法栈：本地方法栈则是为虚拟机使用到的Native方法服务

**线程共享的内存区域**：堆和方法区；

**线程私有的内存区域**：栈和程序计数器；

### 4.2 Java内存模型(JMM)
#### 4.2.1 主内存与工作内存
>Java内存模型中规定了**所有的变量都存储在主内存中，每条线程还有自己的工作内存**（可以与前面将的处理器的高速缓存类比），线程的工作内存中保存了该线程使用到的变量到主内存副本拷贝，线程对变量的所有操作（读取、赋值）都必须在工作内存中进行，而不能直接读写主内存中的变量。
#### 4.2.2 内存间交互操作
- lock，锁定，所用于主内存变量，它把一个变量标识为一条线程独占的状态。
- unlock，解锁，解锁后的变量才能被其他线程锁定。
- read，读取，所用于主内存变量，它把一个主内存变量的值，读取到工作内存中。
- load，载入，所用于工作内存变量，它把read读取的值，放到工作内存的变量副本中
- use，使用，作用于工作内存变量，它把工作内存变量的值传递给执行引擎，当JVM遇到一个变量读取指令就会执行这个操作
- assign，赋值，作用于工作内存变量，它把一个从执行引擎接收到的值赋值给工作内存变量
- store，存储，作用域工作内存变量，它把工作内存变量值传送到主内存中
- write，写入，作用于主内存变量，它把store从工作内存中得到的变量值写入到主内存变量中

#### 操作规则
- 不允许read和load、store和write操作之一单独出现，即不允许加载或同步工作到一半。
- 不允许一个线程丢弃它最近的assign操作，即变量在工作内存中改变了之后，必须吧改变化同步回主内存
- 不允许一个线程无原因地（无assign操作）把数据从工作内存同步到主内存中
- 一个新的变量只能在主内存中诞生
- 一个变量在同一时刻只允许一条线程对其进行lock操作，但lock操作可以被同一条线程重复执行多次，，多次lock之后必须要执行相同次数的unlock操作，变量才会解锁
- 如果对一个对象进行lock操作，那会清空工作内存变量中的值，在执行引擎使用这个变量前，需要重新执行load或assign操作初始化变量的值
- 如果一个变量事先没有被lock，就不允许对它进行unlock操作，也不允许去unlock一个被其他线程锁住的变量
- 对一个变量执行unlock操作之前，必须将此变量同步回主内存中（执行store、write）
#### 4.2.3 重排序
#### 4.2.4 同步机制
#### 4.2.5 原子性、可见性与有序性
### 4.3 Java中的NIO，BIO，AIO分别是什么
1. BIO
	- 同步并阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进
行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然可以通过线程池机制改善。
    - BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，
JDK1.4以前的唯一选择，但程序直观简单易理解。
2. NIO
    - 同步非阻塞，服务器实现模式为一个请求一个线程，即客户端发送的连接请求都会注册到多路复用器上，多
路复用器轮询到连接有I/O请求时才启动一个线程进行处理。
    - NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。
3. AIO
    - 异步非阻塞，服务器实现模式为一个有效请求一个线程，客户端的I/O请求都是由OS先完成了再通知服务器应用去启动线程进行处理
    -  AIO方式使用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持

### 4.4 JVM如何GC，新生代，老年代，持久代，都存储哪些东西
1. 新生代
	- 在方法中去new一个对象，那这方法调用完毕后，对象就会被回收，这就是一个典型的新生代对象。
2. 老年代
    - 在新生代中经历了N次垃圾回收后仍然存活的对象就会被放到老年代中。而且大对象直接进入老年代
    - 当Survivor空间不够用时，需要依赖于老年代进行分配担保，所以大对象直接进入老年代
3. 永久代(1.8已去除)
    - 即方法区
###  
### 4.5 JVM垃圾处理方法(标记清除、复制、标记整理)
1. 标记-清除算法
   - 标记阶段：先通过根节点，标记所有从根节点开始的对象，未被标记的为垃圾对象
   - 清除阶段：清除所有未被标记的对象
2. 复制算法
   - 将原有的内存空间分为两块，每次只使用其中一块，在垃圾回收时，将正在使用的内存中的存活对象复制到未使用的内存块中，然后清除正在使用的内存块中的所有对象。
3. 标记-整理
   - 标记阶段：先通过根节点，标记所有从根节点开始的可达对象，为被标记的为垃圾对象
   - 整理阶段：将所有的存活对象压缩到内存的一段，之后清理边界所有的空间
### 4.6 各个垃圾收集器是怎么工作的

1. Serial收集器
   - 是一个单线程的收集器，不是只能使用一个CPU。在进行垃圾收集时，必须暂停其他所有的工作线程，直到集结束。
   - 新生代采用复制算法，Stop-The-World
   - 老年代采用标记-整理算法，Stop-The-World
   - 简单高效，Client模式下默认的新生代收集器
2. ParNew收集器
   - ParNew收集器是Serial收集器的多线程版本
   - 新生代采用复制算法，Stop-The-World
   - 老年代采用标记-整理算法，Stop-The-World
   - 它是运行在Server模式下首选新生代收集器
   - 除了Serial收集器之外，只有它能和CMS收集器配合工作
3. ParNew Scanvenge收集器
   - 类似ParNew，但更加关注吞吐量。目标是：达到一个可控制吞吐量的收集器。
   - 停顿时间和吞吐量不可能同时调优。我们一方面希望停顿时间少，另外一方面希望吞吐量高，其实这是矛盾的。因为：在GC的时候，垃圾回收的工作总量是不变的，如果将停顿时间减少，那频率就会提高；既然频率提高了，说明就会频繁的进行GC，那吞吐量就会减少，性能就会降低。
4. G1收集器
   - 是当今收集器发展的最前言成果之一，对垃圾回收进行了划分优先级的操作，这种有优先级的区域回收方式保证了它的高效率
   - 最大的优点是结合了空间整合，不会产生大量的碎片，也降低了进行gc的频率
   - 让使用者明确指定指定停顿时间
5. CMS收集器：（Concurrent Mark Sweep：并发标记清除老年代收集器）
   - 一种以获得最短回收停顿时间为目标的收集器，适用于互联网站或者B/S系统的服务器上
   - 初始标记(Stop-The-World)：根可以直接关联到的对象
   - 并发标记(和用户线程一起)：主要标记过程，标记全部对象
   - 重新标记(Stop-The-World)：由于并发标记时，用户线程依然运行，因此在正式清理前，再做修正
   - 并发清除(和用户线程一起)：基于标记结果，直接清理对象
   - 并发收集，低停顿
### 4.7 类加载器
![](https://img.meiwen.com.cn/i1959357/9c9af3a2f8319bdf.png)
#### 4.7.1 加载器
- 启动（Bootstrap）类加载器：是用本地代码实现的类装入器，它负责将JAVA_HOME\lib下面
的类库加载到内存中（比如rt.jar）。由于引导类加载器涉及到虚拟机本地实现细节，开发者无法直接获取到
启动类加载器的引用，所以不允许直接通过引用进行操作。
JAVA代码编译执行整个过程
- 扩展类加载器(Extension ClassLoader)：负责加载 JAVA_HOME\lib\ext 目录中的，或通过java.ext.dirs系统变量指定路径中的类库。
- 应用程序类加载器(Application ClassLoader)：负责加载用户路径（classpath）上的类库。
#### 4.7.2 双亲委派模型
>如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加
载器去完成，每一个层次的加载器都是如此，因此所有的类加载请求都会传给顶层的启动类加载器，只有当
父加载器反馈自己无法完成该加载请求（该加载器的搜索范围中没有找到对应的类）时，子加载器才会尝试
自己去加载。

### 4.7.3 Java 代码编译和执行的整个过程包含了以下三个重要的机制
- Java 源码编译机制
- 类加载机制
- 类执行机制
### 4.8 JAVA四种引用类型
- 强引用：在Java中最常见的就是强引用，把一个对象赋给一个引用变量，这个引用变量就是一个强引用。当一个对象被强引用变量引用时，它处于可达状态，它是不可能被垃圾回收机制回收的，即使该对象以后永远都不会被用到JVM也不会回收。因此强引用是造成Java内存泄漏的主要原因之一。

- 软引用：软引用需要用SoftReference类来实现，对于只有软引用的对象来说，当系统内存足够时它不会被回收，当系统内存空间不足时它会被回收。软引用通常用在对内存敏感的程序中。
- 弱引用：弱引用需要用WeakReference类来实现，它比软引用的生存期更短，对于只有弱引用的对象来说，只要垃圾回收机制一运行，不管JVM的内存空间是否足够，总会回收该对象占用的内存。
- 虚引用：虚引用需要PhantomReference类来实现，它不能单独使用，必须和引用队列联合使用。虚引用的主要作用是跟踪对象被垃圾回收的状态。
### 4.9 类的初始化顺序
1.初始化父类静态成员变量和静态代码块
2.初始化子类静态成员变量和静态代码块
3.初始化父类的普通成员变量和代码块，在执行父类构造方法
4.初始化子类的普通成员变量和代码块，在执行子类的构造方法


## 5、JAVA IO
### JAVA IO体系结构图
![](https://img-blog.csdn.net/20140814122633546?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTUxMjU5MjE1MQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
### Java IO的用途和特征
>Java IO中包含了许多InputStream、OutputStream、Reader、Writer的子类。这样**设计的原因是让每一个类都负责不同的功能**。这也就是为什么IO包中有这么多不同的类的缘故。各类用途汇总如下：
>
>- 文件访问(file)
- 网络访问
- 内存缓存访问(Buffered)
- 线程内部通信(管道)(pipe)
- 缓冲
- 过滤(filter)
- 解析
- 读写文本 (Readers / Writers)
- 读写基本类型数据 (long, int etc.)
- 读写对象

![](https://camo.githubusercontent.com/cf9eb0cf234b9943966973fa30d226f72727f66a/687474703a2f2f69666576652e636f6d2f77702d636f6e74656e742f75706c6f6164732f323031342f31302f515125453625383825414125453525394225424532303134313032303137343134352e706e67)

## 知识总结

### ArrayList扩容

- 初始化容量为10
- 超过容量时，利用Arrays.coppy 进行扩容，生成一个1.5倍元素的数组（即例子中的15个元素的数组。）
- 返回新的数组对象

### HashMap扩容

- 初始数组初始化容量为16
- 默认情况下，数组大小为16，那么当hashmap中元素个数超过16\*0.75=12的时候，就把数组的大小扩展为2\*16=32，即扩大一倍
- 然后重新计算每个元素在数组中的位置