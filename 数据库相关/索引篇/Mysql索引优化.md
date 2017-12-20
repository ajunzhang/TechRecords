### Mysql索引类型
MySQL索引类型包括
#### 普通索引
这是最基本的索引，它没有任何限制。它有以下几种创建方式：
* 1 创建索引
```
CREATE INDEX indexName ON mytable(username(length));
```
> Tips:如果是CHAR，VARCHAR类型，length可以小于字段实际长度；如果是BLOB和TEXT类型，必须指定 length，下同。

* 2 修改表结构
```
ALTER mytable ADD INDEX [indexName] ON (username(length)) 
```

* 3 创建表的时候直接指定
```
CREATE TABLE mytable (
	ID INT NOT NULL,
	username VARCHAR (16) NOT NULL,
	INDEX [ indexName ] (username(length))
);
```
 
* 删除索引的语法：
```
DROP INDEX [indexName] ON mytable;
```
#### 唯一索引
它与前面的普通索引类似，不同的就是：索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。它有以下几种创建方式：

* 1 创建索引
```
CREATE UNIQUE INDEX indexName ON mytable(username(length)) 
```
* 2 修改表结构
```
ALTER mytable ADD UNIQUE [indexName] ON (username(length)) 
```
* 3 创建表的时候直接指定
```
CREATE TABLE mytable (
	ID INT NOT NULL,
	username VARCHAR (16) NOT NULL,
	UNIQUE [ indexName ] (username(length))
);
```
 

#### 主键索引
它是一种特殊的唯一索引，不允许有空值。一般是在建表的时候同时创建主键索引：
```
CREATE TABLE mytable (
	ID INT NOT NULL,
	username VARCHAR (16) NOT NULL,
	PRIMARY KEY (ID)
);
```
当然也可以用 ALTER 命令。记住：一个表只能有一个主键。

#### 组合索引
为了形象地对比单列索引和组合索引，为表添加多个字段：
```
CREATE TABLE mytable (
	ID INT NOT NULL,
	username VARCHAR (16) NOT NULL,
	city VARCHAR (50) NOT NULL,
	age INT NOT NULL
);
```
为了进一步榨取MySQL的效率，就要考虑建立组合索引。就是将 name, city, age建到一个索引里：
```
ALTER TABLE mytable ADD INDEX name_city_age (username(10),city,age); 
```
>建表时，usernname长度为 16，这里用 10。这是因为一般情况下名字的长度不会超过10，这样会加速索引查询速度，还会减少索引文件的大小，提高INSERT的更新速度。如果分别在 usernname，city，age上建立单列索引，让该表有3个单列索引，查询时和上述的组合索引效率也会大不一样，远远低于我们的组合索引。虽然此时有了三个索引，但MySQL只能用到其中的那个它认为似乎是最有效率的单列索引。

建立这样的组合索引，其实是相当于分别建立了下面三组组合索引：
```
usernname,city,age   
usernname,city   
usernname  
```
为什么没有 <code>city，age</code>这样的组合索引呢？这是因为MySQL组合索引“最左前缀”的结果。简单的理解就是只从最左面的开始组合。并不是只要包含这三列的查询都会用到该组合索引，下面的几个SQL就会用到这个组合索引：
```
SELECT * FROM mytable WHREE username="admin" AND city="郑州" 
SELECT * FROM mytable WHREE username="admin" 
```
而下面几个则不会用到：
```
SELECT * FROM mytable WHREE age=20 AND city="郑州"  
SELECT * FROM mytable WHREE city="郑州"
```


### 什么时候建立索引？
一般来说，在WHERE和JOIN中出现的列需要建立索引，但也不完全如此，因为MySQL只对<code><，<=，=，>，>=，BETWEEN，IN，</code>以及某些时候的LIKE才会使用索引。例如：
```
SELECT
	t. NAME
FROM
	mytable t
LEFT JOIN mytable m ON t. NAME = m.username
WHERE
	m.age = 20
AND m.city = '郑州'
```
此时就需要对city和age建立索引，由于mytable表的userame也出现在了JOIN子句中，也有对它建立索引的必要。

#### 索引的缺点
上面都在说使用索引的好处，但过多的使用索引将会造成滥用。因此索引也会有它的缺点：
* 1.虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行INSERT、UPDATE和DELETE。因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件。
* 2.建立索引会占用磁盘空间的索引文件。一般情况这个问题不太严重，但如果你在一个大表上创建了多种组合索引，索引文件的会膨胀很快。
索引只是提高效率的一个因素，如果你的MySQL有大数据量的表，就需要花时间研究建立最优秀的索引，或优化查询语句。

#### 优化常用的小技巧
优化常用简单操作：
```
set profiling = 1; 

expain (查询语句);

show profiles;

show profile for query (查询语句id);

create index aIndex on tablename(columnname);
```
根据explain检测出mysql内部执行查询的步骤和具体参数来优化自己的select语句