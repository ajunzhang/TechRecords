#### web.xml��ǩ������
* ����web.xmlʼ��λ��WEB-INFĿ¼����

![](images/5.png)

* ���ü��������������Ǳ���Ҫ���õġ���ʱ�����ʱ�򱨴�����ΪlibĿ¼��û��jar��

```
	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
```

* ����Servlet
> ����DispatcherServlet��ʾ���ù��̽�����springmvc�ķ�ʽ������ʱҲ��Ĭ����/WEB-INFĿ¼�²���XXX-servlet.xml��Ϊ�����ļ���
  XXX����DispatcherServlet�����֣����ļ��н�����������Ҫ��mvc���ԣ�HandlerMapping,����ΪDispatcherServlet���ǰ�˿��������������Controller��
  ViewResolver,����ΪDispatcherServlet����ModelAndView����ͼ���������˴�ʹ��ָ���������ļ�spring-mvc.xml

```
	<servlet>
		<servlet-name>web-context</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>WEB-INF/spring-mvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>web-context</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>

```

* url-pattern ���õ�ʱ��Ҫע��

###### url-pattern����Ϊ"/"��"/*"������
> ���ȴ�Ҷ�֪��"/*"����ƥ������url����������չ���ģ�һ��ֻ���ڹ������ϡ�
  ��"/"�ܶ������ɲ������ش���չ���ģ���������Ǵ���ģ�����ʵҲ�����ء�.js������.css����".png"�Ⱦ�̬��Դ�ķ��ʡ�
  ���ٷ��ĵ���֪������tomcat��Ĭ��servlet����������url-patternƥ�䲻��ʱ���������servlet���������ܹ�����̬��Դ���ܹ�����HTTP��������
  ý�壨��Ƶ/��Ƶ�����������ļ����ؼ���������������ǵ���Ŀ��������"/"���Ḳ�ǵ�tomcat�е�default servlet��

```
	���Ե�springMVC��ǰ�˿���������Ϊ��/��ʱ����Ҫ���������ļ������÷��о�̬��Դ��
	��һ�֣�
	<!-- ���о�̬��Դ -->
	<mvc:resources location="/js/" mapping="/js/**"/> 
	<mvc:resources location="/css/" mapping="/js/**"/>
	 <mvc:resources location="/images/" mapping="/js/**"/>
	�ڶ��֣�
	<mvc:default-servlet-handler />
```