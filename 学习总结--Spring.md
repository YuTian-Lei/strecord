# Spring

## 七大模块

- **Spring core**：提供了Spring框架基本功能（IOC功能），如BeanFactory创建对象、读取xml文件实例化对象、管理组件生命周期等

- **Spring AOP**：面向切面编程思想，同时也提供了事务管理

- **Spring MVC**：提供了MVC设计模式的实现

- Spring Context：提供了Spring上下文环境，以及其他如国际化、Email等服务

- **Spring ORM**：提供了对Hibernate、myBatis的支持

- Spring DAO：提供了 对Data Access Object模式和JDBC的支持。实现业务逻辑与数据库访问代码分离，降低代码耦合度

- Spring Web：提供了Servlet监听器的Context和Web应用的上下文

  ![](.\img\v2-0abd05fadd67aa418f9fed5410ee96d8_720w.jpg)

## Spring IOC

![](https://img-blog.csdnimg.cn/20191105111441363.png)

### Spring 容器的创建

```Java
//普通程序中的容器创建
public class Application {
    public static void main(String[] args) {
        //创建Spring容器
        ApplicationContext factory = new ClassPathXmlApplicationContext("spring.xml");
        //获取bean
        User user = (User) factory.getBean("user");
        user.setName("张三");
        System.out.println(user.getName());
    }
}

//web程序中的容器创建(未整合SpringMVC)
public class DispatcherServlet extends GenericServlet {
    private ApplicationContext  context;

    public  void init() throws ServletException {
    	//创建Spring容器
        super.init();
        context = new ClassPathXmlApplicationContext("spring-dao.xml");
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
    	//解析ruqest请求
        HttpServletRequest request = (HttpServletRequest)servletRequest;
        HttpServletResponse response = (HttpServletResponse)servletResponse;

        String path = request.getServletPath().substring(1);
        String beanName = null;
        String methodName = null;

        int index = path.indexOf("/");
        if(index != -1) {
            beanName = path.substring(0,index)+"Controller";
            methodName = path.substring(index+1,path.indexOf(".do"));
        } else {
            beanName = "selfController";
            methodName = path.substring(0,path.indexOf(".do"));
        }

        System.out.println(beanName+methodName);
        //从Spring容器获取对象
        Object obj = context.getBean(beanName);
        try {
            Method method = obj.getClass().getMethod(methodName,HttpServletRequest.class,HttpServletResponse.class);
            //调用对象方法
            method.invoke(obj,request,response);
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
    }
}
```

### bean管理

#### bean实例化三种方式

- **类构造器实例化(默认使用无参构造器)** 
- **静态工厂实例化**
- **实例工厂实例化**

#### bean实例化时机

- Spring容器实例化后,加载bean配置文件时,将所有的单例bean全部实例化。（**即spring容器实例化后,容器中的bean也全部实例化**）

#### bean属性的注入

##### xml配置注入

- **构造器注入**
- **Setter注入**
- **名称空间注入**
- **SpEL注入**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--构造器注入-->
    <bean id="cat1" class="com.zhangxin9727.Cat">
        <constructor-arg name="name" value="kitty"/>
    </bean>
    <bean id="person1" class="com.zhangxin9727.Person">
        <constructor-arg name="name" value="Alice"/>
        <constructor-arg index="1" value="16"/>
        <constructor-arg type="com.zhangxin9727.Cat" ref="cat1"/>
    </bean>

    <!--Setter注入-->
    <bean id="cat2" class="com.zhangxin9727.Cat">
        <property name="name" value="Kitty"/>
    </bean>
    <bean id="person2" class="com.zhangxin9727.Person">
        <property name="name" value="Alice"/>
        <property name="age" value="16"/>
        <property name="cat" ref="cat2"/>
    </bean>

    <!--p名称空间注入-->
    <bean id="cat3" class="com.zhangxin9727.Cat" p:name="Kitty"/>
    <bean id="person3" class="com.zhangxin9727.Person" p:name="Alice" p:age="16" p:cat-ref="cat3"/>

    <!--SpEL注入-->
    <bean id="cat4" class="com.zhangxin9727.Cat">
        <property name="name" value="#{'Kitty'}"/>
    </bean>
    <bean id="person4" class="com.zhangxin9727.Person">
        <property name="name" value="#{'Alice'}"/>
        <property name="age" value="#{16}"/>
        <property name="cat" value="#{cat4}"/>
    </bean>
</beans>
```

##### 注解注入

- @Value
- @Autowired
- @Resource

```Java
/**
 * @Description: //TODO
 * @Date: 2020/7/18 16:32
 * @Author: pengfei.L
 */
@Component
public class UserService {
    @Value("UserService")
    private  String serviceName;

    @Autowired
    private UserMapper userMapper1;

    @Resource(name = "userMapper")
    private UserMapper userMapper2;
}
```



#### bean生命周期

- 实例化：Spring启动，查找并加载需要被Spring管理的bean，进行Bean的实例化
- 属性赋值：Bean实例化后对将Bean的引入和值注入到Bean的属性中
- 如果Bean实现了BeanNameAware接口的话，Spring将Bean的Id传递给setBeanName()方法
- 如果Bean实现了BeanFactoryAware接口的话，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入
- 如果Bean实现了ApplicationContextAware接口的话，Spring将调用Bean的setApplicationContext()方法，将bean所在应用上下文引用传入进来
- 如果Bean实现了BeanPostProcessor接口，Spring就将调用他们的postProcessBeforeInitialization()方法
- init-method:bean的init-method方法
- 如果Bean 实现了InitializingBean接口，Spring将调用他们的afterPropertiesSet()方法
- 如果Bean 实现了BeanPostProcessor接口，Spring就将调用他们的postProcessAfterInitialization()方法
- bean自身业务方法,此时Bean已经准备就绪,可以被应用程序使用了。他们将一直驻留在应用上下文中，直到应用上下文被销毁
- destroy-method:bean的destroy-method方法
- 如果bean实现了DisposableBean接口，Spring将调用它的destory()接口方法，同样，如果bean使用了destory-method 声明销毁方法，该方法也会被调用

```Java
//声明：以下顺序在Spring 5.1.5Release版本验证

/**
 * @Description: //TODO
 * @Date: 2020/7/18 15:26
 * @Author: pengfei.L
 */
@Component
public class User implements BeanNameAware, BeanFactoryAware, ApplicationContextAware, InitializingBean , DisposableBean {
    private String name;

    public User(){
        System.out.println("第一步,实例化bean...");
    }

    @Value("张三")
    public void setName(String name) {
        System.out.println("第二步,属性赋值...");
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setBeanName(String s) {
        System.out.println("第三步,设置bean名称...");
    }

    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
        System.out.println("第四步,将BeanFactory容器实例传入...");
    }

    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        System.out.println("第五步,将bean所在应用上下文引用传入...");
    }

    @PostConstruct
    public void init(){
        System.out.println("第七步,init方法执行...");
    }

    public void afterPropertiesSet() throws Exception {
        System.out.println("第八步,属性设置后执行...");
    }

    public void run(){
        System.out.println("第十步,自身业务方法执行...");
    }

    @PreDestroy
    public void preDestroy(){
        System.out.println("第十一步,bean销毁方法...");
    }

    public void destroy() {
        System.out.println("第十二步,执行spring销毁方法...");
    }
}
```

```Java
//BeanPostProcessor 此配置会在所有bean的生命周期中生效

/**
 * @Description: //TODO
 * @Date: 2020/7/18 15:56
 * @Author: pengfei.L
 */
@Component
public class MyBeanPostProcessor implements BeanPostProcessor {

    // 容器加载的时候会加载一些其他的bean，会调用初始化前和初始化后方法
    // 容器中所有bean均会执行此步骤
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("第六步,BeanPostProcessor-初始化bean之前....");
        return bean;
    }
    // 容器中所有bean均会执行此步骤
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("第九步,BeanPostProcessor-初始化bean之后....");
        return bean;
    }
}
```

```Java
/**
 * @Description: //TODO
 * @Date: 2020/7/18 15:25
 * @Author: pengfei.L
 */

public class Application {
    public static void main(String[] args) {
        //创建Spring容器
        ApplicationContext factory = new ClassPathXmlApplicationContext("spring.xml");
        User user = (User) factory.getBean("user");
        user.run();
        ((ClassPathXmlApplicationContext) factory).close();
    }
}

//程序输出
第一步,实例化bean...
第二步,属性赋值...
第三步,设置bean名称...
第四步,将BeanFactory容器实例传入...
第五步,将bean所在应用上下文引用传入...
第六步,BeanPostProcessor-初始化bean之前....
第七步,init方法执行...
第八步,属性设置后执行...
第九步,BeanPostProcessor-初始化bean之后....
第十步,自身业务方法执行...
第十一步,bean销毁方法...
第十二步,执行spring销毁方法...
```

#### bean循环依赖

##### 循环依赖场景

- ### 构造器参数循环依赖(无法解决,因为bean实例化未完成)

- ### setter方式原型，bean作用域prototype(无法解决,Spring容器不进行缓存此bean)

- ### setter方式原型，bean作用域singleton（可以解决,使用三级缓存）

##### Spring解决循环依赖原理

###### Spring的单例对象的初始化

- createBeanInstance：实例化，其实也就是调用对象的构造方法实例化对象
- populateBean：填充属性，这一步主要是多bean的依赖属性进行填充
- initializeBean：调用bean中的init-method方法

###### 理论依据

> Spring循环依赖的理论依据其实是Java基于引用传递，当我们获取到对象的引用时，对象的field或者或属性是可以延后设置的

##### 循环依赖解决方案-三级缓存

```Java
/** Cache of singleton objects: bean name --> bean instance 一级缓存*/ 
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<String, Object>(256);
/** Cache of early singleton objects: bean name --> bean instance 二级缓存*/
private final Map<String, Object> earlySingletonObjects = new HashMap<String, Object>(16);
/** Cache of singleton factories: bean name --> ObjectFactory 三级缓存*/
private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<String, ObjectFactory<?>>(16);
```

```Java
protected  T doGetBean(final String name, @Nullable final Class requiredType,
    @Nullable final Object[] args, boolean typeCheckOnly) throws BeansException {
  // 尝试通过bean名称获取目标bean对象，比如这里的A对象
  Object sharedInstance = getSingleton(beanName);
  // 我们这里的目标对象都是单例的
  if (mbd.isSingleton()) {
    // 这里就尝试创建目标对象，第二个参数传的就是一个ObjectFactory类型的对象，这里是使用Java8的lamada
    // 表达式书写的，只要上面的getSingleton()方法返回值为空，则会调用这里的getSingleton()方法来创建
    // 目标对象
    sharedInstance = getSingleton(beanName, () -> {
      try {
        // 尝试创建目标对象,该方法最终委托给下文的doCreateBean()方法进行
        return createBean(beanName, mbd, args);
      } catch (BeansException ex) {
        throw ex;
      }
    });
  }
  return (T) bean;
}
```

```Java
//Spring会尝试从缓存中获取
//isSingletonCurrentlyInCreation 判断对应的单例对象是否在创建中(即单例对象没有被初始化完全)
//allowEarlyReference 是否允许从singletonFactories中通过getObject拿到对象
protected Object getSingleton(String beanName, boolean allowEarlyReference) {
    //一级缓存,完全创建完全的bean对象
   Object singletonObject = this.singletonObjects.get(beanName);
   if (singletonObject == null && isSingletonCurrentlyInCreation(beanName)) {
      synchronized (this.singletonObjects) {
          //二级缓存,未初始化完全的bean对象,提前暴露出来的bean对象
         singletonObject = this.earlySingletonObjects.get(beanName);
         if (singletonObject == null && allowEarlyReference) {
             //三级缓存,出于AOP考虑
             //即若使用二级缓存，在AOP情形下，注入到其他bean的，不是最终的代理对象，而是原始对象
            ObjectFactory<?> singletonFactory = this.singletonFactories.get(beanName);
            if (singletonFactory != null) {
               singletonObject = singletonFactory.getObject();
               this.earlySingletonObjects.put(beanName, singletonObject);
               this.singletonFactories.remove(beanName);
            }
         }
      }
   }
   return (singletonObject != NULL_OBJECT ? singletonObject : null);
}
```

```Java
protected Object doCreateBean(final String beanName, final RootBeanDefinition mbd, final @Nullable Object[] args)  throws BeanCreationException {
  // 实例化当前尝试获取的bean对象，比如A对象和B对象都是在这里实例化的
  BeanWrapper instanceWrapper = null;
  if (mbd.isSingleton()) {
    instanceWrapper = this.factoryBeanInstanceCache.remove(beanName);
  }
  if (instanceWrapper == null) {
    instanceWrapper = createBeanInstance(beanName, mbd, args);
  }
  // 判断Spring是否配置了支持提前暴露目标bean，也就是是否支持提前暴露半成品的bean
  boolean earlySingletonExposure = (mbd.isSingleton() && this.allowCircularReferences 
    && isSingletonCurrentlyInCreation(beanName));
  if (earlySingletonExposure) {
    
    // 如果支持，这里就会将当前生成的半成品的bean放到singletonFactories中，这个singletonFactories
    // 就是前面第一个getSingleton()方法中所使用到的singletonFactories属性，也就是说，这里就是
    // 封装半成品的bean的地方。而这里的getEarlyBeanReference()本质上是直接将放入的第三个参数，也就是
    // 目标bean直接返回
    // getEarlyBeanReference()方法返回基于AOP的代理对象
    addSingletonFactory(beanName, () -> getEarlyBeanReference(beanName, mbd, bean));
  }
  try {
    // 在初始化实例之后，这里就是判断当前bean是否依赖了其他的bean，如果依赖了，
    // 就会递归的调用getBean()方法尝试获取目标bean
    populateBean(beanName, mbd, instanceWrapper);
  } catch (Throwable ex) {
    // 省略...
  }
  return exposedObject;
}
```

##### 二级缓存可不可以

- **二级缓存同样也能很好解决循环引用问题**
- **三级而非二级缓存并非出于IOC的考虑，而是出于AOP的考虑**
- **若使用二级缓存，在AOP情形下，注入到其他bean的，不是最终的代理对象，而是原始对象**

```Java
	//返回基于AOP的代理对象
	public Object getEarlyBeanReference(Object bean, String beanName) throws BeansException {
		Object cacheKey = getCacheKey(bean.getClass(), beanName);
		this.earlyProxyReferences.add(cacheKey);
		return wrapIfNecessary(bean, beanName, cacheKey);
	}
```

#### bean实例化顺序

- 构造方法依赖
- @DependOn 注解

```Java
//构造方法依赖
@Component
public class CDemo1 {

    private String name = "cdemo 1";

    public CDemo1(CDemo2 cDemo2) {
        System.out.println(name);
    }
}

@Component
public class CDemo2 {

    private String name = "cdemo 1";

    public CDemo2() {
        System.out.println(name);
    }
}
//@DependOn 注解
@DependsOn("rightDemo2")
@Component
public class RightDemo1 {
    private String name = "right demo 1";

    @Autowired
    private RightDemo2 rightDemo2;

    public RightDemo1() {
        System.out.println(name);
    }

    @PostConstruct
    public void init() {
        System.out.println(name + " _init");
    }
}

@Component
public class RightDemo2 {
    private String name = "right demo 2";

    @Autowired
    private RightDemo1 rightDemo1;

    public RightDemo2() {
        System.out.println(name);
    }

    @PostConstruct
    public void init() {
        System.out.println(name + " _init");
    }
}
```

## Spring AOP

#### 源码解读

```Java
//Spring AOP 创建切面增强类源码(使用JDK和cglib动态代理) 
//org.springframework.aop.framework.autoproxy.AbstractAutoProxyCreator#postProcessBeforeInstantiation
public Object postProcessBeforeInstantiation(Class<?> beanClass, String beanName) throws BeansException {
   Object cacheKey = getCacheKey(beanClass, beanName);

   if (!StringUtils.hasLength(beanName) || !this.targetSourcedBeans.contains(beanName)) {
      //advisedBeans保存所有需要增强的bean，
     if (this.advisedBeans.containsKey(cacheKey)) {
        return null;
     }
      //是否是Advice、Pointcut、Advisor、AopInfrastructureBean
      //判断是否是切面（判断是否有aspect注解）
     if (isInfrastructureClass(beanClass) || shouldSkip(beanClass, beanName)) {
        this.advisedBeans.put(cacheKey, Boolean.FALSE);
        return null;
     }
   }

   // Create proxy here if we have a custom TargetSource.
   // Suppresses unnecessary default instantiation of the target bean:
   // The TargetSource will handle target instances in a custom fashion.
   TargetSource targetSource = getCustomTargetSource(beanClass, beanName);
   if (targetSource != null) {
     if (StringUtils.hasLength(beanName)) {
        this.targetSourcedBeans.add(beanName);
     }
     Object[] specificInterceptors = getAdvicesAndAdvisorsForBean(beanClass, beanName, targetSource);
     Object proxy = createProxy(beanClass, beanName, specificInterceptors, targetSource);
     this.proxyTypes.put(cacheKey, proxy.getClass());
     return proxy;
   }

   return null;
}
```

#### AOP的使用

```Java
/**
 * @Description: //TODO
 * @Date: 2020/4/18 19:15
 * @Author: pengfei.L
 */
// 1.表明这是一个切面类
@Aspect
@Component
public class MyLogAspect {

  // 2. PointCut表示这是一个切点，@annotation表示这个切点切到一个注解上，后面带该注解的全类名
  // 切面最主要的就是切点，所有的故事都围绕切点发生
  // logPointCut()代表切点名称
  @Pointcut("@annotation(com.easycode.mmall.web.annotation.ModelUpdate)")
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

```Java
@Aspect
@Component
public class ConsoleLogAspect {

  @Autowired(required = false)
  HttpServletRequest servletRequest;

  @Autowired(required = false)
  private HttpServletResponse response;

  //设置切面点（切面地址根据自己的项目填写）
  @Pointcut(value = "(execution(* com.easycode.mmall.web.controller.*.*(..)))")
  public void webLog() {}

  //指定切点前的处理方法
  @Before("webLog()")
  public void doBefore(JoinPoint joinPoint) throws Exception {
    //获取request对象
    ServletRequestAttributes attributes =
        (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
    HttpServletRequest request = attributes.getRequest();
    StringBuilder sb = new StringBuilder();
    //拼接请求内容
    sb.append("\n请求路径:" + request.getRequestURL().toString() + "  " + request.getMethod() + "\n");
    //判断请求是什么请求
    if (request.getMethod().equalsIgnoreCase(RequestMethod.GET.name())) {
      Map<String, String[]> parameterMap = request.getParameterMap();
      Map<String, String> paramMap = new HashMap<>();
      parameterMap.forEach(
          (key, value) -> paramMap.put(key, Arrays.stream(value).collect(joining(","))));
      sb.append("请求内容:" + JSON.toJSONString(paramMap));
    } else if (request.getMethod().equalsIgnoreCase(RequestMethod.POST.name())) {
      Object[] args = joinPoint.getArgs();

      StringBuilder stringBuilder = new StringBuilder();
      if (args.length > 1) {
        Arrays.stream(args)
            .forEach(object -> stringBuilder.append((object != null ? object.toString() : "" ).replace("=", ":")));
      }
      if (stringBuilder.length() == 0) {
        stringBuilder.append("{}");
      }
      sb.append("请求内容:" + stringBuilder.toString());
    }
    log.info(sb.toString());
  }
}
```

#### Spring事务

##### Spring 事务传播机制

| **传播行为**                                                 | **含义**                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `REQUIRED(TransactionDefinition.PROPAGATION_REQUIRED)`       | 支持当前事务，如果没有事务会创建一个新的事务                 |
| `SUPPORTS(TransactionDefinition.PROPAGATION_SUPPORTS)`       | 支持当前事务，如果没有事务的话以非事务方式执行               |
| `MANDATORY(TransactionDefinition.PROPAGATION_MANDATORY)`     | 支持当前事务，如果没有事务抛出异常                           |
| `REQUIRES_NEW(TransactionDefinition.PROPAGATION_REQUIRES_NEW)` | 创建一个新的事务并挂起当前事务                               |
| `NOT_SUPPORTED(TransactionDefinition.PROPAGATION_NOT_SUPPORTED)` | 以非事务方式执行，如果当前存在事务则将当前事务挂起           |
| `NEVER(TransactionDefinition.PROPAGATION_NEVER)`             | 以非事务方式进行，如果存在事务则抛出异常                     |
| `NESTED(TransactionDefinition.PROPAGATION_NESTED)`           | 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则进行与PROPAGATION_REQUIRED类似的操作。 |

#####  Spring事务隔离级别

| **隔离级别**               | **含义**                                                     |
| -------------------------- | ------------------------------------------------------------ |
| ISOLATION_DEFAULT          | 使用数据库默认的事务隔离级别                                 |
| ISOLATION_READ_UNCOMMITTED | 允许读取尚未提交的修改，可能导致脏读、幻读和不可重复读       |
| ISOLATION_READ_COMMITTED   | 允许从已经提交的事务读取，可防止脏读、但幻读，不可重复读仍然有可能发生 |
| ISOLATION_REPEATABLE_READ  | 对相同字段的多次读取的结果是一致的，除非数据被当前事务自生修改。可防止脏读和不可重复读，但幻读仍有可能发生 |
| ISOLATION_SERIALIZABLE     | 完全服从ACID隔离原则，确保不发生脏读、不可重复读、和幻读，但执行效率最低。 |

##### Spring事务的使用

```Java
@Service
public class EmployeeServiceImpl implements EmployeeService {

    @Autowired
    EmployeeMapper employeeMapper;

    @Autowired
    DepartmentService departmentService;

    /**
     * 添加员工的同时添加部门
     * REQUIRED 传播
     * @param name 员工姓名
     */
    @Override
    @Transactional(value = "READ_UNCOMMITTED" ,propagation = Propagation.REQUIRED)
    public void addEmpByRequired(String name) {
        Employee employee = new Employee();
        employee.setDeptId(1);
        employee.setName(name);
        employee.setAddress("邯郸");
        employeeMapper.insertSelective(employee);
        departmentService.addDept("jishubu");
        int i = 1/0;
    }
}
```

## Spring MVC

#### 父子容器

>在Spring整体框架的核心概念中，容器是核心思想，就是用来管理Bean的整个生命周期的， 而在一个项目中，容器不一定只有一个，Spring中可以包括多个容器，而且容器有上下层关系 ，目前最常见的一种场景就是在一个项目中引入Spring和SpringMVC这两个框架，它其实就是两个容器，Spring是父容器，SpringMVC是其子容器，并且在Spring父容器中注册的Bean对于SpringMVC容器中是可见的，而在SpringMVC容器中注册的Bean对于Spring父容器 中是不可见的，也就是子容器可以看见父容器中的注册的Bean，反之就不行。

#### 工作原理

![](C:\Users\lenovo\Desktop\学习总结\img\714509-20170920165202868-445512458.png)

- 用户发起请求到前端控制器（DispatcherServlet）
- 前端控制器请求处理器映射器（HandlerMappering）去查找处理器（Handle）：通过xml配置或者注解进行查找
- 找到以后处理器映射器（HandlerMappering）像前端控制器返回执行链（HandlerExecutionChain）
- 前端控制器（DispatcherServlet）调用处理器适配器（HandlerAdapter）去执行处理器（Handler）
- 处理器适配器去执行Handler
- Handler执行完给处理器适配器返回ModelAndView
- 处理器适配器向前端控制器返回ModelAndView
- 前端控制器请求视图解析器（ViewResolver）去进行视图解析
- 视图解析器像前端控制器返回View
- 前端控制器对视图进行渲染
- 前端控制器向用户响应结果

#### 相关组件

| 组件名称                    | 作用                                                         |
| --------------------------- | ------------------------------------------------------------ |
| 前端控制器DispatcherServlet | 接收请求，响应结果，相当于转发器，中央处理器。有了dispatcherServlet减少了其它组件之间的耦合度。用户请求到达前端控制器，它就相当于mvc模式中的c，dispatcherServlet是整个流程控制的中心，由它调用其它组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性。 |
| 处理器映射器HandlerMapping  | 根据请求的url查找HandlerHandlerMapping负责根据用户请求找到Handler即处理器，springmvc提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。 |
| 处理器适配器HandlerAdapter  | 按照特定规则（HandlerAdapter要求的规则）去执行Handler通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。 |
| 处理器Handler               | 编写Handler时按照HandlerAdapter的要求去做，这样适配器才可以去正确执行HandlerHandler 是继DispatcherServlet前端控制器的后端控制器，在DispatcherServlet的控制下Handler对具体的用户请求进行处理。由于Handler涉及到具体的用户业务请求，所以一般情况需要工程师根据业务需求开发Handler。 |
| 视图解析器View resolver     | 行视图解析，根据逻辑视图名解析成真正的视图（view）View Resolver负责将处理结果生成View视图，View Resolver首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成View视图对象，最后对View进行渲染将处理结果通过页面展示给用户。 springmvc框架提供了很多的View视图类型，包括：jstlView、freemarkerView、pdfView等。 |
| 视图View                    | View是一个接口，实现类支持不同的View类型（jsp、freemarker、pdf…） |

#### dispacherServlet

#### 视图解析器View resolver

#### 拦截器

## Spring 常用注解

### 核心注解

| 注解            | 含义                                                         |
| --------------- | ------------------------------------------------------------ |
| @Required       | 用于Bean的Setter方法上，以指示该Bean组装时必须要有该属性，否则抛出BeanInitializationException |
| @Autowired      | 自动导入依赖的bean。byType方式。把配置好的Bean拿来用，完成属性、方法的组装。 它可以对类成员变量、方法及构造函数进行标注，完成自动装配的工作。 当加上（required=false）时，就算找不到bean也不报错。 |
| @Qualifier      | 这个注解通常和@Autowired一起使用，当你想对注入的过程做更多的控制，@Qualifier可以帮助你指定做更详细配置。一般在两个或者多个bean是相同的类型，spring在注入的时候会出现混乱。 |
| @Value          | 可用于字段，构造器参数，方法参数，以指示一个默认的值。<br/>支持#{...}和${...}这两种占位符。<br/>注入Spring boot application.properties配置的属性的值 |
| @Configuration  | 等同于spring的XML配置文件；使用Java代码可以检查类型安全。<br/>如果有些第三方库需要用到xml文件，建议仍然通过@Configuration类作为项目的配置主类，<br/>并使用@ImportResource注解加载xml配置文件 |
| @ComponentScan  | 与@Configuration注解一起，发现和装配Bean。<br/>可通过basePackageClasses 或basePackage指定扫描的基类或基包。<br/>如果声明的包未定义的话，会从声明该注解的类所在的包及子包开始扫描 |
| @Bean           | 等价于XML中配置的bean。<br/>放在方法的上面，而不是类，产生一个Bean,并交给Spring容器管理。 |
| @Lazy           | 用在组件类上。Spring默认在启动时自动装载依赖类。使用此注解时，会在第一次请求使用时才初始化该类。<br/>也可与@Configuration一起使用，则所有@Bean注解的方法会延时初始化。 |
| @Import         | 用来导入其他配置类                                           |
| @ImportResource | 用来加载xml配置文件                                          |

### 原型注解

| 注解        | 含义                                                         |
| ----------- | ------------------------------------------------------------ |
| @Component  | 可配合CommandLineRunner使用，在程序启动后执行一些基础任务;<br/>当组件不好归类的时候，可以使用这个注解进行标注 |
| @Controller | 用于定义控制器类，在spring 项目中由控制器负责将用户发来的URL请求转发到对应的服务接口（service层），<br/>一般这个注解在类中，通常方法需要配合注解@RequestMapping |
| @Service    | 一般用于修饰service层的组件                                  |
| @Repository | 可以确保DAO或者repositories提供异常转译，<br/>这个注解修饰的DAO或者repositories类会被ComponetScan发现并配置，<br/>同时也不需要为它们提供XML配置项 |

### Springboot注解

| 注解                     | 含义                                                         |
| ------------------------ | ------------------------------------------------------------ |
| @SpringBootApplication   | 包含了@ComponentScan、@Configuration和@EnableAutoConfiguration注解<br/>其中@ComponentScan让spring Boot扫描到Configuration类并把它加入到程序上下文 |
| @EnableAutoConfiguration | 自动配置。尝试根据你添加的jar依赖自动配置你的Spring应用。<br/>例如，如果classpath下存在HSQLDB，并且你没有手动配置任何数据库连接beans，将自动配置一个内存型（in-memory）数据库”。<br/>你可以将@EnableAutoConfiguration或@SpringBootApplication注解添加到一个@Configuration类上来选择自动配置。<br/>如果发现应用了你不想要的特定自动配置类，可使用@EnableAutoConfiguration注解的排除属性来禁用它们 |
| @MapperScan              | 通过使用@MapperScan可以指定要扫描的Mapper类的包的路径        |

### 任务执行和调度注解

| 注解              | 含义                                                         |
| ----------------- | ------------------------------------------------------------ |
| @Scheduled        | 方法级，具有该注解的方法应无返回值，且不接受任何参数         |
| @Async            | 方法级，每个方法均都在单独的线程中，可接受参数；可返回值，也可不返回值。 |
| @EnableScheduling | 在配置类上使用，开启计划任务的支持（类上）                   |

### 全局异常和事务支持

| 注解                  | 含义                                                         |
| --------------------- | ------------------------------------------------------------ |
| @ControllerAdvice     | 包含@Component。可以被扫描到。<br/>统一处理异常，一般与@ExceptionHandler一起使用 |
| @RestControllerAdvice | @ControllerAdvice 和 @ResponseBody的组合                     |
| @Transactional        | 用于接口、接口中的方法、类、类中的公有方法<br/>光靠该注解并不足以实现事务<br/>仅是一个元数据，运行时架构会使用它配置具有事务行为的Bean |

### SpringMVC注解

| 注解                          | 含义                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| @Controller                   | 用于定义控制器类，在spring 项目中由控制器负责将用户发来的URL请求转发到对应的服务接口（service层），<br/>一般这个注解在类中，通常方法需要配合注解@RequestMapping |
| @RestController               | @Controller和@ResponseBody的合集,表示这是个控制器bean,<br/>并且是将函数的返回值直接填入HTTP响应体中,是REST风格的控制器 |
| @RequestMapping               | RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上。<br/>用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。<br/>该注解有六个属性：<br/>params:指定request中必须包含某些参数值是，才让该方法处理<br/>headers:指定request中必须包含某些指定的header值，才能让该方法处理请求<br/>value:指定请求的实际地址，指定的地址可以是URI Template 模式<br/>method:指定请求的method类型， GET、POST、PUT、DELETE等<br/>consumes:指定处理请求的提交内容类型（Content-Type），如application/json,text/html;<br/>produces:指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回 |
| @CookieValue                  | 用于方法参数上获得cookie                                     |
| @CrossOrigin                  | 用于类和方法上，以实现跨域请求。<br/>有时，运行JavaScript的主机和服务数据的主机不是同一个，此时就涉及到跨域(CORS) |
| @ExceptionHandler             | 用于方法上，指示异常处理类                                   |
| @InitBinder                   | 初始化绑定器，用于数据绑定、设置数据转换器等                 |
| @Mappings<br/>@Mapping        | 用于字段上。<br/>Mapping是一个Meta注解，以指示web映射注解<br/>Mappings可用于多个 |
| @MatrixVariable               | 矩阵变量                                                     |
| @PathVariable                 | 路径变量，获取路径上传过来的参数                             |
| @RequestAttribute             | 绑定请求属性到handler方法参数                                |
| @RequestBody                  | 指示方法参数应该绑定到Http请求Body<br/>HttpMessageConveter负责将HTTP请求消息转为对象 |
| @RequestHeader                | 映射控制器参数到请求头的值                                   |
| @RequestParam                 | 用在方法的参数前面                                           |
| @RequestPart                  | 替代@RequestParam以获得多部的内容并绑定到方法参数            |
| @ResponseBody                 | 指示方法返回值应该直接写入Response Body（不再走视图处理器）<br/>Spring使用HttpMessageConverter实现了返回对象转为响应体 |
| @ResponseStatus               | 用于方法和异常类上。以一个状态码作为指示，且原因必须返回。<br/>也可注解于Controller，其所有的@RequestMapping方法都会继承它 |
| @SessionAttribute             | 用于方法参数。绑定方法参数到会话属性                         |
| @SessionAttributes            | 用于将会话属性用Bean封装                                     |
| @JsonBackReference            | 解决嵌套外链问题                                             |
| @RepositoryRestResourcepublic | 配合spring-boot-starter-data-rest使用                        |
| @ModelAttribute               | 把值绑定到Model中，使全局@RequestMapping可以获取到该值       |
| @Valid                        | 验证器，一般配合@InitBinder使用                              |

### SpringCloud

| 注解                   | 含义       |
| ---------------------- | ---------- |
| @EnableConfigServer    | 配置服务器 |
| @EnableEurekaServer    | 注册服务器 |
| @EnableDiscoveryClient | 发现客户端 |
| @EnableCircuitBreaker  | 熔断器     |
| @HystrixCommand        | 服务降级   |

## Spring自动装配方式

- no：默认的方式是不进行自动装配的，通过手工设置ref属性来进行装配bean。
- byName：通过bean的名称进行自动装配，如果一个bean的 property 与另一bean 的name 相同，就进行自动装配。
- byType：通过参数的数据类型进行自动装配。
- constructor：利用构造函数进行装配，并且构造函数的参数通过byType进行装配。
- autodetect：自动探测，如果有构造方法，通过 construct的方式自动装配，否则使用 byType的方式自动装配。

## @Autowired注解与@Resource注解

- 提供方：@Autowired是由org.springframework.beans.factory.annotation.Autowired提供，换句话说就是由Spring提供；@Resource是由javax.annotation.Resource提供，即J2EE提供，需要JDK1.6及以上。
- 注入方式：@Autowired只按照byType 注入；@Resource默认按byName自动注入，也提供按照byType 注入；
- 属性：@Autowired按类型装配依赖对象，默认情况下它要求依赖对象必须存在，如果允许null值，可以设置它required属性为false。如果我们想使用按名称装配，可以结合@Qualifier注解一起使用。@Resource有两个中重要的属性：name和type。name属性指定byName，如果没有指定name属性，当注解标注在字段上，即默认取字段的名称作为bean名称寻找依赖对象，当注解标注在属性的setter方法上，即默认取属性名作为bean名称寻找依赖对象。需要注意的是，@Resource如果没有指定name属性，并且按照默认的名称仍然找不到依赖对象时， @Resource注解会回退到按类型装配。但一旦指定了name属性，就只能按名称装配了。

### @Resource装配顺序

- 如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常
- 如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常
- 如果指定了type，则从上下文中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常
- 如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配；