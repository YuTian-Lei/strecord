#  tomcat类加载顺序

## 总体结构

### 类加载器架构

![](https://img2018.cnblogs.com/blog/1424165/201905/1424165-20190507181346146-382240906.png)

其中 JDK 内部提供的类加载器分别是：

- **Bootstrap** - 启动类加载器，属于 JVM 的一部分，加载 <JAVA_HOME>/lib/ 目录下特定的文件
- **Extension** - 扩展类加载器，加载 <JAVA_HOME>/lib/ext/ 目录下的类库
- **Application** - 应用程序类加载器，也叫系统类加载器，加载 CLASSPATH 指定的类库

Tomcat 自定义实现的类加载器分别是：

- **Common** - 父加载器是 AppClassLoader，默认加载 ${catalina.home}/lib/ 目录下的类库
- **Catalina** - 父加载器是 Common 类加载器，加载 catalina.properties 配置文件中 server.loader 配置的资源，一般是 Tomcat 内部使用的资源
- **Shared** - 父加载器是 Common 类加载器，加载 catalina.properties 配置文件中 shared.loader 配置的资源，一般是所有 Web 应用共享的资源
- **WebappX** - 父加载器是 Shared 加载器，加载 /WEB-INF/classes 的 class 和 /WEB-INF/lib/ 中的 jar 包
- **JasperLoader** - 父加载器是 Webapp 加载器，加载 work 目录应用编译 JSP 生成的 class 文件

### 具体顺序

1. 最先是$JAVA_HOME/jre/lib/下的jar文件（根类加载器）

2. $JAVA_HOME/jre/lib/ext/下的jar文件。（扩展类加载器）

3. 环境变量CLASSPATH中的jar和class文件。（应用程序类加载器）

4. $CATALINA_HOME/common/classes下的class文件。(common加载器)

5. $CATALINA_HOME/commons/endorsed下的jar文件。(common加载器)

6. $CATALINA_HOME/commons/i18n下的jar文件。(common加载器)

7. $CATALINA_HOME/common/lib 下的jar文件。(common加载器)
（JDBC驱动之类的jar文件可以放在这里，这样就可以避免在server.xml配置好数据源却出现找不到JDBC Driver的情况。）
8. $CATALINA_HOME/server/classes下的class文件。(catalina加载器)
9. $CATALINA_HOME/server/lib/下的jar文件。(catalina加载器)

10. $CATALINA_BASE/shared/classes 下的class文件。(shared加载器)
11. $CATALINA_BASE/shared/lib下的jar文件。(shared加载器)
12. 各自具体的webapp /WEB-INF/classes下的class文件。(WebappClassLoader)
13. 各自具体的webapp /WEB-INF/lib下的jar文件。(WebappClassLoader)



## WebappClassLoader 加载顺序

#### WebappClassLoader 则按规范实现以下顺序的查找并加载

1. 从 JVM 内部的 Bootstrap 仓库加载
2. 从应用程序加载器路径，即 CLASSPATH 下加载
3. **从 Web 程序内的 /WEB-INF/classes 目录**
4. **从 Web 程序内的 /WEB-INF/lib 中的 jar 文件**
5. 从容器 Common 加载器仓库，即所有 Web 程序共享的资源加载

