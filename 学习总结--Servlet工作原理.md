## Servlet
- Servlet如何被加载
- Servlet如何被初始化
- Servlet如何工作
- web.xml详解

### Servlet如何被加载
- Tomcat容器启动，所有的容器都会继承 Lifecycle 接口，所有容器同时启动 
- Context容器调用StandardContext的start()启动
- ContextConfig类会负责整个Context容器（web应用）的配置文件的解析工作
- Context容器的会执行 startInternal方法后，servlet被加载

### Servlet如何被初始化
- ContextConfig的configureStart实现Web应用的初始化，应用的初始化主要是要解析web.xml文件
- web.xml文件描述了一个Web应用的关键信息，也是一个Web应用的入口
- WebXml对象中的属性设置到Context容器中，这里包括创建Servlet 对象、filter、listener 等等
- Servlet 包装成 Context 容器中的 StandardWrapper
- 初始化Servlet在StandardWrapper的initServlet方法中，这个方法很简单就是调用Servlet的init的方法

### Servlet如何工作
- 浏览器向服务器发起一个请求，http://hostname:port/contextpath/servletpath
- hostname 和 port 是用来与服务器建立 TCP 连接
- 后面的URL才是用来选择服务器中那个子容器服务用户的请求
- 当 Request 进入 Container 容器之前，它要访问那个子容器这时就已经确定
- 映射工作有专门一个类来完成的，这个就是 org.apache.tomcat.util.http.mapper

### web.xml配置参数
- context-param：声明应用范围内的初始化参数（Context容器参数）
- filter：过滤器
- listener：监听器
- servlet：wrapper

### web.xml 监听类
- ServletContextAttributeListener
- ServletRequestAttributeListener
- ServletRequestListener
- HttpSessionAttributeListener 
- ServletContextListener(LifecycleListeners)
- HttpSessionListener(LifecycleListeners)

### filter工作原理
- Filter接口中有一个doFilter方法
- 编写Filter，并配置对哪个web资源进行拦截
- WEB服务器每次在调用Servlet（JSP）的service方法之前，都会先调用一下filter的doFilter方法
- Tomcat调用doFilter方法时，会传递一个filterChain对象进来
- 执行filterChain对象的dofilter方法，放行请求

### 总结
>在web.xml中配置的url-pattern就是wrapper容器的地址;请求过来之后，先通过connector容器，建立http连接;然后将请求封装转发给container容器，首先经过engine容器处理，然后通过host容器，在通过context容器，这个就是具体的应用程序了;最后到达wrapper容器，然后通过FilterChain调用所有的dofilter和servlet的service方法，完成请求的处理，然后将处理结果通过response返回。注意每个servlet对应一个wrapper容器，最后url对应到具体的servlet上，在servlet处理具体的请求。**Servlet本身设计不是单例的，但是Tomcat容器会维护Servlet成单例**