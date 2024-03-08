# 第 19 章 Apache Tiles

## 配置 Tiles

### Maven

```

    <dependency>
      <groupId>org.apache.struts</groupId>
	  <artifactId>struts-tiles</artifactId>
      <version>1.3.10</version>
    </dependency>

```

### web.xml

Tiles 有两种配置方法，选择一种最适合你的方式添加到 web.xml 文件即可

第一种方式 Tiles servlet

```

<servlet>
    <servlet-name>tiles</servlet-name>
    <servlet-class>org.apache.tiles.web.startup.TilesServlet</servlet-class>
    ...
    <load-on-startup>2</load-on-startup>
</servlet>

```

第二种方式 Tiles listener

```

<listener>
    <listener-class>org.apache.tiles.web.startup.TilesListener</listener-class>
</listener>

```

## Template 配置模板

WEB-INF/tiles.xml

```

<tiles-definitions>
	<definition name="index" path="/WEB-INF/jsp/index.jsp">
	    <put name="title"  value="Tiles Example" />
	    <put name="header" value="/WEB-INF/jsp/header.jsp" />
	    <put name="menu"   value="/WEB-INF/jsp/menu.jsp" />
	    <put name="body"   value="/WEB-INF/jsp/body.jsp" />
	    <put name="footer" value="/WEB-INF/jsp/footer.jsp" />
	</definition>
	<definition name="template_mobile" template="/WEB-INF/jsp/mobile/template.jsp">
		<put-attribute name="header" value="/WEB-INF/jsp/mobile/header.jsp" />
		<put-attribute name="content" value="" />
		<put-attribute name="footer" value="/WEB-INF/jsp/mobile/footer.jsp" />
	</definition>
</tiles-definitions>

```

/WEB-INF/jsp/mobile/template.jsp

```

<%@page pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<%@taglib prefix="tiles" uri="http://tiles.apache.org/tags-tiles" %>
<html >
<head>
	<title>www.netkiller.cn</title> 
</head>
<body>

<tiles:insertAttribute name="header" ignore="true" />

<tiles:insertAttribute name="content" ignore="true" />

<tiles:insertAttribute name="footer" ignore="true" />

</body>
</html>

```

## Struts tiles

/WEB-INF/tiles.xml

```

	<definition name="Index" extends="template_mobile">
		<put-attribute name="content" value="/WEB-INF/jsp/mobile/Index.jsp" />
	</definition>

```

src/struts.xml

```

<struts>
	<package name="mobile" extends="main" namespace="/mobile">
		<action name="Index" class="cn.netkiller.mobile.action.Index" method="execute">
			<result name="success" type="tiles">Index</result>
		</action>	
	</package>
</struts>

```