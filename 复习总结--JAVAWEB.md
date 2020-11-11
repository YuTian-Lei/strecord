# JAVA WEB知识总结
![](https://img-blog.csdnimg.cn/20190926103300220.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RLX2xUbGVp,size_16,color_FFFFFF,t_70)

### Servlet
>Java Servlet实质上是一种小型的、与平台无关的Java类，其生命周期由服务器的Servlet容器管理。当服务器接收到客户端请求时，服务器调用并执行相应的Servlet动态生成响应内容，然后再由服务器返回给客户端。
### JSP
>JSP技术是将Java代码嵌入到HTML代码中形成JSP文件。JSP技术依然是基于Servlet技术的，虽然JSP在编写时与Servlet不一样，但在执行时，JSP首先要由Servlet容器将JSP转换成Servlet并编译，然后才能执行。
### Servlet和JSP的区别
>- 编程方式不同
>- 编译时机不同

### Servlet生命周期
>同一个Servlet对象会同时为多个客户端服务，即多个客户端共享一个Servlet对象，所以Servlet是线程不安全的。
>
> - 初始化阶段：调用init()方法。
> - 响应客户请求阶段：调用Service()方法。
> - 终止阶段：调用destroy()方法。

### Cookie和Session
- Session在服务器端，cookie在客户端(浏览器)
- Session的运行依赖于Session id，Session id存储在Cookie中。
### 会话跟踪
- cookie
- url重写
- 隐藏表单域
### HTTP1.0 HTTP 1.1主要区别
- 长连接
- 节约带宽
- HOST域
### HTTP1.1 HTTP 2.0主要区别
- 多路复用
- 二进制分帧
- 数据压缩
- 服务器推送
### Get和Post的区别
- get把请求的数据放在url上，即HTTP协议头上；post把数据放在HTTP的包体内（requrest body）。
- get提交的数据最大是2k；post理论上没有限制。实际上IIS4中最大量为80KB，IIS5中为100KB。
- GET产生一个TCP数据包，POST产生两个TCP数据包
- GET在浏览器回退时是无害的，POST会再次提交请求。
- GET产生的URL地址可以被Bookmark，而POST不可以。
- GET请求会被浏览器主动cache，而POST不会，除非手动设置。
- GET请求只能进行url编码，而POST支持多种编码方式。
- GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
- GET只接受ASCII字符的参数的数据类型，而POST没有限制
- POST安全性更高
- GET效率更高
- POST不是幂等
### REST请求
POST、DELETE、GET、PUT对应增删查改四种操作
### Request和Response
### 过滤器和监听器