###  Spring-data-jpa使用
#### 配置使用
- 原文链接：
[https://www.cnblogs.com/chenpi/p/6357527.html](https://www.cnblogs.com/chenpi/p/6357527.html)
- 项目源码：
[https://github.com/peterchenhdu/spring-data-jpa-example](https://github.com/peterchenhdu/spring-data-jpa-example)
#### Spring-data-jpa特性及优点
- 根据实体类生成数据库表
- JpaRepository提供单表基本CRUD
- 基本的业务逻辑操作
#### Spring-data-jpa功能
Spring data JPA提供给用户使用的，主要有以下几个接口：

- Repository：仅仅是一个标识，表明任何继承它的均为仓库接口类，方便Spring自动扫描识别
- CrudRepository：继承Repository，实现了一组CRUD相关的方法
- PagingAndSortingRepository：继承CrudRepository，实现了一组分页排序相关的方法
- JpaRepository：继承PagingAndSortingRepository，实现一组JPA规范相关的方法
- JpaSpecificationExecutor：比较特殊，不属于Repository体系，实现一组JPA Criteria查询相关的方法
#### Spring-data-jpa查询
- 使用 @Query 创建查询
- 使用@NamedQueries创建查询
- 通过解析方法名创建查询

![](https://images2015.cnblogs.com/blog/615156/201601/615156-20160131220456193-145289785.png)
#### Spring-data-jpa进阶  动态查询、多表关联查询
- 示例链接：[https://www.cnblogs.com/cmfwm/p/8109433.html](https://www.cnblogs.com/cmfwm/p/8109433.html)
- EntityManager
- CriteriaBuilder
- CriteriaQuery
- Root
#### Hibernate和Mybatis对比
- Hibernate（对象设计先行）
	- 分析、抽象和归纳出系统中的业务概念，并梳理出各个业务概念之间的关系——创建概念模型
	- 根据概念模型，进一步细化设计系统中的对象类以及类的依赖关系——创建设计模型
	- 将设计好的类映射到数据库的表和字段配置好
	- hibernate可以根据配置信息自动生成数据库表，这个时候也可以集中精力去梳理一下表关系，看看表结构是否合理，并适当调整一下类和表的映射关系，重新生成表结构
- Mybatis（数据库设计先行）
	- 综合整个系统分析出系统需要存储的数据项目，并画出E-R关系图，设计表结构
	- 根据上一步设计的表结构，创建数据库、表
	- 编写MyBatis的SQL 映射文件、Pojos以及数据库操作对应的接口方法
- Mybatis-Generator+通用mapper已经完成了单表操作零sql对标Spring data jpa
- 复杂查询Spring-data-jpa更加繁琐
