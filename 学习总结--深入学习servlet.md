## Servlet生命周期

- 容器加载
- init()
- service()
- destroy ()
- 容器卸载

## Servlet重要的四个生命周期方法

- 构造方法： 创建servlet对象的时候调用。默认情况下，第一次访问servlet的时候创建servlet对象 只调用1次。证明servlet对象在tomcat是单实例的。
- init方法： 创建完servlet对象的时候调用。只调用1次。
- service方法： 每次发出请求时调用。调用n次。
- destroy方法： 销毁servlet对象的时候调用。停止服务器或者重新部署web应用时销毁servlet对象。

## 执行过程

- 客户端发出请求，创建request和response对象
- 根据web.xml文件的配置，找到Servlet类
- 响应请求，访问线程和request和response销毁

## 重要对象

- HttpServletRequest
- HttpServletResponse
- HttpSession