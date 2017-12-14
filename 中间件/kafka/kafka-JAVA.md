### 消息生成方

pom文件引入依赖
```
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka_2.10</artifactId>
    <version>0.10.1.0</version>
</dependency>
```

```
/**
 * Description:访问本地搭建的单体kafka
 * @author sjZhang
 * @date 2017年12月14日下午2:15:48
 */
public class TestProducer {
	private static Properties properties;
	static {
		properties = new Properties();
		properties.put("bootstrap.servers", "localhost:9092");
		properties.put("acks", "1");
		properties.put("retries", 0);
		properties.put("batch.size", 16384);
		properties.put("linger.ms", 0);
		properties.put("buffer.memory", 33554432);
		properties.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
		properties.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

	}
	public static void main(String[] args) {
		String topic = "test";
		KafkaProducer<String, String> kp = new KafkaProducer<String, String>(properties);
		while (true) {
			String key = "0001";
			String value = "aaa1";
			ProducerRecord<String, String> pr = new ProducerRecord<String, String>(topic, key, value);
			kp.send(pr);
		}
	}
}
```

### 消息消费方
```
/**
 * Description:访问本地kafka消息消费方
 * @author sjZhang
 * @date 2017年12月14日下午2:19:13
 */
public class TestConsumer {
	private static Properties properties;

	static {
		properties = new Properties();
		properties.put("zk.connect", "127.0.0.1:2181");
		properties.put("bootstrap.servers", "localhost:9092");
		properties.put("group.id", "gp_list");
		properties.put("enable.auto.commit", "true");
		properties.put("auto.commit.interval.ms", "1000");
		properties.put("session.timeout.ms", "30000");
		properties.put("auto.offset.reset", "earliest");
		properties.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
		properties.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
	}

	public static void main(String[] args) {
		KafkaConsumer<String, String> consumer = new KafkaConsumer(properties);
		consumer.subscribe(Collections.singletonList("my_test"));
		while (true) {
			ConsumerRecords<String, String> records = consumer.poll(2000);
			for (ConsumerRecord<String, String> record : records) {
				// print the offset,key and value for the consumer records.
				System.out.printf("offset = %d, key = %s, value = %s\n", record.offset(), record.key(), record.value());
			}
		}

	}

}
```