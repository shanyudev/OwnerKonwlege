#  第一章JSP入门学习

## 一、JSP基础语法

### 1、JSP模板元素

JSP页面中的HTML内容称之为JSP模版元素。JSP模版元素定义了网页的基本骨架，即定义了页面的结构和外观。

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
```

### 2、JSP脚本片段

JSP脚本片断用于在JSP页面中编写多行Java代码（**在<%%>不能定义方法**）。语法：**<%多行java代码 %>**

例如：

```dart
<%
    int num = 0;
    num = ++num;
    out.println("num:" + num);
%>
```



注意：

1、JSP脚本片断中只能出现Java代码，不能出现其它模板元素， JSP引擎在翻译JSP页面中，会将JSP脚本片断中的Java代码原封不动地放到Servlet的_jspService方法中。

2、JSP脚本片断中的Java代码必须严格遵循Java语法，例如，每执行语句后面必须用分号（;）结束。

3、在一个JSP页面中可以有多个脚本片断，在两个或多个脚本片断之间可以嵌入文本、HTML标记和其他JSP元素。

4、多个脚本片断中的代码可以相互访问

### 3、JSP表达式

JSP脚本表达式（expression）用于将程序数据输出到客户端，语法：**<%=变量或表达式 %>**

例如：

```jsp
<%=name %>
```

```bash
<%="123" %>
```

### 4、JSP声明

 JSP页面中编写的所有代码，默认会被编译到servlet的_jspService方法中， 而Jsp声明中的java代码被翻译到_jspService方法的外面。语法：**<%！java代码 %>**

 JSP声明可用于定义JSP页面转换成的Servlet程序的静态代码块、成员变量和方法。

例如：

```csharp
<%!
static { 
    System.out.println("静态代码块"); 
}
 
private String name = "ydlclass";
 
public void TestFun(){
    System.out.println("成员方法！");
}
%>
<%
    TestFun();
    out.println("name:" + name);
%>
```

## 5JSP注释

在JSP中，注释有显式注释， 隐式注释，JSP自己的注释：

| 显式注释      | 直接使用HTML风格的注释：<!- - 注释内容- -> |
| ------------- | ------------------------------------------ |
| 隐式注释      | 直接使用JAVA的注释：//、/*……*/             |
| JSP自己的注释 | <%- - 注释内容- -%>                        |

区别：

HTML的注释在浏览器中查看源文件的时候是可以看得到的，而JAVA注释和JSP注释在浏览器中查看源文件时是看不到注释的内容的。

## 二、JSP原理

### 1、jsp本质上是什么

 浏览器向服务器发请求，不管访问的是什么资源，其实都是在访问Servlet，所以当访问一个jsp页面时，其实也是在访问一个Servlet，服务器在执行jsp的时候，首先把jsp编译成一个Servlet，所以我们访问jsp时，其实不是在访问jsp，而是在访问jsp编译过后的那个Servlet。

**所以jsp的本质其实就是个html模板，编译器会根据模板生成对应的servlet。**

例如下面的代码：

```xml
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <%!
    static { 
        System.out.println("静态代码块"); 
    }
 
    private String name = "ydl";
 
    public void TestFun(){
        System.out.println("成员方法！");
    }
    %>
    <%
        TestFun()；
    %>
</body>
</html>
```



 当我们通过浏览器访问index.jsp时，服务器首先将index.jsp翻译成一个index_jsp.class，在Tomcat服务器的work\Catalina\localhost\项目名\org\apache\jsp目录下可以看到index_jsp.class的源代码文件index_jsp.java。

 当然，如果我们在idea下启动tomcat，我们需要在这个目录中查看，你的电脑在哪里自行对照：

```url
C:\Users\zn\AppData\Local\JetBrains\IntelliJIdea2021.2\tomcat\dbaebc50-0a4c-46a3-98dd-dd58d5f7ab41\work\Catalina\localhost
```



 编译后的jsp是这个样子的：

```kotlin
package org.apache.jsp;
 
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.jsp.*;
 
public final class index_jsp extends org.apache.jasper.runtime.HttpJspBase
    implements org.apache.jasper.runtime.JspSourceDependent {
 
 // 这是jsp中的声明
    static { 
        System.out.println("静态代码块"); 
    }
 
    private String name = "XinZhi";
 
    public void TestFun(){
        System.out.println("成员方法！");
    }
    
  private static final javax.servlet.jsp.JspFactory _jspxFactory =
          javax.servlet.jsp.JspFactory.getDefaultFactory();
 
  private static java.util.Map<java.lang.String,java.lang.Long> _jspx_dependants;
 
  private volatile javax.el.ExpressionFactory _el_expressionfactory;
  private volatile org.apache.tomcat.InstanceManager _jsp_instancemanager;
 
  public java.util.Map<java.lang.String,java.lang.Long> getDependants() {
    return _jspx_dependants;
  }
 
  public javax.el.ExpressionFactory _jsp_getExpressionFactory() {
    if (_el_expressionfactory == null) {
      synchronized (this) {
        if (_el_expressionfactory == null) {
          _el_expressionfactory = _jspxFactory.getJspApplicationContext(getServletConfig().getServletContext()).getExpressionFactory();
        }
      }
    }
    return _el_expressionfactory;
  }
 
  public org.apache.tomcat.InstanceManager _jsp_getInstanceManager() {
    if (_jsp_instancemanager == null) {
      synchronized (this) {
        if (_jsp_instancemanager == null) {
          _jsp_instancemanager = org.apache.jasper.runtime.InstanceManagerFactory.getInstanceManager(getServletConfig());
        }
      }
    }
    return _jsp_instancemanager;
  }
 
  public void _jspInit() {
  }
 
  public void _jspDestroy() {
  }
 
  public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
        throws java.io.IOException, javax.servlet.ServletException {
 
    final javax.servlet.jsp.PageContext pageContext;
    javax.servlet.http.HttpSession session = null;
    final javax.servlet.ServletContext application;
    final javax.servlet.ServletConfig config;
    javax.servlet.jsp.JspWriter out = null;
    final java.lang.Object page = this;
    javax.servlet.jsp.JspWriter _jspx_out = null;
    javax.servlet.jsp.PageContext _jspx_page_context = null;
 
 
    try {
      response.setContentType("text/html; charset=UTF-8");
      pageContext = _jspxFactory.getPageContext(this, request, response,
                null, true, 8192, true);
      _jspx_page_context = pageContext;
      application = pageContext.getServletContext();
      config = pageContext.getServletConfig();
      session = pageContext.getSession();
      out = pageContext.getOut();
      _jspx_out = out;
 
      out.write("\r\n");
      out.write("<!DOCTYPE html PUBLIC \"-//W3C//DTD HTML 4.01 Transitional//EN\" \"http://www.w3.org/TR/html4/loose.dtd\">\r\n");
      out.write("<html>\r\n");
      out.write("<head>\r\n");
      out.write("<meta http-equiv=\"Content-Type\" content=\"text/html; charset=UTF-8\">\r\n");
      out.write("<title>Insert title here</title>\r\n");
      out.write("</head>\r\n");
      out.write("<body>\r\n");
      out.write("\t");
      out.write('\r');
      out.write('\n');
      out.write('   ');
 
      // 这是我们写的脚本
      testFun();
      out.println("name:" + name);
    
      out.write("\r\n");
      out.write("</body>\r\n");
      out.write("</html>");
    } catch (java.lang.Throwable t) {
      if (!(t instanceof javax.servlet.jsp.SkipPageException)){
        out = _jspx_out;
        if (out != null && out.getBufferSize() != 0)
          try {
            if (response.isCommitted()) {
              out.flush();
            } else {
              out.clearBuffer();
            }
          } catch (java.io.IOException e) {}
        if (_jspx_page_context != null) _jspx_page_context.handlePageException(t);
        else throw new ServletException(t);
      }
    } finally {
      _jspxFactory.releasePageContext(_jspx_page_context);
    }
  }
}
```



 index_jsp这个类是继承org.apache.jasper.runtime.HttpJspBase这个类的，通过查看HttpJspBase源代码，可以知道HttpJspBase类是继承HttpServlet的，所以HttpJspBase类是一个Servlet，而index_jsp又是继承HttpJspBase类的，所以index_jsp类也是一个Servlet，所以当浏览器访问服务器上的index.jsp页面时，其实就是在访问index_jsp这个Servlet，index_jsp这个Servlet使用_jspService这个方法处理请求。

HttpJspBase源码如下：

```java
import java.io.IOException;
 
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.jsp.HttpJspPage;
import javax.servlet.jsp.JspFactory;
 
import org.apache.jasper.compiler.Localizer;
 
public abstract class HttpJspBase extends HttpServlet implements HttpJspPage{
   
    protected HttpJspBase() {
    }
 
    public final void init(ServletConfig config) 
    throws ServletException 
    {
        super.init(config);
    jspInit();
        _jspInit();
    }
    
    public String getServletInfo() {
    return Localizer.getMessage("jsp.engine.info");
    }
 
    public final void destroy() {
    jspDestroy();
    _jspDestroy();
    }
 
    /**
     * Entry point into service.
     */
    public final void service(HttpServletRequest request, HttpServletResponse response) 
    throws ServletException, IOException 
    {
        _jspService(request, response);
    }
    
    public void jspInit() {
    }
 
    public void _jspInit() {
    }
 
    public void jspDestroy() {
    }
 
    protected void _jspDestroy() {
    }
 
    public abstract void _jspService(HttpServletRequest request, 
                     HttpServletResponse response) 
    throws ServletException, IOException;
}
```



### 2、jspService方法

问题1：Jsp页面中的html排版标签是如何被发送到客户端的？

 浏览器接收到的这些数据，都是在_jspService方法中使用如下的代码输出给浏览器的。

```text
 out.write("<html>");
```

1

问题2：Jsp页面中的java代码服务器是如何执行的？

 在jsp中编写的java代码会被翻译到_jspService方法中去，当执行_jspService方法处理请求时，就会执行在jsp编写的java代码了，所以Jsp页面中的java代码服务器是通过调用_jspService方法处理请求时执行的。

### 3、jsp在服务器的执行流程

**第一次执行：**

1. 客户端通过电脑连接服务器，因为请求是动态的，所以所有的请求交给WEB容器来处理
2. 在容器中找到需要执行的*.jsp文件
3. 之后*.jsp文件通过转换变为*.java文件
4. *.java文件经过编译后，形成*.class文件
5. 最终服务器要执行形成的*.class文件

**第二次执行：**

1. 因为已经存在了*.class文件，所以不再需要转换和编译的过程

**修改后执行：**

1. 源文件已经被修改过了，所以需要重新转换，重新编译。

## 三、JSP指令

### 1、JSP指令标识的语法格式

```jsp
<%@ 指令名  属性1 = "属性1的值" 属性2 = "属性2的值" ....%>
```



- 指令名:用于指定指令名称 在JSP中包含page include taglib 这3种指令
- 属性: 用于指定指令属性名称 不同的指令包含不同的属性 在同一个指令中可以设置多个属性 各个属性之间用逗号或者空格隔开
- 属性值:用于指定属性的值

注意点:

> 指令标识<%@%>是一个完整的指令,不能够添加空格,但是便签中定义的属性与指令名之间是有空格的

### 2、Page指令

page指令是JSP页面中最常见的指令,用于定义整个JSP页面的相关属性

> 语法格式

```xml
<%@ page  属性1 = "属性1的值" 属性2 = "属性2的值" ....%>
```



> page指令的相关属

**language属性**

用于设置整个JSP页面的使用的语言,目前只支持JAVA语言,改属性默认值是JAVA

```xml
<%@ page language="java" %>
```



**improt属性**

设置JSP导入的类包

```xml
<%@ page improt="java.util.*" %>
```



**pageEcoding属性**

这种JSP页面的编码格式,也就是指定文件编码

```jsp
<%@ page pageEncoding="GBK" %>
```



设置JSP页面的MIME类型和字符编码

```xml
<%@ page contentType ="text/html;charset=UTF-8" %>
```



**Sesssion属性**

设置页面是否使用HTTP的session会话对象.Boolen类型,默认值是true

```xml
<%@ page session ="false" %>
```



- session是JSP的内置对象之一

**autoFlush属性**

设置JSP页面缓存满时,是否自动刷新缓存,默认值是:true, 如果这种为false,则当页面缓存满时就会抛出异常

```xml
<%@ page autoFlush ="false" %>
```



**isErrorPage属性**

可以把当前页面设置成错误处理页面来处理另外jsp页面的错误

```xml
<%@ page isErrorPage ="true" %>
```



**errorPage属性**

指定当前jsp页面异常错误的另一个JSP页面,指定的JSP页面的isErrorPage属性必须为true,属性值是一个url字符串

```xml
<%@ page errorPage ="errorPage.jsp" %>
```



### 3、include指令

include指令用于引入其它JSP页面，如果使用include指令引入了其它JSP页面，那么JSP引擎将把这两个JSP翻译成一个servlet。所以include指令引入通常也称之为静态引入。

语法：<%@ include file="relativeURL"%>

file属性用于指定被引入文件的路径。路径以"/"开头，表示代表当前web应用。

**注意细节**：

1. 被引入的文件必须遵循JSP语法。
2. 被引入的文件可以使用任意的扩展名，即使其扩展名是html，JSP引擎也会按照处理jsp页面的方式处理它里面的内容，为了见明知意，JSP规范建议使用.jspf（JSP fragments(片段)）作为静态引入文件的扩展名。
3. 由于使用include指令将会涉及到2个JSP页面，**并会把2个JSP翻译成一个servlet**，所以这2个JSP页面的指令不能冲突（除了pageEncoding和导包除外）。

## 四、JSP标签

## 1、Jsp标签分类

 1）内置标签（动作标签）： 不需要在jsp页面导入标签

 2）jstl标签： 需要在jsp页面中导入标签，这个后边我们单独讲

 3）自定义标签 ： 开发者自行定义，需要在jsp页面导入标签

JSP标签也称之为Jsp Action(JSP动作)元素，它用于在Jsp页面中提供业务逻辑功能，避免在JSP页面中直接编写java代码，造成jsp页面难以维护。

## 2、 常用的内置标签

##### **（1）标签**一jsp:include

<jsp:include>标签用于把另外一个资源的输出内容插入进当前JSP页面的输出内容之中，这种在JSP页面执行时的引入方式称之为动态引入。

语法：``

| page  | 用于指定被引入资源的相对路径，它也可以通过执行一个表达式来获得。 |
| ----- | ------------------------------------------------------------ |
| flush | 指定在插入其他资源的输出内容时，是否先将当前JSP页面的已输出的内容刷新到客户端。 |

**标签与include指令的区别：**

<jsp:include>标签是动态引入， <jsp:include>标签涉及到的2个JSP页面会被翻译成2个servlet，这2个servlet的内容在执行时进行合并。 而include指令是静态引入，涉及到的2个JSP页面会被翻译成一个servlet，其内容是在源文件级别进行合并。

##### **（2）标签**<jsp:forward>和<jsp:param>

<jsp:forward>标签用于把请求转发给另外一个资源（服务器跳转，地址不变）。

```xml
<%--使用jsp:forward标签进行请求转发--%>
<jsp:forward page="/index2.jsp" >
    <jsp:param value="10086" name="num"/>
    <jsp:param value="10010" name="num2"/>
</jsp:forward>
```



## 五、JSP属性作用域

JSP中提供了四种属性范围（四大域对象），如下：

1. **当前页（pageContext）**：一个属性只能在一个页面中取得，跳转到其他页面无法取得。
2. **一次服务器请求（request）**：一个页面中设置的属性，只要经过了请求重定向之后的页面可以继续取得。
3. **一次会话（session）**：一个用户设置的内容，只要是与此用户相关的页面都可以访问（一个会话表示一个人，这个人设置的东西只要这个人不走，就依然有效），关了浏览器就不见了。
4. **上下文中（application）**：在整个服务器上设置的属性，所有人都可以访问。

 我们要知道的一点，对于域对象就像我们方法的作用域一样，我们把所有的变量都定义成全局的合适吗？全定义成局部的合适吗？显然是不合适，根据不同的场景选择不同的技术才是正确的。

 在我们的web项目中，一个请求可能会被转发给多个页面，一次会话可能产生多个请求，一个应用上下文又会有多个会话，域对象解决的问题就是在对象传递中的一个作用域的问题。

## 六、九大内置对象

九大内置对象，听起来特别唬人，我们也确实发现，在jsp中是可以直接使用某些对象的，那到底是为什么呢？

其实答案只有一个，我们在编译成servlet的时候就已经为我们准备好了这些对象，当然可以拿来即用啊：

![image-20211013120147547](mdimg/image-20211013120147547.67c7684d.png)

### 1、 request 对象

 代表的是来自客户端的请求 , 客户端发送的请求封装在 request 对象中 , 通过它才能了解到用户的请求信息 , 然后作出响应 , 它是 HTTPServletRequest 的实例 , 作用域为 request ( 响应生成之前 )

**常用方法：**

```java
Object getAttribute(String name);// 返回指定属性的属性值
void setAttribute(String key, Object value);// 设置属性的属性值
Enumeration getAttributeNames();// 返回所有可以用属性名的枚举
String getParameter(String name);// 返回指定name的参数值
Enumeration getParameterNames();// 返回可用参数名的枚举
String[] getParameterValues(String name);// 返回包含参数name的所有制的数组
ServletInputStream geetInputStream();// 得到请求体中一行的二进制流
BufferedReader getReader();// 返回解码过了的请求体


String getServerName();// 返回接收请求的服务器主机名
int getServerPort();// 返回服务器接收此请求所用的端口号
String getRemoteAddr();// 返回发送请求的客户端的IP地址
String getRemoteHost();// 返回发送请求的客户端主机名
String getRealPath();// 返回一个虚拟路径的真实路径
String getCharacterEncoding();// 返回字符编码方式
int geContentLength();// 返回请求体的长度 ( 字节数 )
String getContentType();// 返回请求体的MIME类型
String getProtocol();// 返回请求用的协议类型以及版本号
String getScheme();// 返回请求用的协议名称( 例如 : http  https  ftp )
```



### 2、response 对象

 对象代表的是对客户端的响应 , 也就是说可以通过 response 对象来组织发送到客户端的数据 ; 但是由于组织方式比较底层 , 所以不建议初学者使用 , 需要向客户端发送文字时直接使用 ; 它是 HttpServletResponse 的实例 ; 作用域为 page ( 页面执行期 )

**常用方法：**

```java
String getCharacterEncoding();// 返回响应用的是哪种字符编码
ServletOutputStream getOutputStream();// 返回响应的一个二进制输出流
PrintWriter getWriter();// 返回可以向客户端输出字符的一个对象
void setContentLength(int len);// 设置响应头长度
void setContentType(String type);// 设置响应的MIME类型
void sendRedirect(String location);// 重新定向客户端的请求
```

### 3、 session 对象

 指的是客户端与服务器的一次会话 , 从客户连接到服务器的一个 WebApplication 开始 , 直到客户端与服务器断开连接为止 ; 它是 HTTPSession 类的实例 , 作用域为 session ( 会话期 )

**常用方法：**

```java
long getCreationTime();// 返回SESSION创建时间 
public String getId();// 返回SESSION创建时JSP引擎为它设的惟一ID号 
long getLastAccessedTime();// 返回此SESSION里客户端最近一次请求时间 
int getMaxInactiveInterval();// 返回两次请求间隔多长时间此SESSION被取消(ms) 
String[] getValueNames();// 返回一个包含此SESSION中所有可用属性的数组 
void invalidate();// 取消SESSION，使SESSION不可用 
```

### 4、out 对象

 out 对象是 JspWriter 类的实例,是向客户端输出内容常用的对象 ; 作用域为 page ( 页面执行期 )

**常用方法：**

```java
void clear();// 清除缓冲区的内容 
void clearBuffer();// 清除缓冲区的当前内容 
void flush();// 清空流 
int getBufferSize();// 返回缓冲区以字节数的大小，如不设缓冲区则为0 
int getRemaining();// 返回缓冲区还剩余多少可用 
boolean isAutoFlush();// 返回缓冲区满时，是自动清空还是抛出异常 
void close();// 关闭输出流 
```



### 5、page 对象

 page 对象就是指向当前 JSP 页面本身 , 有点像类中的 this 指针 , 它是 Object 类的实例 ; page 对象代表了正在运行的由 JSP 文件产生的类对象 , 不建议初学者使用 ; 作用域为 page ( 页面执行期 )

**常用方法：**

```java
class getClass();// 返回此Object的类 
int hashCode();// 返回此Object的hash码 
boolean equals(Object obj);// 判断此Object是否与指定的Object对象相等 
void copy(Object obj);// 把此Object拷贝到指定的Object对象中 
Object clone();// 克隆此Object对象 
String toString();// 把此Object对象转换成String类的对象 
void notify();// 唤醒一个等待的线程 
void notifyAll();// 唤醒所有等待的线程 
void wait(int timeout);// 使一个线程处于等待直到timeout结束或被唤醒 
void wait();// 使一个线程处于等待直到被唤醒 
void enterMonitor();// 对Object加锁 
void exitMonitor();// 对Object开锁
```



### 6、application 对象

 实现了用户间数据的共享 , 可存放全局变量 ; 它开始于服务器的启动 , 直到服务器的关闭 , 在此期间 , 此对象将一直存在 ; 这样在用户的前后连接或不同用户之间的连接中 , 可以对此对象的同一属性进行操作 ; 在任何地方对此对象属性的操作 , 都将影响到其他用户对此的访问 ; 服务器的启动和关闭决定了 application 对象的生命 ; 它是 ServletContext 类的实例 ; 作用域为 application

**常用方法：**

```java
Object getAttribute(String name);// 返回给定名的属性值 
Enumeration getAttributeNames();// 返回所有可用属性名的枚举 
void setAttribute(String name,Object obj);// 设定属性的属性值 
void removeAttribute(String name);// 删除一属性及其属性值 
String getServerInfo();// 返回JSP(SERVLET)引擎名及版本号 
String getRealPath(String path);// 返回一虚拟路径的真实路径 
ServletContext getContext(String uripath);// 返回指定WebApplication的application对象 
int getMajorVersion();// 返回服务器支持的Servlet API的最大版本号 
int getMinorVersion();// 返回服务器支持的Servlet API的最大版本号 
String getMimeType(String file);// 返回指定文件的MIME类型 
URL getResource(String path);// 返回指定资源(文件及目录)的URL路径 
InputStream getResourceAsStream(String path);// 返回指定资源的输入流 
RequestDispatcher getRequestDispatcher(String uripath);// 返回指定资源的RequestDispatcher对象
Servlet getServlet(String name);// 返回指定名的Servlet
Enumeration getServlets();// 返回所有Servlet的枚举
Enumeration getServletNames();// 返回所有Servlet名的枚举
void log(String msg);// 把指定消息写入Servlet的日志文件
void log(Exception exception,String msg);// 把指定异常的栈轨迹及错误消息写入Servlet的日志文件
void log(String msg,Throwable throwable);// 把栈轨迹及给出的Throwable异常的说明信息 写入Servlet的日志文件
```



### 7、 pageContext 对象

 提供了对 JSP 页面内所有的对象及名字空间的访问 , 也就是说它可以访问到本页所在的 session , 也可以取本页面所在的 application 的某一属性值 , 它相当于页面中所有功能的集大成者。

**常用方法：**

```java
JspWriter getOut();// 返回当前客户端响应被使用的JspWriter流(out) 
HttpSession getSession();// 返回当前页中的HttpSession对象(session) 
Object getPage();// 返回当前页的Object对象(page) 
ServletRequest getRequest();// 返回当前页的ServletRequest对象(request) 
ServletResponse getResponse();// 返回当前页的ServletResponse对象(response) 
Exception getException();// 返回当前页的Exception对象(exception) 
ServletConfig getServletConfig();// 返回当前页的ServletConfig对象(config) 
ServletContext getServletContext();// 返回当前页的ServletContext对象(application)
void setAttribute(String name,Object attribute);// 设置属性及属性值 
void setAttribute(String name,Object obj,int scope);// 在指定范围内设置属性及属性值 
public Object getAttribute(String name);// 取属性的值 
Object getAttribute(String name,int scope);// 在指定范围内取属性的值 
public Object findAttribute(String name);// 寻找一属性,返回起属性值或NULL 
void removeAttribute(String name);// 删除某属性 
void removeAttribute(String name,int scope);// 在指定范围删除某属性 
int getAttributeScope(String name);// 返回某属性的作用范围 
Enumeration getAttributeNamesInScope(int scope);// 返回指定范围内可用的属性名枚举 
void release();// 释放pageContext所占用的资源 
void forward(String relativeUrlPath);// 使当前页面重导到另一页面 
void include(String relativeUrlPath);// 在当前位置包含另一文件 
```

### 8、config 对象

 config 对象是在一个 Servlet 初始化时 , JSP 引擎向它传递信息用的 , 此信息包括 Servlet 初始化时所要用到的参数 ( 通过属性名和属性值构成 ) 以及服务器的有关信息 ( 通过传递一个 ServletContext 对象 ) ; 作用域为 page

**常用方法：**

```java
ServletContext getServletContext();// 返回含有服务器相关信息的ServletContext对象 
String getInitParameter(String name);// 返回初始化参数的值 
Enumeration getInitParameterNames();// 返回Servlet初始化所需所有参数的枚举
```



### 9、exception 对象

 这是一个例外对象 , 当一个页面在运行过程中发生了例外 , 就产生这个对象 ; 如果一个JSP页面要应用此对象 , 就必须把 isErrorPage 设为true , 否则无法编译 ; 它实际上是 Throwable 的对象 ; 作用域为 page。

**常用方法：**

```java
String getMessage();// 返回描述异常的消息 
String toString();// 返回关于异常的简短描述消息 
void printStackTrace();// 显示异常及其栈轨迹 
Throwable FillInStackTrace();// 重写异常的执行栈轨迹
```

### 10、总结

| 对象名      | 描述           | 类型                           | 作用域      |
| ----------- | -------------- | ------------------------------ | ----------- |
| request     | 请求对象       | javax.servlet.ServletRequest   | Request     |
| response    | 响应对象       | javax.servlet.SrvletResponse   | Page        |
| pageContext | 页面上下文对象 | javax.servlet.jsp.PageContext  | Page        |
| session     | 会话对象       | javax.servlet.http.HttpSession | Session     |
| application | 应用程序对象   | javax.servlet.ServletContext   | Application |
| out         | 输出对象       | javax.servlet.jsp.JspWriter    | Page        |
| config      | 配置对象       | javax.servlet.ServletConfig    | Page        |
| page        | 页面对象       | javax.lang.Object              | Page        |
| exception   | 例外对象       | javax.lang.Throwable           | Page        |

# 第六章 EL表达式和JSTL标签库

## 一、EL表达式

### 1、特点

（1）是一个由java开发的工具包

（2）用于从特定域对象中读取数据，不能向域对象中写入。

（3）EL工具包自动存在Tomcat的lib中（el-api.jar），开发是可以直接使用，无需其他额外的包。

（4）标准格式 ： ${域对象别名.关键字} 到指定的域中获取相应关键字的内容，并将其写入到响应体。

### 2、域对象

| jsp         | el               | 描述             |
| ----------- | ---------------- | ---------------- |
| application | applicationScope | 全局作用域对象   |
| session     | sessionScope     | 会话作用域       |
| request     | requestScope     | 请求作用域对象   |
| pageContext | pageScope        | 当前页作用域对象 |

**注：使用时可以省略域对象别名**

默认查找顺序： pageScope -> requestScope -> sessionScope -> applicationScope

**最好只在pageScope中省略**

注：对应案例

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>jsp</title>
</head>
<body>
  <%
    application.setAttribute("name","application");
    session.setAttribute("name","session");
    request.setAttribute("name","request");
    pageContext.setAttribute("name","pageContext");
  %>
  <br>--------------------使用java语言---------------------------<br>
  application中的值：<%= application.getAttribute("name") %> <br>
  session中的值：<%= session.getAttribute("name") %> <br>
  request中的值：<%= request.getAttribute("name") %> <br>
  pageContext中的值：<%= pageContext.getAttribute("name") %> <br>

  <br>--------------------使用EL表达式---------------------------<br>
  application中的值：${applicationScope.name} <br>
  session中的值：${sessionScope.name} <br>
  request中的值：${requestScope.name} <br>
  pageContext中的值：${pageScope.name} <br>

  <br>----------------使用EL表达式,省略域对象---------------------<br>
  application中的值：${name} <br>

</body>
</html>
```



### 3、支持的运算

（1）数学运算

（2）比较运算 > gt < lt >= ge <= le == eq != !=

（3）逻辑预算 && || ！

注：对应案例

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>EL运算</title>
</head>
<body>
<%
    request.setAttribute("num1","12");
    request.setAttribute("num2","14");

    application.setAttribute("flag1",true);
    application.setAttribute("flag2",false);
%>
<br>--------------------使用java语言---------------------------<br>
<%
    String num1 = (String)request.getAttribute("num1");
    String num2 = (String)request.getAttribute("num2");
    int num3 = Integer.parseInt(num1) + Integer.parseInt(num2);
    
    boolean flag1 = (Boolean) application.getAttribute("flag1");
    boolean flag2 = (Boolean) application.getAttribute("flag2");
    boolean flag3 = flag1 && flag2;
    //输出方式一
    out.write(Boolean.toString(flag3));
%>
<!-- 输出方式二 -->
<h1><%=num3%></h1>

<br>--------------------使用EL表达式--------------------------<br>
<h1>${ requestScope.num1 + requestScope.num2 }</h1>
<h1>${ requestScope.num1 > requestScope.num2 }</h1>
<h1>${ applicationScope.flag1 && applicationScope.flag2 }</h1>

</body>
</html>
```



### 4、EL表达式的缺陷

（1）只能读取域对象中的值，不能写入

（2）不支持if判断和控制语句

## 二、JSTL标签工具类

### 1、基本介绍

（1） JSP Standrad Tag Lib jsp标准标签库

```html
核心标签      对java在jsp上基本功能进行封装，如 if while等    主要学习
sql标签      JDBC在jsp上的使用
xml标签      Dom4j在jsp上的使用
format标签   jsp文件格式转换
```



（4）使用原因：**使用简单，且在JSP编程当中要求尽量不出现java代码。**

### 2、使用方式

（1）tomcat10 以前的导入依赖的jar包 jstl.jar standard.jar

下载地址http://archive.apache.org/dist/jakarta/taglibs/standard/binaries/

tomcat10以后使用 jakarta.servlet.jsp.jstl-2.0.0.jar

当然在tomcat10中也有这两个jar包，找到tomcat10中的例子程序：

```text
D:\javaweb\tomcat\apache-tomcat-10.0.11\apache-tomcat-10.0.11\webapps\examples\WEB-INF\lib
```

1

![image-20211013140248426](mdimg/image-20211013140248426.55a48733.png)

（2）在jsp中引入JSTL的core包依赖约束

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

1

### 3、重要标签的使用

##### [#](https://www.ydlclass.com/doc21xnv/javaweb/servlet/#_1-c-set)（1） <c:set>

在JSP文件上设置域对象中的共享数据

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<html>
<head>
    <title>  c:set  </title>
</head>
    <body>
        <!--
        相当于
        <%--  <%   --%>
        <%--   request.setAttribute("name","zhangsan");--%>
        <%--  %>  --%>
        -->
        <c:set scope="request" var="name" value="zhangsan" />
        通过JSTL标签添加的作用域中的值：${requestScope.name}   <br>
        <c:set scope="application" var="name" value="lisi" />
        通过JSTL标签添加的作用域中的值：${applicationScope.name}   <br>
        <c:set scope="request" var="name" value="wangwu" />
        通过JSTL标签添加的作用域中的值：${requestScope.name}   <br>
        <c:set scope="page" var="name" value="zhaoliu" />
        通过JSTL标签添加的作用域中的值：${pageScope.name}   <br>
    </body>
</html>
```



##### [#](https://www.ydlclass.com/doc21xnv/javaweb/servlet/#_2-c-if)（2）<c:if >

控制哪些内容能够输出到响应体

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head>
    <title> c:if </title>
</head>
<body>
    <c:set scope="page" var="age" value="20"/>
    <br>------------------------------使用java语言-------------------------------------<br>
    <%
        if( Integer.parseInt((String)pageContext.getAttribute("age")) >= 18 ){
    %>
    输入：欢迎光临！
    <%  } else { %>
    输入：未满十八，不准入内！
    <%  }  %>
    <br>------------------------------使用JSTL标签-------------------------------------<br>

    <c:if test="${ age ge 18 }">
        输入：欢迎光临！
    </c:if>
    <c:if test="${ age lt 18 }">
        输入：未满十八，不准入内！
    </c:if>
</body>
</html>
```



##### [#](https://www.ydlclass.com/doc21xnv/javaweb/servlet/#_3-c-choose)（3）<c:choose>

在jsp中进行多分支判断，决定哪个内容写入响应体

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head>
    <title> c:choose </title>
</head>
<body>
    <c:set scope="page" var="age" value="6"/>
    <br>------------------------------使用java语言-------------------------------------<br>
    <%
        if( Integer.parseInt((String)pageContext.getAttribute("age")) == 18 ){
    %>
    输入：您今年成年了
    <%  } else if( Integer.parseInt((String)pageContext.getAttribute("age")) > 18 ){ %>
    输入：您已经成年了
    <%  }  else {%>
    输出：您还是个孩子
    <% } %>
    <br>------------------------------使用JSTL标签-------------------------------------<br>

    <c:choose>
        <c:when test="${age eq 18}">
            输入：您今年成年了
        </c:when>
        <c:when test="${age gt 18}">
            输入：您已经成年了
        </c:when>
        <c:otherwise>
            输入：您还是个孩子
        </c:otherwise>
    </c:choose>
</body>
</html>
```



##### [#](https://www.ydlclass.com/doc21xnv/javaweb/servlet/#_4-c-foreach)（4）<c:forEach>

循环遍历

使用方式

```jsp
<c:forEach var="申明循环变量的名称" begin="初始化循环变量" 
           end="循环变量可以接受的最大值" step="循环变量的递增或递减值">
    *** step属性可以不写，默认递增1
    *** 循环变量默认保存在pageContext中
</c:forEach>
```



例子

```jsp
<%@ page import="com.zn.Student" %>
<%@ page import="java.util.List" %>
<%@ page import="java.util.ArrayList" %>
<%@ page import="java.util.Map" %>
<%@ page import="java.util.HashMap" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head>
    <title> c:forEach </title>
</head>
<body>
    <%
        pageContext.setAttribute("students",new ArrayList(){{
            add(new Student("s1","zhangsan",16));
            add(new Student("s2","lisi",19));
            add(new Student("s3","wangwu",15));
        }});
        pageContext.setAttribute("stuMap", new HashMap(){{
            put("m1",new Student("s1","zhangsan",16));
            put("m2",new Student("s2","lisi",18));
            put("m3",new Student("s3","wangwu",15));
        }});
    %>
    <br>------------------------使用java语言------------------------------<br>
    <table>
        <tr><td>学号</td><td>姓名</td><td>年龄</td></tr>
        <%
            List<Student> stus =            (ArrayList<Student>)pageContext.getAttribute("students");
            for (int i = 0; i < stus.size(); i++) {
        %>
          <tr><td><%=stus.get(i).getSid()%></td>
              <td><%=stus.get(i).getName()%></td>
              <td><%=stus.get(i).getAge()%></td>
          </tr>
        <% } %>
    </table>
    
    <br>----------------------使用JSTL标签读取list-----------------------<br>
    <table>
        <tr><td>学号</td><td>姓名</td><td>年龄</td></tr>
        <c:forEach var="student" items="${students}">
        <tr><td>${student.sid}</td>
            <td>${student.name}</td>
            <td>${student.age}</td>
        </tr>
        </c:forEach>
    </table>

    <br>---------------------使用JSTL标签读取map------------------------<br>
    <table>
        <tr><td>学号</td><td>姓名</td><td>年龄</td></tr>
        <c:forEach var="student" items="${stuMap}">
            <tr>
                <td>${student.key}</td>
                <td>${student.value.sid}</td>
                <td>${student.value.name}</td>
                <td>${student.value.age}</td>
            </tr>
        </c:forEach>
    </table>

    <br>--------------使用JSTL标签读取指定for循环-----------------------<br>
    <select>
      <c:forEach var="item" begin="1" end="10" step="1">
          <option> ${item} </option>
      </c:forEach>
    </select>

</body>
</html>
```



其中使用的java对象：

```java
public class Student {
    
    private String sid;
    private String name;
    private int age;

    public String getSid() {
        return sid;
    }

    public void setSid(String sid) {
        this.sid = sid;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Student(String sid, String name, int age) {
        this.sid = sid;
        this.name = name;
        this.age = age;
    }
}
```



## 三、路径问题

在我们表示一个资源的位置的时候通常有两种方式，一个是绝对路径，一个是相对路径，我们学习html的时候已经学习过，今天重新回顾一下。

1. 绝对路径：从根目录为起点到某一个目录的路径； /C://aa/bb/a.txt
2. 相对路径：从一个目录为起点到另外一个的目录的路径。 ./b.txt b.txt

 同样我们获取一个网络资源的时候，一样可以使用这两种方式，使用绝对路径也就是url，我们就不必重新说了，但是使用相对路径的时候，我们需要掌握以下两个知识点。

1. 站点的根目录：浏览器而言，它的根目录就是站点根目录，可能是你磁盘上的任意一个文件夹，此时一个urlhttp://localhost:9999/可以映射到这个文件夹，这就代表了一个站点的根目录。
2. 项目的根目录：对于咱们的工程而言，服务端的根目录是项目根目录，其实在tomcat中，一个app就是一个独立的文件夹，相对于站点根目录，项目的根目录多了一个app的名字： http://localhost:9999/study01/。

`/`一般是指代某种情况下的根路径，在前端使用就指代站点根路径，在服务器中就是项目跟根路径。

所以一般情况下，

- 绝对路径是以/开头或者使用整体的url。
- 【重要】如果是在浏览器中访问就是站点根目录，如果是在java项目代码中使用就指项目根目录。
- 相对路径使用`./`或者`../`或者文件名开头，其中`./`代表当前文件夹，`../`代表上级文件夹。

以下几个场景中我们使用绝对路径需要注意：

1、在服务端进行请求转发，因为转发的过程在服务端进行，所以不需要加contextPath。

```text
req.getRequestDispatcher("/WEB-INF/pages/error.jsp").forward(req,resp);
```



2、在服务端进行重定向，大家要明白一点，重定向其实是在浏览器中具体执行的，所以必须加contextPath。

```text
response.sendRedirect(request.getContextPath() + "/login.jsp");
```



3、在浏览器端访问一个新的地址。

```html
<a href="/first_web/pages/index.jsp">前往主页</a>
```



4、在浏览器中访问静态资源

```html
<script src="/first_web/static/js/index.js"></script>
```



以下是我们通常的处理方案：

1、在jsp中定义好我们的basePath，这个路径是带有contextPath的。

2、在Head中指定， `<base href="<%=basePath%>"> `。

3、在具体的地址处使用相对于contextPath的路径。

```jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
   String path = request.getContextPath();
   String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>
<html>
<head>
   <title>image调用</title>
   <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">   
   <base href="<%=basePath%>"> 
</head>
<body>
   <h1>图片访问</h1>
   <div>   
     <img alt="图片" src="image/a.png">
   </div>
</body>
</html>
```



以上结构中的图片真实的访问路径是： http://localhost:8080/first_web/image/a.png

### 四、错误页面和404页面

我们可以在web.xml中根据错误码和异常类型，配置不同异常情况下的错误页面。

```jsp
<error-page>
    <error-code>404</error-code>
    <location>/pages/404.jsp</location>
</error-page>

<error-page>
    <exception-type>java.lang.Exception</exception-type>
    <location>/pages/err.jsp</location>
</error-page>
```