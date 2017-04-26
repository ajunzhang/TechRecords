#### 定义feign接口

用FeignClient注解标记feignClient，参数表示要转发的第三方服务，服务名位于 ```/src/main/resources/application.properties```  文件中如下所示：

	#service name
	spring.application.name=lookup

@Consumes注解表示接收消费的是json类型数据。
@Produces注解表示转发出去给调用feign的类用的是json类型
这里传递参数GET请求用路径参数@PathParam接收

	@FeignClient(value="lookup")
	@Consumes("application/json")
	@Produces("application/json")
	public interface FeignLookupService {
		//通过figer转发getLookupByCodes请求
		@GET
		@Path("/lookup/getLookupByCodes/{codesStr}/{groupCode}")
		List<Lookup> getLookupByCodes(@PathParam("codesStr") String codesStr , 
				  @PathParam("groupCode") String groupCode);
		
		/*//通过figer转发getLookupByCodes请求
		@POST
		@Path("/lookup/getLookupByCodes")
		List<Lookup> getLookupByCodes(Map map);*/
	
	}