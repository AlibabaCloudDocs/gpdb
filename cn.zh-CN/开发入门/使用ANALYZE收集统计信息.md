# 使用ANALYZE收集统计信息

正确的统计信息是查询优化器选择高效的查询计划的必要条件，如果统计信息不存在或者信息过时，优化器可能会生成低效的查询计划。使用ANALYZE语句可以更新统计信息。

ANALYZE使用命令如下：

```
ANALYZE [VERBOSE] [ROOTPARTITION [ALL] ] [table [ (column [, ...] ) ]]
```

## AUTO ANALYZE

AUTO ANALYZE可以自动执行ANALYZE命令。AUTO ANALYZE将检查具有大量插入、更新或删除的表，并在需要的时候主动对表执行ANALYZE来收集更新表的统计信息。当前默认情况下，当表改动行数超过10%时，AUTO ANALYZE会自动对表触发一次ANALYZE操作。

对于MULTI MASTER实例，当前暂时只能追踪主MASTER上发生的改动行为，辅助MASTER发生的改动行为将不会触发AUTO ANALYZE。

**说明：** 云原生数据仓库AnalyticDB PostgreSQL版仅20210527及以后版本支持AUTO ANALYZE功能，如何升级小版本，请参见[版本升级](/cn.zh-CN/实例管理/版本管理/版本升级.md)。

## 选择生成统计信息

不带参数运行ANALYZE会为数据库中所有的表更新统计信息，运行时间可能会很长。用户可以指定表名或者列名来指定收集单个表或者某些列的统计信息。当数据被改变时，应该有选择地ANALYZE表。

在大表上运行ANALYZE可能需要很长时间，用户可以只使用`ANALYZE table(column, ...)`为选择的列生成统计信息，例如收集用在连接、WHERE子句、SORT子句、GROUP BY子句或者HAVING子句中的那些列。对于一个分区表，用户可以在pg\_partitions系统目录中寻找分区表的名字，只在更改过的分区上运行ANALYZE：

```
SELECT partitiontablename from pg_partitions WHERE tablename='parent_table';
```

## 何时运行ANALYZE

在以下时候运行ANALYZE：

-   导入数据之后。
-   CREATE INDEX之后。
-   在做大量的INSERT、UPDATE以及DELETE操作之后。

ANALYZE会在表上要求一个读锁，因此它可以与其他数据库活动并行运行，但不要在执行导入、INSERT、UPDATE、DELETE以及CREATE INDEX操作期间会运行ANALYZE。

