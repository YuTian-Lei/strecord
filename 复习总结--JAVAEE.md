# JAVA EE总体框架

- spring
- Spring mvc
- mybatis
- Spring Boot


## Spring
### Spring开发常用包
>- Spring：spring-core、spring-context、spring-beans、spring-expression  
>- AOP:aopalliance、spring-aop、aspectjweaver、spring-aspects
>- 测试：junit、spring-test

### Spring IOC原理
1. 工厂+反射+配置文件 解耦
***
	<bean id="us"  Class="com.imooc.UserServiceImpl">
	Class FactoryBean{
		public Static Object getBean(String id){
			...
			反射
		}
	}
2. 控制反转IOC和依赖注入DI
>IOC：将对象的创建交给容器去创建，不再是程序主动的去new一个对象。DI：依赖注入，在依赖对象的spring配置bean中，通过配置属性，将属性的值注入到依赖对象中。

###Spring Bean管理

![](https://img-blog.csdnimg.cn/20190902115849923.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RLX2xUbGVp,size_16,color_FFFFFF,t_70)

#### Bean属性的注入方式

- 构造方法注入
- setter和getter方法
- 接口注入 
### Spring AOP

![](https://img-blog.csdnimg.cn/2019090323063472.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RLX2xUbGVp,size_16,color_FFFFFF,t_70)

>基于AspectJ的AOP实现：重点是配置XML文件和切面类，具体的操作是通过配置切入点和通知类型来完成。


### Spring 事务处理
#### 基于tx命名空间的事务处理
	<!-- spring管理事务 -->
	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource" />
	</bean>
	<!-- Spring的声明式事务管理 -->
	<!-- 设置与事务有关的各种属性 -->
	<tx:advice id="txAdvice" transaction-manager="transactionManager">
		<tx:attributes>
			<tx:method name="insert*" propagation="REQUIRED" read-only="true" />
			<tx:method name="add*" propagation="REQUIRED" read-only="true"/>
			<tx:method name="update*" propagation="REQUIRED" read-only="true" />
			<tx:method name="del*" propagation="REQUIRED" read-only="true" />
			<tx:method name="*" propagation="REQUIRED"/>
		</tx:attributes>
	</tx:advice>
	<!-- 声明切入点 -->
	<aop:config>
		<aop:pointcut id="txPointCuts" expression="execution(* cn.IBeauty.service.*.*(..))" />
		<aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCuts" />
	</aop:config>
#### 基于注解的事务处理
	<!-- (事务管理)transaction manager, use JtaTransactionManager for global tx -->
	<!-- 配置事务管理器 -->
	<bean id="transactionManager"  class="org.springframework.jdbc.datasource.DataSourceTransactionManager">  
	    <property name="dataSource" ref="dataSource" />  
	</bean> 
	<!--======= 事务配置 End =================== -->
	
	<!-- 配置基于注解的声明式事务 -->
	<!-- enables scanning for @Transactional annotations -->
	<tx:annotation-driven transaction-manager="transactionManager" /> 

### SpringMVC

#### Springmvc原理

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy81MjIwMDg3LWQyYTJjNDdkYzMzNWU5MWIucG5n?x-oss-process=image/format,png)

#### Springmvc框架

![](https://img-blog.csdnimg.cn/20190908160659988.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RLX2xUbGVp,size_16,color_FFFFFF,t_70)



## Mybatis 框架

### 基本使用

- @param:多个参数





