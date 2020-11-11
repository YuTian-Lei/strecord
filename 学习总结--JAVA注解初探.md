# Java注解的使用

![](https://upload-images.jianshu.io/upload_images/3458176-c5642804973c8b6e.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

## 基本概念

> Java 注解是JDK5.0引入的注释机制，可以被使用在类，方法，参数等地方中，并且可以通过Java的反射机制获取注解中的内容，注解相当于标签，可以标识方法，类或属性具有某些特征，在编译器生成的类文件时，可以被嵌入到字节码中。另外用户可以自定义注解，完成定制化的开发，尤其是在利用springboot进行项目开发时，我们会经常使用注解管理spring容器的bean，从而大大提高了开发的效率。

## 基本语法

```java
@Target(ElementType.FIELD,ElementType.Method)
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface MyField {
    String description();
    String author();
    int age() default 18;
}
```

```java
public class MyFieldTest {
    //使用我们的自定义注解
    @MyField(description = "自定义注解", author = "yutian")
    private String username;
}
```

## 元注解

| 元注解          | 说明                           | 参数取值                |
| --------------- | ------------------------------ | ----------------------- |
| **@Target**     | 表示该注解可以用在什么地方     | **ElementType**参数     |
| **@Retention**  | 表示要在什么级别保存该注解信息 | **RetentionPolicy**参数 |
| **@Documented** | 将此注解包含在Javadoc中        | 无参数                  |
| **@Inherited**  | 允许子类继承父类中的注解       | 无参数                  |

```java
package java.lang.annotation;

public enum ElementType {
    TYPE,               /* 类、接口（包括注释类型）或枚举声明  */

    FIELD,              /* 字段声明（包括枚举常量）  */

    METHOD,             /* 方法声明  */

    PARAMETER,          /* 参数声明  */

    CONSTRUCTOR,        /* 构造方法声明  */

    LOCAL_VARIABLE,     /* 局部变量声明  */

    ANNOTATION_TYPE,    /* 注释类型声明  */

    PACKAGE             /* 包声明  */
}


package java.lang.annotation;
public enum RetentionPolicy {
    SOURCE,   /* Annotation信息仅存在于编译器处理期间，编译器处理完之后就没有该Annotation信息了  */

    CLASS,    /* 编译器将Annotation存储于类对应的.class文件中。默认行为  */

    RUNTIME   /* 编译器将Annotation存储于class文件中，并且可由JVM读入 */
}
```

## 注解处理器

> 在上面主要介绍了注解和自定义注解相关的知识点。对于注解，如果没有通过注解处理器来对注解进行处理，那么注解的作用和注释就没有什么区别了。所以这里主要总结两种常见的实现java注解处理器的方式：
>
> - 通过反射实现的**运行时**注解处理器
> - 通过apt工具实现的**编译时**注解处理器

### **编译时**注解处理器(Class范围)

> APT(Annotation Process Tool)，**是一种在代码编译时处理注解，注解作用范围在Class,**按照一定的规则，生成相应的java文件，多用于对自定义注解的处理。

### **运行时**注解处理器(RUNTIME范围)

> 只有定义为`RetentionPolicy.RUNTIME`时，我们才能通过注解反射获取到注解(**最常用的特性**)。
>
> Spring框架中注解大量运用**运行时**注解处理器

```java
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface BindView {
    int value();
}
```

```java
public class InjectUtils {
    public static void inject(Activity activity) {
        //1、 获取指定类上申明的所有字段
        Field[] declaredFields = activity.getClass().getDeclaredFields();
        for (Field field : declaredFields) {
            // 2、获取每次字段中包含BindView注解的字段。
            BindView annotation = field.getAnnotation(BindView.class);
			//多个注解时可使用此方法获取所有注解
            //Annotation[] anns = field.getDeclaredAnnotations();
            
            if (annotation != null) {
                // 3、通过获取注解指定的value，初始化View
                int value = annotation.value();
                View view = activity.findViewById(value);
                try {
                    field.set(activity, view);
                } catch (IllegalAccessException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}
```

## 示例

### 应用场景一：自定义注解+拦截器 实现登录校验

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LoginRequired {
    boolean isRequired() default true;
}
```

```java
@RestController
public class IndexController {

    @GetMapping("/sourceA")
    public String sourceA(){
        return "你正在访问sourceA资源";
    }
    
    @LoginRequired
    @GetMapping("/sourceB")
    public String sourceB(){
        return "你正在访问sourceB资源";
    }

}
```

```java
public class SourceAccessInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(...,Object handler) throws Exception {
        System.out.println("进入拦截器了");
        
          // 反射获取方法上的LoginRequred注解
        HandlerMethod handlerMethod = (HandlerMethod)handler;
        LoginRequired loginRequired = handlerMethod.getMethod().getAnnotation(LoginRequired.class);
        
        if(loginRequired != null && loginRequired.isRequired()){
        //需要登录，提示用户登录
        response.setContentType("application/json; charset=utf-8");
        response.getWriter().print("你访问的资源需要登录");
        return false;
        }
        return true;
    }

    @Override
    public void postHandle(...) throws Exception {

    }

    @Override
    public void afterCompletion(...) throws Exception {

    }
}
```

### 应用场景二：自定义注解+AOP 实现日志打印

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ModelUpdate {
}
```

```java
// 1.表明这是一个切面类
@Aspect
@Component
public class MyLogAspect {

  // 2. PointCut表示这是一个切点，@annotation表示这个切点切到一个注解上，后面带该注解的全类名
  // 切面最主要的就是切点，所有的故事都围绕切点发生
  // logPointCut()代表切点名称
  @Pointcut("@annotation(com.easycode.mmall.annotation.ModelUpdate)")
  public void logPointCut(){};

  // 3. 前置通知
  @Before("logPointCut()")
  public void logAround(JoinPoint joinPoint){
    // 获取方法名称
    String methodName = joinPoint.getSignature().getName();
    // 获取入参
    Object[] param = joinPoint.getArgs();

    StringBuilder sb = new StringBuilder();
    for(Object arg : param){
      sb.append(arg + "; ");
    }
    System.out.println("进入[" + methodName + "]方法,参数为:" + sb.toString());
  }
}
```



## 总结

> - 对于注解，如果没有通过注解处理器来对注解进行处理，那么注解的作用和注释就没有什么区别了
> - **运行时注解处理器**在平时使用时更多
> - **运行时注解处理器核心思想**是通过反射获取到注解，然后进行相应的处理