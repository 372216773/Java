# ==1. jsp介绍==

Java Server Pages

目的: 用编写html的方式编写动态web应用;

- JSP页面是由HTML语句和嵌套在其中的Java代码组成的一个普通文本文件，JSP 页面的文件扩展名必须为.jsp。
- 在JSP页面中编写的Java代码需要嵌套在<%和%>中，嵌套在<%和%>之间的Java代码被称之为脚本片段（Scriptlets），没有嵌套在<%和%>之间的内容被称之为JSP的模版元素。
- JSP文件就像普通的HTML文件一样，它们可以放置在WEB应用程序中的除了WEB-INF及其子目录外的其他任何目录中，JSP页面的访问路径与普通HTML页面的访问路径形式也完全一样。
- 在JSP页面元素，只需将要输出的变量或表达式直中也可以使用一种称之为JSP表达式的接封装在<%= 和 %>之中，就可以向客户端输出这个变量或表达式的运算结果。在JSP表达式中嵌套的变量或表达式后面不能有分号。    

---

# ==2. JSP的运行原理== 

- WEB容器（Servlet引擎）接收到以.jsp为扩展名的URL的访问请求时，它将把该访问请求交给JSP引擎去处理。
- 每个JSP 页面在第一次被访问时，JSP引擎将它翻译成一个Servlet源程序，接着再把这个Servlet源程序编译成Servlet的class类文件，然后再由WEB容器（Servlet引擎）像调用普通Servlet程序一样的方式来装载和解释执行这个由JSP页面翻译成的Servlet程序。 
- 在WEB应用程序正式发布之前，将其中的所有JSP页面预先编译成Servlet程序

---

# ==3. JSP隐式对象==

- **request**
- **response**
- pageContext
- **session**
- **application**
- exception
- ...

在jsp中提供的这些对象我们是直接可以使用的

~~~java
<h2>
    <% session.setAttribute("name","admin");%>
    <%= session.getAttribute("name")%>   <%= session.getAttribute("name")%>
    <% System.out.println("111111");%>
</h2>
~~~



---

# ==4. Jsp与Servlet的比较==

- JSP是一种以产生网页显示内容为中心的WEB开发技术，它可以直接使用模版元素来产生网页文档的内容。 
- JSP页面的源文件要比Servlet源文件简单，并且JSP页面的开发过程要比Servlet的开发过程简单得多。
- JSP引擎可以对JSP页面的修改进行检测，并自动重新翻译和编译修改过的JSP文件。
- JSP技术是建立在Servlet技术基础之上的，所有的JSP页面最终都要被转换成Servlet来运行。 
- 在大型WEB应用程序的开发中，Servlet与JSP经常要混合使用，各司其职，Servlet通常用作控制组件，并处理一些复杂的后台业务，**JSP则作为显示组件**。

---

# 5. JSP模版元素 

- JSP页面中的静态HTML内容称之为JSP模版元素，在静态的HTML内容之中可以嵌套JSP的其他各种元素来产生动态内容和执行业务逻辑。**(这样做前端就难受)**

- JSP模版元素定义了网页的基本骨架，即定义了页面的结构和外观。

---

# ==6. JSP脚本==

在jsp中把 `<% java代码 %>`,这样的东西称为jsp的脚本;可以在jsp执行;

---

# 7. jsp的表达式

- JSP表达式（expression）提供了将一个java变量或表达式的计算结果输出到客户端的简化方式，它将要输出的变量或表达式直接封装在<%= 和 %>之中

`Current time: <%= new java.util.Date() %> `

- JSP表达式中的变量或表达式的计算结果将被转换成一个字符串，然后被插入进整个JSP页面输出结果的相应位置处。

- JSP表达式中的变量或表达式后面**不能有分号（;）**，JSP表达式被翻译成Servlet程序中的一条out.print(…)语句。

---

# ==8. JSP注释==

- JSP注释的格式

  ~~~java
  <%-- 注释信息 --%>
  ~~~

- JSP引擎在将JSP页面翻译成Servlet程序时，忽略JSP页面中被注释的内容。 

---

# 9. pageContext对象 

用法与其他`当前页`的域对象,用法与其他的域对象一致;

---

# 10. 请求重定向与请求转发

- 请求转发: 从头到尾都是一个请求,服务器内部的转发
- 请求重定向: 响应头(Location)+302(响应码),是两次请求

---

# ==11. JSP指令==

JSP指令（directive）是为JSP引擎而设计的，它们并不直接产生任何可见输出，而只是告诉引擎如何处理JSP页面中的其余部分

JSP指令的基本语法格式：
	`<%@ 指令 属性名="值" %>`

jsp指令的分类:

在jsp2.0中有三个指令:

~~~jsp
<%@ page 属性名="值" %>
<%@ include 属性名="值" %>
<%@ taglib 属性名="值" %>
~~~

---

# 11. jsp标签(了解)

~~~jsp
<jsp:forward>标签用于把请求转发给另外一个资源。
<jsp:param>:标签向这个程序传递参数信息。  
~~~

# ==12. EL表达式==

为了计算和输出存储在标志位置的Java对象的值，JSP2.0引入了一种简洁的语言。

el表达式有四个属性范围:

~~~jsp
四种属性范围（page、request、session、application）
~~~

基本语法为: `${表达式}`

​	表达式的值为null，则在页面中显示为一个空字符串，而不是null

---

# ==13. EL运算符==

![image-20210208145355401](upload/image-20210208145355401.png)

![image-20210208145406738](upload/image-20210208145406738.png)

---

# 14. EL表达式的作用域

使用EL的时候，默认会以一定顺序搜索四个作用域，将最先找到的变量值显示出来

![image-20210208145505073](upload/image-20210208145505073.png)

如果有`${username}`这样一个表达式，它会去依次调用

~~~shell
pageContext.getAttribute(“username”) -> request.getAttribute(“username”) -> session.getAttribute(“username”) -> application.getAttribute(“username”)，只要找到某一个不为空的值则调用它的toString()方法并立刻返回调用结果；如果都没有找到，则返回空字符串（而不是null）
~~~

---

# 15. EL表达式的查找域

~~~shell
pageScope: 从pageContext域查找
requestScope:从request域对象中查找
sessionScope: 从session域查找
applicationScope: 从ServletContext(application)域查找
~~~

~~~shell
<h1>${sessionScope.myname}</h1>
~~~

---

# ==16. 什么是JSTL==

为了实现页面无`脚本(java代码)`，还要借助于JSTL

JSTL（JavaServerPages Standard Tag Library）JSP标准标签库

![image-20210208151831820](upload/image-20210208151831820.png)

---

# ==17. 在jsp页面中引入和使用jstl标签库==

![image-20210208152339640](upload/image-20210208152339640.png)

~~~java
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
~~~

> if标签

~~~shell
<c:if test="${myname=='admin'}" scope="session" var="mname">
        <h1>你好</h1>
</c:if>

test: 运算表达式
var(可选):接收运算表达式的结果(true|false)的变量名称;
scope(可选):与var搭配使用,声明把var放入哪个域中进行存放;
~~~

> chose...when...when...otherwise

~~~shell
<c:when test="${age}>20">
<h1>-</h1>
</c:when>
<c:when test="${age}>30">
<h1>--</h1>
</c:when>

<c:when test="${age}>40">
<h1>---</h1>
</c:when>
<c:otherwise>
<h1>----</h1>
</c:otherwise>
~~~

**这个标签就相当于java中的 if...else if..else if..else**

> foreach迭代标签

~~~shell
<c:forEach items="${ages}" var="age" begin="0" end="2" step="2">
    <h1>${age}</h1>
</c:forEach>
~~~

items: 数据

var: 遍历出来的每个变量

begin(可选;开始位置):默认为0

end(可选;开始位置):默认为length-1

step:步长

----

# 18. jsp的正确使用姿势

jsp一般我们会把其当成模板来进行使用,不会在jsp页面中进行业务操作;

所以我们在实际开发过程中,一般会使用jsp+servlet来进行结合开发;

![image-20210208161822522](upload/image-20210208161822522.png)

servlet进行请求处理:

~~~java
public class Servlet01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //请求数据库进行业务处理
        Map<String, Object> map = new HashMap<>();
        map.put("name", "admin");
        map.put("age", 20);

        //请求转发,把数据放到request域中
        req.setAttribute("mapData", map);
        req.getRequestDispatcher("/index07.jsp").forward(req, resp);
    }
}
~~~



**jsp负责模型数据填充**

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
        <h1>
            ${mapData.name}
        </h1>
        <h1>
            ${mapData.age}
        </h1>
</body>
</html>
~~~

---

# ==19. jsp一句话总结==

**jsp虽然很强大,但是我们现在的mvc架构中只是把jsp当成view层的模板来使用**



# 20. Json字符串和对象的转换

~~~json
{
    "name":"admin",
    "age":20
}
~~~

~~~java
public class Person{
    private String name;
    private Integer age;
}
~~~

- 自己也可以完成(但是没必要)
- 有一些优秀的开源的转换工具
  - json lib
  - Gson
  - **fastjson(阿里巴巴开源的json库)**
  - ....

![image-20210208170501497](upload/image-20210208170501497.png)

~~~java
Person person = new Person();
person.setName("小明");
person.setAge(20);
person.setBirthday(new Date(System.currentTimeMillis()));

//对象转换为字符串
String jsonStr = JSON.toJSONString(person);

//字符串转为对象
Person person1 = JSON.parseObject(jsonStr, Person.class);

//对象转字符串,带日期格式化
String s = JSON.toJSONStringWithDateFormat(person, JSON.DEFFAULT_DATE_FORMAT);
System.out.println(s);
~~~

---

# 21. 小作业

![作业](upload/作业.png)