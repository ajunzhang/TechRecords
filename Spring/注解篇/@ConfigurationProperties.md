### @ConfigurationProperties

## 源码解析

	/**
	 * Annotation for externalized configuration. Add this to a class definition or a
	 * {@code @Bean} method in a {@code @Configuration} class if you want to bind and validate
	 * some external Properties (e.g. from a .properties file).
	 * <p>
	 * Note that contrary to {@code @Value}, SpEL expressions are not evaluated since property
	 * values are externalized.
	 *
	 * @author Dave Syer
	 * @see ConfigurationPropertiesBindingPostProcessor
	 * @see EnableConfigurationProperties
	 */
	@Target({ ElementType.TYPE, ElementType.METHOD })
	@Retention(RetentionPolicy.RUNTIME)
	@Documented
	public @interface ConfigurationProperties 

## 获取邮件配置
用@ConfigurationProperties注解就可以绑定属性
ignoreUnknownFields = false告诉Spring Boot在有属性不能匹配到声明的域的时候抛出异常。
prefix 用来选择哪个属性的prefix名字来绑定。

	@ConfigurationProperties(locations = "classpath:mail.properties", 
                         ignoreUnknownFields = false, 
                         prefix = "mail")
	public class MailProperties { 
	  public static class Smtp {  
	    private boolean auth;  
	    private boolean starttlsEnable;  
	    // ... getters and setters 
	  }
	  @NotBlank private String host;
	  private int port;  
	  private String from; 
	  private String username;
	  private String password; 
	  @NotNull private Smtp smtp; 
	  // ... getters and setters
	}
	
	
## smartas 获取env配置文件信息

	/**
	 * 引用配置参数
	 * 
	 * 包括引用名称，数据库名，业务表前缀，系统群
	 * 
	 * @author chenb
	 *
	 */
	@ConfigurationProperties(prefix = "App")
	public class AppEnv implements Serializable {
	
	  /**
	   * 
	   */
	  private static final long serialVersionUID = -6955087189769303995L;
	
	  private boolean dev = false;
	
	  /**
	   * 应用名称
	   */
	  private String name;
	  /**
	   * 数据名称
	   */
	  private String dbName;
	
	  /**
	   * 表前缀名
	   */
	  private String tablePrefix;
	  /**
	   * 
	   */
	  private String scope;
	
	  private String tenantId;
	
	  /**
	   * @return the Name
	   */
	  public String getName() {
	    return name;
	  }