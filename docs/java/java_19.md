# 部分 II. Java UnitTest

## 第 30 章 Junit5

JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage

## 项目初始化

### Maven

[`github.com/junit-team/junit5-samples/tree/r5.3.2/junit5-jupiter-starter-maven`](https://github.com/junit-team/junit5-samples/tree/r5.3.2/junit5-jupiter-starter-maven)

## JUnit 5 注解

JUnit 5 常用注解

```

名称				说明
@Test			表明一个测试方法
@DisplayName	测试类或方法的显示名称
@BeforeEach		表明在单个测试方法运行之前执行的方法
@AfterEach		表明在单个测试方法运行之后执行的方法
@BeforeAll		表明在所有测试方法运行之前执行的方法
@AfterAll		表明在所有测试方法运行之后执行的方法
@Disabled		禁用测试类或方法
@Tag			为测试类或方法添加标签

```

### @Disabled

```

	@Test
    @Disabled("这条用例暂时跑不过，忽略!")
    void myFailTest(){
        assertEquals(1,2);
    }			

```

### @Tag

元注解标签

```

@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Tag("remote")
public @interface Remote {
}			

```

```

@DisplayName("Remote test")
public class RemoteTest {
	@Test
	@Remote
	public void testGetUser() {
		System.out.println("Get user");
	}
}

```

### @Nested

```

package junit5;

import static org.junit.jupiter.api.Assertions.*;

import java.util.HashMap;
import java.util.Map;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;

@DisplayName("Nested tests for HashMap")
class Junit5NestedTest {

	Map<String, Object> map;

	@Nested

	@DisplayName("when a new")
	class WhenNew {

		@BeforeEach
		void create() {
			map = new HashMap<>();
		}

		@Test

		@DisplayName("is empty")
		void isEmpty() {
			assertTrue(map.isEmpty());
		}

		@Nested

		@DisplayName("after adding a new entry")
		class AfterAdd {

			String key = "key";
			Object value = "value";

			@BeforeEach
			void add() {
				map.put(key, value);
			}

			@Test

			@DisplayName("is not empty")
			void isNotEmpty() {
				assertFalse(map.isEmpty());
			}

			@Test

			@DisplayName("returns value when getting by key")
			void returnValueWhenGettingByKey() {
				assertEquals(value, map.get(key));
			}

			@Nested

			@DisplayName("after removing the entry")
			class AfterRemove {

				@BeforeEach
				void remove() {
					map.remove(key);
				}

				@Test

				@DisplayName("is empty now")
				void isEmpty() {
					assertTrue(map.isEmpty());
				}

				@Test

				@DisplayName("returns null when getting by key")
				void returnNullForKey() {
					assertNull(map.get(key));
				}
			}
		}
	}

}

```

### @TestFactory

```

	@TestFactory
	public Collection<DynamicTest> simpleDynamicTest() {
		return Collections.singleton(dynamicTest("simple dynamic test", () -> assertTrue(2 > 1)));
	}			

```

DynamicTest 提供了一个静态方法 stream 来根据输入生成动态测试

```

	@TestFactory
	public Stream<DynamicTest> streamDynamicTest() {
	 return stream(
	       Stream.of("Hello", "World").iterator(),
	       (word) -> String.format("Test - %s", word),
	       (word) -> assertTrue(word.length() > 4)
	 );
	}

```

## JUnit 5 断言

```

常用的断言方法
方法				说明
assertEquals	判断两个对象或两个原始类型是否相等
assertNotEquals	判断两个对象或两个原始类型是否不相等
assertSame	判断两个对象引用是否指向同一个对象
assertNotSame	判断两个对象引用是否指向不同的对象
assertTrue	判断给定的布尔值是否为 true
assertFalse	判断给定的布尔值是否为 false
assertNull	判断给定的对象引用是否为 null
assertNotNull	判断给定的对象引用是否不为 null		

```

### assertArrayEquals

assertArrayEquals 方法的示例

```

	@Test
	@DisplayName("array assertion")
	public void array() {
		assertArrayEquals(new int[]{1, 2}, new int[] {1, 2});
	}			

```

### assertAll

```

	@Test
    @DisplayName("运行一组断言")
    public void assertAllCase() {
        assertAll("groupAssert",
                () -> assertEquals(2, 1 + 1),
                () -> assertTrue(1 > 0)
        );
    }

```

assertThrows

通过 assertThrows 或 expectThrows 来判断是否抛出期望的异常类型

assertThrows 和 expectThrows 方法的示例

```

	@Test
	@DisplayName("throws exception")
	public void exception() {
		assertThrows(ArithmeticException.class, () -> System.out.println(1 / 0));
	}

```

### fail

通过 fail 方法直接使得测试失败

```

	@Test
	@DisplayName("fail")
	public void shouldFail() {
		fail("This should fail");
	}			

```

### JUnit 5 前置条件

前置条件（assumptions）类似于断言，不同之处在于不满足的断言会使得测试方法失败，而不满足的前置条件只会使得测试方法的执行终止。前置条件可以看成是测试方法执行的前提，当该前提不满足时，就没有继续执行的必要。

```

	@DisplayName("Assumptions")
	public class AssumptionsTest {
		private final String environment = "DEV";

		 @Test
		 @DisplayName("simple")
		 public void simpleAssume() {
		    assumeTrue(Objects.equals(this.environment, "DEV"));
		    assumeFalse(() -> Objects.equals(this.environment, "PROD"));
		 }

		 @Test
		 @DisplayName("assume then do")
		 public void assumeThenDo() {
		    assumingThat(
		       Objects.equals(this.environment, "DEV"),
		       () -> System.out.println("In DEV")
		    );
		 }
	}			

```

assumeTrue 和 assumFalse 确保给定的条件为 true 或 false，不满足条件会使得测试执行终止。assumingThat 的参数是表示条件的布尔值和对应的 Executable 接口的实现对象。只有条件满足时，Executable 对象才会被执行；当条件不满足时，测试执行并不会终止。

## 依赖注入

### TestInfo

```

	@Test
	@DisplayName("test info")
	public void testInfo(final TestInfo testInfo) {
		System.out.println(testInfo.getDisplayName());
	}			

```

### TestReporter

```

	@Test
	@DisplayName("test reporter")
	public void testReporter(final TestReporter testReporter) {
		testReporter.publishEntry("name", "Alex");
	}			

```

## Junit4

### 生成 HTML 报告

```

<project   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>demo</groupId>
	<artifactId>junit4</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>junit4</name>
	<url>http://maven.apache.org</url>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>

	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.11</version>
			<scope>test</scope>
		</dependency>
	</dependencies>
	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-surefire-plugin</artifactId>
				<version>2.22.1</version>
				<configuration>
					<useSystemClassLoader>false</useSystemClassLoader>
				</configuration>
			</plugin>
		</plugins>
	</build>
	<reporting>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-surefire-report-plugin</artifactId>
				<version>2.22.1</version>
			</plugin>
		</plugins>
	</reporting>
</project>

```

生成报告

```

neo@MacBook-Pro ~/workspace/junit4 % mvn surefire-report:report-only

```

### Junit4 输出格式定义

```

package cn.netkiller.api.test;

import java.util.ArrayList;
import java.util.List;

import org.junit.experimental.categories.Category;
import org.junit.runner.Description;
import org.junit.runners.BlockJUnit4ClassRunner;
import org.junit.runners.model.FrameworkMethod;
import org.junit.runners.model.InitializationError;

public class ApiTestRunner extends BlockJUnit4ClassRunner {

	public ApiTestRunner(Class<?> class) throws InitializationError {
		super(class);
	}

	@Override
	protected Description describeChild(FrameworkMethod method) {
		Api api = method.getAnnotation(Api.class);
		if (api == null) {
			return super.describeChild(method);
		}

		List<String> list = new ArrayList<String>();
		Class<?>[] categorys;

		if (method.getAnnotation(Category.class) != null) {
			categorys = method.getAnnotation(Category.class).value();
		} else {
			categorys = this.getTestClass().getJavaClass().getAnnotation(Category.class).value();
		}

		for (Class<?> category : categorys) {
			if (category.getSimpleName().equals("EnterpriseCategory")) {
				list.add("Enterprise");
			} else if (category.getSimpleName().equals("ProfessionalCategory")) {
				list.add("Professional");
			}
		}

		String tag = "";
		if (list != null) {
			tag = String.join(",", list);
		}
		String name = String.format("%s - %s %s - %s", method.getName(), api.method(), api.uri(), tag);
		return Description.createTestDescription(getTestClass().getJavaClass(), name, method.getAnnotations());
	}
}

```

```

package cn.netkiller.api.test;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(value = RetentionPolicy.RUNTIME)
@Target(value = { ElementType.METHOD })
public @interface Api {
    public ApiMethod method() default ApiMethod.GET;

    public String uri() default "/";
}

```

```

public interface ProfessionalCategory {
    String name = "Professional";
}

public interface EnterpriseCategory {
    String name = "Enterprise";
}		

```

## 第 31 章 TestNG

## 第 32 章 JaCoCo - Java code coverage tool

## Maven

```

mvn help:describe -Dplugin=org.jacoco:jacoco-maven-plugin -Ddetail		

```

```

<project   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>demo</groupId>
	<artifactId>junit4</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>junit4</name>
	<url>http://maven.apache.org</url>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>

	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.11</version>
			<scope>test</scope>
		</dependency>
	</dependencies>
	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-surefire-plugin</artifactId>
				<version>2.22.1</version>
				<configuration>
					<useSystemClassLoader>false</useSystemClassLoader>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.jacoco</groupId>
				<artifactId>jacoco-maven-plugin</artifactId>
				<version>0.8.2</version>
				<executions>
					<execution>
						<goals>
							<goal>prepare-agent</goal>
						</goals>
					</execution>
					<execution>
						<id>report</id>
						<phase>test</phase>
						<goals>
							<goal>report</goal>
						</goals>
					</execution>
				</executions>

			</plugin>
		</plugins>
	</build>
	<reporting>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-surefire-report-plugin</artifactId>
				<version>2.22.1</version>
			</plugin>
		</plugins>
	</reporting>
</project>

```

运行 mvn test 便会生成 jacoco 报告

```

neo@MacBook-Pro ~/workspace/junit4 % find target/site/jacoco 
target/site/jacoco
target/site/jacoco/jacoco.csv
target/site/jacoco/demo.junit4
target/site/jacoco/demo.junit4/index.source.html
target/site/jacoco/demo.junit4/index.html
target/site/jacoco/demo.junit4/App.java.html
target/site/jacoco/demo.junit4/App.html
target/site/jacoco/index.html
target/site/jacoco/jacoco-sessions.html
target/site/jacoco/jacoco-resources
target/site/jacoco/jacoco-resources/branchnc.gif
target/site/jacoco/jacoco-resources/sort.js
target/site/jacoco/jacoco-resources/greenbar.gif
target/site/jacoco/jacoco-resources/sort.gif
target/site/jacoco/jacoco-resources/source.gif
target/site/jacoco/jacoco-resources/report.gif
target/site/jacoco/jacoco-resources/prettify.js
target/site/jacoco/jacoco-resources/branchpc.gif
target/site/jacoco/jacoco-resources/branchfc.gif
target/site/jacoco/jacoco-resources/class.gif
target/site/jacoco/jacoco-resources/prettify.css
target/site/jacoco/jacoco-resources/report.css
target/site/jacoco/jacoco-resources/group.gif
target/site/jacoco/jacoco-resources/package.gif
target/site/jacoco/jacoco-resources/redbar.gif
target/site/jacoco/jacoco-resources/up.gif
target/site/jacoco/jacoco-resources/session.gif
target/site/jacoco/jacoco-resources/down.gif
target/site/jacoco/jacoco-resources/method.gif
target/site/jacoco/jacoco-resources/bundle.gif
target/site/jacoco/jacoco.xml

```

## Gradle

https://docs.gradle.org/current/userguide/jacoco_plugin.html

plugins 中加入 id 'jacoco'

```

/*
 * This file was generated by the Gradle 'init' task.
 *
 * This generated file contains a sample Java Library project to get you started.
 * For more details take a look at the Java Libraries chapter in the Gradle
 * user guide available at https://docs.gradle.org/5.0/userguide/java_library_plugin.html
 */

plugins {
    // Apply the java-library plugin to add support for Java Library
    id 'java-library'
    // id 'java'
    id 'eclipse' // optional (to generate Eclipse project files)
	id 'idea' // optional (to generate IntelliJ IDEA project files)
	id 'jacoco'
}

repositories {
    // Use jcenter for resolving your dependencies.
    // You can declare any Maven/Ivy/file repository here.
    jcenter()
    //mavenCentral()
}

dependencies {

    testCompile('org.junit.jupiter:junit-jupiter-api:5.3.2')
	testCompile('org.junit.jupiter:junit-jupiter-params:5.3.2')
	testRuntime('org.junit.jupiter:junit-jupiter-engine:5.3.2')

	testCompile 'io.rest-assured:rest-assured:3.2.0'
	testCompile 'io.rest-assured:json-schema-validator:3.2.0'
	compile 'io.rest-assured:rest-assured:3.2.0'
	compile 'io.rest-assured:json-path:3.2.0'
	compile 'io.rest-assured:xml-path:3.2.0'

}

test {
	useJUnitPlatform()
	testLogging {
		events "passed", "skipped", "failed"
	}
}		

```

运行 gradlew build jacocoTestReport 生成测试报告

```

neo@MacBook-Pro ~/workspace/junit5 % ./gradlew build jacocoTestReport

BUILD SUCCESSFUL in 5s
6 actionable tasks: 2 executed, 4 up-to-date
neo@MacBook-Pro ~/workspace/junit5 % find build/reports/jacoco
build/reports/jacoco
build/reports/jacoco/test
build/reports/jacoco/test/html
build/reports/jacoco/test/html/index.html
build/reports/jacoco/test/html/jacoco-sessions.html
build/reports/jacoco/test/html/jacoco-resources
build/reports/jacoco/test/html/jacoco-resources/branchnc.gif
build/reports/jacoco/test/html/jacoco-resources/sort.js
build/reports/jacoco/test/html/jacoco-resources/greenbar.gif
build/reports/jacoco/test/html/jacoco-resources/sort.gif
build/reports/jacoco/test/html/jacoco-resources/source.gif
build/reports/jacoco/test/html/jacoco-resources/report.gif
build/reports/jacoco/test/html/jacoco-resources/prettify.js
build/reports/jacoco/test/html/jacoco-resources/branchpc.gif
build/reports/jacoco/test/html/jacoco-resources/branchfc.gif
build/reports/jacoco/test/html/jacoco-resources/class.gif
build/reports/jacoco/test/html/jacoco-resources/prettify.css
build/reports/jacoco/test/html/jacoco-resources/report.css
build/reports/jacoco/test/html/jacoco-resources/group.gif
build/reports/jacoco/test/html/jacoco-resources/package.gif
build/reports/jacoco/test/html/jacoco-resources/redbar.gif
build/reports/jacoco/test/html/jacoco-resources/up.gif
build/reports/jacoco/test/html/jacoco-resources/session.gif
build/reports/jacoco/test/html/jacoco-resources/down.gif
build/reports/jacoco/test/html/jacoco-resources/method.gif
build/reports/jacoco/test/html/jacoco-resources/bundle.gif
build/reports/jacoco/test/html/net.coding.api.task
build/reports/jacoco/test/html/net.coding.api.task/Task.java.html
build/reports/jacoco/test/html/net.coding.api.task/index.source.html
build/reports/jacoco/test/html/net.coding.api.task/index.html
build/reports/jacoco/test/html/net.coding.api.task/Task.html
build/reports/jacoco/test/html/junit5
build/reports/jacoco/test/html/junit5/Library.java.html
build/reports/jacoco/test/html/junit5/index.source.html
build/reports/jacoco/test/html/junit5/index.html
build/reports/jacoco/test/html/junit5/UnitTest.java.html
build/reports/jacoco/test/html/junit5/Library.html
build/reports/jacoco/test/html/junit5/UnitTest.html
build/reports/jacoco/test/html/junit5/Config.html
build/reports/jacoco/test/html/junit5/Config.java.html
build/reports/jacoco/test/html/com.example.project
build/reports/jacoco/test/html/com.example.project/index.source.html
build/reports/jacoco/test/html/com.example.project/index.html
build/reports/jacoco/test/html/com.example.project/Calculator.java.html
build/reports/jacoco/test/html/com.example.project/Calculator.html

```

如果你希望自动执行，不需要携带 jacocoTestReport 任务，添加下面代码到 build.gradle 文件最下面

```

test.finalizedBy(project.tasks.jacocoTestReport)		

```

排除 class

```

jacocoTestReport {
  afterEvaluate {
    classDirectories = files(classDirectories.files.collect {
      fileTree(dir: it, exclude: [
        'io/reflectoring/coverage/ignored/**',
        'io/reflectoring/coverage/part/**'
      ])
    })
  }
}		

```

## 第 33 章 Rest Assured

## 打印出 response 的 body

```

	@Test()
    public void getHttpTest() {
        Response response = given()
                .get("http://www.netkiller.cn"); 
        response.print();
    }

```