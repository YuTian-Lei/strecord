# cookie

## 定义

> Cookie 技术产生源于 HTTP 协议在互联网上的急速发展。随着互联网的快速发展，人们需要更复杂的互联网交互活动，就必须同服务器保持活动状态。为了适应用户的需求，技术上推出了各种保持Web浏览状态的手段，其中就包括了Cookie技术

## 工作原理

- 浏览器第一次访问服务器
- 服务器生成COOKIE,并设置SESSIONID
- 服务器返回COOKIE给浏览器
- 浏览器本地保存COOKIE
- 之后每次HTTP请求浏览器都会将COOKIE发送给服务器端

## cookie构成

| 字段     | 作用                                                         |
| -------- | ------------------------------------------------------------ |
| name     | 一个唯一确定的cookie名称。通常来讲cookie的名称是不区分大小写的 |
| value    | 存储在cookie中的字符串值。最好为cookie的name和value进行url编码 |
| domain   | cookie对于哪个域是有效的。所有向该域发送的请求中都会包含这个cookie信息。这个值可以包含子域(如：yq.aliyun.com)，也可以不包含它(如：.aliyun.com，则对于aliyun.com的所有子域都有效) |
| path     | 表示这个cookie影响到的路径，浏览器跟会根据这项配置，像指定域中匹配的路径发送cookie。 |
| expires  | 失效时间，表示cookie何时应该被删除的时间戳(也就是，何时应该停止向服务器发送这个cookie)。如果不设置这个时间戳，浏览器会在页面关闭时即将删除所有cookie；不过也可以自己设置删除时间。这个值是GMT时间格式，如果客户端和服务器端时间不一致，使用expires就会存在偏差。 |
| max-age  | 与expires作用相同，用来告诉浏览器此cookie多久过期（单位是秒），而不是一个固定的时间点。正常情况下，max-age的优先级高于expires |
| HttpOnly | 告知浏览器不允许通过脚本document.cookie去更改这个值，同样这个值在document.cookie中也不可见。但在http请求中仍然会携带这个cookie。注意这个值虽然在脚本中不可获取，但仍然在浏览器安装目录中以文件形式存在。这项设置通常在服务器端设置 |
| secure   | 安全标志，指定后，只有在使用SSL链接时候才能发送到服务器，如果是http链接则不会传递该信息。就算设置了secure 属性也并不代表他人不能看到你机器本地保存的 cookie 信息，所以不要把重要信息放cookie就对了 |

## 服务端cookie

### 设置cookie

```Java
        //设置COOKIE
        Cookie cookie = new Cookie("JSEESIONID", UUID.randomUUID().toString());
        //设置生效域
        cookie.setDomain(".baidu.com");
        //设置路径,代表根目录
        cookie.setPath("/");
        //如果不设置此项,cookie将不会写入硬盘,而是写入内存中,只在本页面有效
        //-1 代表永久生效
        cookie.setMaxAge(60 * 60 * 24 * 365);
        cookie.setHttpOnly(true);
        response.addCookie(cookie);
```

### 解析cookie

```Java
        //解析cookie
        Cookie[] cookies = request.getCookies();

        for (Cookie ck : cookies) {
            log.info("cookie名称:{}", ck.getName());
            //修改cookie的值
            if ("modify".equals(ck.getName())) {
                ck.setHttpOnly(false);
                ck.setMaxAge(-1);
                ck.setPath("/path");
                response.addCookie(ck);
            }
        }
```

## 浏览器cookie

### 发送规则

> 当我们请求某个URL路径时,浏览器会根据这个**URL路径**将**符合条件**的Cookie放在Request请求头中传回给服务端,服务端通过request.getCookies()来取得所有cookie

### Jquery操作Cookie

```javascript
//添加一个"会话cookie"
$.cookie('the_cookie', 'the_value');
//创建一个cookie并设置有效时间为 7天
$.cookie('the_cookie', 'the_value', { expires: 7 });
//创建一个cookie并设置 cookie的有效路径
$.cookie('the_cookie', 'the_value', { expires: 7, path: '/' });
//读取cookie
$.cookie('the_cookie');
//删除cookie
$.cookie('the_cookie', null); //通过传递null作为cookie的值即可
//可选参数
$.cookie('the_cookie','the_value',{
    expires:7, 
    path:'/',
    domain:'jquery.com',
    secure:true
})　
```

### 参数说明

- expires：（天|日期）有效期；设置整数时，单位是天；也可以设置日期对象作为Cookie的过期日期；
- path：（String）创建该Cookie的页面路径；
- domain：（String）创建该Cookie的页面域名；
- secure：（Booblean）如果设为true，那么此Cookie的传输会要求一个安全协议，例如：HTTPS；

## cookie在跨域中的问题(如何在跨域请求中携带cookie)

### 原因

- cookie路径:不同路径下的URL请求,cookie不能互相访问
- cookie域：不同域下的URL请求,cookie不能互相访问

### 同源策略的限制

1. 不能获取不同源的cookie，LocalStorage 和 indexDB
2. 不能获取非同源的DOM
3. 不能发送非同源的ajax请求。（**准确说应该是可以向非同源的服务器发起请求，但是返回的数据会被浏览器拦截**）

### 解决方案

- nginx反向代理
- jsonp
- cors--前后端搭配(前端设置withCredentials为true，后端设置Header的方式)

### nginx反向代理

```shell
//反向代理原理就是将前端的地址和后端的地址用nginx用同一个地址代理,如5500端口和3000端口都转到3003端口下
server
{
   listen 3003;
   server_name localhost;
   ##  = /表示精确匹配路径为/的url，真实访问为http://localhost:5500
   location = / {
       proxy_pass http://localhost:5500;
   }
   ##  /no 表示以/no开头的url，包括/no1,no/son，或者no/son/grandson
   ##  真实访问为http://localhost:5500/no开头的url
   ##  若 proxy_pass最后为/ 如http://localhost:3000/;匹配/no/son，则真实匹配为http://localhost:3000/son
   location /no {
       proxy_pass http://localhost:3000;
   }
   ##  /ok/表示精确匹配以ok开头的url，/ok2是匹配不到的，/ok/son则可以
   location /ok/ {
       proxy_pass http://localhost:3000;
   }
}
```



### jsonp

#### 服务器

```Java
@WebServlet("/JsonServlet")
public class JsonServlet extends HttpServlet 
{
    private static final long serialVersionUID = 4335775212856826743L;

    protected void doPost(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException 
    {
        String callbackfun = request.getParameter("mycallback");
        System.out.println(callbackfun);    // callbackFun
        response.setContentType("text/json;charset=utf-8");
        
        User user = new User();
        user.setName("yuanfang");
        user.setAge(100);
        Object obj = JSON.toJSON(user);
        
        System.out.println(user);            // com.tz.servlet.User@164ff87
        System.out.println(obj);            // {"age":100,"name":"yuanfang"}
        callbackfun += "(" + obj + ")";    
        System.out.println(callbackfun);    // callbackFun({"age":100,"name":"yuanfang"})
        
        response.getWriter().println(callbackfun);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
            throws ServletException, IOException 
    {
        this.doPost(request, response);
    }

}
```

#### 前端

```javascript
<script type="text/javascript">
function callbackFun(data)
{
    console.log(111);
    console.log(data.name);
    //data.age = 10000000;
    //alert(0000);
}

$("#submit").on("click", function(){     
        $.ajax({
            type:"post",
            url:"http://localhost:8080/html5/JsonServlet",
            dataType:'jsonp',
            jsonp:'mycallback',//传递给后台,以获得jsonp回调函数名
            jsonpCallback:'callbackFun',
            success:function(data) {   
                console.log(data.age);
            }
        });
    })
</script>
```

### cors

> **CORS 需要浏览器和后端同时支持。IE 8 和 9 需要通过 XDomainRequest 来实现**。
>
> 这是因为我们的WEB服务器没有设置`ACCESS-CONTROL-ALLOW-ORIGIN`头部，默认情况下是禁止跨域引用资源的。
>
> **当然这里有一点要注意，其实这个跨域请求是发送成功了的，服务器也有返回test.js内容，只是浏览器禁止Javascript去取response的数据而已**。
>
> **跨域是浏览器行为，不是服务器行为。**实际上，**请求已经到达服务器了，只不过在回来的时候被浏览器限制了**。

#### 服务器

```Java
//跨域请求携带Cookie
//如果需要把Cookie发到服务端，需要指定Access-Control-Allow-Credentials字段为true;
response.setHeader("Access-Control-Allow-Credentials", "true");

//Access-Control-Allow-Origin的值不能为”*”,必须是一个确定的域,与前端配合
response.setHeader("Access-Control-Allow-Origin",request.getHeader("Origin"));

//表明服务器支持的头信息字段
response.setHeader("Access-Control-Allow-Headers","content-type");

//springboot
@Configuration
public class GlobalCorsConfig {
    /**
     * 允许跨域调用的过滤器
     */
    @Bean
    public CorsFilter corsFilter() {
        CorsConfiguration config = new CorsConfiguration();
        //允许所有域名进行跨域调用
        config.addAllowedOrigin("*");
        //允许跨越发送cookie
        config.setAllowCredentials(true);
        //放行全部原始头信息
        config.addAllowedHeader("*");
        //允许所有请求方法跨域调用
        config.addAllowedMethod("*");
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", config);
        return new CorsFilter(source);
    }
}
```

#### 前端

```javascript
$.ajax({
    type: "POST",
    url: "实际的请求地址",
    data: {参数：参数值},
    dataType: "json",
    crossDomain:true, //设置跨域为true
    xhrFields: {
        withCredentials: true //默认情况下，标准的跨域请求是不会发送cookie的
    },
    success: function(data){
        alert("请求成功");      
    }
});
```

## Ajax请求携带Cookie

- **同源：**ajax会自动带上同源的cookie，不会带上不同源的cookie
- **不同源：**前端设置withCredentials为true，后端设置Header的方式让ajax自动带上不同源的cookie

## 服务器跨域请求配置

```Java
//跨域请求配置(此跨域请求配置不能携带Cookie)
//允许跨域的域名，*号为允许所有,存在被 DDoS攻击的可能。
response.setHeader("Access-Control-Allow-Origin","*");

//表明服务器支持的所有头信息字段
response.setHeader("Access-Control-Allow-Headers", "Origin, No-Cache, X-Requested-With, If-Modified-Since, Pragma,Last-Modified, Cache-Control, Expires, Content-Type, X-E4M-With,userId,token");

/** 目前测试来看为了兼容所有请求方式，上面2个必须设 **/

//如果需要把Cookie发到服务端，需要指定Access-Control-Allow-Credentials字段为true;
response.setHeader("Access-Control-Allow-Credentials", "true");

//首部字段 Access-Control-Allow-Methods 表明服务器允许客户端使用 POST, GET和OPTIONS方法发起请求。
//该字段与 HTTP/1.1 Allow: response header 类似，但仅限于在需要访问控制的场景中使用。
response.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE");

//表明该响应的有效时间为 86400 秒，也就是 24 小时。在有效时间内，浏览器无须为同一请求再次发起预检请求。
//请注意，浏览器自身维护了一个最大有效时间，如果该首部字段的值超过了最大有效时间，将不会生效。
response.setHeader("Access-Control-Max-Age", "86400");

// IE8 引入XDomainRequest跨站数据获取功能,也就是说为了兼容IE
response.setHeader("XDomainRequestAllowed","1");
```

