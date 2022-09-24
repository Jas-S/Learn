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

**注意：**
SHOW PROFILE命令将被弃用，之后我们可以从information_schema中的profiling数据表进行查看。