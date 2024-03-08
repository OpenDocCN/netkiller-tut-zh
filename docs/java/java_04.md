# 第 3 章 Servlet

## Example

```
package cn.netkiller.helloworld;

import java.io.IOException;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class HelloWorld
 */
@WebServlet("/HelloWorld")
public final class HelloWorld extends HttpServlet {
	private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public HelloWorld() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException {
		// TODO Auto-generated method stub
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() {
		// TODO Auto-generated method stub
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath()).append("<br></br> Helloworld");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}

```

## Session

web.xml 定义默认过期时间

```

<web-app ...>
	<session-config>
	    <session-timeout>Minutes</session-timeout>
	</session-config>
</web-app>

```

修改 Session 时间

```

HttpSession session = request.getSession();
session.setMaxInactiveInterval(20*60);

```

## HttpServletRequest

request.getParameter() 获取 form 参数

```

HttpServletRequest request = ServletActionContext.getRequest();
String name = request.getParameter("name");
String age = request.getParameter("age");

```

## Filter

### web.xml

配置过滤器

```

<filter>
    <filter-name>LoginFilter</filter-name>
    <filter-class>cn.netkiller.Filter</filter-class>
    <init-param>
        <param-name>username</param-name>
        <param-value>netkiller</param-value>
    </init-param>
</filter>

<filter>
      <filter-name>LoginFilter</filter-name>
      <filter-class>cn.netkiller.Filter</filter-class>
</filter>
<filter-mapping>
      <filter-name>LoginFilter</filter-name>
      <url-pattern>/*</url-pattern>
</filter-mapping>

```

### Filter 类

实现 Filter 接口

```

public class LoginFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init LoginFilter");
        //获取 Filter 初始化参数
    	String username = filterConfig.getInitParameter("username");
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {
        //把 ServletRequest 和 ServletResponse 转换成真正的类型
        HttpServletRequest req = (HttpServletRequest)request;
        HttpSession session = req.getSession();

        //由于 web.xml 中设置 Filter 过滤全部请求，可以排除不需要过滤的 url
        String requestURI = req.getRequestURI();
        if(requestURI.endsWith("login.jsp")){
            chain.doFilter(request, response);
            return;
        }

        //判断用户是否登录，进行页面的处理
        if(null == session.getAttribute("user")){
            //未登录用户，重定向到登录页面
            ((HttpServletResponse)response).sendRedirect("login.jsp");
            return;
        } else {
            //已登录用户，允许访问
            chain.doFilter(request, response);
        }
    }

    @Override
    public void destroy() {
        System.out.println("destroy!!!");
    }
}

```

## Listener

### web.xml

配置监听器

```

	<listener>
		<listener-class>cn.netkiller.listener.NewsListener</listener-class>
	</listener>

```

### NewsListener 类

实现 ServletContextListener 接口

```

package cn.netkiller.listener;   

import java.util.Timer;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

import org.apache.log4j.Logger;

public class NewsListener implements ServletContextListener {   

	private static final Logger log = Logger.getLogger(NewsListener.class);

	private Timer timer = null;

	@Override
	public void contextInitialized(ServletContextEvent event) {

		log.info("Listener start");

		timer = new Timer(true);		
		timer.schedule(new NewsTask(event.getServletContext()), 3*1000, 5*60*1000);
	}

	@Override
	public void contextDestroyed(ServletContextEvent event) {
		if (timer != null) {
			timer.cancel();
		}
		log.info("Listener end");
	}   
} 

```

### NewsTask 类

继承 TimerTask

```

package cn.netkiller.listener;

import java.util.List;
import java.util.TimerTask;
import javax.servlet.ServletContext;

import org.apache.log4j.Logger;
import org.springframework.web.context.support.WebApplicationContextUtils;

import cn.netkiller.service.interface.NewsService;

public class NewsTask extends TimerTask{

	private ServletContext context;
	private static final Logger log = Logger.getLogger(NewsTask.class);

	public NewsTask(ServletContext context) {
		this.context = context;
	}

	@Override
	public void run() {
		NewsService newsService = (NewsService) WebApplicationContextUtils.getWebApplicationContext(context).getBean("newsService");

		try {
			List<cn.netkiller.listener.News> newsList = newsService.getNews();
			context.setAttribute("newsList", newsList);

			log.info("Getting News Finished");
		} catch (Exception e) { e.printStackTrace(); }
	}
}

```

### JSP 中心显示

使用 c:forEach 显示列表

```

	<div class="news">
       	<c:forEach items="${newsList}" var="news" varStatus="index">
       		<a href="/news/${news.Id}">${news.title}</a>
       	</c:forEach>
	</div>

```

## JSP

### 注释

JSP 页面中的注释有以下几种样式的注释方法：

```

JSP 页面中的 HTML 注释 
     <!-- 注释内容 -->  

JSP 页面中的普通注释,不会再浏览器中输出

    <% // 注释内容 %>    
    <% /* 注释内容 */ %> 

JSP 页面中的隐藏注释,不会再浏览器中输出 
<%-- 注释内容 --%>

```

### pageContext

#### queryString

输出？问号后面的变量

```

${pageContext.request.queryString}
<c:out value = "${pageContext.request.queryString}" />		

```

### request

```
new URL(
	request.getScheme(), 
    request.getServerName(), 
    request.getServerPort(), 
    request.getContextPath()
    );		

```

获取主机名

```

<%=request.getServerName() %>

```

#### Form

```

<%
   Enumeration paramNames = request.getParameterNames();

   while(paramNames.hasMoreElements()) {
      String paramName = (String)paramNames.nextElement();
      String paramValue = request.getParameter(paramName);
      out.println(paramName + ": " + paramValue + "<br/>\n");
   }
%>			

```

```

<%= request.getParameter("first_name")%>

<c:set var="UA" value="${param.first_name}" />
<c:out value="${param.first_name}"/>

```

#### sendRedirect

页面跳转

```
response.sendRedirect("/content/test.jsp");			

```

### cookie

获取 cookie 值

```

<html>
<head>
<title>Reading Cookies</title>
</head>
<body>
<center>
<h1>Reading Cookies</h1>
</center>
<%
   Cookie cookie = null;
   Cookie[] cookies = null;
   // Get an array of Cookies associated with this domain
   cookies = request.getCookies();
   if( cookies != null ){
      out.println("<h2> Found Cookies Name and Value</h2>");
      for (int i = 0; i < cookies.length; i++){
         cookie = cookies[i];
         out.print("Name : " + cookie.getName( ) + ",  ");
         out.print("Value: " + cookie.getValue( )+" <br/>");
      }
  }else{
      out.println("<h2>No cookies founds</h2>");
  }
%>
</body>
</html>

```

### session

获取 Session 值

```

<% out.print(session.getAttribute("random")); %>

```

### page

```

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>		

```

#### Session

禁用 Session

```

<% @page session="false" %>

```

启用 Session

```

<%@ page session="true" %>

```

### trimDirectiveWhitespaces

删除 JSP 编译产生的空行

```

<%@ page trimDirectiveWhitespaces="true" %> 

```

### include

```

<%@ include file="analytics.jsp" %>

```

```

<jsp:include page="telephone.jsp" />
<jsp:include page="address.jsp" flush="true"/> 
<jsp:include page="${request.getContentType()}/module/html.body.begin.html" flush="true" />

```

file 与 page 的区别是，file 将文件包含进当前文件，然后变成成 servlet。 page 是运行的时候才会包含文件输出结果包含进当前文件。

```

<pre>

<%@include file="inc.html" %>
=============
<%@include file="/inc.html" %>
=============
<jsp:include page="inc.html" />
=============
<jsp:include page="inc.html" flush="true" />
==============
<jsp:include page="/inc.html" flush="true" />
==============
<jsp:include page="${request.getContentType()}/inc.html" flush="true" />

</pre>

```

注意上面包含结果相同，使用上略有差异。

### jsp

#### jsp:forward

jsp:forward 只能跳转到存在的物理页面

```

<jsp:forward page="/test.jsp" />

```

### error-page

在 web.xml 中配置 error-page

```

<error-page>
	<error-code>400</error-code>
	<location>/error.jsp</location>
</error-page>

<error-page>
	<error-code>404</error-code>
	<location>/error.jsp</location>
</error-page>

<error-page>
	<error-code>500</error-code>
	<location>/error.jsp</location>
</error-page>
<!-- java.lang.Exception 异常错误,依据这个标记可定义多个类似错误提示 -->
<error-page>
	<exception-type>java.lang.Exception</exception-type>
	<location>/error.jsp</location>
</error-page>
<!-- java.lang.NullPointerException 异常错误,依据这个标记可定义多个类似错误提示 -->
<error-page>
	<exception-type>java.lang.NullPointerException </exception-type>
	<location>/error.jsp</location>
</error-page>

<error-page>
	<exception-type>javax.servlet.ServletException</exception-type>
	<location>/error.jsp</location>
</error-page>

```

### JSP 编程

#### 随机数

```

<%= (int) (Math.random() * 1000) %>			

```

### FAQ

#### http://www.netkiller.cn/test.html;jsessionid=7D25CE666FF437F2094AA945E97CEB37

URL 经常会出现 jsessionid，这是因为当你使用 c:url 的时候，它会判断当前域下是否已经创建 jsessionid 的 cookie 变量，如果没有并且你链接的是静态页面，那么 c:url 将传递 jsessionid 变量，迫使 Tomcat 为当前域创建 jsessionid cookie。

当你使用 Apache Nginx 作为 Tomcat 前端是，由于 Apache Nginx 不识别;jsessionid=7D25CE666FF437F2094AA945E97CEB37 这种 URL，会跳出 404 页面。

解决方案一，放弃使用 c:url，如果你需要保持 context 的 path 设置，可以使用下面方法

```

<a href="${pageContext.request.contextPath}/test.html">Netkiller Test</a>

```

方案二，使用 rewrite 截取 jsessionid 做忽略处理

Apache：

```
ReWriteRule ^/(\w+);jsessionid=\w+$ /$1 [L,R=301]
ReWriteRule ^/(\w+\.do);jsessionid=\w+$ /$1 [L,R=301]

```

Nginx：

```
rewrite ^(.*)\;jsessionid=(.*)$ $1 break;

```

## JSTL(JavaServer Pages Standard Tag Library)

```

<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

```

### c:set

设置变量

```

<c:set var="foo" scope="request" value="helloworld">

or

<%request.setAttribute("foo","helloworld") %>

<c:out value="${requestScope.foo}"/>

```

#### c:remove

```

<c:remove var="message" scope="session" />

```

### c:out

输出变量 variable 的内容

```

<c:out value="${variable}"/>

<%=request.getParameter("UA")%>
相当于
<c:out value="${param.UA}"/>

默认值
<c:out value="${param.UA}" default="UA-69658002-1" />

```

### c:url

生成 URL

```

<c:url value="/news/china/"/>

```

### c:redirect

```

<c:redirect url="/index.html"/>
<c:redirect url="http://www.netkiller.cn"/>		

```

### c:import

```

<c:import url="http://www.netkiller.cn" />		

<c:import var="html" url="http://www.netkiller.cn"/>
<c:out value="${html}"/>	

```

传递 GET 参数

```

<c:import url="http://www.netkiller.cn" > 
	<c:param name="id" value="10" />
	<c:param name="name" value="neo" /> 
</c:import> 			

```

异常处理

```

<c:catch var="exception">

  <c:import url="ftp://ftp.example.com/package/README"/>

</c:catch>

<c:if test="${not empty exception}">

  Sorry, the remote content is not currently available.

</c:if>			

```

在 Context 间切换

```

server.conf: 
<Context  reloadable="true" crossContext="true" /> 

<c:import url="/path/to/file.jsp" context=/project1" /> 			
<c:import url="/path/to/file.jsp" context=/project2" />

```

### c:if

```

<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<html>
<head>
<title><c:if> Tag Example</title>
</head>
<body>
<c:set var="salary" scope="session" value="${2000*2}"/>
<c:if test="${salary > 2000}">
   <p>My salary is: <c:out value="${salary}"/><p>
</c:if>
</body>
</html>	

```

#### boolean

```

<c:if test="${theBooleanVariable}">It's true!</c:if>
<c:if test="${not theBooleanVariable}">It's false!</c:if>

```

### c:choose

```

<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<c:choose>
	<c:when test="${session.auth eq 'true' }">

	</c:when>
	<c:otherwise>

	</c:otherwise>
</c:choose>			

```

实现 if else/else if / else

```

<c:choose>
   <c:when test="${..}">...</c:when> <!-- if condition -->
   <c:when test="${..}">...</c:when> <!-- else if condition -->
   <c:otherwise>...</c:otherwise>    <!-- else condition -->
</c:choose>			

```

### c:forEach

#### List 处理

```

<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
	pageEncoding="ISO-8859-1"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
	${bookList}
	<br>

	<c:forEach items="${bookList}" var="node">
		<c:out value="${node}"></c:out><br>
	</c:forEach>

</body>
</html>

```

#### Map 处理

```

<%@ page language="java" contentType="text/html; charset=utf-8"
    pageEncoding="utf-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
	<c:forEach items="${channel}" var="node">
		<a href="<c:out value="${node.value}"></c:out>"><c:out value="${node.key}"></c:out></a>
        <br/>
	</c:forEach>
</body>
</html>

```

### empty 判断是否为空

```

	<c:if test="${empty session.member }">

	</c:if>

```

### JSTL fmt Tag setBundle Example

#### fmt:message

src/resources/config.properties

```

Name=Neo
Address=Shenzhen
Number=13322993040

```

```

<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>JSTL fmt:setBundle Tag</title>
</head>
<body>
	<fmt:setBundle basename="resources.config" var="config" />
	<fmt:message key="Name" bundle="${config}" />

	<fmt:message key="Address" bundle="${config}" />

	<fmt:message key="Number" bundle="${config}" />
</body>
</html>

```

```

	<fmt:bundle basename="lang">
		<fmt:message key="Name" />
		<fmt:message key="Address" />
	</fmt:bundle>			

```

##### Language Package

```

<fmt:setLocale value="en" />				

```

##### fmt:message 变量

```

<fmt:message key="js" bundle="${config}" var="val" />
<c:out value="${val}"/>				

```

```

<fmt:setTimeZone value="Europe/London" scope="session"/>			

```

```

<fmt:formatDate value="${isoDate}" type="both"/>
2004-5-31 23:59:59

<fmt:formatDate value="${date}" type="date"/>
2004-4-1

<fmt:formatDate value="${isoDate}" type="time"/>
23:59:59

<fmt:formatDate value="${isoDate}" type="date" dateStyle="default"/>
2004-5-31

<fmt:formatDate value="${isoDate}" type="date" dateStyle="short"/>
04-5-31

<fmt:formatDate value="${isoDate}" type="date" dateStyle="medium"/>
2004-5-31

<fmt:formatDate value="${isoDate}" type="date" dateStyle="long"/>
2004 年 5 月 31 日

<fmt:formatDate value="${isoDate}" type="date" dateStyle="full"/>
2004 年 5 月 31 日 星期一

<fmt:formatDate value="${isoDate}" type="time" timeStyle="default"/>
23:59:59

<fmt:formatDate value="${isoDate}" type="time" timeStyle="short"/>
下午 11:59

<fmt:formatDate value="${isoDate}" type="time" timeStyle="medium"/>
23:59:59

<fmt:formatDate value="${isoDate}" type="time" timeStyle="long"/>
下午 11 时 59 分 59 秒

<fmt:formatDate value="${isoDate}" type="time" timeStyle="full"/>
下午 11 时 59 分 59 秒 CDT

<fmt:formatDate value="${date}" type="both" pattern="EEEE, MMMM d, yyyy HH:mm:ss Z"/>
星期四, 四月 1, 2004 13:30:00 -0600

<fmt:formatDate value="${isoDate}" type="both" pattern="d MMM yy, h:m:s a zzzz/>
31 五月 04, 11:59:59 下午 中央夏令时 

格式模式：
  d   月中的某一天。一位数的日期没有前导零。    
  dd   月中的某一天。一位数的日期有一个前导零。    
  ddd   周中某天的缩写名称，在   AbbreviatedDayNames   中定义。    
  dddd   周中某天的完整名称，在   DayNames   中定义。    
  M   月份数字。一位数的月份没有前导零。    
  MM   月份数字。一位数的月份有一个前导零。    
  MMM   月份的缩写名称，在   AbbreviatedMonthNames   中定义。    
  MMMM   月份的完整名称，在   MonthNames   中定义。    
  y   不包含纪元的年份。如果不包含纪元的年份小于   10，则显示不具有前导零的年份。    
  yy   不包含纪元的年份。如果不包含纪元的年份小于   10，则显示具有前导零的年份。    
  yyyy   包括纪元的四位数的年份。    
  gg   时期或纪元。如果要设置格式的日期不具有关联的时期或纪元字符串，则忽略该模式。    
  h   12   小时制的小时。一位数的小时数没有前导零。    
  hh   12   小时制的小时。一位数的小时数有前导零。    
  H   24   小时制的小时。一位数的小时数没有前导零。    
  HH   24   小时制的小时。一位数的小时数有前导零。     
  m   分钟。一位数的分钟数没有前导零。    
  mm   分钟。一位数的分钟数有一个前导零。    
  s   秒。一位数的秒数没有前导零。    
  ss   秒。一位数的秒数有一个前导零。

<fmt:formatDate value="${xx}" pattern="dd/MM/yyyy HH:mm aa"/>和

<fmt:formatDate value="${xx}" pattern="dd/MM/yyyy hh:mm aa"/>  对于 0 点显示的结果不一样

<fmt:formatDate value="${dateValue}" pattern="yyyy-MM-dd HH:mm:ss z" timeZone="GMT"/>

<fmt:formatDate value="${dateValue}" pattern="yyyy-MM-dd HH:mm:ss z" timeZone="US/Eastern"/>

```

## WebSocket

环境：Java8 + Tomcat8

### Server

```

package websocket;

/**
 * Websocket Server
 * 
 * @author netkiller<netkiller@msn.com>
 */

import java.util.Collections;
import java.util.HashSet;
import java.util.Set;

import javax.websocket.OnClose;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.ServerEndpoint;

@ServerEndpoint(value = "/echo")
public class PriceServer {

	private Set<Session> sessions = Collections.synchronizedSet(new HashSet<Session>());

	/**
	 * Callback hook for Connection open events. This method will be invoked
	 * when a client requests for a WebSocket connection.
	 * 
	 * @param session
	 *            the session which is opened.
	 */
	@OnOpen
	public void onOpen(Session session) {
		sessions.add(session);
	}

	/**
	 * Callback hook for Connection close events. This method will be invoked
	 * when a client closes a WebSocket connection.
	 * 
	 * @param session
	 *            the session which is opened.
	 */
	@OnClose
	public void onClose(Session session) {
		sessions.remove(session);
	}

	/**
	 * Callback hook for Message Events. This method will be invoked when a
	 * client send a message.
	 * 
	 * @param message
	 *            The text message
	 * @param session
	 *            The session of the client
	 */
	@OnMessage
	public void onMessage(String message, Session session) {
		System.out.println("Message Received: " + message);
		for (Session remote : sessions) {
			System.out.println("Sending to " + remote.getId());
			remote.getAsyncRemote().sendText(message);
		}
	}
}

```

### Client

```

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

	<script language="JavaScript">
		var wsuri = "ws://localhost:8080/m.example.com/echo";
		var ws = null;

		function connectEndpoint() {
			ws = new WebSocket(wsuri);
			ws.onmessage = function(evt) {
				//alert(evt.data);
				document.getElementById("echo").value = evt.data;
			};

			ws.onclose = function(evt) {
				//alert("close");
				document.getElementById("echo").value = "end";
			};

			ws.onopen = function(evt) {
				//alert("open");
				document.getElementById("echo").value = "open";
			};
		}

		function sendmsg() {
			ws.send(document.getElementById("send").value);
		}
	</script>
<body onload="connectEndpoint()">
	<input type="text" size="20" value="5" id="send">
	<input type="button" value="send" onclick="sendmsg()">
	<br>
	<input type="text" id="echo">
</body>
</html>

```