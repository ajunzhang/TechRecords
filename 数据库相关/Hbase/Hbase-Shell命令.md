### Hbase Shell命令
* 1 进入HbaseShell：

<code>hbase shell</code>

* 2 查看状态

<code>stats</code>

* 3 版本号

<code>version</code>

* 4 列出表名

<code>list</code>

* 5 描述表

<code>describe 'test'</code>

* 6 获取一个id的所有数据(TableName，RowKey)

<code>get 'T_CCV3_MOVEMENT','66787358'</code>

* 7 获取一个id，一个列族的所有数据(TableName, RowKey, ColumnFamily)

<code>get 'T_CCV3_MOVEMENT','66787358','cf'</code>

* 8 获取一个id，一个列族中一个列的所有数据(TableName, RowKey, ColumnFamily:column)

<code>get 'T_CCV3_MOVEMENT','66787358','cf:RISK_RATE_AMENDED_ON'</code>

* 9 表的条数 很费时间 一般不要用吧

<code>count('T_CCV3_MOVEMENT')</code>

* 10 查询表前几个rowKey

<code>scan 'T_CCV3_MOVEMENT',{LIMIT=>1}</code>

* 11 创建表

<code>create <table>, {NAME => <family>, VERSIONS => <VERSIONS>}</code>

<code>create 't1',{NAME => 'f1', VERSIONS => 2},{NAME => 'f2', VERSIONS => 2}</code>

* 12 删除表 首先disable，然后drop

<code>disable 't1'</code>
<code>drop 't1'</code>

* 13 修改表结构,修改表结构必须先disable

```
# 语法：alter 't1', {NAME => 'f1'}, {NAME => 'f2', METHOD => 'delete'}
# 例如：修改表test1的cf的TTL为180天

hbase(main)> disable 'test1'
hbase(main)> alter 'test1',{NAME=>'body',TTL=>'15552000'},{NAME=>'meta', TTL=>'15552000'}
hbase(main)> enable 'test1'

```

* 14 添加数据

```
# 语法：put <table>,<rowkey>,<family:column>,<value>,<timestamp>
# 例如：给表t1的添加一行记录：rowkey是rowkey001，family name：f1，column name：col1，value：value01，timestamp：系统默认

hbase(main)> put 't1','rowkey001','f1:col1','value01'
```

*  15 更新一条记录(TableName, RowKey, ColumnFamily:column),将scutshuxue的年龄改成99.
<code>put 'member','scutshuxue','info:age' ,'99'</code>

*

<code></code>