## HTTP协议
### 定义
>http是一个简单的请求-响应协议，它通常运行在TCP之上。它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。相关知识点：URI\Request\Response\状态码
### 请求消息Request
>http请求由三部分组成，分别是：请求行、消息报头(请求头Header)、请求正文(Body)

![](https://upload-images.jianshu.io/upload_images/2964446-fdfb1a8fce8de946.png?imageMogr2/auto-orient/strip%7CimageView2/2)

- 请求行：请求方法（GET\POST\HEAD\PUT\DELETE）、URL、HTTP版本
- 请求头：请求报头允许客户端向服务器端传递请求的附加信息以及客户端自身的信息
- 请求正文：请求数据也叫主体，可以添加任意的其他数据

![](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=2041886745,3108178362&fm=173&app=25&f=JPEG?w=602&h=187&s=E3926822D0A9490906F584DA020080B3)
### 响应消息Response
>HTTP响应也是由三个部分组成，分别是：状态行、消息报头、响应正文

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xODU2NDE5LTZkYjE0MWNmZDM0NmNhMGYuanBn?x-oss-process=image/format,png)

![](https://upload-images.jianshu.io/upload_images/2964446-1c4cab46f270d8ee.jpg?imageMogr2/auto-orient/strip%7CimageView2/2)

- 状态行：由HTTP协议版本号， 状态码， 状态消息 三部分组成。
- 消息报头：用来说明客户端要使用的一些附加信息
- 响应正文：服务器返回给客户端的文本信息。
### 状态码
- 1xx：指示信息--表示请求已接收，继续处理
- 2xx：成功--表示请求已被成功接收、理解、接受
- 3xx：重定向--要完成请求必须进行更进一步的操作
- 4xx：客户端错误--请求有语法错误或请求无法实现
- 5xx：服务器端错误--服务器未能实现合法的请求
### HTTP请求方法
>HTTP1.0定义了三种请求方法： GET, POST 和 HEAD方法。
HTTP1.1新增了五种请求方法：OPTIONS, PUT, DELETE, TRACE 和 CONNECT 方法。

- GET：请求指定的页面信息，并返回实体主体。
- HEAD：类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头
- POST：向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。
- PUT：从客户端向服务器传送的数据取代指定的文档的内容。
- DELETE：请求服务器删除指定的页面。
- CONNECT：HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。
- OPTIONS：允许客户端查看服务器的性能。
- TRACE：回显服务器收到的请求，主要用于测试或诊断。
### HTTP工作原理
- 客户端连接到Web服务器
- 发送HTTP请求
- 服务器接受请求并返回HTTP响应
- 释放连接TCP连接
- 客户端浏览器解析HTML内容
### GET和POST请求的区别
- GET提交的数据会放在URL之后，以?分割URL和传输数据，参数之间以&相连，如EditPosts.aspx?name=test1&id=123456. POST方法是把提交的数据放在HTTP包的Body中.
- 传输数据的大小
- 安全性

### 请求头（Request-Header）
![](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=1285049589,1050572351&fm=173&app=25&f=JPEG?w=602&h=187&s=E78A7E22997848010AF5A4DB020080B3)

- Accept：请求的对象类型。如果是“/”表示任意类型，如果是指定的类型，则会变成“type/”
- Accept-Charset：浏览器可接受的字符集
- Accept-Language：浏览器所希望的语言种类
- Accept-Encoding：浏览器能够进行解码的数据编码方式，比如gzip
- User-Agent：提供了客户端浏览器的类型和版本
- Host：连接的目标主机
- Connection：处理完这次请求后是否断开连接还是继续保持连接
- Content-Length：表示请求消息正文的长度
- Cookie：客户机通过这个头可以向服务器带数据，这是最重要的请求头信息之一
- Authorization：授权信息
- If-Modified-Since：客户机通过这个头告诉服务器，资源的缓存时间
- Referer：客户机通过这个头告诉服务器，它是从哪个资源来访问服务器的(防盗链)
- Pragma：指定“no-cache”值表示服务器必须返回一个刷新后的文档
- From：请求发送者的email地址，由一些特殊的Web客户程序使用，浏览器不会用到它
- Range：Range头域可以请求实体的一个或者多个子范围

### 响应头
![](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=1186800663,3507835201&fm=173&app=25&f=JPEG?w=639&h=206&s=9A8A78229FA849010C75C0D20000C0B1)

- Date：当前的GMT时间
- Content-Type：表示后面的文档属于什么MIME类型
- Content-Length：表示内容长度
- Allow：服务器支持哪些请求方法(如GET、POST等)
- Content-Encoding：文档的编码(Encode)方法
- Expires：告诉浏览器把回送的资源缓存多长时间，-1或0则是不缓存
- Last-Modified：文档的最后改动时间
- Location：这个头配合302状态码使用，用于重定向接收者到一个新URI地址
- Refresh：告诉浏览器隔多久刷新一次，以秒计
- Server：服务器通过这个头告诉浏览器服务器的类型
- Set-Cookie：设置和页面关联的Cookie
- Transfer-Encoding：告诉浏览器数据的传送格式

### 请求头（Request-Header）中的Content-Type字段
#### GET 请求
>GET 请求不存在请求实体部分，键值对参数放置在 URL 尾部，因此请求头不需要设置 Content-Type 字段
>
>Http协议中，如果不指定Content-Type，则默认传递的参数就是application/x-www-form-urlencoded类型。

#### POST 请求
>第一类：raw 原始类型，可以上传任意格式的文本，比如 text、json、xml、html（中文不进行编码）

- text请求：text/plain
- json 请求：application/json
- html 请求：text/html

>application/x-www-form-urlencoded，会将表单内的数据转换拼接成 key-value 对（非 ASCII 码进行编码）

- Content-Type: application/x-www-form-urlencoded

>multipart/form-data，将表单的数据处理为一条消息，以标签为单元，用分隔符分开。既可以上传键值对，也可以**上传文件**

- Content-Type: multipart/form-data;boundary=------FormBoundary15e896376d1

### @RequestParam和@RequestBody区别

#### @RequestParam

- RequestParam解析application/x-www-form-urlencoded编码的内容
-  form 表单，如果不设置 enctype 属性，默认以 application/x-www-form-urlencoded 方式提交数据
- @RequestParam解析get方法参数和post方法中application/x-www-form-urlencoded 参数(包括表单)

#### @RequestBody

- @RequestBody处理**非Content-Type: application/x-www-form-urlencoded编码格式的数据**
- 不能是get请求，无Content-Type
- @RequestBody主要处理Content-Type:application/json内容，请求参数以json格式放在Body中

### response的contentType 几种类型

-  服务端须要返回一段普通文本给client，Content-Type="text/plain"
- 服务端须要返回一段HTML代码给client ，Content-Type="text/html"
- 服务端须要返回一段XML代码给client ，Content-Type="text/xml"
- 服务端须要返回一段javascript代码给client，Content-Type="application/javascript"
- 服务端须要返回一段json串给client，Content-Type="application/Json"

