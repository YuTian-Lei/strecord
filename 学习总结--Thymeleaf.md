##  变量
- 标准语法：th:text="${user.name}"
- 不支持HTML5:th:text 换成 data-th-text="${user.name}"
- 不进行格式化输出：th:utext

## 自定义变量
	<h2 th:object="${user}">
    	<p>Name: <span th:text="*{name}">Jack</span>.</p>
    	<p>Age: <span th:text="*{age}">21</span>.</p>
    	<p>friend: <span th:text="*{friend.name}">Rose</span>.</p>
	</h2>
## 方法
	<h2 th:object="${user}">
    	<p>FirstName: <span th:text="*{name.split(' ')[0]}">Jack</span>.</p>
    	<p>LastName: <span th:text="*{name.split(' ')[1]}">Li</span>.</p>
	</h2>

	<p>
  		今天是: <span th:text="${#dates.format(today,'yyyy-MM-dd')}">2018-04-25</span>
	</p>
## Thymeleaf内置对象
对象	| 作用
-|-
ctx	|获取Thymeleaf自己的Context对象
requset	|如果是web程序，可以获取HttpServletRequest对象
response|如果是web程序，可以获取HttpServletReponse对象
session |如果是web程序，可以获取HttpSession对象
servletContext|	如果是web程序，可以获取HttpServletContext对象

## Thymeleaf提供的全局对象
对象|	作用
-|-
dates|	处理java.util.date的 工具 对象
calendars|	处理java.util.calendar的工具对象
numbers|	用来对数字格式化的方法
strings|	用来处理字符串的方法
bools|	用来判断布尔值的方法
arrays|	用来护理数组的方法
lists|	用来处理List集合的方法
sets|	用来处理set集合的方法
maps|	用来处理map集合的方法

## 拼接
	<span th:text="|欢迎您:${user.name}|"></span>

## 运算
- 算术运算：<span th:text="${user.age}%2 == 0"></span>
- 比较运算：gt (>), lt (<), ge (>=), le (<=), not (!). Also eq (==), neq/ne (!=)
- 条件运算：<span th:text="${user.sex} ? '男':'女'"></span>

## 循环

>${users} 是要遍历的集合，可以是以下类型：
>
- Iterable，实现了Iterable接口的类
- Enumeration，枚举
- Interator，迭代器
- Map，遍历得到的是Map.Entry
- Array，数组及其它一切符合数组结果的对象

	<tr th:each="user : ${users}">
    	<td th:text="${user.name}">Onions</td>
    	<td th:text="${user.age}">2.41</td>
	</tr>

>stat对象包含以下属性：
>
- index，从0开始的角标
- count，元素的个数，从1开始
- size，总元素个数
- current，当前遍历到的元素
- even/odd，返回是否为奇偶，boolean值
- first/last，返回是否为第一或最后，boolean值

	<tr th:each="user,stat : ${users}">
    	<td th:text="${user.name}">Onions</td>
    	<td th:text="${user.age}">2.41</td>
	</tr>

## 逻辑判断
	<span th:if="${user.age} < 24">小鲜肉</span>
## 分支控制switch
	<div th:switch="${user.role}">
  		<p th:case="'admin'">用户是管理员</p>
  		<p th:case="'manager'">用户是经理</p>
  		<p th:case="*">用户是别的玩意</p>
	</div>
## JS模板
	<script th:inline="javascript">
    	const user = /*[[${user}]]*/ {};
    	const age = /*[[${user.age}]]*/ 20;
    	console.log(user);
    	console.log(age)
	</script>

	const user = /*[[Thymeleaf表达式]]*/ "静态环境下的默认值";
## URL表达式
	<!— 绝对路径 —>
	<!-- Will produce 'http://localhost:8080/gtvg/order/details?orderId=3' (plus rewriting) -->
	<a href="details.html" th:href="@{http://localhost:8080/gtvg/order/details(orderId=${o.id})}">view</a>

	<!— 相对路径 带参数—>
	<!-- Will produce '/gtvg/order/details?orderId=3' (plus rewriting) -->
	<a href="details.html" th:href="@{/order/details(orderId=${o.id})}">view</a>

	<!-- Will produce '/gtvg/order/3/details' (plus rewriting) -->
	<a href="details.html" th:href="@{/order/{orderId}/details(orderId=${o.id})}">view</a>

## thymeleaf中的模板布局
- 定义fragment
- th:insert:保留自己的主标签，保留th:fragment的主标签。
- th:replace:不要自己的主标签，保留th:fragment的主标签。
- th:include:保留自己的主标签，不要th:fragment的主标签。（官方3.0后不推荐）