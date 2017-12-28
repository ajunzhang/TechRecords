### SpringBoot集成Redis

#### 序列化的问题

spring-data-redis提供了4种内置的serializer策略:

* JdkSerializationRedisSerializer：使用JDK的序列化手段(serializable接口，ObjectInputStrean，ObjectOutputStream)，数据以字节流存储
* StringRedisSerializer：字符串编码，数据以string存储
* JacksonJsonRedisSerializer：json格式存储
* OxmSerializer：xml格式存储

jedis默认序列化用的是<code>JdkSerializationRedisSerializer</code>,

#### 引入Starter
```
<!-- spring-boot-starter-data-redis依赖引入 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
    <version>1.5.4.RELEASE</version>
</dependency>
```

#### 配置Configuration
```
@Configuration
public class RedisConfiguration {

@Bean(name = "jedisFactory")
public JedisConnectionFactory jedisConnectionFactory() {
    JedisConnectionFactory connectionFactory = new JedisConnectionFactory();
    connectionFactory.setHostName("10.110.200.17");
    connectionFactory.setPassword("6385pwd123");
    connectionFactory.setPort(6385);
    connectionFactory.setDatabase(10);
    connectionFactory.setUsePool(true);// 连接池
    return connectionFactory;
}

@Bean(name = "redisTemplate")
public RedisTemplate<?, ?> redisTemplate(JedisConnectionFactory connectionFactory) {
    RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
    redisTemplate.setConnectionFactory(connectionFactory);
    redisTemplate.setEnableTransactionSupport(true);// 使用redis事物
    redisTemplate.setKeySerializer(redisTemplate.getStringSerializer());// StringRedisSerializer
    redisTemplate.setValueSerializer(redisTemplate.getDefaultSerializer());// JdkSerializationRedisSerializer
    redisTemplate.afterPropertiesSet();
    return redisTemplate;
}
}
```

#### 写入和读取Pojo从redis中

```
@Autowired
	private RedisTemplate<String, Serializable> redisTemplate;

	@Override
	public User readPojo(String key) {
		User user = redisTemplate.execute(new RedisCallback<User>() {
			@Override
			public User doInRedis(RedisConnection connection) throws DataAccessException {
				RedisSerializer<String> keySer = redisTemplate.getStringSerializer();
				byte[] keyByte = keySer.serialize(key);

				RedisSerializer<User> valueSer = (RedisSerializer<User>) redisTemplate.getDefaultSerializer();
				byte[] valueByte = connection.get(keyByte);

				return valueSer.deserialize(valueByte);
			}
		});
		return user;
	}

	@Override
	public void writePojo(String key, User user) {
		boolean result = redisTemplate.execute(new RedisCallback<Boolean>() {
			@Override
			public Boolean doInRedis(RedisConnection connection) throws DataAccessException {
				RedisSerializer<String> keySer = redisTemplate.getStringSerializer();
				RedisSerializer<User> valueSer = (RedisSerializer<User>) redisTemplate.getDefaultSerializer();
				byte[] value = valueSer.serialize(user);
				connection.set(keySer.serialize(key), value);
				return true;
			}
		});
	}
```