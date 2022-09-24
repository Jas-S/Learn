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


