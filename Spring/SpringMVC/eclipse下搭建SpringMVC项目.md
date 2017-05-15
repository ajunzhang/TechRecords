#### Eclipse�´SpringMVC��Ŀ
* �����½�һ��Javaproject<br/>

* ���project facts����SE��Ŀconvert��web��Ŀ<br/>

![](images/1.png)

* eclipse���Զ����һ��webContentĿ¼<br/>

![](images/2.png)


* ���蹤�̵�springmvc���ԣ�����web.xml��ʹ�����springmvc���ԣ���Ҫ����������һ����ContextLoaderListener��һ����DispatcherServlet��<br/>

	<!-- ����contextConfigLocation -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath*:applicationContext.xml</param-value>
	</context-param>

	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<!-- ����DispatcherServlet��ʾ���ù��̽�����springmvc�ķ�ʽ������ʱҲ��Ĭ����/WEB-INFĿ¼�²���XXX-servlet.xml��Ϊ�����ļ���
        XXX����DispatcherServlet�����֣����ļ��н�����������Ҫ��mvc���ԣ�HandlerMapping,����ΪDispatcherServlet���ǰ�˿��������������Controller��
        ViewResolver,����ΪDispatcherServlet����ModelAndView����ͼ��������
                     �˴�ʹ��ָ���������ļ�spring-mvc.xml -->
	<servlet>
		<servlet-name>web-context</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath*:/spring-mvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>web-context</servlet-name>
		<url-pattern>/.*</url-pattern>
	</servlet-mapping>

* ����ContextLoaderListener��ʾ���ù���Ҫ��spring�ķ�ʽ����������ʱ��Ĭ����/WEB-INFĿ¼�²��� applicationContext.xml��Ϊspring�����������ļ���������Գ�ʼ��һЩbean����DataSource��<br/>

* ����DispatcherServlet��ʾ���ù��̽�����springmvc�ķ�ʽ������ʱҲ��Ĭ����/WEB-INFĿ¼�²���XXX- servlet.xml��Ϊ�����ļ���XXX����DispatcherServlet�����֣����ļ��н�����������Ҫ��mvc���ԣ�ViewResolver,����ΪDispatcherServlet����ModelAndView����ͼ����������������ʹ��ָ����xml�����ļ�spring-mvc.xml<br/>

* ��Tomcat��װĿ¼���ҵ�\conf\Catalina\localhostĿ¼������ճ��һ��xml�ļ����ļ����ƾ�����Ŀ�������ġ���Ҫ�������޸�docBase����ֵ��ѡ��ʹ�õ����ݿ⡣<br/>


	<Context reloadable="true" docBase="E:\JAVA_WORKSPACE\SpringMVCDemo2\WebContent" >
		<Loader className="org.apache.catalina.loader.DevLoader" reloadable="true" debug="1" useSystemClassLoaderAsParent="false" />
		<!-- MySQL -->
		<Resource 
			 name="jdbc/SmartAsDS"
			 factory="com.alibaba.druid.pool.DruidDataSourceFactory"
			 auth="Container"
			 type="javax.sql.DataSource"
			 driverClassName="com.mysql.jdbc.Driver"
			 url="jdbc:mysql://localhost/oic?useUnicode=true&amp;characterEncoding=UTF-8&amp;useFastDateParsing=false&amp;allowMultiQueries=true"
			 username="root"
			 password="123"
			 maxActive="50"
			 maxWait="10000"
			 removeabandoned="true"
			 removeabandonedtimeout="60"
			 logabandoned="false"
			 poolPreparedStatements="false"
			 maxPoolPreparedStatementPerConnectionSize="20"
			 filters="wall,stat,log4j"/>
	</Context>

* ����Tomcat������������ַ

![](images/3.png)