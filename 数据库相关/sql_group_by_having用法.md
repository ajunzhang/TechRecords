### sql  group by  having的用法

> GROUP BY 是分组查询, 一般 GROUP BY 是和聚合函数配合使用

> group by 有一个原则,就是 select 后面的所有列中,没有使用聚合函数的列,必须出现在 group by 后面（重要）


- 例如,有如下数据库表：

```
A    B 
1    abc 
1    bcd 
1    asdfg
```

> 如果有如下查询语句（该语句是错误的，原因见前面的原则）

```
select A,B from table group by A  
```

> 该查询语句的意图是想得到如下结果(当然只是一相情愿) 

```
A     B 
       abc 
1     bcd 
       asdfg      
```

> 右边3条如何变成一条,所以需要用到聚合函数，如下(下面是正确的写法):

```
select A,count(B) as 数量 from table group by A 
```
```
这样的结果就是 
A    数量 
1    3  
```

#### Having

> where 子句的作用是在对查询结果进行分组前，将不符合where条件的行去掉，即在分组之前过滤数据，条件中不能包含聚组函数，使用where条件显示特定的行。

> having 子句的作用是筛选满足条件的组，即在分组之后过滤数据，条件中经常包含聚组函数，使用having 条件显示特定的组，也可以使用多个分组标准进行分组。

> having 子句被限制子已经在SELECT语句中定义的列和聚合表达式上。通常，你需要通过在HAVING子句中重复聚合函数表达式来引用聚合值，就如你在SELECT语句中做的那样。例如：

```
SELECT A COUNT(B) FROM TABLE GROUP BY A HAVING COUNT(B)>2
```
