# 第 18 章 Apache Struts

[`struts.apache.org/`](http://struts.apache.org/)

You can checkout all the example applications from the Struts 2 GitHub repository at https://github.com/apache/struts-examples.

## struts.xml

web.xml

```

<?xml version="1.0" encoding="UTF-8"?>
<web-app 

	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
	id="WebApp_ID" version="3.0">
	<display-name>helloworld</display-name>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.htm</welcome-file>
		<welcome-file>default.jsp</welcome-file>
	</welcome-file-list>

	<filter>
		<filter-name>struts2</filter-name>
		<filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
	</filter>

	<filter-mapping>
		<filter-name>struts2</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

</web-app>		

```

### include

```

	<include file="/cn/netkiller/struts/ajax.xml" />
	<include file="/cn/netkiller/struts/admin.xml" />	
	<include file="/cn/netkiller/struts/logs.xml" />		

```

## Struts Tags

使用 Struts Tags 需要在 jsp 页面中加入下面一行。

```

<%@ taglib prefix="s" uri="/struts-tags" %>

```

### property

```

<%@ taglib prefix="s" uri="/struts-tags" %>

<html>
<head>
    <title>Hello</title>
</head>
<body>

Hello, <s:property value="name"/>

</body>
</html>	

```

```

<s:property value="messageStore.message" />
<s:property value="#session.user.username" />

<s:bean name="cn.netkiller.Person" var="personBean" />
<s:property value="#personBean.name" />

```

### set

```

<s:set var="personName" value="person.name"/>
Hello, <s:property value="#personName"/>

<s:set var="janesName">Jane Doe</s:set>
<s:property value="#janesName"/>

```

禁止 HTML 转义，如果你的字符串中含有&, <, > 等字符输出就会出现 &amp;, &lt;, &gt; escapeHtml="false" 可以禁止这样的转义，原样输出。

```

<s:property value="url" escapeHtml="false"/>		

```

https://struts.apache.org/docs/property.html

```
Name	Required	Default	Evaluated	Type	Description
default	false		false	String	The default value to be used if value attribute is null
escapeCsv	false	false	false	Boolean	Whether to escape CSV (useful to escape a value for a column)
escapeHtml	false	true	false	Boolean	Whether to escape HTML
escapeJavaScript	false	false	false	Boolean	Whether to escape Javascript
escapeXml	false	false	false	Boolean	Whether to escape XML		

```

### url

```

<p><a href="<s:url action='hello'/>">Hello World</a></p>

<s:url action="hello" var="helloLink">
  <s:param name="userName">Bruce Phillips</s:param>
</s:url>

<p><a href="${helloLink}">Hello Bruce Phillips</a></p>

```

### s:include

```

<s:include value="/pages/example.jsp"></s:include>			

```

### s:action

```

<%@ taglib prefix="s" uri="/struts-tags" %>
<s:action name="index" namespace="/news" executeResult="true" />

```

```

<s:action name="index" namespace="/member" executeResult="true">
	<s:param name="name">Neo</s:param>
</s:action>

```

### HTML Form

#### form

```

<p>Get your own personal hello by filling out and submitting this form.</p>

<s:form action="hello">

  <s:textfield name="userName" label="Your name" />

   <s:submit value="Submit" />

</s:form>

```

#### textfield

```

<s:textfield name="variable"/>			

```

#### s:hidden

隐藏表单

```

<s:hidden id="unique" name="form.unique" value=""/>			

```

#### select

```

<s:select name="city" list="{'Beijing','Shanghai','Guangdong','Shenzhen'}" theme="simple" headerKey="Shenzhen" headerValue="Shenzhen"></s:select>

<select name="city" id="searchCriteriaForm_city">
    <option value="Shenzhen">Shenzhen</option>
    <option value="Beijing">Beijing</option>
    <option value="Shanghai">Shanghai</option>
    <option value="Guangdong">Guangdong</option>
    <option value="Shenzhen">Shenzhen</option>
</select>

```

```

<s:select name="city" id="city" list="#{1:'Beijing',2:'Shanghai',3:'Guangdong',4:'Shenzhen'}"  label="city" listKey="key" listValue="value"  headerKey="4" headerValue="Shenzhen" />

<select name="city" id="city">
    <option value="4">Shenzhen</option>
    <option value="1">Beijing</option>
    <option value="2">Shanghai</option>
    <option value="3">Guangdong</option>
    <option value="4">Shenzhen</option>
</select>

```

### iterator

```

<s:iterator value="people">
	<s:property value="lastName"/>, <s:property value="firstName"/>
</s:iterator>

```

### if elseif else

```

<s:if test="%{false}">
    <div>Will Not Be Executed</div>
</s:if>
<s:elseif test="%{true}">
    <div>Will Be Executed</div>
</s:elseif>
<s:else>
    <div>Will Not Be Executed</div>
</s:else>		

```

## Action

struts.xml

```

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">

<struts>

	<constant name="struts.devMode" value="true" />

	<package name="basicstruts2" extends="struts-default">
		<action name="index">
			<result>/index.jsp</result>
		</action>

		<action name="hello"
			class="org.apache.struts.helloworld.action.HelloWorldAction" method="execute">
			<result name="success">/HelloWorld.jsp</result>
		</action>

		<action name="register" class="org.apache.struts.register.action.Register"
			method="execute">
			<result name="success">/thankyou.jsp</result>
			<result name="input">/register.jsp</result>
		</action>

	</package>
</struts>

```

### redirect

```

<action name="hello" 
   class="com.tutorialspoint.struts2.HelloWorldAction"
   method="execute">
   <result name="success" type="redirect">
       <param name="location">
         /NewWorld.jsp
      </param >
   </result>
</action>

```

### redirectAction

```

<package name="public" extends="struts-default">
    <action name="login" class="...">
        <!-- Redirect to another namespace -->
        <result type="redirectAction">
            <param name="actionName">dashboard</param>
            <param name="namespace">/secure</param>
        </result>
    </action>
</package>

<package name="secure" extends="struts-default" namespace="/secure">
    <-- Redirect to an action in the same namespace -->
    <action name="dashboard" class="...">
        <result>dashboard.jsp</result>
        <result name="error" type="redirectAction">error</result>
    </action>

    <action name="error" class="...">
        <result>error.jsp</result>
    </action>
</package>

<package name="passingRequestParameters" extends="struts-default" namespace="/passingRequestParameters">
   <!-- Pass parameters (reportType, width and height) -->
   <!--
   The redirectAction url generated will be :
   /genReport/generateReport.action?reportType=pie&amp;width=100&amp;height=100#summary
   -->
   <action name="gatherReportInfo" class="...">
      <result name="showReportResult" type="redirectAction">
         <param name="actionName">generateReport</param>
         <param name="namespace">/genReport</param>
         <param name="reportType">pie</param>
         <param name="width">100</param>
         <param name="height">100</param>
         <param name="empty"></param>
         <param name="suppressEmptyParameters">true</param>
         <param name="anchor">summary</param>
      </result>
   </action>

   <action name="Auth" class="cn.netkiller.api.action.Auth">
		<result name="credit" type="redirectAction">
			<param name="actionName">history</param>
			<param name="namespace">/api/report</param>
			<param name="LoginName">${LoginName}</param>
			<param name="Direction">${Direction}</param>
			<param name="StartDate">${StartDate}</param>
			<param name="EndDate">${EndDate}</param>
		</result>
	</action>

</package>

```

### JSON

JSON 拦截器

```

	<action name="withdraw" class="cn.netkiller.api.action.Report" method="getHistory">
		<interceptor-ref name="defaultStack" />
		<interceptor-ref name="json">
			<param name="enableSMD">true</param>
		</interceptor-ref>
		<result name="success" type="json">
			<param name="enableGZIP">true</param>
			<param name="excludeProperties">.*direction</param>
		</result>
	</action>

```

#### enableGZIP 压缩传输

```

		<result name="success" type="json">
			<param name="enableGZIP">true</param>
		</result>

```

#### excludeProperties 排除 Properties

```

		<result name="success" type="json">
			<param name="excludeProperties">.*direction</param>
		</result>

```

### 传递 Timestamp 变量

Struts 从 URL 传递 Timestamp 类型的变量 StartDate

```

http://www.netkiller.cn/api/report/history.do?LoginName=888666&StartDate=2016-01-01&EndDate=2016-02-29

```

解决方法 参考 setStartDate / setEndDate 两个方法，以字符串方式传入，然后转换为 Timestamp 类型

```

package cn.netkiller.api.action;

import java.sql.Timestamp;
import java.util.Calendar;
import org.apache.struts2.json.annotations.JSON;
import com.opensymphony.xwork2.Action;
import com.opensymphony.xwork2.ActionSupport;

public class Report extends ActionSupport {
	private static final long serialVersionUID = 6484202866632836225L;

	private String loginName;
	private Timestamp startDate = null;
	private Timestamp endDate = null;

	public Report() {

	}

	public String execute() {
		return Action.SUCCESS;
	}

	public String getLoginName() {
		return loginName;
	}

	public void setLoginName(String loginName) {
		this.loginName = loginName;
	}

	public Timestamp getStartDate() {
		return startDate;
	}

	public void setStartDate(String startDate) {
		this.startDate = Timestamp.valueOf(startDate + " 00:00:00");
	}

	public Timestamp getEndDate() {
		return endDate;
	}

	public void setEndDate(String endDate) {
		this.endDate = Timestamp.valueOf(endDate + " 00:00:00");
	}

}

```

## Ajax + JSON

struts.xml 中加入

```

		<action name="Captcha" class="com.example.action.ajax.Captcha">
			<result name="success" type="json"></result>
		</action>

```

Java 文件

```

package com.example.action.ajax;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import com.opensymphony.xwork2.Action;
import com.opensymphony.xwork2.ActionSupport;

public class Captcha extends ActionSupport {
	private static final long serialVersionUID = 7284583098398030297L;

	private String string1 = "A";
	private String[] stringarray1 = {"A1","B1"};
	private int number1 = 123456789;
	private int[] numberarray1 = {1,2,3,4,5,6,7,8,9};
	private List<String> lists = new ArrayList<String>();
	private Map<String, String> maps = new HashMap<String, String>();

	//no getter method, will not include in the JSON

	public Captcha(){
		lists.add("list1");
		lists.add("list2");
		lists.add("list3");
		lists.add("list4");
		lists.add("list5");

		maps.put("key1", "value1");
		maps.put("key2", "value2");
		maps.put("key3", "value3");
		maps.put("key4", "value4");
		maps.put("key5", "value5");
	}

	public String execute() {
               return Action.SUCCESS;
        }

	public String getString1() {
		return string1;
	}

	public void setString1(String string1) {
		this.string1 = string1;
	}

	public String[] getStringarray1() {
		return stringarray1;
	}

	public void setStringarray1(String[] stringarray1) {
		this.stringarray1 = stringarray1;
	}

	public int getNumber1() {
		return number1;
	}

	public void setNumber1(int number1) {
		this.number1 = number1;
	}

	public int[] getNumberarray1() {
		return numberarray1;
	}

	public void setNumberarray1(int[] numberarray1) {
		this.numberarray1 = numberarray1;
	}

	public List<String> getLists() {
		return lists;
	}

	public void setLists(List<String> lists) {
		this.lists = lists;
	}

	public Map<String, String> getMaps() {
		return maps;
	}

	public void setMaps(Map<String, String> maps) {
		this.maps = maps;
	}

}

```

测试 URL http://localhost:8080/ajax/Captcha.action

### GET/POST JSON

struts.xml 文件加入 enableSMD

```

		<action name="Captcha" class="com.example.action.ajax.Captcha">
			<interceptor-ref name="defaultStack" />
			<interceptor-ref name="json">
				<param name="enableSMD">true</param>
			</interceptor-ref>
			<result name="success" type="json"></result>
		</action>

```

Java 代码

```

package com.example.action.ajax;

import com.opensymphony.xwork2.Action;
import com.opensymphony.xwork2.ActionSupport;

public class Captcha extends ActionSupport {
	private static final long serialVersionUID = 7284583098398030297L;

	private String phone = "13113668890";
	private String email = "netkiller@msn.com";
	private Boolean status = false;

	//no getter method, will not include in the JSON

	public Captcha() {

	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String execute() {
		this.status = true;
		System.out.printf("%s, %s, %s, %b\n", this.getClass().getName(), this.getPhone(), this.getEmail(), this.getStatus());
		return Action.SUCCESS;
	}

	public String getPhone() {
		return this.phone;
	}

	public void setPhone(String phone) {
		this.phone = phone;
	}

	public void setStatus(Boolean status) {
		this.status = status;
	}
	public boolean getStatus() {
		return this.status;
	}
}

```

使用 curl 模拟 post 测试

```

# curl http://192.168.4.34:8080/ajax/Captcha.do
{"email":"netkiller@msn.com","phone":"13113668890","status":true} 

# curl -X POST -H "Content-Type:application/json" -d '{"phone":"13066884444", "email":"neo.chan@live.com"}' http://192.168.4.34:8080/ajax/Captcha.do
{"email":"neo.chan@live.com","phone":"13066884444","status":true}

```

GET 例子

```

# curl "http://192.168.4.34:8080/ajax/Captcha.do?phone=13322993040&email=netkiller@mac.com"
{"email":"netkiller@mac.com","phone":"13322993040","status":true}

```

## Json 内容展示

Struts 配置文件

```

	<package name="information" extends="main" namespace="/inf">
		<action name="Information" class="com.example.action.Infomation">
			<result type="tiles">information</result>
		</action>
	</package>

```

Action 文件

```

package cn.netkiller.action;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.StringReader;
import java.net.URL;
import java.net.URLConnection;
import java.nio.charset.Charset;

import javax.json.Json;
import javax.json.JsonObject;
import javax.json.JsonReader;

import com.opensymphony.xwork2.Action;
import com.opensymphony.xwork2.ActionSupport;

public class Infomation extends ActionSupport{
	/**
	 * 
	 */
	private static final long serialVersionUID = 1L;

	private JsonObject jsonObject = null;
	private String jsonString = "";

	@Override
	public String execute() throws IOException {

		String URL = "http://inf.example.com/list/json/93/20/0.html";
		System.out.printf("%s Requeted URL is %s", this.getClass().getName(), URL);

		StringBuilder sb = new StringBuilder();
		URLConnection urlConn = null;
		InputStreamReader in = null;
		try {
			URL url = new URL(URL);
			urlConn = url.openConnection();
			if (urlConn != null)
				urlConn.setReadTimeout(60 * 1000);
			if (urlConn != null && urlConn.getInputStream() != null) {
				in = new InputStreamReader(urlConn.getInputStream(), Charset.defaultCharset());
				BufferedReader bufferedReader = new BufferedReader(in);
				if (bufferedReader != null) {
					int cp;
					while ((cp = bufferedReader.read()) != -1) {
						sb.append((char) cp);
					}
					bufferedReader.close();
				}
			}
			in.close();

			jsonString = sb.toString();

			System.out.println(jsonString);

			JsonReader reader = Json.createReader(new StringReader(jsonString));

			JsonObject jsonObject = reader.readObject();
			this.setJsonObject(jsonObject);

			reader.close();

			// System.out.println(jsonObject.size());

			/*for (int i = 0; i < jsonObject.size() - 2; i++) {
				JsonObject rowObject = jsonObject.getJsonObject(Integer.toString(i));
				// System.out.println(rowObject.toString());
				System.out.printf("%s\t%s\t%s\n", rowObject.getJsonString("id"), rowObject.getJsonString("title"),
						rowObject.getJsonString("ctime"));
			}*/

		} catch (Exception e) {
			throw new RuntimeException("Exception while calling URL:" + URL, e);
		}

		return Action.SUCCESS;
	}

	public JsonObject getJsonObject() {
		return jsonObject;
	}

	public void setJsonObject(JsonObject jsonObject) {
		this.jsonObject = jsonObject;
	}

	public String getJsonString() {
		return jsonString;
	}

	public void setJsonString(String jsonString) {
		this.jsonString = jsonString;
	}
}

```

JSP 文件

```

<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib prefix="s" uri="/struts-tags"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

<table>
	<c:forEach items="${jsonObject.entrySet()}" var="json"
		varStatus="status">
		<c:if test="${not status.last}">
			<tr>
				<td>${json.key+1}</td>
				<td>${json.value.getJsonString("id")}</td>
				<td>${json.value.getJsonString("title")}</td>
				<td>${json.value.getJsonString("ctime")}</td>
			</tr>
		</c:if>
	</c:forEach>
</table>
===================

<c:forEach items="${jsonObject.entrySet()}" var="json">
      ${json.value.toString()}
</c:forEach>

===========

<s:property value="jsonString" />

```

解决双引号问题

```

<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>

<%@ taglib prefix="s" uri="/struts-tags"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn"%>

<table>
	<c:forEach items="${jsonObject.entrySet()}" var="json"
		varStatus="status">
		<c:if test="${not status.last}">

			<c:set var="id"
				value="${fn:replace(json.value.getJsonString('id'),'\"', '')}" />
			<c:set var="title" value="${fn:replace(json.value.getJsonString('title'),'\"', '')}" />
			<c:set var="ctime"
				value="${fn:replace(json.value.getJsonString('ctime'),'\"', '')}" />
			<tr>
				<td><c:out value="${status.count}" /></td>
				<td><a href="/inf/Detail.do?id=<c:out value="${id}"/>"><c:out
							value="${title}" /></a></td>
				<td><c:out value="${ctime}" /></td>
			</tr>
		</c:if>
	</c:forEach>
</table>

<c:set var="pages" value="${jsonObject.getJsonObject('pages')}" />

<a href="${pages.first}">首页</a>
<a href="${pages.before}">上一页</a>
<a href="${pages.next}">下一页</a>
<a href="${pages.last}">尾页</a>

<span>Count: ${pages.count}, Total: ${pages.total}</span>

```

### 禁止方法

```

	@JSON(serialize = false)
	public String getDatas() {
		return datas;
	}

```

使用 excludeProperties 在 Action 中排除

```

	<action name="withdraw" class="cn.netkiller.api.action.Report" method="getHistory">
		<interceptor-ref name="defaultStack" />
		<interceptor-ref name="json">
			<param name="enableSMD">true</param>
		</interceptor-ref>
		<result name="success" type="json">
			<param name="enableGZIP">true</param>
			<param name="excludeProperties">.*direction</param>
		</result>
	</action>

```

### 格式化日期

```
@JSON(format="yyyy-MM-dd") 
public Date getDate(){ 
	return date; 
}			

```

### 重命名变量名

```
@JSON(name="mypassword") 
public String getPassword() { 
	return password; 
}

```

### org.apache.struts2.json

```
JSONUtil.serialize(sb.toString());
JSONUtil.deserialize(sb.toString());			

```

## Interceptor

### Session

在 web.xml 文件中定义 Session 超时时间

```

<session-config>  
    <session-timeout>30</session-timeout>  
</session-config>			

```

创建拦截器程序

```

package cn.netkiller.interceptor;

import java.util.Map;
import java.lang.Override;

import com.opensymphony.xwork2.ActionInvocation;
import com.opensymphony.xwork2.interceptor.AbstractInterceptor;

public class SessionInterceptor extends AbstractInterceptor {

	private static final long serialVersionUID = 8347994918002285514L;

	@Override
	public String intercept(ActionInvocation invocation) throws Exception {
		Map<String, Object> session = invocation.getInvocationContext().getSession();
		if (session.isEmpty())
			return "nosession"; // session is empty/expired
		return invocation.invoke();
	}
}

```

配置拦截器

```

	<package name="mobile" extends="main" namespace="/mobile">
		<global-results>
			<result name="nosession" type="redirectAction">
				<param name="actionName">Index</param>
				<param name="namespace">/mobile</param>
			</result>
		</global-results>			
		<interceptor name="session" class="cn.netkiller.SessionInterceptor" />
		<interceptor-stack name="sessionExpirayStack">
    		<interceptor-ref name="defaultStack"/>
    		<interceptor-ref name="session"/>
   		</interceptor-stack>
   		<default-interceptor-ref name="sessionExpirayStack" />

		<action name="testAction" class="TestClass">
    		<interceptor-ref name="sessionExpirayStack" />
    		<result name="success">success.jsp</result>
    		<result name="error">error.jsp</result>
  		</action>
  	</package>

```

## Action 中使用线程

背景，在 Action 中发送邮件，阻塞程序继续执行并返回 500，使用 Thread 实现异步发送，因为我们并不关心邮件是否到达,只需正常发送即可。

```

	public String execute(){

		...
		...

			try {
				// Send email

				Thread sendmail = new Thread(new Runnable() {
					@Override
					public void run() {
						try {
							log.info("sendEmail Begin");
							sender.sendEmail(form.getEmail(), form.getText());
							log.info("sendEmail End");
						} catch (Exception e) {
							e.printStackTrace();
						}
					}
				});
				sendmail.setName("sendmail" + sendmail.getId() + "logingName:" + form.getLoginname());
				sendmail.start();

			} catch (Exception e) {
				e.printStackTrace();
				log.info("sendEmail Error");
			}

			...
			...

			log.info("CreateTrialAccount:" + form.toString());
			return Action.SUCCESS;
	}

```

## 日志

WEB-INF/classes/log4j.properties

```
## # Struts2
log4j.logger.freemarker=INFO
log4j.logger.org.apache.struts2=DEBUG
log4j.logger.org.apache.struts2.components=INFO
log4j.logger.org.apache.struts2.dispatcher=INFO
log4j.logger.org.apache.struts2.convention=INFO
log4j.logger.org.apache.struts2.util=DEBUG
log4j.logger.org.apache.tiles.access.TilesAccess=DEBUG
log4j.logger.org.apache.struts2.interceptor=DEBUG

## # OpenSymphony Stuff
log4j.logger.com.opensymphony=INFO
log4j.logger.com.opensymphony.xwork2.ognl=INFO
log4j.logger.com.opensymphony.xwork2.util=INFO		

```

## FAQ

### Struts 怎样判断用户来自电脑还是移动设备

```

package cn.netkiller.action;

import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;
import javax.servlet.http.HttpServletRequest;

public class Index extends ActionSupport {

	private static final long serialVersionUID = 7782483098148890753L;

	public String execute() {
		HttpServletRequest request = (HttpServletRequest) ActionContext.getContext().get(org.apache.struts2.StrutsStatics.HTTP_REQUEST);
		String userAgent = request.getHeader("User-Agent").toLowerCase();
		System.out.println(this.getClass().getName() + ": " + userAgent);
		String[] mobileAgents = { "iphone", "android", "phone", "mobile", "wap", "netfront", "java", "opera mobi", "opera mini", "ucweb", "windows ce", "symbian", "series", "webos", "sony", "blackberry", "dopod", "nokia", "samsung", "palmsource", "xda", "pieplus", "meizu", "midp", "cldc", "motorola", "foma", "docomo", "up.browser", "up.link", "blazer", "helio", "hosin", "huawei", "novarra", "coolpad", "webos", "techfaith", "palmsource", "alcatel", "amoi", "ktouch", "nexian", "ericsson", "philips", "sagem", "wellcom", "bunjalloo", "maui", "smartphone", "iemobile", "spice", "bird", "zte-", "longcos", "pantech", "gionee", "portalmmm", "jig browser", "hiptop", "benq", "haier", "^lct", "320x320", "240x320", "176x220", "w3c ", "acs-", "alav", "alca", "amoi", "audi", "avan", "benq", "bird", "blac", "blaz", "brew", "cell", "cldc", "cmd-", "dang", "doco", "eric", "hipt", "inno", "ipaq", "java", "jigs", "kddi", "keji", "leno", "lg-c", "lg-d", "lg-g", "lge-", "maui", "maxo", "midp", "mits", "mmef", "mobi", "mot-",
				"moto", "mwbp", "nec-", "newt", "noki", "oper", "palm", "pana", "pant", "phil", "play", "port", "prox", "qwap", "sage", "sams", "sany", "sch-", "sec-", "send", "seri", "sgh-", "shar", "sie-", "siem", "smal", "smar", "sony", "sph-", "symb", "t-mo", "teli", "tim-", "tsm-", "upg1", "upsi", "vk-v", "voda", "wap-", "wapa", "wapi", "wapp", "wapr", "webc", "winw", "winw", "xda", "xda-", "Googlebot-Mobile" };
		if (userAgent != null) {
			for (String mobileAgent : mobileAgents) {
				if (userAgent.indexOf(mobileAgent) >= 0) {
					return "mobile";
				}
			}
		}

		return "computer";
	}
}			

```

```

	<action name="Index" class="cn.netkiller.action.Index">
		<result name="computer" type="redirectAction">
			<param name="actionName">index</param>
			<param name="namespace">/computer</param>
		</result>
		<result name="mobile" type="redirect">
			<param name="location">http://m.netkiller.cn/index.html</param>
		</result>
	</action>			

```