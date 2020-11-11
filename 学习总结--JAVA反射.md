# JAVA反射

## 获取class对象引用的三种方式

- #### Object类中的getClass()方法，object.getClass();

- #### 通过对象静态属性 .class来获取对应的Class对象（类字面常量）

- **Class.forName("")**

```java
import cn.easycode.mycode.po.Student;
public class Solution {
	public static void main(String[] args) {
		Student stu = new Student();
		Class<?> stuClass1 = stu.getClass();
		Class<?> stuClass2 = Student.class;
		try {
			Class<?> stuClass3 = Class.forName("cn.easycode.mycode.po.Student");
			System.out.println(stuClass1.getName());
			System.out.println(stuClass2.getName());
			System.out.println(stuClass3.getName());
		} catch (ClassNotFoundException e) {
			System.out.println("ClassNotFoundException");
		}
	}
}
```

## 创建对象的五种方式

#### 使用new关键字

```java
Student student = new Student();
```

#### 使用Class类的newInstance方法

```java
Class<?> studentClass = Class.forName("cn.easycode.mycode.po.Student");
Student student = (Student) studentClass.newInstance();
```

#### 使用Constructor类的newInstance方法

```java
Class<?> studentClass = Class.forName("cn.easycode.mycode.po.Student");
Student student = (Student) studentClass.getConstructor(String.class,int.class).newInstance("张三",20);
```

#### 使用clone方法

```java
@Data
public class Student implements Serializable,Cloneable{
	private static final long serialVersionUID = -1377794110170113031L;
	
	private int age;
	private String name;
	private String grade;
	private String school;
	private String schoolClass;
	
	public Student() {}
	
	public Student(String name,int age) {
		this.age = age;
		this.name = name;
	}

	@Override
	public Student clone() throws CloneNotSupportedException {
		// TODO Auto-generated method stub
		return (Student)super.clone();
	}
}
```

```java
public class Solution {
	public static void main(String[] args) {
		try {
			Class<?> studentClass = Class.forName("cn.easycode.mycode.po.Student");
			Student student = (Student) studentClass.getConstructor(String.class, int.class).newInstance("张三", 20);
			Student clone = student.clone();
			System.out.println(clone);
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}

Student(age=20, name=张三, grade=null, school=null, schoolClass=null)
```

#####　使用反序列化

```java
	@Test
	public void run() {
		try {
			serializeObject();
			Student student = (Student)deserializeObject();
			System.out.println("反序列化对象:" + student.toString());
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} 
	}
//具体可参考本文对象的序列化和反序列化章节
```



## Class类的方法

| 返回值                     | 方法名                                                       | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| static Class<?>            | forName()                                                    | 获取Class对象的一个引用                                      |
| String                     | getName()                                                    | 取全限定的类名(包括包名)，即类的完整名字。                   |
| String                     | getSimpleName()                                              | 获取类名(不包括包名)                                         |
| String                     | getCanonicalName()                                           | 获取全限定的类名(包括包名)                                   |
| AnnotatedType[]            | getAnnotatedInterfaces()                                     | 返回注解的AnnotatedType                                      |
| AnnotatedType              | getAnnotatedSuperclass()                                     | 返回父类的注解的AnnotatedType                                |
| Annotation[]               | getAnnotations()                                             | 返回此元素上存在的所有注解。                                 |
| <A extends Annotation> A   | getAnnotation(Class<A> annotationClass)                      | 如果存在该元素的指定类型的注解，则返回这些注解，否则返回 null。 |
| <A extends Annotation> A[] | getAnnotationsByType(Class<A> annotationClass)               | 返回一个该类型的注解数组                                     |
| Annotation[]               | getDeclaredAnnotations()                                     | 返回直接存在于此元素上的所有注解。                           |
| <A extends Annotation> A   | getDeclaredAnnotation(Class<A> annotationClass)              | 返回直接存在于此元素上的该类型注解。                         |
| <A extends Annotation> A[] | getDeclaredAnnotationsByType(Class<A> annotationClass)       | 返回直接存在于此元素上的该类型的注解数组。                   |
| ClassLoader                | getClassLoader()                                             | 返回该类的类加载器。                                         |
| Field                      | getField(String name)                                        | 返回一个 Field 对象，它反映此 Class 对象所表示的类或接口的指定公共成员字段。（public） |
| Field[]                    | getFields()                                                  | 返回一个包含某些 Field 对象的数组，这些对象反映此 Class 对象所表示的类或接口的所有可访问公共字段。 |
| Field                      | getDeclaredField(String name)                                | 返回一个 Field 对象，该对象反映此 Class 对象所表示的类或接口的指定已声明字段。（private也可以访问到） |
| Field[]                    | getDeclaredFields()                                          | 返回 Field 对象的一个数组，这些对象反映此 Class 对象所表示的类或接口所声明的所有字段。（private也可以访问到） |
| Method                     | getMethod(String name, Class<?>... parameterTypes)           | 返回一个 Method 对象，它反映此 Class 对象所表示的类或接口的指定公共成员方法。 |
| Method[]                   | getMethods()                                                 | 返回一个包含某些 Method 对象的数组，这些对象反映此 Class 对象所表示的类或接口（包括那些由该类或接口声明的以及从超类和超接口继承的那些的类或接口）的公共 member 方法。 |
| Method                     | getDeclaredMethod(String name, Class<?>... parameterTypes)   | 返回一个 Method 对象，该对象反映此 Class 对象所表示的类或接口的指定已声明方法。（private也可以访问到） |
| Method[]                   | getDeclaredMethods()                                         | 返回 Method 对象的一个数组，这些对象反映此 Class 对象表示的类或接口声明的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法。（private也可以访问到） |
| Method                     | getEnclosingMethod()                                         | 如果此 Class 对象表示某一方法中的一个本地或匿名类，则返回 Method 对象，它表示底层类的立即封闭方法。 |
| Constructor<T>             | getConstructor(Class<?>... parameterTypes)                   | 返回一个 Constructor 对象，它反映此 Class 对象所表示的类的指定公共构造方法。 |
| Constructor<?>[]           | getConstructors()                                            | 返回一个包含某些 Constructor 对象的数组，这些对象反映此 Class 对象所表示的类的所有公共构造方法。 |
| Constructor<T>             | getDeclaredConstructor(Class<?>... parameterTypes)           | 根据指定参数类型顺序返回一个 Constructor 对象，该对象反映此 Class 对象所表示的类或接口的指定构造方法。可获取非公共构造方法。（private也可以访问到） |
| Constructor<?>[]           | getDeclaredConstructors()                                    | 返回 Constructor 对象的一个数组，这些对象反映此 Class 对象表示的类声明的所有构造方法。不仅是公共的。（private也可以访问到） |
| Constructor<?>             | getEnclosingConstructor()                                    | 如果该 Class 对象表示构造方法中的一个本地或匿名类，则返回 Constructor 对象，它表示底层类的立即封闭构造方法。 |
| Type[]                     | getGenericInterfaces()                                       | 返回表示某些接口的 Type，这些接口由此对象所表示的类或接口直接实现。 |
| Class<?>[]                 | getInterfaces()                                              | 确定此对象所表示的类或接口实现的接口。                       |
| Class<? super T>           | getSuperclass()                                              | 返回表示此 Class 所表示的实体（类、接口、基本类型或 void）的超类的 Class。 |
| Package                    | getPackage()                                                 | 获取此类的包。                                               |
| URL                        | getResource(String name)                                     | 查找带有给定名称的资源。                                     |
| InputStream                | getResourceAsStream(String name)                             | 查找具有给定名称的资源。                                     |
| String                     | getTypeName()                                                | 返回该类型的名称                                             |
| boolean                    | isAnnotation()                                               | 如果此 Class 对象表示一个注解类型则返回 true。               |
| boolean                    | isAnnotationPresent(Class<? extends Annotation> annotationClass) | 如果指定类型的注解存在于此元素上，则返回 true，否则返回 false。 |
| boolean                    | isAnonymousClass()                                           | 当且仅当底层类是匿名类时返回 true。                          |
| boolean                    | isArray()                                                    | 判定此 Class 对象是否表示一个数组类。                        |
| boolean                    | isInstance(Object obj)                                       | 判定此 Class 对象所表示的类或接口与指定的 Class 参数所表示的类或接口是否相同，或是否是其超类或超接口。 |
| boolean                    | isInterface()                                                | 判定指定的 Class 对象是否表示一个接口类型。                  |
| boolean                    | isLocalClass()                                               | 当且仅当底层类是本地类时返回 true。                          |
| boolean                    | isMemberClass()                                              | 当且仅当底层类是成员类时返回 true。                          |
| boolean                    | isPrimitive()                                                | 判定指定的 Class 对象是否表示一个基本类型。                  |
| T                          | newInstance()                                                | 如果此类是复合类，则返回 true，否则 false。有匿名类部类的类可以称作为复合类。 |

## java.lang.reflect类

### java.lang.reflect.AccessibleObject

#### 简介

> **java.lang.reflect.AccessibleObject** 类是**Field，Method和Constructor对象**的基类.它提供了将反射对象标记为在使用时禁止默认Java语言访问控制检查的功能.当使用Fields，Methods或Constructors设置或获取字段，调用方法或创建和初始化类的新实例时，将执行对公共，默认(包)访问，受保护和私有成员的访问检查.在反射对象中设置可访问标志允许具有足够权限的复杂应用程序(例如Java对象序列化或其他持久性机制)以通常被禁止的方式操作对象.

#### 类方法

| 返回值                   | 方法名                                                       | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| <T extends Annotation> T | getAnnotation(Class<T> annotationClass)                      | 如果存在这样的注解，则返回该元素的指定类型的注解，否则为null. |
| Annotation []            | getAnnotations()                                             | 返回此元素上的所有注解.                                      |
| Annotation []            | getDeclaredAnnotations()                                     | 返回此元素上的所有注解.                                      |
| boolean                  | isAccessible()                                               | 获取此对象的可访问标志的值.                                  |
| boolean                  | isAnnotationPresent(Class<? extends Annotation> annotationClass) | 如果此元素上存在指定类型的注解，则返回true，否则返回false.   |
| static void              | setAccessible(AccessibleObject[] array, boolean flag)        | 通过单一安全检查为效率设置对象数组的可访问标志的便捷方法.    |
| void                     | setAccessible(boolean flag)                                  | 将此对象的可访问标志设置为指示的布尔值.                      |

### java.lang.reflect.Field

#### 简介

> **java.lang.reflect.Field** 类提供有关类或接口的单个字段的信息和动态访问.反射字段可以是类(静态)字段或实例字段.字段允许在获取或设置访问操作期间进行扩展转换，但如果发生缩小转换，则抛出IllegalArgumentException.

#### 类方法

| 返回值                    | 方法名                                   | 说明                                                         |
| ------------------------- | ---------------------------------------- | ------------------------------------------------------------ |
| < T extends Annotation> T | getAnnotation(Class< T> annotationClass) | 如果存在这样的注解，则返回该元素的指定类型的注释，否则为null. |
| Annotation []             | getDeclaredAnnotations()                 | 返回所有注解直接出现在这个元素上.                            |
| Class<？>                 | getDeclaringClass()                      | 返回表示声明由此Field对象所处的类或接口的Class对象.          |
| Type                      | getGenericType()                         | 返回类型对象ct，表示由此Field对象表示的字段的声明类型.       |
| Class<?>                  | getType()                                | 返回一个Class对象，该对象标识由此Field对象表示的字段的声明类型. |
| String                    | getName()                                | 选择此Field对象表示的字段的名称.                             |
| boolean                   | isEnumConstant()                         | 返回如果此字段表示枚举类型的元素，则返回true;否则返回false.否则返回false. |
| boolean                   | isSynthetic()                            | 将指定对象参数上此Field对象表示的字段设置为指定的新价值.     |
| Object                    | get(Object obj)                          | 返回指定对象上此Field表示的字段的值.                         |
| boolean                   | getBoolean(Object obj)                   | 获取静态或实例布尔字段的值.                                  |
| void                      | setBoolean(Object obj，boolean z)        | 将字段的值设置为指定的布尔值对象.                            |
| char                      | getChar(Object obj)                      | 获取char类型的静态或实例字段的值，或通过扩展转换获得可转换为char类型的另一种基本类型的值. |
| void                      | setChar(Object obj，char c)              | 将字段的值设置为指定对象上的char.                            |
| byte                      | getByte(Object obj)                      | 获取静态或实例字节字段的值.                                  |
| void                      | setByte(Object obj，byte b)              | 将字段的值设置为指定对象上的字节.                            |
| short                     | getShort (Object obj)                    | 获取short或其他基本类型的静态或实例字段的值，可通过扩展转换转换为short类型. |
| void                      | setShort(Object obj，short s)            | 将字段的值设置为指定对象的short.                             |
| int                       | getInt(Object obj)                       | 获取值一个int或另一个基本类型的静态或实例字段，可以通过扩展转换转换为int类型. |
| void                      | setInt(Object obj，int i)                | 将字段的值设置为指定对象上的int.                             |
| long                      | getLong(Object obj)                      | 获取long或其他基本类型的静态或实例字段的值，可通过扩展转换转换为long类型. |
| void                      | setLong(Object obj，long l)              | 将字段的值设置为指定对象上的long.                            |
| float                     | getFloat(Object obj)                     | 获取float或float类型的静态或实例字段的值，该字段可通过扩展转换转换为float类型 |
| void                      | setFloat(Object obj，float f)            | 将字段的值设置为指定对象上的float.                           |
| double                    | getDouble (Object obj)                   | 获取double类型或另一个可通过扩展转换转换为double类型的基本类型的静态或实例字段的值. |
| void                      | setDouble(Object obj，double d)          | 将字段的值设置为指定对象的double.                            |
| int                       | getModifiers()                           | 以整数形式返回此Field对象表示的字段的Java语言修饰符.         |

### java.lang.reflect.Method

#### 简介

> **java.lang.reflect.Method** 类提供有关类或接口上单个方法的信息和访问权限.反射的方法可以是类方法或实例方法(包括抽象方法).方法允许在将实际参数与基础方法的形式参数进行匹配时进行扩展转换，但如果发生缩小转换，则会抛出IllegalArgumentException.

#### 类方法

| 返回值                    | 方法名                                   | 说明                                                         |
| ------------------------- | ---------------------------------------- | ------------------------------------------------------------ |
| < T extends Annotation> T | getAnnotation(Class< T> annotationClass) | 如果存在这样的注解，则返回该元素的指定类型的注释，否则为null. |
| Annotation []             | getDeclaredAnnotations()                 | 返回所有注解直接出现在这个元素上.                            |
| Class<？>                 | getDeclaringClass()                      | 返回表示声明由此Method对象表示的方法的类的Class对象.         |
| Class<？> []              | getExceptionTypes()                      | 返回一个Class对象数组，表示声明由此Constructor对象表示的底层构造函数抛出的异常类型. |
| Type []                   | getGenericExceptionTypes()               | 返回一个数组Type对象，表示声明由此Constructor对象抛出的异常. |
| Type[]                    | getGenericParameterTypes()               | 返回Type对象的数组，以声明顺序表示此Constructor对象表示的方法的形式参数类型. |
| Type                      | getGenericReturnType()                   | 返回一个Type对象，该对象表示此Method对象表示的方法的正式返回类型. |
| Annotation [] []          | getParameterAnnotations()                | 以声明顺序返回表示形式参数注释的二维数组，此Method对象表示的方法. |
| Class<？> []              | getParameterTypes()                      | 返回一个数组表示此Method对象所表示的构造函数的声明顺序的形式参数类型的类对象. |
| Class<？>                 | getReturnType()                          | 返回一个Class对象，该对象表示此Method对象表示的方法的正式返回类型. |
| Object                    | invoke(Object obj, Object... args)       | 在具有指定参数的指定对象上调用此Method对象表示的基础方法.    |
| boolean                   | isBridge()                               | 如果此方法是桥接方法，则返回true;否则返回false.              |
| boolean                   | isSynthetic()                            | 如果此方法是合成方法，则返回true;否则返回false.              |
| boolean                   | isVarArgs()                              | 如果声明此方法采用可变数量的参数，则返回true;否则返回false.  |
| Object                    | getDefaultValue()                        | 返回此Method实例表示的注释成员的默认值.                      |
| String                    | getName()                                | 以字符串形式返回此方法的名称.                                |
| int                       | getModifiers()                           | 返回由表示的方法的Java语言修饰符这个Method对象，作为一个整数. |

### java.lang.reflect.Constructor

#### 简介

>**java.lang.reflect.Constructor** 类提供有关类的单个构造函数的信息和访问权限.构造函数允许在将实际参数与newInstance()与底层构造函数的形式参数匹配时进行扩展转换，但如果发生缩小转换，则抛出IllegalArgumentException.

#### 类方法

| 返回值                    | 方法名                                   | 说明                                                         |
| ------------------------- | ---------------------------------------- | ------------------------------------------------------------ |
| < T extends Annotation> T | getAnnotation(Class< T> annotationClass) | 如果存在这样的注解，则返回该元素的指定类型的注释，否则为null. |
| Annotation []             | getDeclaredAnnotations()                 | 返回所有注解直接出现在这个元素上.                            |
| Class<？>                 | getDeclaringClass()                      | 返回表示声明由此Method对象表示的方法的类的Class对象.         |
| Class<？> []              | getExceptionTypes()                      | 返回一个Class对象数组，表示声明由此Constructor对象表示的底层构造函数抛出的异常类型. |
| Type []                   | getGenericExceptionTypes()               | 返回一个数组Type对象，表示声明由此Constructor对象抛出的异常. |
| Type[]                    | getGenericParameterTypes()               | 返回Type对象的数组，以声明顺序表示此Constructor对象表示的方法的形式参数类型. |
| Annotation [] []          | getParameterAnnotations()                | 以声明顺序返回表示形式参数注释的二维数组，此Method对象表示的方法. |
| Class<？> []              | getParameterTypes()                      | 返回一个数组表示此Method对象所表示的构造函数的声明顺序的形式参数类型的类对象. |
| String                    | getName()                                | 以字符串形式返回此方法的名称.                                |
| boolean                   | isSynthetic()                            | 如果这样，则返回true构造函数是一个合成构造函数;否则返回false. |
| boolean                   | isVarArgs()                              | 如果声明此构造函数采用可变数量的参数，则返回true;否则返回false. |
| T                         | newInstance(Object ... initargs)         | 使用此Constructor对象表示的构造函数来创建和使用指定的初始化参数初始化构造函数声明类的新实例. |
| int                       | getModifiers()                           | 以整数形式返回此Constructor对象表示的构造函数的Java语言修饰符. |

### java.lang.reflect.Array

#### 简介

> **java.lang.reflect.Array** 类提供动态创建和访问Java数组的静态方法.数组允许在get或set操作期间进行扩展转换，但如果发生收缩转换，则抛出IllegalArgumentException.

#### 类方法

| 返回值         | 方法名                                                   | 说明                                                |
| -------------- | -------------------------------------------------------- | --------------------------------------------------- |
| static int     | getLength(Object array)                                  | 以int形式返回指定数组对象的长度.                    |
| static Object  | get(Object array, int index)                             | 返回指定数组对象中索引组件的值                      |
| static boolean | getBoolean(Object array，int index)                      | 以布尔值的形式返回指定数组对象中索引组件的值        |
| static byte    | getByte(Object array，int index)                         | 返回指定数组对象中索引组件的值                      |
| static char    | getChar(Object array，int index)                         | 以char形式返回指定数组对象中索引组件的值.           |
| static double  | getDouble(Object array，int index)                       | 以double形式返回指定数组对象中索引组件的值          |
| static float   | getFloat(Object array，int index)                        | 以float形式返回指定数组对象中索引组件的值.          |
| static int     | getInt(Object array，int index)                          | 以int形式返回指定数组对象中索引组件的值.            |
| static long    | getLong(Object array，int index)                         | 返回指定数组对象中索引组件的值，为long.             |
| static short   | getShort(Object array，int index)                        | 返回指定数组对象中索引组件的值，作为short.          |
| static Object  | newInstance(Class<？> componentType，int ... dimensions) | 创建一个具有指定组件类型和尺寸的新数组.             |
| static Object  | newInstance(类<？> componentType，int length)            | 创建具有指定组件类型和长度的新数组.                 |
| static void    | set(Object array，int index，Object value)               | 将指定数组对象的索引组件的值设置为指定的新值.       |
| static void    | setBoolean(Object array，int index，boolean z)           | 将指定数组对象的索引组件的值设置为th e指定的布尔值. |
| static void    | setByte(Object array，int index，byte b)                 | 设置索引的值指定数组对象的组件到指定的字节值.       |
| static void    | setChar(Object array，int index，char c)                 | 将指定数组对象的索引组件的值设置为指定的char值.     |
| static void    | setDouble(Object array，int index，double d)             | 将指定数组对象的索引组件的值设置为指定的double值.   |
| static void    | setFloat(Object array，int index，float f)               | 将指定数组对象的索引组件的值设置为指定的浮点值.     |
| static void    | setInt(Object array，int index，int i)                   | 将指定数组对象的索引组件的值设置为指定的int值.      |
| static void    | setLong(Object array ，int index，long l)                | 将指定数组对象的索引组件的值设置为指定的long值.     |
| static void    | setShort(Object array，int index，short s)               | 将指定数组对象的索引组件的值设置为指定的short值.    |

### java.lang.reflect.Modifier

#### 简介

>**java.lang.reflect.Modifier** 类提供静态方法和常量来解码类和成员访问修饰符.修饰符集表示为具有表示不同修饰符的不同位位置的整数.表示修饰符的常量值取自Java虚拟机规范的4.1,4.4,4.5和4.7节中的表.

#### Fields

- **static int FINAL** : 表示最终修饰符的int值.
- **static int INTERFACE** : 表示接口修饰符的int值.
- **static int NATIVE** : 表示本机修饰符的int值.
- **static int PRIVATE** : 表示私有修饰符的int值.
- **static int PROTECTED** : 表示受保护修饰符的int值.
- **static int PUBLIC** : 表示公共修饰符的int值.
- **static int STATIC** : 表示静态修饰符的int值.
- **static int STRICT** : 表示strictfp修饰符的int值.
- **static int SYNCHRONIZED** : 表示同步修饰符的int值.
- **static int TRANSIENT** : 表示瞬态修饰符的int值.
- **static int VOLATILE** : 表示volatile修饰符的int值.

#### 类方法

| 返回值                   | 方法名                                                       | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| static InvocationHandler | getInvocationHandler(Object proxy)                           | 返回指定代理实例的调用处理程序.                              |
| static Class<？>         | getProxyClass(ClassLoader loader，Class<？> ... interfaces)  | 给定类加载器和接口数组，返回代理类的java.lang.Class对象.     |
| static boolean           | isProxyClass(Class<？> cl)                                   | 当且仅当指定的类是动态生成的时候才返回true使用getProxyClass方法或newProxyInstance方法的代理类. |
| static Object            | newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h) | 返回指定接口的代理类的实例，该接口将方法调用分派给指定的调用处理程序. |



| 返回值         | 方法名                  | 说明                                                         |
| -------------- | ----------------------- | ------------------------------------------------------------ |
| static int     | classModifiers()        | 返回一个int值，或者将可以应用于类的源语言修饰符组合在一起.   |
| static int     | constructorModifiers()  | 返回一个int值，或者将可以应用于构造函数的源语言修饰符组合在一起. |
| static int     | fieldModifiers()        | 返回一个int值，或者将可以应用于字段的源语言修饰符组合在一起. |
| static int     | interfaceModifiers()    | 返回一个int值或者可以应用于接口的源语言修饰符.               |
| static boolean | isAbstract(int mod)     | 如果整数参数包含abstract修饰符，则返回true，否则返回false.   |
| static boolean | isFinal(int mod)        | 如果整数参数包含final修饰符，则返回true，否则返回false.      |
| static boolean | isInterface(int mod)    | 如果整数参数包含接口修饰符，则返回true ，否则为假.           |
| static boolean | isNative(int mod)       | 如果整数参数包含native修饰符，则返回true，否则返回false .    |
| static boolean | isPrivate(int mod)      | 如果整数参数包含private修饰符，则返回true，否则返回false.    |
| static boolean | isProtected(int mod)    | 如果整数参数包含protected修饰符，则返回true，否则返回false.  |
| static boolean | isPublic(int mod)       | 如果整数参数包含public修饰符，则返回true，否则返回false.     |
| static boolean | isStatic( int mod)      | 如果整数参数包含静态修饰符，则返回true，否则返回false.       |
| static boolean | isStrict(int mod)       | 如果整数参数包含strictfp修饰符，则返回true，否则返回false.   |
| static boolean | isSynchronized(int mod) | 如果整数参数包含synchronized修饰符，则返回true，否则返回false. |
| static boolean | isTransient(int mod)    | 如果整数参数包含transient修饰符，则返回true，否则返回false.  |
| static boolean | isVolatile(int mod)     | 如果整数参数包含volatile修饰符，则返回true，否则返回false.   |
| static int     | methodModifiers()       | 返回一个int值或者将可以应用于方法的源语言修饰符组合在一起.   |

### java.lang.reflect.Proxy

#### 简介

> **java.lang.reflect.Proxy** 类提供了用于创建动态代理类和实例的静态方法，它也是所有类的超类由这些方法创建的动态代理类.

#### 类方法

| 返回值                   | 方法名                                                       | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| static InvocationHandler | getInvocationHandler(Object proxy)                           | 返回指定代理实例的调用处理程序.                              |
| static Class<？>         | getProxyClass(ClassLoader loader，Class<？> ... interfaces)  | 给定类加载器和接口数组，返回代理类的java.lang.Class对象.     |
| static boolean           | isProxyClass(Class<？> cl)                                   | 当且仅当指定的类是动态生成的时候才返回true使用getProxyClass方法或newProxyInstance方法的代理类. |
| static Object            | newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h) | 返回指定接口的代理类的实例，该接口将方法调用分派给指定的调用处理程序. |

### **java.lang.reflect接口**

#### 简介

> **java.lang.reflect接口**包含用于获取有关类和对象的反射信息的接口.

#### 接口摘要

| **接口**                                        | **说明**                                                     |
| ----------------------------------------------- | ------------------------------------------------------------ |
| **AnnotatedElement**                            | 表示当前在此VM中运行的程序的带注释元素.                      |
| **GenericArrayType**                            | GenericArrayType代表数组类型，其组件类型是参数化类型或类型变量. |
| **GenericDeclaration**                          | 声明类型变量的所有实体的通用接口.                            |
| **InvocationHandler**                           | InvocationHandler是由代理实例的调用处理程序实现的接口.       |
| **Member**                                      | 会员是反映单个成员(字段或方法)或构造函数的标识信息的接口.    |
| **ParameterizedType**                           | ParameterizedType表示参数化类型，例如Collection< String>.    |
| **Type**                                        | 类型是Java编程语言中所有类型的公共超接口.                    |
| **List< E>**                                    | 这是一个有序集合(也称为序列).                                |
| **TypeVariable< D extends GenericDeclaration>** | TypeVariable是种类变量的通用超接口.                          |
| **WildcardType**                                | WildcardType表示通配符类型表达式，例如？，？扩展数字，或？超级整数. |

### **java.lang.reflect异常**

#### 简介

> **java.lang.reflect Exceptions** 包含在反射操作期间可能发生的异常.

#### 异常摘要

| 异常                                    | 说明                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| **InvocationTargetException**           | InvocationTargetException是一个已检查的异常，它包装被调用的方法或构造函数抛出的异常. |
| **MalformedParameterizedTypeException** | 当需要实例化的反射方法遇到语义错误的参数化类型时抛出.        |
| **UndeclaredThrowableException**        | 由代理实例上的方法调用引发它的调用处理程序的invoke方法抛出一个已检查的异常(一个不能分配给RuntimeException或Error的Throwable)，该异常不能分配给在代理实例上调用并分派给该方法的方法的throws子句中声明的任何异常类型.调用处理程序. |

### **java.lang.reflect错误**

#### 简介

> **java.lang.reflect错误**包含反射操作期间可能发生的错误.

#### 错误摘要

| **错误**                        | **说明**                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| **GenericSignatureFormatError** | 当需要解释类型，方法或构造函数的通用签名信息的反射方法遇到语法错误的签名属性时抛出. |

## 动态代理

#### 被代理类和接口

```java
//接口
public interface ProxyInterface {
	int add();
	void show();
}
//实现
public class ProxyInterfaceImpl implements ProxyInterface {
    private static int count = 1;
    
	public int add() {
		// TODO Auto-generated method stub
		System.out.println("add ProxyInterfaceImpl");
		return count++;
	}
	
	public void show() {
		// TODO Auto-generated method stub
		System.out.println("show ProxyInterfaceImpl");
	}
}
```

#### JDK动态代理

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyInvocationHandler implements InvocationHandler {
	private Object target;

	public Object bind(Object target) {
		this.target = target;
		return Proxy.newProxyInstance(this.target.getClass().getClassLoader(), this.target.getClass().getInterfaces(),this);
	}

	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		// TODO Auto-generated method stub
		System.out.println("doSomething before");
		Object result  = method.invoke(target, args);
		System.out.println("doSomething after");
		return result;
	}
}
```

#### cglib动态代理

```java
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;
import net.sf.cglib.proxy.Enhancer;
import java.lang.reflect.Method;


public class ProxyMethodInterceptor implements MethodInterceptor {
	// 业务类对象，供代理方法中进行真正的业务方法调用
	private Object target;

	// 相当于JDK动态代理中的绑定
	public Object bind(Object target) {
		// 给业务对象赋值
		this.target = target;
		// 创建加强器，用来创建动态代理类
		Enhancer enhancer = new Enhancer();
		// 为加强器指定要代理的业务类（即：为下面生成的代理类指定父类）
		enhancer.setSuperclass(this.target.getClass());
		// 设置回调：对于代理类上所有方法的调用，都会调用CallBack，而Callback则需要实现intercept()方法进行拦
		enhancer.setCallback(this);
		// 创建动态代理类对象并返回
		return enhancer.create();
	}

	public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
		// TODO Auto-generated method stub
		System.out.println("预处理——————");
		// 调用业务类（父类中）的方法
		Object result = proxy.invokeSuper(obj, args);
		System.out.println("调用后操作——————");
		return result;
	}
}
```

#### 调用和结果

```java
import cn.easycode.mycode.interfaces.ProxyInterface;
import cn.easycode.mycode.po.ProxyInterfaceImpl;
import cn.easycode.mycode.po.ProxyInvocationHandler;
import cn.easycode.mycode.po.ProxyMethodInterceptor;

public class Solution {
	public static void main(String[] args) {
		ProxyInterfaceImpl target = new ProxyInterfaceImpl();
		System.out.println("JDK动态代理 --开始");
		ProxyInvocationHandler handler = new ProxyInvocationHandler();
		ProxyInterface jdkProxy = (ProxyInterface)handler.bind(target);
		int result1 = jdkProxy.add();
		System.out.println("jdkProxy add方法调用结果:" + result1);
		System.out.println("JDK动态代理 --结束");
		
		System.out.println("-----------------------------------");
		
		System.out.println("CGLIB动态代理--开始");
		ProxyMethodInterceptor interceptor = new ProxyMethodInterceptor();
		ProxyInterface cglibProxy = (ProxyInterface)interceptor.bind(target);
		int result2 = cglibProxy.add();
		System.out.println("cglibProxy add方法调用结果:" +result2);
		System.out.println("CGLIB动态代理--结束");
	}
}


JDK动态代理 --开始
doSomething before
add ProxyInterfaceImpl
doSomething after
jdkProxy add方法调用结果:1
JDK动态代理 --结束
-----------------------------------
CGLIB动态代理--开始
预处理——————
add ProxyInterfaceImpl
调用后操作——————
cglibProxy add方法调用结果:2
CGLIB动态代理--结束
```

## 对象序列化和反序列化

```java
//序列化对象
@Data
public class Student implements Serializable{
	private static final long serialVersionUID = -1377794110170113031L;
	
	private int age;
	private String name;
	private String grade;
	private String school;
	private String schoolClass;
}

//序列化和反序列化
public class SerializableSolution {
	private static final String objectFile = "D:\\Object\\student.txt";
	//序列化对象
	public void serializeObject() throws FileNotFoundException, IOException {
		Student student = new Student();
		student.setName("张三");
		student.setSchool("清华大学");
		student.setAge(20);
		File file = new File(objectFile);
		if (!FileUtil.exist(file)) {
			// 创建文件目录
			FileUtil.mkParentDirs(file);
			// 创建文件
			file.createNewFile();
		}
		ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream(file));
		out.writeObject(student);
		out.close();
	}
	//反序列化对象
	public Object deserializeObject() throws FileNotFoundException, IOException, ClassNotFoundException {
		File file = new File(objectFile);
		if (!FileUtil.exist(file)) {
			System.out.println("文件不存在");
			return null;
		}
		ObjectInputStream in = new ObjectInputStream(new FileInputStream(file));
		Object obj = in.readObject();
		in.close();
		return obj;
	}
	
	@Test
	public void run() {
		try {
			serializeObject();
			Student student = (Student)deserializeObject();
			System.out.println("反序列化对象:" + student.toString());
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} 
	}
}

反序列化对象:Student(age=20, name=张三, grade=null, school=清华大学, schoolClass=null)
```

