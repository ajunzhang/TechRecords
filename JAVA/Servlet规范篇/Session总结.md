### session

* 创建session

```
HttpServletRequest request;
HttpSession session = request.getSession(true);
```

> 可以在Servlet中新建session，也可以在Filter中新建session。

* 删除session

```
HttpSession session = req.getSession();
session.invalidate();//将session置为无效状态，即删除session
```