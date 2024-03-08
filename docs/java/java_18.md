# 第 29 章 Kafka

## 安装 Kafka 环境

请参考 [《Netkiller Linux 手札》电子书](http://www.netkiller.cn/linux/index.html) 的 Kafka 章节，电子书中又详细的安装步骤。

## Maven

```

		<!-- https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients -->
		<dependency>
			<groupId>org.apache.kafka</groupId>
			<artifactId>kafka-clients</artifactId>
			<version>1.0.0</version>
		</dependency>

```

## 启动 kafka

有两种方式启动 kafka, 一种是命令行，另一种是通过 Java 程序，命令行方式请参考《Netkiller Linux 手札》，这里只介绍如何使用 Java 程序启动 Kafka。

首先启动 Zookeeper

```

QuorumPeerConfig config = new QuorumPeerConfig();
InputStream inputStream = KafkaTest.class.getResourceAsStream("/srv/kafka/config/zookeeper.properties");
Properties properties = new Properties();	    
properties.load(inputStream);
inputStream.close();
config.parseProperties(properties);
ServerConfig   serverConfig = new ServerConfig();
serverConfig.readFrom(config);
new ZooKeeperServerMain().runFromConfig(serverConfig);		

```

然后启动 Kafka

```

InputStream inputStream = KafkaTest.class.getResourceAsStream("/srv/kafka/config/server.properties");
Properties properties = new Properties();
properties.load(is);
inputStream.close();
KafkaServerStartable kafkaServerStartable = KafkaServerStartable.fromProps(properties);
kafkaServerStartable.startup();
kafkaServerStartable.awaitShutdown();

```

## 入门例子

### 订阅例子

```

package cn.netkiller.kafka.test;

import java.util.Arrays;
import java.util.Properties;

import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.KafkaConsumer;

public class KafkaConsumerExample {

	private static KafkaConsumer<String, String> consumer;

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Properties props = new Properties();
		props.put("bootstrap.servers", "kafka.netkiller.cn:9092");
		props.put("group.id", "test");
		props.put("enable.auto.commit", "true");
		props.put("auto.commit.interval.ms", "1000");
		props.put("session.timeout.ms", "30000");
		props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
		props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
		consumer = new KafkaConsumer<String, String>(props);
		consumer.subscribe(Arrays.asList("test"));
		while (true) {
			ConsumerRecords<String, String> records = consumer.poll(100);
			for (ConsumerRecord<String, String> record : records)
				System.out.printf("offset = %d, key = %s, value = %s\n", record.offset(), record.key(), record.value());
		}
	}

}

```

测试方法

```

root@netkiller ~ % /srv/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
>Helloworld

```

下面详细讲解上面的程序。首先我们通过 Properties 文件来配置消费属性。

```

		Properties props = new Properties();
		props.put("bootstrap.servers", "kafka.netkiller.cn:9092");
		props.put("group.id", "test");
		props.put("enable.auto.commit", "true");
		props.put("auto.commit.interval.ms", "1000");
		props.put("session.timeout.ms", "30000");
		props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
		props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");

```

然后订阅 TOPIC，从那个 TOPIC 读取消息，Kafka 可以同时订阅多个 TOPIC。下面的例子中，同时订阅了 foo 和 bar 两个 topic：

```

consumer.subscribe(Arrays.asList("foo", "bar"));

```

取出消息（消费消息），通过循环调用 poll 方法，从队列中取出消息。

```

		while (true) {
			ConsumerRecords<String, String> records = consumer.poll(100);
			for (ConsumerRecord<String, String> record : records)
				System.out.printf("offset = %d, key = %s, value = %s\n", record.offset(), record.key(), record.value());
		}		

```

poll 方法里面的参数是等待消息的时间(Long 类型)，如果队列里面有消息，会立马返回，如果没有，会等待指定的时间然后返回。

### 发布例子

```

package cn.netkiller.kafka.test;

import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.Producer;
import org.apache.kafka.clients.producer.ProducerRecord;

import java.util.Properties;

public class KafkaProducerExample {
    public static void main(String[] args) {
        Properties props = new Properties();
        props.put("bootstrap.servers", "kafka.netkiller.cn:9092");
        props.put("acks", "all");
        props.put("retries", 0);
        props.put("batch.size", 16384);
        props.put("linger.ms", 1);
        props.put("buffer.memory", 33554432);
        props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

        Producer<String, String> producer = new KafkaProducer<>(props);
        for(int i = 0; i < 100; i++)
            producer.send(new ProducerRecord<>("test", Integer.toString(i), Integer.toString(i)));

        producer.close();
    }
}		

```

## 线程例子

```

package cn.netkiller.ipo.test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.common.errors.WakeupException;
import org.apache.kafka.common.serialization.StringDeserializer;

public class KafkaConsumerThread implements Runnable {
	private final KafkaConsumer<String, String> consumer;
	private final List<String> topics;

	public KafkaConsumerThread(String groupId, List<String> topics) {
		this.topics = topics;
		Properties props = new Properties();
		props.put("bootstrap.servers", "kafka.netkiller.cn:9092");
		props.put("group.id", groupId);
		props.put("key.deserializer", StringDeserializer.class.getName());
		props.put("value.deserializer", StringDeserializer.class.getName());
		this.consumer = new KafkaConsumer<String, String>(props);
	}

	public void run() {
		try {
			consumer.subscribe(this.topics);

			while (true) {
				ConsumerRecords<String, String> records = consumer.poll(Long.MAX_VALUE);
				for (ConsumerRecord<String, String> record : records) {
					Map<String, Object> data = new HashMap<>();
					data.put("partition", record.partition());
					data.put("offset", record.offset());
					data.put("value", record.value());
					System.out.println(data);
				}
			}
		} catch (WakeupException e) {
			// ignore for shutdown
		} finally {
			consumer.close();
		}
	}

	public void shutdown() {
		consumer.wakeup();
	}

	public static void main(String[] args) {
		int numConsumers = 3;
		String groupId = "consumer-tutorial-group";
		List<String> topics = Arrays.asList("test");
		ExecutorService executor = Executors.newFixedThreadPool(numConsumers);

		final List<KafkaConsumerThread> consumers = new ArrayList<>();
		for (int i = 0; i < numConsumers; i++) {
			KafkaConsumerThread consumer = new KafkaConsumerThread(groupId, topics);
			consumers.add(consumer);
			executor.submit(consumer);
		}

		Runtime.getRuntime().addShutdownHook(new Thread() {
			@Override
			public void run() {
				for (KafkaConsumerThread consumer : consumers) {
					consumer.shutdown();
				}
				executor.shutdown();
				try {
					executor.awaitTermination(5000, TimeUnit.MILLISECONDS);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
		});
	}
}

```