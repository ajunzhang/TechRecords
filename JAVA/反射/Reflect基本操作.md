
获取一个class的所有field

```
Field[] fields = Org.class.getDeclaredFields();
Method[] methods = Org.class.getMethods();
for(int i=0;i<fields.length;i++){
    System.out.println(fields[i].getType().getSimpleName()+" "+fields[i].getName());
}
```

获取一个class的所有method
```
public static void getMethods(Class<?> clazz){
    Method[] methods = clazz.getMethods();
    for(int j=0;j<methods.length;j++){
        String methodName = methods[j].getName();
        System.out.println(methodName);
    }
}
```

执行class中的一个方法用invoke方法，其中第一个参数是调用该方法的实例，第二个参数是传值
```
Org simpleOrg=new Org();
Method m1;
try {
    m1 = Org.class.getMethod("setFiled1", String.class);
    try {
        m1.invoke(simpleOrg, "11");
        System.out.println(simpleOrg.getFiled1());
    } catch (IllegalAccessException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }
```