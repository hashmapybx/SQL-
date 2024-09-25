<!--
 * @Author: hashmapybx 15868861416@163.com
 * @Date: 2023-09-12 16:00:43
 * @LastEditors: hashmapybx 15868861416@163.com
 * @LastEditTime: 2024-09-25 22:55:31
 * @FilePath: /SQL-/README.md
 * @Description: 这是默认设置,请设置`customMade`, 打开koroFileHeader查看配置 进行设置: https://github.com/OBKoro1/koro1FileHeader/wiki/%E9%85%8D%E7%BD%AE 
-->
# SQL学习
相关数据库SQL总结

![Alt text](image-1.png)
## Postgresql总结

控制台操作命令

|操作|命令|
|----|----|
|设置密码|  \password dbuser| 
|退出控制台| \q|
查看sql命令的解释| \h sql语句
列出所有数据库| \l
进入其他数据库| \c [db_name]
列出当前数据库的所有表| \d
列出某一张表的结果| \d [table_name]
列出所有用户 |\du 
打开文本编辑器| \e 
列出数据库连接信息| \conninfo

查询当前的连接数:
select * from pg_stat_activity;
查看当前最大的连接数 show max_conections;

pgsql 动态复制表
```sql
create table t_key_event_file_student_101 (like t_key_event_file_student);

```
```
create table t_key_event_file_student_100 as select * from t_key_event_file_student;
```
上面两种复制的区别：
like 不会复制数据，并且复制对应字段的约束。
create table as 会复制数据。所有的约束，注释和序列都没有复制成功。

Tchouse 的优化策略


## 列存储和行存储的区别

这篇文章写的很详细：我不在重复轮子了。

https://www.cnblogs.com/xuwc/p/14037950.html

**行存储和列存储最大的区别：**

- 行存储有利于数据的写入，写入性能高，读取性能差，常于OLTP系统
- 列存储有利于数据的读取，写入性能差，适用于现在流行的OLAP系统

## Starrocks 学习

### 表模型

在**明细表**中排序键是Duplicate key 指定的字段，不满足唯一性约束的。

在**聚合表**中排序键是Aggregate key 指定的字段。并且排序键需要满足唯一性约束

在**更新表**排序键是UNIQUe key 指定的字段，并且必须要满足唯一性约束。

在**主键表**中，可以定义主键和排序键，主键primary key需要满足唯一性和非空的约束，主键相同的数据进行REPLACE操作，排序键由ORDER BY指定。


### 明细表

默认的建表类型，在创建表的时候未指定任何key, 则默认都是明细表。

创建表时，支持定义排序键。如果查询的过滤条件包含排序键，则 StarRocks 能够快速地过滤数据，提高查询效率。明细表适用于日志数据分析等场景，**支持追加新数据，不支持修改历史数据**

适用场景：

导入历史数据，不更新历史数据，只会追加新的数据。（日志或者是时序数据）

详细教程
https://blog.csdn.net/ult_me?spm=1000.2115.3001.5343