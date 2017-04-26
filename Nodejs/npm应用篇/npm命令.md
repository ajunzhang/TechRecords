##    光电产业云门户

###   1 安装

####  如未安装过cnpm则需要运行该命令，如已安装过则直接跳过此步，-g是全局模式安装

	npm install -g cnpm --registry=https://registry.npm.taobao.org

####  安装完成后执行以下命令
	
	cnpm install 或者 npm install

####  安装jquery
	
	cnpm install jquery

####  安装request
	
	npm install request –save

####  安装body-parser
	
	npm install body-parser –save

####  安装webpack
	
	npm install webpack

###   2 打包项目

####  将项目所有资源打包到文件夹中

	npm run build


###   3 启动项目

####  调试模式启动
	
	npm start

####  打包后启动，在打包文件夹中进入命令模式，输入命令

	http-server -p 8004 (指定端口号)

####  打包后启动，如果需要在URL中端口号后面添加上下文，则将保存打包文件的文件夹命名为上下文，在该文件夹外面进入命令模式，输入命令

	http-server -p 8004 (指定端口号)

###   4 停止项目

	ctrl+c

###   5 卸载模块
####  卸载cnpm
	
	npm uninstall -g cnpm


===================================================================================================

#### 门户QueryFilter新规则
- 以前的版本：

qs对象的写法：qs是一个对象，有几个查询参数就有几个属性，注意key的规则

	var qs = {
	  Q_orginfo_EQ: "001",
	  Q_status_I_EQ："1",
	};

URL拼接的效果：

	?Q_orginfo_EQ=001&Q_status_I_EQ=1
	
- 现在的版本：

qs对象的写法：qs是一个对象，有一个key为'Q',value为数组的属性.注意数组里面的规则。

	var qs = {
	  Q:['groupCode_S_EQ=ENTSCALE','pId_L_EQ=512']
	};
	
URL拼接的效果：

	?Q=groupCode_S_EQ%3DENTSCALE&Q=pId_L_EQ%3D512

为了兼容以前的写法，可以在一个地方统一将之前的版本翻译成现在版本，输入一个对象，输出也是一个对象


	if(typeof data == 'object' && type == 'get'){
		const format=(obj)=>{
			  let keys=Object.keys(obj),newKey=[],result={};
			  for(let i=0,j=keys.length;i<j;i++){
			    newKey.push(keys[i].slice(2)+'='+obj[keys[i]]);
			  }
			  result["Q"]=newKey;
			  return result;
			};
		data = format(data);}

smartms 对jsx文件中的QueryFilter也做了统一翻译的过程，所以兼容之前的写法，位于 /fae-ms-web/src/main/resources/static/web/core/main.js：
	
	//查询参数Q处理	
	if(data && type === 'get'){
	    var query = {},Q= query.Q || [];
	    _.forIn(data,function(v,k){
	        if(!_.startsWith(k,'Q_') || v === undefined){
	            query[k]=v;
	            return;
	        }
	        Q.push(_.trimStart(k,'Q_') + '=' + v);
	    });
	    query.Q = Q;
	    data = query;
	}

===================================================================================================
#### 这三个全局URL说明
publishUrl 是指portal后端服务端的路径在publish服务中。
portalUrl 是指在门户项目中有时候页面的跳转 门户访问路径公共部分。
securityUrl 是指直接在页面访问fae-ms服务中访问路径。

	publishUrl:'http://www.oic.com:5557/services/service/publish/portal',
	
	portalUrl:'http://www.oic.com:5557/portal',
	
	securityUrl:'http://www.oic.com:5557/services',


===================================================================================================
#### query()函数
	
	function query(url,type,data,success,error,dataType)

请求都是通过query()函数发出的:
url 为服务端接口路径(类和方法的"@Path"注解value值组成).
type 为 get post
data 为传递的值

#### get请求传递参数：

路径参数 URL传递了一个id值为1的参数到服务端："abc/efg/1"。 服务端接收该值通过"@PathParam"注解: 


	@Get
	@Path("abc/efg/{id}")
	public String getValue(@PathParam("id") final long id){}

查询参数 URL传递一个 id = 1 到服务端："abc/efg?id=1". 服务端接收该值通过"@QueryParam"注解: 

	@Get
	@Path("abc/efg")
	public String getValue(@QueryParam("id") final long id){}

QueryFilter对象传值，见门户QueryFilter新规则

#### post请求传递一个对象：


	var yzm=document.getElementById('yzm').value;
	var userName=document.getElementById('username').value;
	var password=document.getElementById('password').value;
	var loginfo={
    		username:userName,
    		password:password,
		captcha :yzm,
		};
		$.ajax({
        beforeSend: function(xhr) {
          var hash = location.hash;
          if(hash){
            xhr.setRequestHeader("X-Requested-URL", hash.substr(2));
          }
        },
        type : 'post',
        url : `${App.securityUrl}/security/login`,
        contentType : "application/json",
        data : JSON.stringify(loginfo),
        success : function(data) {
        if (data.status == 100) {
           alert("验证码输入错误！");
           return true; 
         } else if (data.status == 101){
           alert("验证码输入为空！");
           return true;
         }else if (data.status == 200) {
           sessionStorage.setItem('curUser',data.context);
           //  this.context.router.push(path);
           window.history.go(-1);  
           return false;         
         } else if (data.status == 400) {// 账号密码错误
           alert("账号密码错误");
           return true; 
         } else if (data.status == 499) {
           alert("账号状态无效，请联系管理员启用账户后重新登陆");
           return true; 
         } else {
           alert('服务器忙,稍后再试');
           return true;    
         }              
        },
        error : function() {
          alert('服务器忙,稍后再试');
          return true; 
        }});};
