如下代码所示，其中<code>Class.forName("reflect.Gum").newInstance();</code>就是通过class来实例化一个指定类


```
package reflect;

class Bread{
	static{
		System.out.println("Load Class Bread");
	}
}

class Gum{
	static{
		System.out.println("load Class Gum");
	}
	
	public double price(){
		return 5.0;
	}
}

public class SweetShop {
	public static void main(String[] args) throws InstantiationException, IllegalAccessException {
		Bread bread = new Bread();
		System.out.println("after bread");
		try {
			Object gumInstance = Class.forName("reflect.Gum").newInstance();
			System.out.println(((Gum)gumInstance).price());
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
	}

}
```