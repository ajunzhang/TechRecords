### sqlserver根据某个父节点查询所有子节点的数据

> 根据 ID=201，这个节点查询出他下面的所有子节点的SQL
```
;with cte as 
(
    select * from TPL_LOOKUP_T WHERE ID= 201
    union all
    select T.* from TPL_LOOKUP_T as T inner join cte on T.PARENT_ID = cte.ID
)
SELECT * from cte where ID > 201;
```

> 删除ID=201下面的所有子节点
```
;with cte as 
(
    select * from TPL_LOOKUP_T WHERE ID= 201
    union all
    select T.* from TPL_LOOKUP_T as T inner join cte on T.PARENT_ID = cte.ID
)
delete from TPL_LOOKUP_T WHERE ID IN (SELECT ID from cte where ID > 201);
```