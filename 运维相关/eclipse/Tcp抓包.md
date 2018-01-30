### Tcp/Ip抓包分析

* Add一个TCP/IP Monitor。对应输入的含义为：
Local monitoring port : 本地给一个端口用于代理Monitor的端口。
Host name: 输入要代理的IP地址。
port:输入端口号。
点击保存，这个配置过程的目的是要用localhost:本地端口 去代理Monitor的端口。然后每次触发本地请求就能就行抓包，从而分析Request的Head 和 Response的Head。

* 本地测试用例如下：

* 首先在服务器 10.110.200.103部署了一个服务对外暴露端口为9981，这个即Monitor的配置。然后用localhost:80来监听。

* 在Main方法中用jdk自带的 <code>HttpURLConnection</code>,发起一个http请求。

* 在控制台查看TCP/IP Monitor即可。

```
/**
 * Description:自定义了一个Tcp/ip Monitor用于抓取如下请求，这里做了一下转发，新建一个tcp/ip monitor，当遇到请求路径为
 * http://localhost:80/VehicleRiskRating/ 就用 http://10.110.200.103:9981/VehicleRiskRating/来代理。然后可以观察request Head和
 * Response Head。
 * 
 * 该类的逻辑主要用于测试 url对特殊字符转义的效果。
 * @author sjZhang
 * @date 2018年1月30日下午3:09:21
 */
public class EncodeMain {
	public static void main(String[] args) throws Exception{
		String movementId = "mo?vem=ent%00003";
		String permitID="perm&it*00003";
		String Datetime="20180%117";
		//movementId = URLEncoder.encode(movementId,"GBK");
		//movementId = URLEncoder.encode(movementId,"utf-8");
		movementId = URLEncoder.encode(movementId,"ASCII");
		String path = "http://localhost:80/VehicleRiskRating/"+movementId+"/"+permitID+"/"+Datetime;
		Long start = System.currentTimeMillis();
		HttpUrlConnectionService httpClientUtil = new HttpUrlConnectionService();
		httpClientUtil.getDG(path);
		Long end = System.currentTimeMillis();
		System.out.println("HttpClient执行时间为" + (end - start) + "毫秒");
	}

}
```