### json序列化

> 序列化是将对象状态转换为可保持或传输的格式的过程。与序列化相对的是反序列化，它将流转换为对象。这两个过程结合起来，可以轻松地存储和传输数据。比如两个系统通过HttpClient交互，数据的传输一定是序列化的，获取到流要反序列化为对象。所以在传输的时候用什么格式的序列化就有分类了，一般分为三类，对象，json，xml.不同的类别反序列化的操作也不一样。

#### Spring对Json字符串的反序列化操作。
> 在代码中用HttpClient调用一个接口，要声明一下请求头的一些信息如下：

```
URL restServiceURL = new URL(path);
HttpURLConnection httpConnection = (HttpURLConnection) restServiceURL.openConnection();
httpConnection.setRequestMethod("GET");//声明请求类型
httpConnection.setRequestProperty("Accept", "application/json");//声明客户端接受的数据类型是json格式
httpConnection.setRequestProperty("Accept-Charset", "utf-8");//声明客户端接受的数据格式是utf-8编码
```

同时也能获取服务端的响应头信息，如下：
```
//获取服务端响应头的信息
if (200 != httpConnection.getResponseCode()) {
    throw new RuntimeException("failed, error code is " + httpConnection.getResponseCode());
}
```

#### Spring反序列化json字符串
这个过程分两步走：
* 1 获取流，然后转换成String。
* 2 将json字符串转换成JAVA对象.

```
//1 获取流，然后转换成String
byte[] temp = new byte[httpConnection.getInputStream().available()];						
if (httpConnection.getInputStream().read(temp) != -1) {
    result = new String(temp);
}
//2 将json字符串转换成JAVA对象
ObjectMapper objectMapper = new ObjectMapper();
carRTSVO = objectMapper.readValue(result, CarRealTimeScoreVO.class);
```