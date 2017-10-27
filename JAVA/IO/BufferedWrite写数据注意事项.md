### 换行标志的发送

> Java Socket 使用BufferedWriter和BufferedReader要注意readLine 以及换行标志的发送.
在客户端中，如果write的时候并没有发送换行标识符，那么服务器在接收的时候，readLine是读取一行，没遇到换行就读取不出来,服务器接收不到客户端的信息。
```
Socket socket = new Socket("localhost", 8888);
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
String str = "i am client";
bw.write(str);
bw.newLine();//要加换行标记位
bw.close();
socket.close();
```

或者
```
Socket socket = new Socket("localhost", 8888);
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
String str = "i am client";
bw.write(str+"\n");//要加换行标记位
bw.close();
socket.close();
```