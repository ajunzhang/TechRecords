### Event机制

来一个实例：发送邮件事件(MailEvent) 邮件发送监听(MailListener) 

```
/**
 * 邮件发送事件
 * @author DELL
 *
 */
public class MailEvent extends ApplicationContextEvent {

	private String to;// 目的地
	

	public MailEvent(ApplicationContext source) {
		super(source);
	}

	public MailEvent(ApplicationContext source, String to) {
		super(source);
		this.to = to;
	}

	public String getTo() {
		return to;
	}

	public void setTo(String to) {
		this.to = to;
	}

}
```

```
@Component
public class MailListener implements ApplicationListener<MailEvent> {

	@Override
	public void onApplicationEvent(MailEvent event) {
		// TODO Auto-generated method stub
		MailEvent mEvent = (MailEvent) event;
		System.out.println("MailListener:监听到向" + mEvent.getTo() + "发送了一封邮件。");

	}

}
```

```
public class MailSender implements ApplicationContextAware {

	@Autowired
	private ApplicationContext context;

	@Override
	public void setApplicationContext(ApplicationContext context) throws BeansException {
		// TODO Auto-generated method stub
		this.context = context;
	}

	/**
	 * 在该方法中首先模拟发送邮件，然后产生一个MailEvent事件，并且通过Context向容器的所有事件监听器发送事件
	 * 
	 * @param to
	 */
	public void sendMail(String to) {
		System.out.println("MailSender:模拟发送邮件。。。");
		MailEvent event = new MailEvent(context, to);
		context.publishEvent(event);
	}

}
```