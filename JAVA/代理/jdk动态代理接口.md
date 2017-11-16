### jdk动态代理接口

代理是一种设计模式，目的在于增强代理对象的逻辑，本着不去修改原有类的前提。用代理的思想去增强目的类的逻辑。

jdk提供了API，代码示例如下：

1 定义接口及类实现接口
```
public class ATarget implements Target {
	public void save() {
		System.out.println("ATarget Save function");
	}
}

interface Target {
	void save();
}
```

2 加强目标对象的逻辑
```
public class ProxyTarget {
	// 维护一个目标对象
	private Object target;

	public ProxyTarget(Object target) {
		this.target = target;
	}

	// 给目标对象生成代理对象
	public Object getProxyInstance() {
		return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(),
				new InvocationHandler() {
					@Override
					public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
						System.out.println("BEFORE EHANCE");
						// 执行目标对象方法
						Object returnValue = method.invoke(target, args);
						System.out.println("AFTER EHANCE");
						return returnValue;
					}
				});
	}

}
```

3 测试类
```
public class Main {
	public static void main(String[] args) {
		Target atarget = new ATarget();
		Object pt = new ProxyTarget(atarget).getProxyInstance();
		((Target) pt).save();
	}
}
```