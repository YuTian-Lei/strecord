## SpringMVC

### 工作原理
![](C:\Users\lenovo\Desktop\学习总结\img\714509-20170920165202868-445512458.png)
### 拦截器
#### 拦截器配置
#### 拦截器使用
#### 拦截器使用场景
- 日志记录 ：记录请求信息的日志
- 权限检查，如登录检查
- 性能检测：检测方法的执行时间
#### 拦截器和过滤器区别
- 拦截器是基于java的反射机制的，而过滤器是基于函数回调，依赖于Servlet容器
- 过滤器是servlet规范规定的，只能用于web程序中，而拦截器是在spring容器中，它不依赖servlet容器
- 在action的生命周期中，拦截器可以多次被调用，而过滤器只能在容器初始化时被调用一次
- 拦截器可以获取IOC容器中的各个bean，在拦截器里注入一个service，可以调用业务逻辑
### 参数绑定（自定义转换器）
### 数据校验
### 数据返回（自定义消息转换器）
### 注解详解
#### @RequestMapping：路径映射（注解可选参数如下）
- value：表示需要匹配的url的格式表示需要匹配的url的路径
- method：指定请求的method类型， GET、POST、PUT、DELETE等
- consumes：指定处理请求的提交内容类型 （Content-Type），例如 application/json, text/html
- produces：指定返回的内容类型，仅当request请求头中的（Accept）类型包含该指定类型才能返回
- params：指定request 中必须包含某些参数值，才能让该方法执行
- headers：指定request中必须包含某些指定的header值，才能让该方法处理请求
#### @RequestParam：获取请求参数（注解可选参数如下）
> RequestParam 用于将url 中的的参数，映射到方法的参数上

- value：即url 路径中参数的名字
- required：是否必须，默认为 true  ，表示请求中一定要有相应的参数，否者会抛出异常
- defaultValue：默认值，表示若请求中没有同名参数时的默认值，设置该参数时，自动将required设为false

#### @PathVariable:绑定URL模板变量值
#### @RequestBody
>　@RequestBody 是作用在形参列表上，表示将HTTP 请求正文出入到方法中，使用适合HTTPMessageConverter 将请求体写入某个对象
#### @ResponseBody
>使用@RequestMapping 注解后，需要将返回值解析，然后将解析结果为跳转路径,加上@ResponseBody 注解后，返回结果不会被解析，而是直接写入HTTP response body 中

#### @ModelAttribute:用在方法上/用在方法参数列表上
>- @ModelAttribute最主要的作用是将数据添加到模型对象中，用于视图页面展示时使用
>- @ModelAttribute等价于 model.addAttribute("attributeName", abc)
>- 根据@ModelAttribute注释的位置不同，和其他注解组合使用，致使含义有所不同

- @ModelAttribute注释的方法会在此controller每个方法执行前被执行
- @ModelAttribute注释一个方法的参数(1、从model中获取2、将对象绑定到视图)

#### @SessionAttributes：将值放到session 域中
>- 在默认情况下，ModelMap中的属性作用域是request级别，Spring 允许我们有选择地指定 ModelMap 中的哪些属性需要转存到 session 中
>- @ModelAttribute和@SessionAttributes搭配着使用可以用来在controller内部共享 model 属性的
### 返回值
#### String
- 转发(至jsp)：return "userEdit";（走视图解析器,可以用于保护/web-inf下jsp）（默认转发）
- 转发(至jsp)：return "forward:/WEB-INF/login.jsp";（不会走视图解析器，可以访问/web-inf下jsp）
- 重定向(至jsp)：return "redirect:/main.jsp";（不会走视图解析器，不可以访问/web-inf下jsp）
- 转发(至控制器)：return "forward:/login";
- 重定向(至控制器)：return "redirect:/main";
#### ModelAndView
- 转发(至jsp)：mv.setViewName("main");（走视图解析器，默认转发）
- 转发(至jsp)：mv.setViewName("forward:/WEB-INF/main.jsp");
- 重定向(至jsp)：mv.setViewName("redirect:/main.jsp");
- 转发(至控制器)：mv.setViewName("forward:/main");
- 重定向(至控制器)：mv.setViewName("redirect:/main");
#### void
- 转发：request.getRequestDispatcher("xxx").forward(request, response);
- 重定向：response.sendRedirect("/xxx");
### model、modelMap和ModelAndView的区别
>- SpringMVC在调用方法前会创建一个隐含的数据模型，作为模型数据的存储容器， 成为”隐含模型”,即会创建一个model或modelMap
>- springMVC创建的是ExtendedModelMap的实现类，而model和modelMap是它的父接口和父类，均可以接收

- model和modelMap由springMVC框架创建，但无法寻找视图
- ModelAndView需要主动去new对象，可以存放视图路径
### restful风格配置
### SSM配置和SpringBoot配置区别