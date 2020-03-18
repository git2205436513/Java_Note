# JavaWeb ver 1.0
```
2020/3/3 fanfuli
在此感谢狂神说java老师
```
## 1.基本概念
### 1.1前言
web开发：
web 网页
静态web：数据不会变化
动态web：数据会发生变化

在java中，动态web资源开发的技术统称为javaweb

### 1.2web应用程序
含义：可以提供浏览器访问的程序
组成：（静态web，动态web）
html css js
jsp servlet
java程序
jar包
配置文件（Properties）

web应用程序编写完毕后，若想提供给外界访问需要一个服务器来统一管理

### 1.3静态web
缺点：无法动态更新，所有用户看到的都是同一个页面
(解决：轮播图、点击特效：伪动态
          JS VBS)
          它无法和数据交互（数据无法持久化，用户无法交互）

### 1.4动态web
缺点：服务器的动态web资源出现了错误，我们需要重新编写后台程序，重新发布。
优点：可以动态更新，可以与数据库交互。

---

## 2Web服务器

### 2.1 技术讲解
ASP：微软，最早流行 在HTML中嵌入了VB脚本 页面混乱
JSP/Servlet：sun公司主推B/S架构，基于java ，可承载三高问题（高并发，高可用，高性能）
	     语法像ASP
PHP：开发速度快，功能强大，跨平台，代码简单，无法承载大访问量

### 2.2Web服务器
服务器是一种被动的操作，用来处理用户的一些请求和给用户一些响应信息

lls
微软 window自带
tomcat
是一个免费的开放源代码的Web应用服务器，始于轻量级应用服务器，是开发和调试JSP程的
首选，属于Apache软件基金会，技术先进、稳定、免费。

---

## 3Tomcat
### 3.1常用端口号 tomcat 8080
```
mysql 3306
http 80
https 443
```	   
默认主机名为 localhost 127.0.0.1
默认网站应用存放位置为：webapps

### 3.2网站是如何进行访问的：
url输入 回车-》检查本机的host的配置文件有没有这个域名映射，有则直接返回对应ip地址
-》没有则访问DNS服务器（本地-》根,并存入浏览器DNS缓存），找到则返回对应地址，否则返回错误

---

## 4.Http
### 4.1什么是Http
Http（超文本传输协议）是一个简单的请求-响应协议，它通常运行在TCP之上。
文本：html、字符串
超文本：图片、音乐、视频、定位、地图
Https：s代表安全的

### 4.2http的两个时代
http1.0
	http/1.0 客户端可以与Web服务器链接，只能获得一个web资源，断开链接。
http2.0
	http/1.1 客户端可以与Web服务器链接，只能获得多个web资源。

### 4.3Http请求
客户端--发请求--服务器

请求地址
方法 ：post/get
状态码
远程地址

请求方式类型 Get Post HEAD DELETE TRACT....
Get：请求能够携带的参数比较少，大小有限制，会在浏览器的URL地址栏显示，不安全，但高效
post：请求能够携带的参数无限制，大小无限制，不会在浏览器的URL地址栏显示，安全但低效

状态码 
200 成功
3** 请求重定向
      将请求重新定位到新位置	
404 找不到资源
5** 服务器代码错误


### 4.4Http响应
服务器--响应--客户端

缓存控制
连接
编码 GBK UTF-8 GB2312 
类型

---

## 5.Servlet
### 5.1 Servlet简介
Sun公司开发动态web的一门技术
Sun在java api中提供了一个接口叫做Servlet，如想想要开发Servlet需要做两件事
编写一个类，实现Servlet接口
把开发好的Java类部署到web服务器中

### 5.2编写一个servlet程序
1.编写一个普通类

2.实现servlet接口，这里直接继承HttpServlet（sun公司关于servlet已经实现了两个接口GenericServlet和HttpServlet）

3.编写Servlet的映射（编写的是JAVA程序，但是要通过浏览器访问，而浏览器需要4.连接web服务器，所以我们需要在web服务中注册我们的写的servlet，还需要给它一个浏览器能够访问的路径）

5配置tomcat

6启动测试

### 5.3Servlet原理

### 5.4Mapping问题
一个请求可以指定一个映射路径
一个请求可以指定多个映射路径
一个请求可以指定通用映射路径 （使用通配符*）
指定前缀或后缀等等（*.funi）（*前不能加项目映射路径）
指定固有路径优先级最高，找不到则走*路径

### 5.5ServletCOntext
web容器在启动的时候，它会为每个web程序创建一个对应的ServletContext对象，它代表了当前的web应用：
+ 共享数据：在这个Servlet中保存的数据可以在另一个Servlet中拿到。
+ 获取初始化参数：在web.xml中可以设置初始参数，从而可以在Servlet中通过context拿到
+ 请求转发：
+ 读取资源文件

### 5.6HttpServletRespose
web服务器接收到客户端的http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest对象，一个代表响应的HttpServletRespose；
+ 如果要获取客户端请求来的参数：找HttpServletRequest
+ 如果要给客户端响应一些信息：找HttpServlerRespose

#### 1 简单分类
负责向浏览器发送数据的方法
```
+  getOutputStream() throws IOException;
+  getWriter() throws IOException;
```
负责向浏览器发送响应头的方法
```
+ void setCharacterEncoding(String var1);
+ void setContentLength(int var1);
+ void setContentLengthLong(long var1);
+ void setContentType(String var1);
+ void setDateHeader(String var1, long var2);
+ void addDateHeader(String var1, long var2);
+ void setHeader(String var1, String var2);
+ void addHeader(String var1, String var2);
+ void setIntHeader(String var1, int var2);
+ void addIntHeader(String var1, int var2);
```
状态码
```
+ int SC_CONTINUE = 100;
+ int SC_SWITCHING_PROTOCOLS = 101;
+ int SC_OK = 200;
+ int SC_CREATED = 201;
+ int SC_ACCEPTED = 202;
+ int SC_NON_AUTHORITATIVE_INFORMATION = 203;
+ int SC_NO_CONTENT = 204;
+ int SC_RESET_CONTENT = 205;
+ int SC_PARTIAL_CONTENT = 206;
+ int SC_MULTIPLE_CHOICES = 300;
+ int SC_MOVED_PERMANENTLY = 301;
+ int SC_MOVED_TEMPORARILY = 302;
+ int SC_FOUND = 302;
+ int SC_SEE_OTHER = 303;
+ int SC_NOT_MODIFIED = 304;
+ int SC_USE_PROXY = 305;
+ int SC_TEMPORARY_REDIRECT = 307;
+ int SC_BAD_REQUEST = 400;
+ int SC_UNAUTHORIZED = 401;
+ int SC_PAYMENT_REQUIRED = 402;
+ int SC_FORBIDDEN = 403;
+ int SC_NOT_FOUND = 404;
+ int SC_METHOD_NOT_ALLOWED = 405;
+ int SC_NOT_ACCEPTABLE = 406;
+ int SC_PROXY_AUTHENTICATION_REQUIRED = 407;
+ int SC_REQUEST_TIMEOUT = 408;
+ int SC_CONFLICT = 409;
+ int SC_GONE = 410;
+ int SC_LENGTH_REQUIRED = 411;
+ int SC_PRECONDITION_FAILED = 412;
+ int SC_REQUEST_ENTITY_TOO_LARGE = 413;
+ int SC_REQUEST_URI_TOO_LONG = 414;
+ int SC_UNSUPPORTED_MEDIA_TYPE = 415;
+ int SC_REQUESTED_RANGE_NOT_SATISFIABLE = 416;
+ int SC_EXPECTATION_FAILED = 417;
+ int SC_INTERNAL_SERVER_ERROR = 500;
+ int SC_NOT_IMPLEMENTED = 501;
+ int SC_BAD_GATEWAY = 502;
+ int SC_SERVICE_UNAVAILABLE = 503;
+ int SC_GATEWAY_TIMEOUT = 504;
+ int SC_HTTP_VERSION_NOT_SUPPORTED = 505
```
### 常见应用
#### 1.向浏览器输出消息
#### 2.下载文件
    1.获取下载文件路径
    2.指定下载文件名
    3.让浏览器支持
    4.获取下载文件的输入流
    5.创建缓冲群
    6.获取OutputStream对象
    7.将FileOutputStream流写入到buffer缓冲区
    8.使用OutputStream将缓冲区中的数据输出到客户端
    ```
    public class FileServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
      //        1.获取下载文件路径
        String realPath = "D:\\Study Files\\Java rear end\\javawebstudy\\upload\\src\\main\\resource\\imgs\\1.png";
        System.out.println("下载路径"+realPath);
        //        2.指定下载文件名
        String filename = realPath.substring(realPath.lastIndexOf("\\")+1);
      //        3.让浏览器支持
        resp.setHeader("Content-Disposition","attachment;filename="+filename);
      //        4.获取下载文件的输入流
        FileInputStream in = new FileInputStream(realPath);
      //        5.创建缓冲群
        int len = 0;
        byte[] buffer = new byte[1024];
      //        6.获取OutputStream对象
        ServletOutputStream out = resp.getOutputStream();
        //        7.将FileOutputStream流写入到buffer缓冲区
        while ((len = in.read(buffer))>0){
            out.write(buffer,0,len);
        }
        in.close();
        out.close();
      //        8.使用OutputStream将缓冲区中的数据输出到客户端

    }


}
    ```
#### 3验证码功能
+ 前端实现
+ 后端实现，需要图片功能
```
public class ImgServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //如何让浏览器5秒刷新一次
        resp.setHeader("refresh","3");
        //在内存中创建一个图片
        BufferedImage bufferedImage = new BufferedImage(80,20,BufferedImage.TYPE_INT_RGB);
        //得到图片
        Graphics2D g = (Graphics2D) bufferedImage.getGraphics();
        //设置背景颜色
        g.setColor(Color.white);
        g.fillRect(0,0,80,20);
        //给图片写数据
        g.setColor(Color.red);
        g.setFont(new Font(null,Font.BOLD,20));
        g.drawString(makeNum(),0,20);

        //告诉浏览器，这个请求用图片的形式打开
        resp.setContentType("image/png");
        //网站存在缓存 不让浏览器缓存
        resp.setDateHeader("expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","no-cache");

        //把图片写给浏览器
        ImageIO.write(bufferedImage,"png",resp.getOutputStream());
    }


    private String makeNum(){
        Random rand = new Random();
        String num = rand.nextInt(9999999)+"";
        StringBuffer buffer = new StringBuffer();
        for (int i = 0; i < 7-num.length(); i++) {
            buffer.append("0");
        }
        num = buffer.toString() + num;
        return num;
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

}
}

```
#### 4实现重定向
一个web资源收到客户端请求后，它会通知客户端去访问另外一个web资原，
这个过程叫做重定向。
```
void sendRedirect（String var1）throws IOException
/*
    resp.sendRedirect("/r/img");
    等价于 resp.setHeader("Location","/r/image");
           resp.setStarus(302);
*/
```
重定向和转发的区别
相同点：页面都会跳转
不同点：请求转发，url不会变化 状态码是307，重定向则相反 状态码是302

### 5.7HttpServletRequest
HttpServletRequest代表客户端请求，用户通过http协议来访问服务器，http请求中的所有信息会被封装到HttpServletRequest，通过HttpServletRequrest方法，获得客户端的所有信息
#### 获取前端传递的参数，请求转发

---

## 6 Cookie、Session
### 6.1 会话
**会话**：用户打开一个浏览器，点击很多超链接，访问多个web资源，关闭浏览器，这个过程可以称之为会话

**状态会话**：用户访问网站（网站给cookie），下次再进入网站，网站会知道你曾经来过（匹配session）。
### 6.2 保存会话的两种技术
#### cookie
- 客户端技术（响应与请求）


#### session
- 服务器技术，我们可以把信息数据放在session中

常见：网站登录后，你下次登录会自动登录

### 6.3cookie
1.从请求中拿到cookie信息
2.服务器响应给客户端cookie
```
public class CookieDemo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设置编码
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");

        PrintWriter out = resp.getWriter();

        Cookie[] cookies = req.getCookies(); //获得cookie

        if(cookies != null){
            for (Cookie cookie : cookies) {
                if(cookie.getName().equals("LoginTime")){ //获得cookie的key
                    String time = cookie.getValue(); //获得cookie的value
                    long l = Long.parseLong(time);
                    Date date = new Date(l);
                    out.write("您上一次进入的时间为"+date.toLocaleString());
                }
            }
        }else {
           out.write("欢迎第一次进入网站");
        }

        //服务器设置cookie
        Cookie cookie = new Cookie("LoginTime",System.currentTimeMillis()+""); //新建cookie
        cookie.setMaxAge(60*60*24); //设置cookie时间
        resp.addCookie(cookie);//添加cookie

    }

```
**cookie:一般会保存在本地的用户目录下appdate**

一个网站的cookie是否存在上限？
- 一个cookie只能保存一个信息
- 一个web站点可以给浏览器发送多个cookie，最多20个
- Cookie大小有限制4kb
- 300个cookie浏览器上限

删除Cookie
- 不设置有效期，关闭浏览器，自动失效
- 设置有效期时间为0


### 6.4Session（重点）
#### 是什么？
- 服务器会给每一个用户（浏览器）创建一个Session对象，session在网站打开得时候就已经存在了
- 一个Session独占一个浏览器，只要浏览器没有关闭，这个Session就存在
- 用户登录之后，整个网站都可以访问
```
public class SessionDemo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决编码问题
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset= utf-8");

        //获取session
        HttpSession session = req.getSession();
        //设置session
        session.setAttribute("name","fanfuni");
        //获得session值
        String id = session.getId();
        //判断session是否已经存在了
        if(session.isNew()){
            resp.getWriter().write("Session创建成功，服务器中的id"+id);
        }else{
            resp.getWriter().write("Session已经存在"+id);
        }
    }

```
#### session的销毁
- session.removeAttribute()适用于清空指定的属性 
- session.invalidate()是清除当前session的所有相关信息
- 当然你也可以去web.xml中配置session的失效时间
```
这表示session 5分钟销毁一次
<session-config>
    <session-timeout>5</session-timeout>
  </session-config>
```
#### session和cookie的区别
- Cookie时把用户的数据写给用户的浏览器，浏览器保存（可以保存多个）
- Session把用户的数据写到用户独占Session中，服务器端保存（保存重要信息，避免服务器资源浪费）
- Session对象由服务器在创建

#### 使用场景
- 保存一个登录用户信息
- 购物车信息
- 在整个网站中经常使用的数据

---
## 7JSP
### 7.1是什么？
Java Server Pages：java服务端界面，和Servlet一样，用于动态web技术

特点：
- 写jsp就像写HTML
- 区别：Html只提供静态的数据，JSP页面中可以嵌入JAVA代码，为用户提供动态数据。

### 7.2原理
浏览器向服务器发送请求，不管访问什么资源，其实都是在访问Servlet。

JSP最终也会被转为一个java类

JSP其实就是一个Servlet
进入idea目录查看jsp的代码
```
C:\Users\Alex FAN\.IntelliJIdea2019.2\system\tomcat\Unnamed_javawebstudy\work\Catalina\localhost\s5\org\apache\jsp
每个人目录可能不同，但大体应该都是这样
```
```
public final class index_jsp extends org.apache.jasper.runtime.HttpJspBase

public abstractclass HttpJspBase extends HttpServlet
implements HttpJspPage
```
jsp内置的一些对象
```
    final javax.servlet.jsp.PageContext pageContext; 页面上下文
    javax.servlet.http.HttpSession session = null; session
    final javax.servlet.ServletContext application; application
    final javax.servlet.ServletConfig config; config
    javax.servlet.jsp.JspWriter out = null; out
    final java.lang.Object page = this; page
    HttpServletRequest request；请求
    HttpServletResponse response; 响应
    
```
输出页面前增加的代码
```
response.setContentType("text/html"); //设置响应的页面类型
      pageContext = _jspxFactory.getPageContext(this, request, response,
      			null, true, 8192, true);
      _jspx_page_context = pageContext;
      application = pageContext.getServletContext();
      config = pageContext.getServletConfig();
      session = pageContext.getSession();
      out = pageContext.getOut();
      _jspx_out = out;
```
在jsp中 java代码会原封不动的输出，html代码会被out.wirte转义输出
```
out.write("<html>\n");
      out.write("<body>\n");
      out.write("<h2>Hello World!</h2>\n");
      out.write("\n");

    String name = "fanfuni";

      out.write("\n");
      out.write("</body>\n");
      out.write("</html>\n");
```
### 7.3 基础语法
**jsp表达式**
```
<%= java变量/表达式%>
<%= new java.util.Date()%>
```
**jsp脚本片段**
```
<%
    int sum = 0;
    for(int i = 0;i <= 100;i++){
        sun+=i;
    }
    out.println("<h1>Sum="+sun+"</h1>");
%>
```
**jsp声明**
```
<%!
    static{
        System.out.println("Loading Servlet!");
        }
    private int globalvar = 0;
    
    public void kuang(){
        System.out.println;
    }
%>
```
JSP声明：会被编译到JSP生成的java类中，其余的会被生成到jsoService方法中

jsp的注释不会在页面中显示，html的则会

### 7.4JSP指令
```
<%@page args...%>
<%@include file=""%>

jsp标签
<jsp:include page = "xxxxx"/>

@include会将两个界面合二为一，jsp：include 则是拼接界面
```

### 7.5 九大内置对象
- PageContext：通过该对象可以获取其他对象
- Request：封装客户端请求
- Response：封装服务器响应
- Session：封装用户会话
- Application（ServletContext）：封装服务器运行环境
- config（ServletConfig）：Web应用的配置对象
- out：输出服务器响应的对象
- exception：封装页面抛出异常的对象
- page：JSP页面本身

#### 作用域比较
PageContext：本页面
Request：该请求
Session：该会话 从页面打开到关闭
Application：该服务器 从服务器打开到关闭

## 7.6 jsp标签、jstl标签、el表达式
### EL表达式：${}
- 获取数据
- 执行预算
- 获取web开发的常用对象

- EL表达式获取表单中的数据 ${param.参数名}
### jsp标签
-jsp：include
-jsp：forward
-jsp：param

### jstl表达
- JSP标准标签库（JSTL）是一个JSP标签集合，它封装了JSP应用的通用核心功能，用于弥补html标签的不足

#### 核心标签
-<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 需要引入后才能使用jstl
- pom.xml引入相应依赖
- tomcat中也需要引入对应的包
- **<c:out>**	用于在JSP中显示数据，就像<%= ... >
- **<c:set>**	用于保存数据
- <c:remove>	用于删除数据
- <c:catch>	用来处理产生错误的异常状况，并且将错误信息储存起来
- **<c:if>**	与我们在一般程序中用的if一样
- <c:choose>	本身只当做<c:when>和<c:otherwise>的父标签
- <c:when>	<c:choose>的子标签，用来判断条件是否成立
- <c:otherwise>	<c:choose>的子标签，接在<c:when>标签后，当<c:when>标签 判断为false时被执行
- <c:import>	检索一个绝对或相对 URL，然后将其内容暴露给页面
- **<c:forEach>**	基础迭代标签，接受多种集合类型
- <c:forTokens>	根据指定的分隔符来分隔内容并迭代输出
- **<c:param>**	用来给包含或重定向的页面传递参数
- <c:redirect>	重定向至一个新的URL.
- <c:url>	使用可选的查询参数来创造一个URL


#### 格式化标签
#### sql标签
#### xml标签

## 8 JavaBean
实体类
JavaBean有特定的写法：
- 必须要有一个无参构造
- 属性必须私有化
- 必须有对应的get/set方法

一般用来和数据库的字段做映射 ORM（对象关系映射）：
- 表---类
- 字段---属性
- 行记录 ---对象


## 9 MVC
MVC： Model（模型）、view（视图）、Controller（控制器） 
- M：Service（业务逻辑）、pojo（CRUD）
- V：jsp（展示数据、提供链接发送Servlet请求）
- C：Servlet（接受用户请求、交给业务层处理对应的代码、控制视图的跳转）

---

## 10 Filter 过滤器
Filter:过滤器（处理中文乱码、登录验证....）

Filter
- 导入依赖包（jsp、servlet、jstl、jsp、standard...）
- 实现过滤器接口，重写对应方法
```
public class FilterDemo  implements Filter {
    @Override
    //初始化过滤器
    public void init(FilterConfig filterConfig) throws ServletException {
        
    }

    @Override
    //执行过滤器
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
    
        //过滤器中一定要执行该方法以便程序的继续进行
        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    //销毁过滤器
    public void destroy() {

    }
}
```
- 在web.xml中配置Filter
```
<filter>
    <filter-name>filterdemo</filter-name>
    <filter-class>com.funi.servlet.FilterDemo</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>filterdemo</filter-name>
    <!--配置那些请求会被过滤    -->
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```

过滤器在服务器开启时被创建，关闭时被销毁

---


## 12 listen 监听器
- 编写一个监听器：实现监听器的接口
```
public class ListenerDemo implements HttpSessionListener {
    @Override
    //创建session监听，每次创建session都会进行操作
    public void sessionCreated(HttpSessionEvent httpSessionEvent) {

    }

    @Override
    //销毁session监听，每次销毁session都会进行操作
    public void sessionDestroyed(HttpSessionEvent httpSessionEvent) {

    }
}

```
- 配置监听器
```
  <listener>
    <listener-class>com.funi.servlet.ListenerDemo</listener-class>
  </listener>
```
- 看情况是否使用