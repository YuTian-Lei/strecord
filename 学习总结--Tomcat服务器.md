## web服务器 -- Tomcat浅析系统架构
### 定义
>Web服务器一般指网站服务器，是指驻留于因特网上某种类型计算机的程序，可以向浏览器等Web客户端提供文档，也可以放置网站文件，让全世界浏览；可以放置数据文件，让全世界下载。

### 分类
- Http服务器 nginx
- 应用服务器 Tomcat

### Tomcat关键组件
>Tomcat是一个基于组件的服务器，他的构建组件都是可以配置的，其中最外层的组件是Catalina Servlet容器(Container)

- Connector 
	- port：指定服务器端要创建的端口号，并在这个断口监听来自客户端的请求(8080)
	- minProcessors：服务器启动时创建的处理请求的线程数 
	- maxProcessors：最大可以创建的处理请求的线程数 
	- enableLookups：如果为true，则可以通过调用request.getRemoteHost()进行DNS查询来得到远程客户端的实际主机名
	- redirectPort：指定服务器正在处理http请求时收到了一个SSL传输请求后重定向的端口号 
	- acceptCount 指定当所有可以使用的处理请求的线程数都被使用时，可以放到处理队列中的请求数，超过这个数的请求将不予处理 
	- connectionTimeout：指定超时的时间数(以毫秒为单位) 
- Engine：表示指定service中的请求处理机，接收和处理来自Connector的请求
- Context：表示一个web应用程序：
	- docBase 应用程序的路径或者是WAR文件存放的路径 
    - path 表示此web应用程序的url的前缀，这样请求的url为http://localhost:8080/path/**** 
    - reloadable 这个属性非常重要，如果为true，则tomcat会自动检测应用程序的/WEB-INF/lib 和/WEB-INF/classes目录的变化，自动装载新的应用程序，我们可以在不重起tomcat的情况下改变应用程序 
- Host
	- name：指定主机名（localhost）
	- appBase：应用程序基本目录，即存放应用程序的目录
	- unpackWARs：如果为true，则tomcat会自动将WAR文件解压，否则不解压，直接
	- autoDeploy：自动部署

### Tomcat架构
#### Tomcat顶层架构
![](https://img-blog.csdn.net/20180108192645999?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGxnZW4xNTczODc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

- Server：Tomcat中只有一个Server，Server生命周期就是Tomcat生命周期
- Service：Server至少包含一个Service，向外提供服务，由两个核心组件Container和Connector（Tomcat心脏）组成
- Container:一个Service包含一个Container，用于封装和管理Servlet，以及具体处理request请求
- Connector:一个Service包含多个Connector，用于接受请求并将请求封装成Request和Response来具体处理

![](https://img-blog.csdn.net/20180108194817763?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGxnZW4xNTczODc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### Container架构
![](https://img-blog.csdn.net/20180108201104048?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGxnZW4xNTczODc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

- Engine：引擎，用来管理多个站点，一个Service最多只能有一个Engine
- Host：代表一个站点，也可以叫虚拟主机，通过配置Host就可以添加站点
- Context：代表一个应用程序，对应着平时开发的一套程序，或者一个WEB-INF目录以及下面的web.xml文件
- Wrapper：每一Wrapper封装着一个Servlet

#### Tomcat工作原理
1. Connector用于接受请求并将请求封装成Request和Response，然后交给Container进行处理
2. Container处理完之后在交给Connector返回给客户端
##### Connector工作流程
- Connector使用ProtocolHandler来处理请求的
- ProtocolHandler包含了三个部件：Endpoint、Processor、Adapter
- Endpoint：Endpoint用来处理底层Socket的网络连接，用来实现TCP/IP协议
- Processor：用于将Endpoint接收到的Socket封装成Request，用来实现HTTP协议
- Adapter：将请求适配到Servlet容器进行具体的处理

![](https://img-blog.csdn.net/20180108205854139?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGxnZW4xNTczODc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##### Container工作流程
##### 概念介绍
- Container处理请求是使用Pipeline-Valve管道，Pipeline-Valve是责任链模式
- 每个Pipeline都有特定的Valve，叫做BaseValve，在管道的最后一个执行，BaseValve是不可删除的
- 在上层容器的管道的BaseValve中会调用下层容器的管道
- Engine：StandardEngineValve
- Host：StandardHostValve
- Context：StandardContextValve
- Wrapper：StandardWrapperValve

![](https://img-blog.csdn.net/20180116093931129?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGxnZW4xNTczODc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##### 工作流程
1. Connector在接收到请求后会首先调用最顶层容器的Pipeline来处理，这里的最顶层容器的Pipeline就是EnginePipeline（Engine的管道
2. 在Engine的管道中依次会执行EngineValve1、EngineValve2等等，最后会执行StandardEngineValve，在StandardEngineValve中会调用Host管道，然后再依次执行Host的HostValve1、HostValve2等，最后在执行StandardHostValve，然后再依次调用Context的管道和Wrapper的管道，最后执行到StandardWrapperValve
3. 当执行到StandardWrapperValve的时候，会在StandardWrapperValve中创建FilterChain，并调用其doFilter方法来处理请求，这个FilterChain包含着我们配置的与请求相匹配的Filter和Servlet，其doFilter方法会依次**调用所有的Filter的doFilter方法和Servlet的service方法**，这样请求就得到了处理
4. 当所有的Pipeline-Valve都执行完之后，并且处理完了具体的请求，这个时候就可以将返回的结果交给Connector了，Connector在通过Socket的方式将结果返回给客户端
