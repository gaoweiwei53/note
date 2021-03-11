# 1. Hive 简介
## 1.1 什么是hive
Hive本质：将HSQL转化为Mapreduce程序  
1) Hive处理的数据存储在HDFS  
2) Hive分析数据的底层实现是Mapreduce，可以换成Spark  
3) 运行在yarn上
4) hive不需要搭建分布式

## 1.2 Hive的结构
- Client
- Metastore  
推荐使用MySql
- HDFS
- Driver
  - SQL sparser
  - Query Optimizer
  - Execution
## 1.3 与数据库的对比
- SQL语言相似
- 不善于更新数据，不建议更新操作
- 没有索引，查询的时候需要扫描整张表，加上MapReduce的特性，执行延迟高
- 支持大数据规模

## 1.4 Configure mysql metastore
1) install mysql and create a new user
```sql
create user 'hive_user'@'localhost' identified by '123';
grant all on metastore.* to 'hive_user'@'localhost';
```
if remote, replace 'localhost' with '%' or a ip

2) create `hive-site.xml` in `conf` folder.
```xml
<?xml version="1.0"?>    
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>    
<configuration>    
<property>        
    <name>javax.jdo.option.ConnectionURL</name>        
    <value>jdbc:mysql://localhost/metastore?createDatabaseIfNotExist=true</value>        
  </property>           
  <property>        
    <name>javax.jdo.option.ConnectionDriverName</name>        
    <value>com.mysql.cj.jdbc.Driver</value>        
  </property>           
  <property>        
    <name>javax.jdo.option.ConnectionUserName</name>        
    <value>hive_user</value>        
  </property>           
  <property>        
    <name>javax.jdo.option.ConnectionPassword</name>        
    <value>123</value>        
  </property>           
</configuration>
```
3) mv a [mysql-connector.jar](https://mvnrepository.com/artifact/mysql/mysql-connector-java) to `hive/lib` folder
4) 修改hadoop的`core-site.xml`文件
```xml
   <property>
        <name>hadoop.proxyuser.xxx.hosts</name>
        <value>*</value>
    </property>
    <property>
        <name>hadoop.proxyuser.xxx.groups</name>
        <value>*</value>
    </property>
```
其中xxx为beeline登录的用户名  

5) initial mysql metastore 
```bash
$HIVE_HOME/bin/schematool -dbType mysql -initSchema
```
6)  start metastore service  
```
$HIVE_HOME/bin/hive --service metastore
```  
7) start hiveserver2
```bash
$HIVE_HOME/bin/hiveserver2
$HIVE_HOME/bin/beeline -u jdbc:hive2://$HS2_HOST:$HS2_PORT
$HIVE_HOME/bin/beeline -u jdbc:hive2://localhost:10000
```
By default, it will be (localhost:10000), so the address will look like jdbc:hive2://localhost:10000.  
A Web User Interface (UI) for HiveServer2 provides configuration, logging, metrics and active session information. The Web UI is available at port 10002 (127.0.0.1:10002) by default. 
# 2. 语句
## 2.1 DDL
## 2.2 DML
`show create table test;`查看建表语句

`where`后不能用分组函数，`having` 在 `group by`语句后生效，只用于`group by`分组统计语句。
## 排序
`Order By`：全局排序，只有一个 Reducer。

`Sort By`：对于大规模的数据集 `order by` 的效率非常低。在很多情况下，并不需要全局排序，此时可以使用` sort by`。`sort by`每个reduce局部有序。

`Sort by` 为每个 reducer 产生一个排序文件。每个 Reducer 内部进行排序，对全局结果集来说不是排序。  

`Distribute By`在有些情况下，我们需要控制某个特定行应该到哪个 reducer，通常是为了进行后续的聚集操作。通过指定某个字段进行分区，结合`sort by`从而达到分区内有序。`distribute by` 类似 MR 中 partition
（自定义分区），进行分区。  Hive 要求 `DISTRIBUTE BY` 语句要写在 `SORT BY` 语句之前。 
```sql
select * from emp distribute by deptno sort by sal;
```
当 `distribute by` 和 `sorts by` 字段相同时，可以使用 `cluster by` 方式。

## `WITH`
`with`可用在`select`语句之前，配合`as alias`可用别名代替该子查询，以复用。
```sql
WITH t1 as (SELECT 1), 
t2 as (SELECT 2),
t3 as (SELECT 3)
SELECT * from t1 
UNION ALL
SELECT * from t2
UNION ALL 
SELECT * from t3;

WITH t11 as (SELECT 10),
t12 as (SELECT 20),
t13 as (SELECT 3) 
INSERT INTO t1 
SELECT * from t11 
UNION ALL 
SELECT * from t12 
UNION ALL 
SELECT * from t13;
```


# 3. 外部表、内部表(管理表)
## Managed Table与External Table的区别
默认Hive创建的是managed table. 

Managed table的数据，元数据、统计信息是由Hive所拥有，它的属性、数据结构只能由Hive命令行改变。

在hive中删除管理表的时候，元数据和HDFS中的文件都会被一起删除，而删除外部表的时候，只会删除相应的元数据，hdfs里的文件不会被删除。
## 使用场景
一般将收集到的原始数据，如日志，存为**External table**，在外部表的基础上进行分析，产生的中间表和结果表使用**管理表**进行存储。
# 4. 分区表
可以使用`Partitioned BY`子句创建分区表。一个表可以有一个或多个分区列，并且为分区列中的每个不同的值组合创建一个单独的数据目录。
分区就相当于创建不同的文件夹，分区字段就是文件夹的名称。将表数据按照分区字段放在不同文件夹下。

分区实在原始的一张表内进一步划分，为了方便管理和提高查询效率。比如可将按照日期的来存储一张信息表，以后便可对表按日期来分析。
# 5. 动态分区
关系型数据库中，对分区表 Insert 数据时候，数据库自动会根据分区字段的值，将数据插入到相应的分区中，Hive 中也提供了类似的机制，即动态分区(Dynamic Partition)。
```sql
insert into table dept_no_par partition(deptno)
select dname, loc, deptno from dept;
```
会以`select`语句后最后一个字段的查询结果来进行分区

要设置
```shell
set hive.exec.dynamic.partition=true
set hive.exec.dynamic.partiotion.mode=nonstrict
```

Hive3新特性：`partition(deptno)`可以省略
```sql
insert into table dept_no_par
select dname, loc, deptno from dept;
```
# 6. 分桶
分区针对的是数据的存储路径；分桶针对的是数据文件。
## 创建分桶表
```
create table stu_buck(id int, name string)
clustered by(id)
into 4 buckets
row format delimited fields terminated by '\t';
```
## 分桶规则
根据结果可知：Hive 的分桶采用对分桶字段的值进行哈希，然后除以桶的个数求余的方式决定该条记录存放在哪个桶当中
# 7. 窗口函数
`over()`?
# 8. 压缩与存储
## 8.1 存储
Hive 支持的存储数据的格式主要有：TEXTFILE 、SEQUENCEFILE、ORC、PARQUET。**其中前两个按行存储，后两个是按列存储的。**  
### 行存储与列存储的区别
- 行存储：在查询一整行的数据的时候，行存储更快
- 列存储：在查询特定字段的时侯，列存储更快

### Parquet
- 完美支持嵌套式结构
- 不支持ACID
- 不支持Update操作(delete, update等)
- 支持索引
### Orc存储
到每个 Orc 文件由 1 个或多个 stripe 组成，每个 stripe 一般为 HDFS的块大小，每一个 stripe 包含多条记录，这些记录按照列进行独立存储，对应到 Parquet中的 row group 的概念。每个 Stripe 里有三部分组成，分别是 Index Data，Row Data，StripeFooter： 
- 多层级嵌套表达起来复杂，性能和空间损失较大
- 支持ACID，Update操作
- 支持索引
- 性能比Parquet稍高
# 9. 调优
## Join优化
将 key 相对分散，并且数据量小的表放在 join 的左边，可以使用 map join 让小的维度表先进内存。在 map 端完成 join。
#### 两个大表Join
使用分桶，桶的个数不要超过CPU个数
#### GroupJoin
默认情况下，Map 阶段同一 Key 数据分发给一个 reduce，当一个 key 数据过大时就倾斜了。
并不是所有的聚合操作都需要在 Reduce 端完成，很多聚合操作都可以先在 Map 端进行部分聚合，最后在 Reduce 端得出最终结果。
开启 Map 端聚合参数设置
- 是否在 Map 端进行聚合，默认为 True `set hive.map.aggr = true`
- 在 Map 端进行聚合操作的条目数目 `set hive.groupby.mapaggr.checkinterval = 100000`
- 有数据倾斜的时候进行负载均衡（默认是 false）`set hive.groupby.skewindata = true`
#### Count(Distinct) 去重统计
数据量小的时候无所谓，数据量大的情况下，由于 COUNT DISTINCT 操作需要用一个Reduce Task 来完成，这一个 Reduce 需要处理的数据量太大，就会导致整个 Job 很难完成，一般 COUNT DISTINCT 使用先 GROUP BY 再 COUNT 的方式替换,但是需要注意 group by 造成的数据倾斜问题。
#### 行列过滤
- 列处理：在 SELECT 中，只拿需要的列，如果有分区，尽量使用分区过滤，少用 SELECT \*。
- 行处理：在分区剪裁中，当使用外关联时，如果将副表的过滤条件写在 Where 后面，那么就会先全表关联，之后再过滤。
#### 复杂文件增加 Map 数
当 input 的文件都很大，任务逻辑复杂，map 执行非常慢的时候，可以考虑增加 Map 数，来使得每个 map 处理的数据量减少，从而提高任务的执行效率。
####  小文件进行合并
- 在 map 执行前合并小文件，减少 map 数：CombineHiveInputFormat 具有对小文件进行合并的功能（系统默认的格式）。HiveInputFormat 没有对小文件合并功能。 
```
set hive.input.format=
org.apache.hadoop.hive.ql.io.CombineHiveInputFormat;
```
- 在 Map-Reduce 的任务结束时合并小文件的设置
在 map-only 任务结束时合并小文件，默认 true
```
SET hive.merge.mapfiles = true;
```
在 map-reduce 任务结束时合并小文件，默认 false
```
SET hive.merge.mapredfiles = true;
```
合并文件的大小，默认 256M
```
SET hive.merge.size.per.task = 268435456;
```
当输出文件的平均大小小于该值时，启动一个独立的 map-reduce 任务进行文件 merge
```
SET hive.merge.smallfiles.avgsize = 16777216;
```
####  合理设置 Reduce 数

# 函数
## 1. 内置函数
## 2. 常用内置函数

### `explode(array or map)`
将hive一列中复杂的Array或Map结构拆分多行

`lateral view`用于和`split`, `explode`等UDTF一起使用，将一列数据拆成多行数据后，它将拆分得到的多行数据与其它某个原始字段进行对应形成一个临时表。例如：
```sql
SELECT
movie,
category_name
FROM
movie_info
lateral VIEW
explode(split(category,",")) movie_info_tmp AS category_name;
```
其中`movie_info_tmp`是形成的临时表名称，`category_name`是拆分得到的列名称

### 日期函数
- `date_format`
- `date_add`
- `next_day`
- `last_day`
## 3. 自定义函数
### 根据UDF类别分为以下三种
1) UDF(User-Define-Function) 一进一出
2) UDAF(User-Defined Aggregation Function) 多进一出
3) UDTF(User-Defined TRable-Generating Function) 一进多出
### 编程步骤
1) 继承Hive提供的类
2) 实现类中的抽象方法
3) 在hive的命令行窗口创建函数

## Hive数据类型
### 复杂数据类型
- map
- array
- struct

