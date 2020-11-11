# Nginx服务器
## Nginx架构
>发展结果变成是一个**模块化的，事件驱动的，异步的，单线程的非阻塞架构**的nginx代码基础

![](https://images2015.cnblogs.com/blog/760221/201602/760221-20160225165312146-1047279011.png)

### 模块化的
>Nginx的模块从结构上分为核心模块、基础模块和第三方模块：

- 核心模块：HTTP模块、EVENT模块和MAIL模块
- 基础模块：HTTP Access模块、HTTP FastCGI模块、HTTP Proxy模块和HTTP Rewrite模块，
- 第三方模块：HTTP Upstream Request Hash模块、Notice模块和HTTP Access Key模块。
>Nginx的模块从功能上分为如下四类：

- Core(核心模块)：构建nginx基础服务、管理其他模块。
- Handlers（处理器模块）：此类模块直接处理请求，并进行输出内容和修改headers信息等操作。
- Filters （过滤器模块）：此类模块主要对其他处理器模块输出的内容进行修改操作，最后由Nginx输出。
- Proxies （代理类模块）：此类模块是Nginx的HTTP Upstream之类的模块，这些模块主要与后端一些服务比如FastCGI等进行交互，实现服务代理和负载均衡等功能。

### Nginx的web请求处理机制
>每个工作进程使用异步非阻塞方式，可以处理多个客户端请求。当某个工作进程接收到客户端的请求以后，调用IO进行处理，如果不能立即得到结果，就去处理其他的请求；

![](https://images2017.cnblogs.com/blog/1183448/201802/1183448-20180210145226654-1347579045.png)
## Nginx工作原理
![](https://img-blog.csdnimg.cn/20191202174733362.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1RLX2xUbGVp,size_16,color_FFFFFF,t_70)

### Nginx功能介绍
#### web服务器
- http请求协议的解析 
- 以http协议格式响应请求
- 缓存
- 日志处理
#### 反向代理服务器
- 请求的转发
- 负载均衡
- 鉴权
- 限流
- 缓存
- 日志处理

## Nginx配置详解
1. 全局块：配置影响nginx全局的指令。一般有运行nginx服务器的用户组，nginx进程pid存放路径，日志存放路径，配置文件引入，允许生成worker process数等。
2. events块：配置影响nginx服务器或与用户的网络连接。有每个进程的最大连接数，选取哪种事件驱动模型处理连接请求，是否允许同时接受多个网路连接，开启多个网络连接序列化等。
3. http块：可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。如文件引入，mime-type定义，日志自定义，是否使用sendfile传输文件，连接超时时间，单连接请求数等。
4. server块：配置虚拟主机的相关参数，一个http中可以有多个server。
5. location块：配置请求的路由，以及各种页面的处理情况。
6. upstream：用于进行负载均衡的配置