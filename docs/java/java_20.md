# 第 34 章 Software Development Kit

## Google

### com.google.gson

https://github.com/google/gson

```

<project   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>sender</groupId>
	<artifactId>sender</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>Sender</name>
	<description>EDM</description>

	<dependencies>
		<dependency>
			<groupId>com.rabbitmq</groupId>
			<artifactId>amqp-client</artifactId>
			<version>3.6.0</version>
		</dependency>
		<dependency>
			<groupId>com.google.code.gson</groupId>
			<artifactId>gson</artifactId>
			<version>2.6.2</version>
			<scope>compile</scope>
		</dependency>
	</dependencies>

</project>		

```

#### map 处理

Example

```

package io.github.netkiller;

import java.util.HashMap;
import java.util.Map;

import com.google.gson.*;

public class GsonTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		Gson gson = new Gson();
		String json = "{\"k1\":\"v1\",\"k2\":\"v2\"}";
		Map<String, String> map = new HashMap<String, String>();
		map = (Map<String, String>) gson.fromJson(json, map.getClass());
		System.out.println(map.get("k1"));
	}

}

```

多层 Map 剥离

```

		Gson gson = new Gson();
		String inf= "{\"0\":{\"id\":\"2\",\"category_id\":\"1\",\"title\":\"Test2\",\"author\":\"\",\"ctime\":\"2016-03-05 11:59:56\"},\"1\":{\"id\":\"1\",\"category_id\":\"1\",\"title\":\"Test1\",\"author\":\"\u6d4b\u8bd5\",\"ctime\":\"2016-03-05 11:57:30\"},\"pages\":{\"count\":2,\"first\":0,\"last\":0,\"before\":0,\"current\":0,\"next\":0,\"total\":0}}";
		Map<String, Map> map = new HashMap<String, Map>();

		map = (Map<String, Map>) gson.fromJson(inf, map.getClass());
		System.out.println(map.get("1").get("title"));
		System.out.println(map.get("pages").get("count"));

```

#### POJO

```

package cn.netkiller.gson;

import java.util.ArrayList;
import java.util.List;

public class Personal {
	private int age = 30;
	private String name = "neo";
	private List<String> telphone = new ArrayList<String>() {
		{
			add("13113668890");
			add("13322993040");
			add("29812080");
		}
	};

	// getter and setter methods

	@Override
	public String toString() {
		return "Personal [age=" + age + ", name=" + name + ", telphone=" + telphone + "]";
	}
}

```

#### toJson

```

package cn.netkiller.gson;

import com.google.gson.Gson;

public class GsonExampleToJson {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Personal obj = new Personal();
		Gson gson = new Gson();

		// convert java object to JSON format, and returned as JSON formatted string
		String json = gson.toJson(obj);
		System.out.println(json);
	}

}

```

#### fromJson

```

package cn.netkiller.gson;

import com.google.gson.Gson;

public class GsonExampleFromJson {
	public static void main(String[] args) {

		Personal obj = new Personal();
		Gson gson = new Gson();

		// convert the json string back to object
		obj = gson.fromJson("{\"age\":30,\"name\":\"neo\",\"telphone\":[\"13113668890\",\"13322993040\",\"29812080\"]}", Personal.class);

		System.out.println(obj);
	}
}

```

#### JsonParser

```

package cn.netkiller.gson;

import java.util.Map.Entry;

import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import com.google.gson.JsonArray;
import com.google.gson.JsonParser;

public class GsonJsonParser {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		String jsonString = "{\"age\":30,\"name\":\"neo\",\"telphone\":[\"13113668890\",\"13322993040\",\"29812080\"],\"address\":{\"province\":\"Guangdong\",\"city\":\"Shenzhen\"}}";

		JsonElement root = new JsonParser().parse(jsonString);
		System.out.println(root.toString());
		System.out.println(root.getAsJsonObject().get("age").getAsInt());
		System.out.println(root.getAsJsonObject().get("name").getAsString());

		// Get the content of the first map
		JsonArray jsonArray = root.getAsJsonObject().get("telphone").getAsJsonArray();

		for (JsonElement tel : jsonArray) {
			System.out.println(tel);
		}

		JsonObject object = root.getAsJsonObject().get("address").getAsJsonObject();
		for (Entry<String, JsonElement> entry : object.entrySet()) {
			System.out.println(entry.getKey() + ":" + entry.getValue());
		}
	}

}

```

#### Exmaple 范例

##### Map to Json

```

Gson gson = new GsonBuilder().enableComplexMapKeySerialization().create();
String jsonString = gson.toJson(map);

```

#### Exmaple 范例

##### Map to Json

```

Gson gson = new GsonBuilder().enableComplexMapKeySerialization().create();
Map<String, String> source = gson.fromJson(output, HashMap.class);

```

#### 处理复杂的类型

通过 TypeToken 定义复杂数据库类型。

```

package cn.netkiller.ipo.process.json;

import java.util.Map;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.reflect.TypeToken;

import cn.netkiller.ipo.process.ProcessInterface;

public class JsonValueLength implements ProcessInterface {
	private final static Logger logger = LoggerFactory.getLogger(JsonValueLength.class);
	private Gson gson = new GsonBuilder().enableComplexMapKeySerialization().create();
	private int maxLength = 0;

	public JsonValueLength(int maxLength) {
		this.maxLength = maxLength;
	}

	@Override
	public String run(String line) {

		Map<String, String> map = gson.fromJson(line, new TypeToken<Map<String, String>>() {
		}.getType());
		for (String key : map.keySet()) {
			if (map.get(key).length() > this.maxLength) {
				map.put(key, map.get(key).substring(0, this.maxLength));
			}
		}
		logger.debug("{}", map);
		return gson.toJson(map);
	}

}

```

### Guava

https://github.com/google/guava/wiki

#### maven

```

		<dependency>
			<groupId>com.google.guava</groupId>
			<artifactId>guava</artifactId>
			<version>23.0</version>
		</dependency>			

```

27 版本需要指定 27.1-jre，如果是安卓系统 27.1-android

```

		<dependency>
			<groupId>com.google.guava</groupId>
			<artifactId>guava</artifactId>
			<version>27.1-jre</version>
		</dependency>			

```

#### 删除不可显示的字符

```

package cn.netkiller.test;

import com.google.common.base.CharMatcher;

public class Test {

	public static void main(String[] args) {

		String string = "佛山市南海区 123 华泰 ABC 精密 abc 机械有限公司消防维保􁵪􁻠􁴹􄲀􀞜􀨨,";

		// 版本 23.0
		// String printable = CharMatcher.INVISIBLE.removeFrom(string);
		// String clean = CharMatcher.ASCII.retainFrom(printable);

		String printable = CharMatcher.invisible().removeFrom(string);
		String clean = CharMatcher.ascii().retainFrom(printable);
		System.out.println(printable);
		System.out.println(clean);

	}

}

```

## Mahout

### 推荐系统

#### Maven pom.xml

```

<project   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>cn.netkiller</groupId>
	<artifactId>mahout</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>mahout</name>
	<url>http://maven.apache.org</url>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.apache.mahout</groupId>
			<artifactId>mahout-core</artifactId>
			<version>0.9</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-nop</artifactId>
			<version>1.7.30</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>
	</dependencies>
</project>

```

#### 推荐程序

```

package cn.netkiller.mahout;

import java.io.File;
import java.util.List;

import org.apache.mahout.cf.taste.impl.model.file.FileDataModel;
import org.apache.mahout.cf.taste.impl.neighborhood.NearestNUserNeighborhood;
import org.apache.mahout.cf.taste.impl.recommender.GenericUserBasedRecommender;
import org.apache.mahout.cf.taste.impl.similarity.PearsonCorrelationSimilarity;
import org.apache.mahout.cf.taste.model.DataModel;
import org.apache.mahout.cf.taste.neighborhood.UserNeighborhood;
import org.apache.mahout.cf.taste.recommender.RecommendedItem;
import org.apache.mahout.cf.taste.recommender.Recommender;
import org.apache.mahout.cf.taste.similarity.UserSimilarity;

public class App {

	public App() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		try {
			// 从文件加载数据
			DataModel model = new FileDataModel(new File("target/classes/test.csv"));
			// 指定用户相似度计算方法，这里采用皮尔森相关度
			UserSimilarity similarity = new PearsonCorrelationSimilarity(model);
			// 指定用户邻居数量，这里为 2
			UserNeighborhood neighborhood = new NearestNUserNeighborhood(2, similarity, model);
			// 构建基于用户的推荐系统
			Recommender recommender = new GenericUserBasedRecommender(model, neighborhood, similarity);
			// 得到指定用户的推荐结果，这里是得到用户 1 的两个推荐
			List<RecommendedItem> recommendations = recommender.recommend(1, 2);
			// 打印推荐结果
			for (RecommendedItem recommendation : recommendations) {
				System.out.println(recommendation);
			}
		} catch (Exception e) {
			System.out.println(e);
		}
	}

}

```

#### 数据文件

```

1,101,5.0
1,102,3.0
1,103,2.5
2,101,2.0
2,102,2.5
2,103,5.0
2,104,2.0
3,101,2.5
3,104,4.0
3,105,4.5
3,107,5.0
4,101,5.0
4,103,3.0
4,104,4.5
4,106,4.0
5,101,4.0
5,102,3.0
5,103,2.0
5,104,4.0
5,105,3.5
5,106,4.0			

```

## Hessian

基于 Binary-RPC 协议实现

## quartz-scheduler

http://quartz-scheduler.org/

## Redisson

http://redisson.org/