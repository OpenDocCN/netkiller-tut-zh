# 第 24 章 NoSQL

## MongoDB

### pom.xml

```

<dependency>
	<groupId>org.mongodb</groupId>
	<artifactId>mongodb-driver</artifactId>
	<version>3.2.2</version>
</dependency>

```

### 插入操作

```

package cn.netkiller.controller;

import java.net.UnknownHostException;

import com.mongodb.BasicDBObject;
import com.mongodb.DB;
import com.mongodb.DBCollection;
import com.mongodb.MongoClient;

public final class Tracker {

	public Tracker() {

	}

	public static void main(String[] args) {

		MongoClient mongo = null;
		try {
			mongo = new MongoClient("192.168.4.1", 27017);

			DB db = mongo.getDB("finance");
			DBCollection table = db.getCollection("tracker");
			BasicDBObject document = new BasicDBObject();
			document.put("test", "helloworld");
			table.insert(document);
		} catch (UnknownHostException e) {
		e.printStackTrace();
		}
	}
}			

```

### 读取操作