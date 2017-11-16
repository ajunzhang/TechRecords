### CGLIB动态代理类

CGLIB能够动态代理类，原理是字节码增强。依赖两个jar包 cglib 和 asm。

1:定义类
```
public class ATarget {
	public void save() {
		System.out.println("ATarget Save function");
	}
}
```

2：生成代理，增强逻辑。
```
public class ProxyTarget implements MethodInterceptor {
	// 维护一个目标对象
	private Object target;

	public ProxyTarget(Object target) {
		this.target = target;
	}

	public Object createObject(Object target) {
		this.target = target;
		Enhancer hancer = new Enhancer();
		hancer.setSuperclass(this.target.getClass());
		hancer.setCallback(this);
		hancer.setClassLoader(target.getClass().getClassLoader());
		return hancer.create();
	}

	@Override
	public Object intercept(Object arg0, Method arg1, Object[] arg2, MethodProxy arg3) throws Throwable {
		// TODO Auto-generated method stub
		System.out.println("BEFORE EHANCE");
		// 执行目标对象方法
		Object returnValue = arg3.invokeSuper(arg0, arg2);
		System.out.println("AFTER EHANCE");
		return returnValue;
	}

}
```

3：测试类
```
public class Main {
	public static void main(String[] args) {
		ATarget atarget = new ATarget();
		ProxyTarget proxyTarget = new ProxyTarget(atarget);
		Object obj = proxyTarget.createObject(atarget);
		((ATarget) obj).save();
	}
}
```