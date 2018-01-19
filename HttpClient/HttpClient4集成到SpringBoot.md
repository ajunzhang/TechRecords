### HttpClient集成到SpringBoot中

* 1 maven引入依赖,注意版本。

```
<!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.3</version>
</dependency>
```

* 2 添加配置application.properties
> application.properties文件添加Http连接池的配置

```
#最大连接数
http.maxTotal = 100
#并发数
http.defaultMaxPerRoute = 20
#创建连接的最长时间
http.connectTimeout=1000
#从连接池中获取到连接的最长时间
http.connectionRequestTimeout=500
#数据传输的最长时间
http.socketTimeout=10000
#提交请求前测试连接是否可用
http.staleConnectionCheckEnabled=true
```

* 3 显示配置@Configuration,主要为实例化一个连接池管理器，实例化连接池。
```
@Configuration
public class HttpClientConfig {

	@Value("${http.maxTotal}")
	private Integer maxTotal;

	@Value("${http.defaultMaxPerRoute}")
	private Integer defaultMaxPerRoute;

	@Value("${http.connectTimeout}")
	private Integer connectTimeout;

	@Value("${http.connectionRequestTimeout}")
	private Integer connectionRequestTimeout;

	@Value("${http.socketTimeout}")
	private Integer socketTimeout;

	@Value("${http.staleConnectionCheckEnabled}")
	private boolean staleConnectionCheckEnabled;

	/**
	 * 首先实例化一个连接池管理器，设置最大连接数、并发连接数
	 * 
	 * @return
	 */
	@Bean(name = "httpClientConnectionManager")
	public PoolingHttpClientConnectionManager getHttpClientConnectionManager() {
		PoolingHttpClientConnectionManager httpClientConnectionManager = new PoolingHttpClientConnectionManager();
		// 最大连接数
		httpClientConnectionManager.setMaxTotal(maxTotal);
		// 并发数
		httpClientConnectionManager.setDefaultMaxPerRoute(defaultMaxPerRoute);
		return httpClientConnectionManager;
	}

	/**
	 * 实例化连接池，设置连接池管理器。 这里需要以参数形式注入上面实例化的连接池管理器
	 * 
	 * @param httpClientConnectionManager
	 * @return
	 */
	@Bean(name = "httpClientBuilder")
	public HttpClientBuilder getHttpClientBuilder(
			@Qualifier("httpClientConnectionManager") PoolingHttpClientConnectionManager httpClientConnectionManager) {

		// HttpClientBuilder中的构造方法被protected修饰，所以这里不能直接使用new来实例化一个HttpClientBuilder，可以使用HttpClientBuilder提供的静态方法create()来获取HttpClientBuilder对象
		HttpClientBuilder httpClientBuilder = HttpClientBuilder.create();

		httpClientBuilder.setConnectionManager(httpClientConnectionManager);

		return httpClientBuilder;
	}

	/**
	 * 注入连接池，用于获取httpClient
	 * 
	 * @param httpClientBuilder
	 * @return
	 */
	@Bean
	public CloseableHttpClient getCloseableHttpClient(
			@Qualifier("httpClientBuilder") HttpClientBuilder httpClientBuilder) {
		return httpClientBuilder.build();
	}

	/**
	 * Builder是RequestConfig的一个内部类 通过RequestConfig的custom方法来获取到一个Builder对象
	 * 设置builder的连接信息 这里还可以设置proxy，cookieSpec等属性。有需要的话可以在此设置
	 * 
	 * @return
	 */
	@Bean(name = "builder")
	public RequestConfig.Builder getBuilder() {
		RequestConfig.Builder builder = RequestConfig.custom();
		return builder.setConnectTimeout(connectTimeout).setConnectionRequestTimeout(connectionRequestTimeout)
				.setSocketTimeout(socketTimeout).setStaleConnectionCheckEnabled(staleConnectionCheckEnabled);
	}

	/**
	 * 使用builder构建一个RequestConfig对象
	 * 
	 * @param builder
	 * @return
	 */
	@Bean
	public RequestConfig getRequestConfig(@Qualifier("builder") RequestConfig.Builder builder) {
		return builder.build();
	}

}
```

* 4 在service层写业务代码GET请求

> 注意在Http请求的流中，要不就是序列化一个对象的流，要不就是序列化json的流，要不就是序列化xml的流。所以获取的响应的流

```
@Autowired
private CloseableHttpClient httpClient;

@Autowired
private RequestConfig config;

/**
    * 不带参数的get请求，如果状态码为200，则返回body，如果不为200，则返回null
    * 
    * @param url
    * @return
    * @throws Exception
    */
public String doGet(String url) throws Exception {
    String contextString=null;
    // 声明 http get 请求
    HttpGet httpGet = new HttpGet(url);
    // 装载配置信息
    httpGet.setConfig(config);
    // 发起请求
    CloseableHttpResponse response = this.httpClient.execute(httpGet);
    try {
        HttpEntity entity = response.getEntity();
        if(entity != null){
            InputStream instream = entity.getContent();//得到响应的流
            try {
                // 判断状态码是否为200
                if (response.getStatusLine().getStatusCode() == 200) {
                    byte [] byteArr = new byte[1024];
                    int len = 0;
                    StringBuffer sb = new StringBuffer();
                    while((len =instream.read(byteArr, 0, byteArr.length))!= -1){
                        String str = new String(byteArr, 0, len);
                        sb.append(str);
                    }
                    contextString = sb.toString();
                }
            } finally {
                instream.close();
            }
        }
    } finally {
        response.close();
    }
    // 判断状态码是否为200
//		if (response.getStatusLine().getStatusCode() == 200) {
//			// 返回响应体的内容
//			return EntityUtils.toString(response.getEntity(), "UTF-8");
//		}
//		return null;
    return contextString;
}
```

