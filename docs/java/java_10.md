# 第 21 章 Log

## Logback

http://logback.qos.ch/index.html

Logback 是 log4j 作者开发，目前的趋势 Log4j 逐步被 Logback 取代。

### Maven 包

```

		<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>1.7.25</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-core -->
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-core</artifactId>
			<version>1.2.3</version>
		</dependency>
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-access</artifactId>
			<version>1.2.3</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-classic -->
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-classic</artifactId>
			<version>1.2.3</version>
		</dependency>

```

### Example

```

package com.logs;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class MyApp {
	final static Logger logger = LoggerFactory.getLogger("MyApp.class");
	public static void main(String[] args) {

		logger.trace("trace");
		logger.debug("debug str");
		logger.info("info str");
		logger.warn("warn");
		logger.error("error");
	}
}

```

## slf4j

http://www.slf4j.org/

log4j 已经被 Logback

## log4j

### 安装 Log4j

#### 手工安装

log4j

http://logging.apache.org/

```
wget http://government-grants.org/mirrors/apache.org/logging/log4j/1.2.14/logging-log4j-1.2.14.tar.gz
tar zxvf logging-log4j-1.2.14.tar.gz
cd logging-log4j-1.2.14
cp dist/lib/log4j-1.2.14.jar /srv/java/lib

```

#### Maven

```

	<dependencies>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-api</artifactId>
			<version>2.5</version>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-core</artifactId>
			<version>2.5</version>
		</dependency>
	</dependencies>			

```

### log4j 环境变量

${catalina.home}

```
log4j.appender.R=org.apache.log4j.RollingFileAppender 
log4j.appender.R.File=${catalina.home}/logs/logs_tomcat.log 
log4j.appender.R.MaxFileSize=10KB		

```

### Log4j Example

```

<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
	<Appenders>
		<Console name="Console" target="SYSTEM_OUT">
			<PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />
		</Console>
	</Appenders>
	<Loggers>
		<Logger name="cn.netkiller.Logging" level="trace">
			<AppenderRef ref="Console" />
		</Logger>
		<Logger name="cn.netkiller" level="debug">
			<AppenderRef ref="Console" />
		</Logger>
		<Root level="error">
			<AppenderRef ref="Console" />
		</Root>
	</Loggers>
</Configuration>

```

```

package cn.netkiller;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

public class Logging {
	private static final Logger logger = LogManager.getLogger("appender");

	public void application() {

		String parameter = "sssssssssssss";
		if (logger.isDebugEnabled()) {
			logger.debug("This is debug : " + parameter);
		}

		if (logger.isInfoEnabled()) {
			logger.info("This is info : " + parameter);
		}

		logger.trace("trace");
		logger.debug("debug");
		logger.info("info");
		logger.warn("warn");
		logger.error("error");
		logger.fatal("fatal");

	}

	public static void main(String[] args) {
		Logging log = new Logging();
		log.application();

	}
}

```

### log4j.properties

stdout 标准输出

```
# Root logger option
log4j.rootLogger=INFO, stdout

# Direct log messages to stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n

```