### 新建索引

SQLServer2016数据库，针对查询语句order by createDate新建降续索引

```
select * from CUS_TABLE Where flag='1' order by createDate
```

```
CREATE INDEX [createDateIndex] ON [dbo].[CUS_CAR_FLOW]
([CREATE_DATE] DESC) 
GO
```