## SpringBoot(整合+配置+使用)
## springboot核心注解
### SpringBootApplication
核心注解，组合组件：@SpringBootConfiguration ,@EnableAutoConfiguration ,@ComponentScan

- @SpringBootConfiguration：继承自@Configuration，二者功能也一致，标注当前类是配置类，并会将当前类内声明的一个或多个以@Bean注解标记的方法的实例纳入到spring容器中，并且实例名就是方法名。（标识bean）
- @EnableAutoConfiguration:完成依赖的自动配置，比如添加了spring-boot-starter-web依赖，会自动添加tomcat和SpringMVC的依赖，那么SpringBoot会对Tomcat和SpringMVC进行自动配置。(依赖自动配置)
- @ComponentScan：会自动扫描@SpringBootApplication所在类的同级包(com.dpb.springboot)以及子包中的Bean，所以建议将入口类放置在groupId+arctifactID组合的包名下。(开启自动扫描)

### Spring配置文件
- SpringBoot使用一个全局的配置文件application.properties或application.yml
- SpringBoot建议基于JAVA的配置，使用@Configuration作为配置源
- 配置类之间可用@Import导入其他配置类
- 使用@ImportResource导入XML配置
- JAVA类的配置和XML的配置

### Spring常用注解
- @RestController：@Controller+@ResponseBody

### SSM中线程安全
- Spring根本就没有对bean的多线程安全问题做出任何保证与措施
- 每个单例的无状态对象都是线程安全的（Service、DAO和Controller，这些对象并没有自己的状态，它们只是用来执行某些操作的）
- 不要在bean中声明任何有状态的实例变量或类变量
- 如果必须如此，那么就使用ThreadLocal把变量变为线程私有的
- 如果bean的实例变量或类变量需要在多个线程之间共享，那么就只能使用synchronized、lock、CAS等这些实现线程同步的方法了

### SpringBoot配置
- 整合pom.xml
- application.properties或application.yml
- 编写xml文件(如果需要)
- 编写配置类
- 本质上还是基于Spring的Bean管理

## Spring-AOP
- JDK动态代理
- cglib代理

## Spring-AOP步骤
- 编写切面类实现通知接口
- 配置xml文件
- 配置目标类
- 配置通知
- 配置切面（通知+切入点）
- 配置自动代理

## AspectJ-AOP
### 注解（Annotation）
- 编写切面类（@Aspect）
- 编写通知（@Before @AfterReturning @After）
- 编写切入点（@Pointcut）
- 通知+切入点配置切面（@Before(value="myPointcut1()")）

### XML方式
- 编写目标类
- 编写切面类
- 配置XML文件
- 配置目标类
- 配置切面类
- 配置切入点
- 配置切面

## Spring事务
-|-
PROPAGATION_REQUIRED | 支持当前事务，如果当前没有事务，就新建一个事务。这是最常见的选择。
PROPAGATION_SUPPORTS | 支持当前事务，如果当前没有事务，就以非事务方式执行。 
PROPAGATION_MANDATORY | 支持当前事务，如果当前没有事务，就抛出异常。 
PROPAGATION_REQUIRES_NEW | 新建事务，如果当前存在事务，把当前事务挂起。 
PROPAGATION_NOT_SUPPORTED | 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。 
PROPAGATION_NEVER | 以非事务方式执行，如果当前存在事务，则抛出异常。 
PROPAGATION_NESTED | 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则进行与PROPAGATION_REQUIRED类似的操作。


