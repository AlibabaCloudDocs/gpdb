# Bulk update {#concept_mjz_zq2_v2b .concept}

Update, also called Merge, indicates updating the latest data to AnalyticDB for PostgreSQL. If the updated data already exists, it replaces the old version. If the updated data does not exist, it is inserted to the database. Such data merge is usually completed offline. For example, you can set to update data on a daily basis to AnalyticDB for PostgreSQL. Some users may require real-time updates, that is, the latency is at the minute or second level.

This document describes how to merge data in AnalyticDB for PostgreSQL and explains the principle behind it. In addition, you can learn how to use the bulk operation to update multiple data.

## Simple update {#section_c2v_pzz_gfb .section}

Data merge is about modifying the data, that is, running the Update, Delete, Insert, or Copy operations. Take an Update operation for example, updating the record on a single row in a column-store table. The following figure shows the data updating process in AnalyticDB for PostgreSQL.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16862/15578250339770_en-US.png)

The procedure is described as follows:

1.  The user sends an Update SQL request to the master node.

2.  The master node initiates distributed transactions, locking the table to be updated \(AnalyticDB for PostgreSQL does not allow concurrent updates to the same table\), and distributing updating requests to matched slave nodes.

3.  Slave nodes scan the index to locate the data to update, and update the data. For column-store tables, the updating logic is to delete the old data row and write the new data row at the end of the table. The updated data page in the column-store table is written to the memory cache, and the change in the corresponding table file length \(because data is written to the table end, the length of the corresponding table file is increased\) is written to the log \(xlog file\).

4.  Before the Update process ends, the updated data page and xlog file in the memory are both synchronized to the mirror node. After the synchronization is complete, the master node ends the distributed transaction, and returns the message about successful execution to the user.


The whole process is long and contains lots of operations, such as SQL statement parsing, transactions distributing, locking, connection establishment between the master node and slave nodes, and the synchronization of data and log between slave nodes and the mirror node. These operations all consume CPU or I/O resources and prolong the response of the request.

Therefore, for AnalyticDB for PostgreSQL, we recommend that you avoid updates to a single data row, and try to update data by using bulk operations as much as possible. That is:

-   Put updates in one SQL statement to reduce the overhead for statement parsing, node communications, and data synchronization.
-   Put updates in one transaction to avoid unnecessary overhead.

## Bulk update {#section_g2v_pzz_gfb .section}

Follow these steps to use one SQL statement to update multiple independent data rows.

**1. Prepare the target table**

Suppose that the table to be updated is target\_table. The target\_table is defined as follows.

```
create table target_table(c1 int, c2 int, primary key (c1));
insert into target_table select generate_series(1, 10000000);
```

The target table is usually quite big. Suppose that you want to insert 10 million rows of data to target\_table. The target\_table is indexed to facilitate updates. A primary key is defined and a unique index is included consequently.

**2. Prepare the stage table**

The stage table \(source\_table in this example\) is necessary for bulk update. It is a temporary table created for updating data. To update the data in target\_table, you first insert the new data to source\_table, and then import the new data by using the [Copy command](../../../../intl.en-US/Quick Start/Import data/Migrate data to AnalyticDB for PostgreSQL by using the Copy command.md#), [OSS external table](../../../../intl.en-US/Quick Start/Import data/Parallel import from OSS or export to OSS.md#), or other means to target\_table.

In the following example, some data is directly generated in source\_table.

```
create table source_table(c1 int, c2 int);
insert into source_table select generate_series(1, 100), generate_series(1,100);
```

**3. Bulk update**

After the source\_table data is ready, run the `update set … from … where ..` statement.

**Note:** To utilize the index to a maximum extent, you can use `set optimizer=on` to start the ORCA optimizer before the update operation. If the ORCA optimizer is not started, you can run `set enable_nestloop = on` to use the index.

```
set optimizer=on;
update target_table set c2 = source_table.c2 from source_table where target_table.c1= source_table.c1;
```

The update operation’s query plan is as follows:

```
=> explain update target_table set c2 = source_table.c2 from source_table where target_table.c1= source_table.c1;
                                                         QUERY PLAN
-----------------------------------------------------------------------------------------------------------------------------
 Update  (cost=0.00..586.10 rows=25 width=1)
   ->  Result  (cost=0.00..581.02 rows=50 width=26)
         ->  Redistribute Motion 4:4  (slice1; segments: 4)  (cost=0.00..581.02 rows=50 width=22)
               Hash Key: public.target_table.c1
               ->  Assert  (cost=0.00..581.01 rows=50 width=22)
                     Assert Cond: NOT public.target_table.c1 IS NULL
                     ->  Split  (cost=0.00..581.01 rows=50 width=22)
                           ->  Nested Loop  (cost=0.00..581.01 rows=25 width=18)
                                 Join Filter: true
                                 ->  Table Scan on source_table  (cost=0.00..431.00 rows=25 width=8)
                                 ->  Index Scan using target_table_pkey on target_table  (cost=0.00..150.01 rows=1 width=14)
                                       Index Cond: public.target_table.c1 = source_table.c1
```

From the plan, AnalyticDB for PostgreSQL uses the index. But if you add more data to source\_table, the optimizer may deem that using Nest Loop associated method and index scanning is not as efficient as dropping the index. As a result, it may use Hash associated method and table scanning for the execution. For example,

```
postgres=> insert into source_table select generate_series(1, 1000), generate_series(1,1000);
INSERT 0 1000
postgres=> analyze source_table;
ANALYZE
postgres=> explain update target_table set c2 = source_table.c2 from source_table where target_table.c1= source_table.c1;
                                              QUERY PLAN
------------------------------------------------------------------------------------------------------
 Update  (cost=0.00..1485.82 rows=275 width=1)
   ->  Result  (cost=0.00..1429.96 rows=550 width=26)
         ->  Assert  (cost=0.00..1429.94 rows=550 width=22)
               Assert Cond: NOT public.target_table.c1 IS NULL
               ->  Split  (cost=0.00..1429.93 rows=550 width=22)
                     ->  Hash Join  (cost=0.00..1429.92 rows=275 width=18)
                           Hash Cond: public.target_table.c1 = source_table.c1
                           ->  Table Scan on target_table  (cost=0.00..477.76 rows=2500659 width=14)
                           ->  Hash  (cost=431.01..431.01 rows=275 width=8)
                                 ->  Table Scan on source_table  (cost=0.00..431.01 rows=275 width=8)
```

The bulk update approach described reduces SQL compilation, inter-node communications, transactions, and other overheads, and can greatly boost data updating performance and reduce resource consumption.

## Bulk delete {#section_z2v_pzz_gfb .section}

For delete operations, you can use a stage table similar to that used for bulk update, and use the following delete command with a “Using” clause to delete data by bulk:

```
delete from target_table using source_table where target_table.c1 = source_table.c1;
```

The bulk delete operation also uses the index.

```
explain delete from target_table using source_table where target_table.c1 = source_table.c1;
                                             QUERY PLAN
-----------------------------------------------------------------------------------------------------
 Delete (slice0; segments: 4)  (rows=50 width=10)
   ->  Nested Loop  (cost=0.00..41124.40 rows=50 width=10)
         ->  Seq Scan on source_table  (cost=0.00..6.00 rows=50 width=4)
         ->  Index Scan using target_table_pkey on target_table  (cost=0.00..205.58 rows=1 width=14)
               Index Cond: target_table.c1 = source_table.c1
```

## Merge data by using Delete and Insert {#section_efv_pzz_gfb .section}

To merge data, you must first put the data to merge to the stage table. If you know in advance that the data to be merged already exists in the target table, you can use update statements to merge the data. But in most cases, part of the data to be merged already exists in the target table, and part of it is new, with no matched records in the target table. In this case, you can use a combination of bulk delete and bulk insert. The sample code is as follows.

```
set optimizer=on;
delete from target_table using source_table where target_table.c1 = source_table.c1;
insert into target_table select * from source_table;
```

## Update data in real time by using Values\(\) expressions {#section_gfv_pzz_gfb .section}

To use the stage table, you must maintain its lifecycle. Some users want to update data to AnalyticDB for PostgreSQL by bulk in real time, that is, to continuously synchronize data or merge data to AnalyticDB for PostgreSQL.

If you use the aforementioned method, you must create and delete \(or truncate\) the stage table repeatedly. In fact, you can use Values expressions to achieve an effect similar to stage tables, without the effort to maintain the table. The approach is to first splice the data to update into a Values expression, and then run the update or delete commands by using the following method:

```
update target_table set c2 = t.c2 from (values(1,1),(2,2),(3,3),…(2000,2000)) as t(c1,c2) where target_table.c1=t.c1
delete from target_table using (values(1,1),(2,2),(3,3),…(2000,2000)) as t(c1,c2) where target_table.c1 = t.c1
```

**Note:** Both `set optimizer=on;` and `set enable_nestloop=on;` generate query plans that use indexes. In complicated cases, however, such as multiple index fields or partition tables are involved, the ORCA optimizer must be used to match the index.

