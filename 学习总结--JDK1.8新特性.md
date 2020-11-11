# JDK1.8新特性

## **函数式接口**

> - 如果一个接口定义个唯一一个抽象方法，那么这个接口就成为函数式接口

### 常见的四大函数式接口

- Consumer 《T》：消费者接口，有参无返回值,consumer.accept(x);
- Supplier 《T》：供给型接口，无参有返回值,supplier.get();
- Function 《T,R》：函数式接口，有参有返回值,function.apply(x);
- Predicate《T》： 断言型接口，有参有返回值，返回值是boolean类型,predicate.test(x);

## **Lambda表达式**

### 简介

> - lambda表达式本质是匿名内部类,也是一段可以传递的代码
> - lambda表达式无法单独出现，需要一个函数式接口来盛放
> - lambda表达式仅适用于接口,不适用于抽象类

### Lmabda表达式的语法总结

| 前置                                       | 语法                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| 无参数无返回值                             | () -> System.out.println(“Hello World”)                      |
| 有一个参数无返回值                         | (x) -> System.out.println(x)                                 |
| 有且只有一个参数无返回值                   | x -> System.out.println(x)                                   |
| 有多个参数，有返回值，有多条lambda体语句   | (x，y) -> {System.out.println(“Hello World”);return x + y;}； |
| 有多个参数，有返回值，只有一条lambda体语句 | (x，y) -> {return x+y;};                                     |

## **方法引用和构造器调用**

### 简介

> - 若lambda体中的内容有方法已经实现了,那么可以使用“方法引用”
>
> - 在lambda表达式中,方法引用是一种简化写法,引用的方法就是Lambda表达式的方法体的实现 

### 方法引用

#### 静态方法引用

```java
//静态方法引用 -> 类名::静态方法名
Comparator<Integer> compare1 = (x,y) -> {return Integer.compare(x, y);};
Comparator<Integer> compare2 = Integer::compare;
```

#### 实例方法引用

```JAVA
//实例方法引用 -> 对象::实例方法
LambdaInterface lambda1 = (x) ->{System.out.println(x);};
LambdaInterface lambda2 = System.out::println;
lambda1.test("Hello World");
lambda2.test("Hello World");

//实例方法引用 -> 类名::实例方法名
//（lambda参数列表中第一个参数是实例方法的调用 者，第二个参数是实例方法的参数时可用）
BiFunction<String, String, Boolean> fun1 = (str1, str2) -> return str1.equals(str2);
BiFunction<String, String, Boolean> fun2 = String::equals;
Boolean result2 = fun2.apply("hello", "world");


//函数式接口
public interface LambdaInterface {
	public void  test(String x);
}
```

### 构造器引用

#### 构造方法引用

```java
//构造方法引用
LambdaInterface lambda1 = (name,age) ->{return new Student(name,age);};
LambdaInterface lambda2 = Student::new;
Student student1 = lambda1.newStudent("张三", 20);
Student student2 = lambda2.newStudent("李四", 21);
System.out.println(student1);
System.out.println(student2);

//函数式接口
public interface LambdaInterface {
	public Student newStudent(String name,int age);
}
```

## **Stream API**

### 简介

> Stream 作为 Java 8 的一大亮点，它与 java.io 包里的 InputStream 和 OutputStream 是完全不同的概念。它也不同于 StAX 对 XML 解析的 Stream，也不是 Amazon Kinesis 对大数据实时处理的 Stream。Java 8 中的 Stream 是对集合（Collection）对象功能的增强，它专注于对集合对象进行各种非常便利、高效的聚合操作（aggregate operation），或者大批量数据操作 (bulk data operation)。

### Stream的操作步骤

#### **创建Stream**

> **数据源** 流的来源:可以是集合，数组，I/O channel， 产生器generator 等。
>
> - **stream()** − 为集合创建串行流。
> - **parallelStream()** − 为集合创建并行流。并行流需要搭配同步容器或并发容器使用！注意线程安全问题

```java
// 1. Arrays 
String [] strArray = new String[] {"a", "b", "c"};
stream = Stream.of(strArray);
stream = Arrays.stream(strArray);
// 2. Collections
List<String> list = Arrays.asList(strArray);
stream = list.stream();
//stream = list.parallelStream();
```

#### **中间操作**(Stream类中间操作方法)

##### 筛选与切片

| 方法                                             | 说明                                                         |
| ------------------------------------------------ | ------------------------------------------------------------ |
| Stream<T> filter(Predicate<? super T> predicate) | 接收Lambda，从流中排除元素。接收的lambda是一个断言型接口，接收一个参数，返回boolean值 |
| Stream<T> distinct()                             | 对流中元素去重, 通过equals函数                               |
| Stream<T> limit(long maxSize)                    | 截取流的前long个元素                                         |
| Stream<T> skip(long n)                           | 丢弃流中的前long个元素                                       |

##### 映射

| 方法                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <R> Stream<R> map(Function<? super T, ? extends R> mapper)   | map 方法用于映射每个元素到对应的结果                         |
| DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper) | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的DoubleStream. |
| IntStream mapToInt(ToIntFunction<? super T> mapper)          | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的IntStream。 |
| LongStream mapToLong(ToLongFunction<? super T> mapper)       | 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的LongStream。 |
| <R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper) | 接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流。 |

##### 排序

| 方法                                               | 说明                                 |
| -------------------------------------------------- | ------------------------------------ |
| Stream<T> sorted()                                 | 对流中元素根据自然顺序进行排序       |
| Stream<T> sorted(Comparator<? super T> comparator) | 根据方法的比较结果对流中元素进行排序 |

#### **终止操作**

##### 查找与匹配

| 方法                                              | 说明                     |
| ------------------------------------------------- | ------------------------ |
| boolean allMatch(Predicate<? super T> predicate)  | 检查是否匹配所有元素     |
| boolean anyMatch(Predicate<? super T> predicate)  | 检查是否至少匹配一个元素 |
| boolean noneMatch(Predicate<? super T> predicate) | 检查是否没有匹配所有元素 |
| Optional<T> findFirst()                           | 返回第一个元素           |
| Optional<T> findAny()                             | 返回当前流中的任意元素   |
| long count()                                      | 返回流中元素总数         |
| Optional<T> max(Comparator<? super T> comparator) | 返回流中最大值           |
| Optional<T> max(Comparator<? super T> comparator) | 返回流中最小值           |
| void forEach(Consumer<? super T> action)          | **内部迭代**             |

##### 归约

| 方法                                                | 说明                                                 |
| --------------------------------------------------- | ---------------------------------------------------- |
| Optional<T> reduce(BinaryOperator<T> accumulator)   | 可以将流中元素反复结合起来，得到一个值。返回Optional |
| T reduce(T identity, BinaryOperator<T> accumulator) | 可以将流中元素反复结合起来，得到一个值               |

##### 收集

| 方法                                                   | 说明                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| <R, A> R collect(Collector<? super T, A, R> collector) | 将流转换为其他形式。接收一个Collector接口的实现，用于给Stream中元素 |

#### **Collector接口**(JDK1.8)

>**Collector接口中方法的实现决定了如何对流执行收集操作(如收集到List、Set、Map)。但是Collectors实用类提供了很多静态方法，可以方便地创建常见收集器实例**

| 方法              | 返回类型             | 作用                                                         |
| ----------------- | -------------------- | ------------------------------------------------------------ |
| toList            | List                 | 把流中元素收集到List：`List emps = list.stream().collect(Collectors.toList());` |
| toSet             | Set                  | 把流中元素收集到Set：`Set emps = list.stream().collect(Collectiors.toSet());` |
| toCollection      | Collection           | 把流中元素收集到创建的集合：`Collection emps = list.stream().collect(Collectors.toCollection(ArrayList::new));` |
| counting          | Long                 | 计算流中元素的个数：`long count = list.stream.collect(Collectors.counting());` |
| summingInt        | Integer              | 对流中元素的整数属性求和：`int total = list.stream().collect(Collectors.summingInt(Employee::getSalary));` |
| averagingInt      | Double               | 计算流中元素Integer属性的平均值：`double avg = list.stream().collect(Collectors.averagingInt(Employee::getSalay));` |
| summarizingInt    | IntSummaryStatistics | 收集流中Integer属性的统计值。如：平均值：`IntSummaryStatiscs iss = list.stream().collect(Collectiors.summarizingInt(Empliyee::getSalary));` |
| joining           | String               | 连接流中每个字符串：`String str = list.stream().map(Employee::getName).collect(Collectors.joining());` |
| maxBy             | Optional             | 根据比较器选择最大值：`Optional max = list.stream().collect(Collectors.maxBy(comparingInt(Employee::getSalay)));` |
| minBy             | Optional             | 根据比较器选择最小值：`Optional min = list.stream().collect(Collectors.minBy(ComparingInt(Employee::getSalary)));` |
| reducing          | 归约产生的类型       | 从一个作为累加器的初始值开始，利用BinaryOperator与流中元素逐个结合，从而归约成单个值。`int total = list.stream.collect(Collectors.reducing(0,Employee::getSalary,Integer::sum));` |
| collectingAndThen | 转换函数返回的类型   | 包裹另一个收集器，对其结果转换函数：`int how=list.stream().collect(Collectors.collectingAndThen(Collectors.toList(),List::size));` |
| groupingBy        | Map<K,List>          | 根据某属性值对流分组，属性为K，结果为V:：`Map> map = list.stream().collect(Collectors.groupingBy(Employee::getStatus));` |
| partitioningBy    | Map<Boolean,List>    | 根据true或false进行分区：Map<Boolean,List<Emp>> vd = list.stream().collect(Collectors.partitioningBy(Employee::getManage)); |

## Stream 求和

### BigDecimal

```java
BigDecimal bb = list.stream().map(Plan::getAmount).reduce(BigDecimal.ZERO,BigDecimal::add);
```

### int、double、long

```Java
double max = list.stream().mapToDouble(User::getHeight).sum();
```



## Optional容器

### 简介

>Optional类是Java8为了解决null值判断问题，借鉴google guava类库的Optional类而引入的一个同名Optional类，使用Optional类可以避免显式的null值判断（null的防御性检查），避免null导致的NPE（NullPointerException）

### 类方法

| 方法                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| static Optional of(T value)                                  | 获取一个值为非null的Optional实例，如果传入的参数为null，则报NullPointerException； |
| static Optional empty()                                      | 创建一个空的Optional 实例                                    |
| static Optional ofNullable(T value)                          | 若value 不为null,创建Optional 实例,否则创建空实例            |
| boolean isPresent()                                          | 判断是否包含值                                               |
| T orElse(T other)                                            | 如果调用对象包含值，返回该值，否则返回other                  |
| T orElseGet(Supplier<? extends T> other)                     | 如果调用对象包含值，返回该值，否则返回Supplier接口产生的返回值 |
| Optional < U> map(Function<? super T, ? extends U> mapper)   | 如果Optional容器中有值，则执行对Function接口的Lambda表达式的逻辑；无值时，Function接口就不会被调用，返回一个空的Optional容器； |
| Optional< U> flatMap(Function< ? super T, Optional< U> > mapper) | flatMap()方法与map()方法类似，所不同的是map()方法的返回值为泛型，而flatMap()返回值必须封装为Optional； |

### 使用实践

```java
public class Solution {
	public static void main(String[] args) {
		//创建optional实例
		Optional<Student> optional = Optional.ofNullable(new Student("lpf",20));
		//判断容器中是否包含值
		if(optional.isPresent()) {
			//获取Optional容器中的值
			Student student0 = optional.orElse(new Student("lpf",20));
			Student student1 = optional.orElseGet(()->{return new Student("lpf",20);});
			//对optional容器中的值进行操作,并返回
			Optional<Student> student2 = optional.map(u->{
				u.setSchoolClass("计算机141班");
				return u;
				});
			Optional<Student> student3 = optional.flatMap(u -> {
				u.setGrade("大学四");
				return Optional.ofNullable(u);
			});
			
			System.out.println(student0);
			System.out.println(student1);
			System.out.println(student2.orElse(null));
			System.out.println(student3.orElse(null));
		}
	}
}

//输出
Student(age=20, name=lpf, grade=大学四, school=null, schoolClass=计算机141班)
Student(age=20, name=lpf, grade=大学四, school=null, schoolClass=计算机141班)
Student(age=20, name=lpf, grade=大学四, school=null, schoolClass=计算机141班)
Student(age=20, name=lpf, grade=大学四, school=null, schoolClass=计算机141班)
```

## **接口中的默认方法和静态方法**

### 默认方法

>Java 8 允许给接口添加一个非抽象的方法实现，只需要使用 **default 关键字**即可，这个特征又叫做***\*扩展方法（也称为\**\**默认方法\**\**或\**\**虚拟扩展方法\**\**或\**\**防护方法\**\**）\****。
>
>**使用注意事项:**
>
>- 实现类中，该默认扩展方法在子类上可以直接使用，它的使用方式类似于抽象类中非抽象成员方法。
>
>- 默认方法**不能够重写** Object 中的方法，却**可以重载**Object 中的方法。
>- toString、equals、 hashCode 不能在接口中被覆盖，却可以被重载
>- **接口默认方法的”类优先”原则** 
>- 选择父类中的方法。如果一个父类提供了具体的实现，那么 接口中具有相同名称和参数的默认方法会被忽略
>- 若一个实现类实现了两个接口，如果一个父接口提供一个默认方法，而另一个父接口也提供了一个具有相同名称和参数列表的方法（不管方法是否是默认方法），那么必须覆盖该方法来解决冲突,否则会报错。

#### JDK中使用示例

> 例如在JDK1.8中，Java集合框架的Collection接口增加了stream()等默认方法，这些默认方法即增强了集合的功能，又能保证对低版本的JDK的兼容。

### 静态方法

```java
//不能被继承
public interface MyInterface1 {

    default void method1() {
        System.out.println("MyInterface1中的默认方法");
    }
    
    public static void say() {
        System.out.println("这是MyInterface1中的静态方法");
    }
}

MyInterface1.say();
```

## **Date API**

- LocalDate：表示日期的
- LocalTime：表示时间的
- LocalDateTime：表示日期时间的

```java
System.out.println(LocalDate.now());
System.out.println(LocalTime.now());
System.out.println(LocalDateTime.now());
//输出
2020-05-15
18:29:33.472
2020-05-15T18:29:33.472
```

## Objects方法新特性

```java
//比较两个对象是否相等（首先比较内存地址，然后比较a.equals(b)，只要符合其中之一返回true）
public static boolean equals(Object a, Object b);

//深度比较两个对象是否相等(首先比较内存地址,相同返回true;如果传入的是数组，则比较数组内的对应下标值是否相同)
public static boolean deepEquals(Object a, Object b);

//返回对象的hashCode，若传入的为null，返回0
public static int hashCode(Object o);

//返回传入可变参数的所有值的hashCode的总和（这里说总和有点牵强，具体参考Arrays.hashCode()方法）
public static int hash(Object... values);

//返回对象的String表示，若传入null，返回null字符串
public static String toString(Object o)

//返回对象的String表示，若传入null，返回默认值nullDefault
public static String toString(Object o, String nullDefault)

//使用指定的比较器c 比较参数a和参数b的大小（相等返回0，a大于b返回整数，a小于b返回负数）
public static <T> int compare(T a, T b, Comparator<? super T> c) 

//如果传入的obj为null抛出NullPointerException,否者返回obj
public static <T> T requireNonNull(T obj) 

//如果传入的obj为null抛出NullPointerException并可以指定错误信息message,否者返回obj
public static <T> T requireNonNull(T obj, String message)

-----------------------------以下是jdk8新增方法---------------------------

//判断传入的obj是否为null，是返回true,否者返回false
public static boolean isNull(Object obj)

//判断传入的obj是否不为null，不为空返回true,为空返回false （和isNull()方法相反）
public static boolean nonNull(Object obj)

//如果传入的obj为null抛出NullPointerException并且使用参数messageSupplier指定错误信息,否者返回obj
public static <T> T requireNonNull(T obj, Supplier<String> messageSupplier)
```

