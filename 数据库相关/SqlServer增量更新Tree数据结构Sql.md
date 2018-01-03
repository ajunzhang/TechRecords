### SqlServer增量更新树数据结构Sql技巧

#### 问题描述
> 在SqlServer中有Lookup数据字典表，这是一种Tree数据结构，如新插入一条父节点的记录，再插入子节点的记录，有一个PARENTId字段用来维护父子关系。这时候在同步Sql的时候如从开发环境同步到测试环境，因为lookup数据字典表的自增Id不一致，所以要解决增量同步SQL要定义一个全局变量用来代替父节点的Id.

```
declare @parentId int
INSERT INTO [dbo].[TPL_LOOKUP_T] ( [CODE], [DESP], [TYPE], [PARENT_ID], [REVISION], [CREATE_USER_ID], [CREATE_DATE], [LAST_UPDATE_USER_ID], [LAST_UPDATE_DATE], [GROUP_CODE], [APP_NAME], [TENANT_ID], [APP_SCOPE]) VALUES ( N'INOUTCERTIFICATETYPE', N'出入境證件類別', N'0', N'0', N'1', N'0', N'2017-12-27 15:46:09.293', N'0', N'2017-12-27', N'INOUTCERTIFICATETYPE', N'CUS', null, null)
set @parentId=@@identity

INSERT INTO [dbo].[TPL_LOOKUP_T] ( [CODE], [DESP], [TYPE], [PARENT_ID], [REVISION], [CREATE_USER_ID], [CREATE_DATE], [LAST_UPDATE_USER_ID], [LAST_UPDATE_DATE], [GROUP_CODE], [APP_NAME], [TENANT_ID], [APP_SCOPE]) VALUES ( N'ChinaPassport', N'中華人民共和國護照', null, @parentId, N'1', N'0', N'2017-12-27 15:47:26.563', N'0', N'2017-12-27', N'INOUTCERTIFICATETYPE', N'CUS', null, null)
INSERT INTO [dbo].[TPL_LOOKUP_T] ( [CODE], [DESP], [TYPE], [PARENT_ID], [REVISION], [CREATE_USER_ID], [CREATE_DATE], [LAST_UPDATE_USER_ID], [LAST_UPDATE_DATE], [GROUP_CODE], [APP_NAME], [TENANT_ID], [APP_SCOPE]) VALUES ( N'ChinaEEEPCard', N'港澳通行證', null, @parentId, N'1', N'0', N'2017-12-27 15:49:05.200', N'0', N'2017-12-27', N'INOUTCERTIFICATETYPE', N'CUS', null, null)


INSERT INTO [dbo].[TPL_LOOKUP_T] ( [CODE], [DESP], [TYPE], [PARENT_ID], [REVISION], [CREATE_USER_ID], [CREATE_DATE], [LAST_UPDATE_USER_ID], [LAST_UPDATE_DATE], [GROUP_CODE], [APP_NAME], [TENANT_ID], [APP_SCOPE]) VALUES ( N'INOUTCERTIFICATETYPE1', N'出入境證件類別', N'0', N'0', N'1', N'0', N'2017-12-27 15:46:09.293', N'0', N'2017-12-27', N'INOUTCERTIFICATETYPE', N'CUS', null, null)
set @parentId=@@identity

INSERT INTO [dbo].[TPL_LOOKUP_T] ( [CODE], [DESP], [TYPE], [PARENT_ID], [REVISION], [CREATE_USER_ID], [CREATE_DATE], [LAST_UPDATE_USER_ID], [LAST_UPDATE_DATE], [GROUP_CODE], [APP_NAME], [TENANT_ID], [APP_SCOPE]) VALUES ( N'ChinaPassport1', N'中華人民共和國護照', null, @parentId, N'1', N'0', N'2017-12-27 15:47:26.563', N'0', N'2017-12-27', N'INOUTCERTIFICATETYPE', N'CUS', null, null)

```

* 其中定义变量为 <code>declare @parentId int</code>
* 设置变量的值为Insert语句自增的主键ID值<code>set @parentId=@@identity</code>
* 运行完第一棵树，再运行第二棵树的父节点，重新给变量赋值，再运行第二棵树的子节点。

#### Tips:
在SQL Server中，全局变量是一种特殊类型的变量,服务器将维护这些变量的值。全局变量以@@前缀开头,不必进行声明,它们属于系统定义的函数。<br>
一般变量已@前缀开头，要用<code>declare @parentId int</code>声明。<br>
下表就是SQL Server中一些常用的全局变量。<br>
<link>http://www.cnblogs.com/minideas/archive/2010/10/07/1845064.html</link>