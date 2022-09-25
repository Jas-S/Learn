- [性能分析工具的使用](#性能分析工具的使用)
  - [查看SQL执行成本：SHOW PROFILE](#查看sql执行成本show-profile)
    - [show profile的常用查询参数：](#show-profile的常用查询参数)
    - [日常开发需要注意的结论：](#日常开发需要注意的结论)
  - [分析查询语句：EXPLAIN](#分析查询语句explain)
    - [EXPLAIN各列作用](#explain各列作用)
      - [1.table](#1table)
      - [2.id](#2id)
      - [3.select_type](#3select_type)
      - [4.partitions(可略)](#4partitions可略)
      - [5.type☆](#5type)
      - [6.possible_key和key](#6possible_key和key)
      - [7.key_len☆](#7key_len)
      - [8.ref](#8ref)
      - [9.rows☆](#9rows)
      - [10.filtered](#10filtered)
      - [11.Extra☆](#11extra)
      - [12.小结](#12小结)


  
    
# 性能分析工具的使用
通过使用`EXPLAIN SHOW PROFILING`确认问题点。  
* SQL等待时间长
    * 调优服务器参数
* SQL执行时间长
    * 索引设计优化
    * JION表过多（超过3个），需要优化
    * 数据表设计优化

注：如通过以上方法仍无法解决，则需考虑SQL查询是否到达瓶颈。如果是，则可考虑以下对应方法。
* 读写分离（主从架构）
* 分库分表（垂直分库，垂直分表，水平分表）

## 查看SQL执行成本：SHOW PROFILE
默认情况下处于关闭状态。
```sql
mysql > show variables like 'profiling';
```

可通过执行如下语句在会话级别开启该功能。
```sql
mysql > set profiling = 'ON';
```

开启后即可执行
```sql
mysql > show profiles;
mysql > show profile [cpu, block io] [for query n];
 ```
### show profile的常用查询参数：
* ALL: 显示所有的开销信息。
* BLOCK IO：显示块儿IO开销。
* CONTEXT SWITCHES：上下文切换开销。
* CPU：CPU开销信息。
* IPC：发送及接受开销信息。
* MEMORY：内存开销信息。
* PAGE FAULTS：页面错误开销信息。
* SOURCE：显示和Source_function, Source_file, Source_line相关的开销信息。

### 日常开发需要注意的结论：
* converting HEAP to MyISAM：查询结果太大，内存不够，数据往磁盘上搬了。
* Creating tmp table：创建临时表。先拷贝数据到临时表，用完后再删除临时表。
* Copying to tmp table on disk：把内存中临时表复制到磁盘上，警惕！
* locked。  
  
如果在show profile诊断结果中出现了以上4条结果中的任何一条，则sql语句需要优化。  

>**注意：**
SHOW PROFILE命令将被弃用，之后我们可以从information_schema中的profiling数据表进行查看。

## 分析查询语句：EXPLAIN
定位了查询慢的SQL之后，便可使用EXPLAIN或DESCRIBE工具做针对性的分析。（DESCRIBE语句基本等同于EXPLAIN语句。）

>MYSQL 5.6.3以后EXPLAIN支持了**UPDATE**及**DELETE**语句。

### EXPLAIN各列作用
#### 1.table
每一行记录都对应着一个单表。对于SQL执行中产生的临时表也会显示为一行记录。
#### 2.id
一般情况下在一个大的查询语句中每个**SELECT**关键字都对应一个唯一的id，但查询优化器可能对涉及子查询的语句进行重构，转变为多表查询的语句，这时就可能原语句中存在多个SELECT关键字而id为同一值。
>多行id相同时按照从上至下的顺序执行，多行id不同时按照id值从大到小执行。  
>关注点：每个id值，表示一次独立查询，一个sql查询次数越少越好。
#### 3.select_type
|名称|说明|
| --- | --- |
|SIMPLE|查询语句中不包含`UNION`或者`子查询`|
|PRIMARY|对于复杂查询的外层（外查询），该类型通常在DERIVED和UNION类型混合使用时见到。|
|DERIVED|当一个表不是物理表时（派生表），就被叫做DERIVED|
|DEPENDENT SUBQUERY|为使用子查询而定义的|
|UNION|联合查询的副表|
|UNION RESULT|`UNION`查询时用来完成去重工作的临时表|
|MATERIALIZED|当查询优化器在执行包含子查询的语句时，选择将子查询物化（*1）之后与外层进行连接查询时。子查询对应的属性。|

*1 物化为一个内存中的临时表，表内的数据做了去重。
#### 4.partitions(可略)
代表分区表中的命中情况。一般情况下该项的值都是NULL（未使用分区表）。
#### 5.type☆
访问方法，也叫访问类型。  
* <font color='blue'>system</font>：当表中`只有一条记录`并且该表使用的存储引擎的统计数据时精确地，比如MyISAM，Memory，则对该表的访问方法就是system。
* <font color='blue'>const</font>：根据`主键`或者`唯一`二级索引列与常数进行`等值匹配`时。
* <font color='blue'>eq_ref</font>：连接查询中，被驱动表是通过`主键`或者`唯一`二级索引列`等值匹配`时。
* <font color='blue'>ref</font>：通过`普通的二级索引列`与`常量`进行`等值匹配`时。
* <font color='orange'>fulltext</font>
* <font color='orange'>ref_or_null</font>：当普通二级索引列的值可以为NULL时。
* <font color='orange'>index_merge</font>
* <font color='orange'>unique_subquery</font>
* <font color='orange'>index_subquery</font>
* <font color='red'>range</font>：范围条件
* <font color='red'>index</font>：可以使用索引覆盖，但需要扫描`全部的索引记录`时。
* <font color='red'>ALL</font>：全表扫描
>※上面的越靠前越好。  
>SQL性能优化的目标：至少要达到range级别，理想是ref级别，最好是const级别。

#### 6.possible_key和key
可能用到的索引和实际使用的索引。
#### 7.key_len☆
#### 8.ref
#### 9.rows☆
#### 10.filtered
#### 11.Extra☆
#### 12.小结