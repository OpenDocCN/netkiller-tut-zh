# 第 17 章 MyBatis

http://blog.mybatis.org/

## Mybatis 入门

创建数据库与表并插入测试数据

```
CREATE DATABASE `mybatis` /*!40100 COLLATE 'utf8_general_ci' */;

CREATE USER 'mybatis'@'192.168.%' IDENTIFIED BY 'mybatis';
GRANT USAGE ON *.* TO 'mybatis'@'192.168.%';
GRANT SELECT, EXECUTE, SHOW VIEW, ALTER, ALTER ROUTINE, CREATE, CREATE ROUTINE, CREATE TEMPORARY TABLES, CREATE VIEW, DELETE, DROP, EVENT, INDEX, INSERT, REFERENCES, TRIGGER, UPDATE, LOCK TABLES  ON `mybatis`.* TO 'mybatis'@'192.168.%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
SHOW GRANTS FOR 'mybatis'@'192.168.%';

CREATE TABLE IF NOT EXISTS `user` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(50) DEFAULT NULL,
  `age` int(10) unsigned DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;

INSERT INTO `user` (`id`, `name`, `age`) VALUES
	(1, 'Neo', 35),
	(2, 'Jerry', 36);

```

Maven pom.xml 中加入依赖包

```

	<dependencies>
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.3.0</version>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.37</version>
		</dependency>
	</dependencies>

```

pom.xml

```

<project  
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>MyBatis</groupId>
	<artifactId>MyBatis</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.3.0</version>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.37</version>
		</dependency>
	</dependencies>
	<build>
		<sourceDirectory>src</sourceDirectory>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.3</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>

```

src/mybatis.xml

```

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			<!-- 配置数据库连接信息 -->
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.jdbc.Driver" />
				<property name="url" value="jdbc:mysql://192.168.6.1:3306/mybatis" />
				<property name="username" value="mybatis" />
				<property name="password" value="mybatis" />
			</dataSource>
		</environment>
	</environments>
	<mappers>

		<mapper resource="cn/netkiller/mapping/userMapping.xml" />
	</mappers>
</configuration>

```

src/cn/netkiller/mapping/userMapping.xml

```

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="cn.netkiller.mapping.UserMapping">
	<select id="getUser" parameterType="String" resultType="cn.netkiller.model.User">
		select * from user where id=#{id}
	</select>
</mapper>

```

resultType 文件

```

package cn.netkiller.model;

public class User {
	private String id;
	private String name;
	private int age;

	public String getId() {
		return id;
	}

	public void setId(String id) {
		this.id = id;
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

	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + ", age=" + age + "]";
	}
}

```

测试代码

```

package cn.netkiller.test;

import java.io.InputStream;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import cn.netkiller.model.*;

public class Tests {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		String resource = "mybatis.xml";

		InputStream is = Tests.class.getClassLoader().getResourceAsStream(resource);
		SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(is);
		SqlSession session = sessionFactory.openSession();

		String statement = "cn.netkiller.mapping.UserMapping.getUser";// 映射 sql 的标识字符串

		User user = session.selectOne(statement, "2");
		System.out.println(user.toString());
	}

}		

```

## 接口注解

```

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="com.mysql.jdbc.Driver" />
				<property name="url" value="jdbc:mysql://192.168.6.1:3306/mybatis" />
				<property name="username" value="mybatis" />
				<property name="password" value="mybatis" />
			</dataSource>
		</environment>
	</environments>
	<mappers>
		<mapper class="cn.netkiller.mapper.UserMapper" />
	</mappers>
</configuration>

```

```

package cn.netkiller.mapper;

import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.Param;

import cn.netkiller.model.User;

public interface UserMapper {
	@Select("SELECT * FROM `user` WHERE id = #{id}")
	public User findById(@Param("id") int id); 
}		

```

```

package cn.netkiller.test;

import java.io.InputStream;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import cn.netkiller.mapper.UserMapper;
import cn.netkiller.model.User;

public class AnnotationsTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		String resource = "mybatis.xml";

		InputStream is = XmlTest.class.getClassLoader().getResourceAsStream(resource);
		SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(is);
		SqlSession session = sessionFactory.openSession();

		UserMapper mapper = session.getMapper(UserMapper.class);
		User user = mapper.findById(2);

		System.out.println(user.toString());

	}
}

```