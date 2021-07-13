# 使用EXPLAIN阅读查询计划

查询优化器使用数据库的数据统计信息来选择具有最小总代价的查询计划，查询代价通过磁盘I/O取得的磁盘页面数作为单位来度量。 可以使用EXPLAIN和EXPLAIN ANALYZE语句发现和改进查询计划。

EXPLAIN的语法如下：

```
EXPLAIN [ANALYZE] [VERBOSE] statement
```

EXPLAIN展示查询优化器对该查询计划估计的代价。例如：

```
EXPLAIN SELECT * FROM names WHERE id=22; 
```

EXPLAIN ANALYZE不仅会显示查询计划，还会实际运行语句，显示实际执行的行数、时间等额外信息。例如：

```
EXPLAIN ANALYZE SELECT * FROM names WHERE id=22;
```

**阅读EXPLAIN输出**

查询计划类似于一棵有节点的树，执行和阅读的顺序是自底而上。计划中的每个节点表示一个操作，例如表扫描、表连接、聚集或者排序。阅读的顺序是从底向上：每个节点会把结果输出给直接在它上面的节点。一个计划中的底层节点通常是表扫描操作：顺序扫描表、通过索引或者位图索引扫描表等。如果该查询要求那些行上的连接、聚集、排序或者其他操作，就会有额外的节点在扫描节点上面负责执行这些操作。最顶层的计划节点通常是数据库的移动（MOTION）节点：重分布（REDISTRIBUTE\)、广播（BROADCAST）或者收集（GATHER）节点。这些操作在查询处理时在实例节点之间移动数据。

EXPLAIN的输出对于查询计划中的每个节点都显示为一行并显示该节点类型和下面的执行的代价估计：

-   cost：以磁盘页面获取为单位度量。1.0等于一次顺序磁盘页面读取。第一个估计是得到第一行的启动代价，第二个估计是得到所有行的总代价。
-   rows：这个计划节点输出的总行数。这个数字根据条件的过滤因子会小于被该计划节点处理或者扫描的行数。最顶层节点的是估算的返回、更新或者删除的行数。
-   width：这个计划节点输出的所有行的总字节数。

需要注意以下两点：

-   一个节点的代价包括其子节点的代价。最顶层计划节点有对于该计划估计的总执行代价。这是优化器估算出来的最小的数字。
-   代价只反映了在数据库中执行的时间，并没有计算在数据库执行之外的时间，例如将结果行传送到客户端花费的时间。

**EXPLAIN举例**

下面的例子描述了如何阅读一个查询的EXPLAIN查询代价：

```
EXPLAIN SELECT * FROM names WHERE name = 'Joelle';
                     QUERY PLAN
------------------------------------------------------------
Gather Motion 4:1 (slice1) (cost=0.00..20.88 rows=1 width=13)

   -> Seq Scan on 'names' (cost=0.00..20.88 rows=1 width=13)
         Filter: name::text ~~ 'Joelle'::text
```

查询优化器会顺序扫描names表，对每一行检查WHERE语句中的filter条件，只输出满足该条件的行。 扫描操作的结果被传递给一个Gather Motion操作。Gather Motion是Segment把所有行发送给Master节点。在这个例子中，有4个Segment节点会并行执行，并向Master节点发送数据。这个计划估计的启动代价是00.00（没有代价）而总代价是20.88次磁盘页面获取。优化器估计这个查询将返回一行数据。

EXPLAIN ANALYZE除了显示执行计划还会运行语句。EXPLAIN ANALYZE计划会把实际执行代价和优化器的估计一起显示，同时显示额外的下列信息：

-   查询执行的总运行时间（以毫秒为单位）。
-   查询计划每个Slice使用的内存，以及为整个查询语句保留的内存。
-   计划节点操作中涉及的Segment节点数量，其中只会统计返回行的Segment。
-   操作产生最多行的Segment节点返回的行最大数量。如果多个Segment节点产生了相等的行数，EXPLAIN ANALYZE会显示那个用了最长结束时间的Segment节点。
-   为一个操作产生最多行的Segment节点的ID。
-   相关操作使用的内存量（work\_mem）。如果work\_mem不足以在内存中执行该操作，计划会显示溢出到磁盘的数据量最少的Segment的溢出数据量。例如：

    ```
    Work_mem used: 64K bytes avg, 64K bytes max (seg0).
    Work_mem wanted: 90K bytes avg, 90K byes max (seg0) to lessen
    workfile I/O affecting 2 workers.
    ```

-   产生最多行的Segment节点检索到第一行的时间（以毫秒为单位）以及该Segment节点检索到所有行花掉的时间。

下面的例子用同一个查询描述了如何阅读一个EXPLAIN ANALYZE查询计划。这个计划中粗体部分展示了每一个计划节点的实际计时和返回行，以及整个查询的内存和时间统计信息。

```
**EXPLAIN ANALYZE** SELECT * FROM names WHERE name = 'Joelle';
                     QUERY PLAN
------------------------------------------------------------
Gather Motion 2:1 (slice1; segments: 2) (cost=0.00..20.88 rows=1 width=13)
**Rows out: 1 rows at destination with 0.305 ms to first row, 0.537 ms to end, start offset by 0.289 ms.**
        -> Seq Scan on names (cost=0.00..20.88 rows=1 width=13)
**Rows out: Avg 1 rows x 2 workers. Max 1 rows \(seg0\) with 0.255 ms to first row, 0.486 ms to end, start offset by 0.968 ms.**
                 Filter: name = 'Joelle'::text
 Slice statistics:

      (slice0) Executor memory: 135K bytes.

    (slice1) Executor memory: 151K bytes avg x 2 workers, 151K bytes max (seg0).

**Statement statistics: Memory used: 128000K bytes Total runtime: 22.548 ms**
```

运行这个查询花掉的总时间是22.548毫秒。Sequential scan操作只有Segment（seg0）节点返回了1行，用了0.255毫秒找到第一行且用了0.486毫秒来扫描所有的行。Segment向Master发送数据的Gather Motion接收到1行。这个操作的总消耗时间是0.537毫秒。

**常见的查询算子**

表扫描操作算子（SCAN）扫描表中的行以寻找一个行的集合，包括以下一些类型：

-   Seq Scan ：顺序扫描表中的所有行。
-   Append-only Scan ： 扫描行存追加优化表。
-   Append-only Columnar Scan ：扫描列存追加优化表中的行。
-   Index Scan ：遍历一个B树索引以从表中取得行。
-   Bitmap Append-only Row-oriented Scan ：从索引中收集仅追加表中行的指针并且按照磁盘上的位置进行排序。
-   Dynamic Table Scan — 使用一个分区选择函数来选择分区。Function Scan节点包含分区选择函数的名称，可以是下列之一：

    -   gp\_partition\_expansion ：选择表中的所有分区。
    -   gp\_partition\_selection ：基于一个等值表达式选择一个分区。
    -   gp\_partition\_inversion ： 基于一个范围表达式选择分区。
    Function Scan节点将动态选择的分区列表传递给Result节点，该节点又会被传递给Sequence节点。


表连接操作算子（JOIN）包括以下一些类型：

-   Hash Join ：从较小的表构建一个哈希表，用连接列作为哈希键扫描较大的表，为连接列计算哈希键并寻找具有相同哈希键的行。哈希连接通常是数据库中最快的连接。计划中的Hash Cond标识要被连接的列。
-   Nested Loop Join ：选择在较大的表作为外表，迭代扫描较小的表中的行。Nested Loop Join要求广播其中的一个小表，这样一个表中的所有行才能与其他表中的所有行进行连接操作。Nested Loop Join在较小的表或者通过使用索引约束的表上执行得不错，但在使用Nested Loop连接大型表时可能会有性能影响。设置配置参数enable\_nestloop为OFF（默认）能够让优化器更偏向选择Hash Join。
-   Merge Join ：排序两个表并且将它们连接起来。Merge Join对于预排序好的数据很快。为了使查询优化器偏向选择Merge Join，可将参数enable\_mergejoin设置为ON。

移动操作算子（MOTION）在Segment节点之间移动数据，包括以下一些类型：

-   Broadcast motion ：每一个Segment节点将自己的行发送给所有其他Segment节点，这样每一个Segment节点都有表的一份完整的本地拷贝。优化器通常只为小型表选择Broadcast motion。对大型表来说，Broadcast motion是会比较慢的。在连接操作没有按照连接键分布的情况下，可能会将把一个表中所需的行动态重分布到别的Segment节点上。
-   Redistribute motion ： 每一个Segment节点重新哈希数据并且把行发送到对应于哈希键的Segment节点上。
-   Gather motion ：来自所有Segment的结果数据被合并在一起发送到节点上（通常是Master节点）。对大部分查询计划来说这是最后的操作。

查询计划中出现的其他操作算子包括：

-   Materialize ：优化器将一个子查询结果进行物化。
-   InitPlan ：预查询，被用在动态分区消除中，当执行时还不知道优化器需要用来标识要扫描分区的值时，会执行这个预查询。
-   Sort ： 为另操作（例如Aggregation或者Merge Join）进行所有数据排序。
-   Group By ： 通过一个或者更多列对行进行分组。
-   Group/Hash Aggregate ： 使用哈希对行进行聚集操作。
-   Append ：串接数据集，例如在整合从分区表中各分区扫描的行时会用到。
-   Filter ：使用来自于一个WHERE子句的条件选择行。
-   Limit ： 限制返回的行数。

**查询优化器的选择**

可以通过查看EXPLAIN输出来判断计划是由ORCA还是传统查询优化器生成。这一信息出现在EXPLAIN输出的末尾。Settings行显示配置参数OPTIMIZER的设置。Optimizer status行显示该解释计划是由ORCA还是传统查询优化器生成。

使用GPORCA优化器的例子：

```
                       QUERY PLAN
------------------------------------------------------------------------------------
 Aggregate  (cost=0.00..296.14 rows=1 width=8)
   ->  Gather Motion 2:1  (slice1; segments: 2)  (cost=0.00..295.10 rows=1 width=8)
         ->  Aggregate  (cost=0.00..294.10 rows=1 width=8)
               ->  Table Scan on part  (cost=0.00..97.69 rows=100040 width=1)
 Settings:  optimizer=on
 Optimizer status: PQO version 1.609
(5 rows)
explain select count(*) from part;
```

使用传统优化器的例子：

```
                       QUERY PLAN
----------------------------------------------------------------------------------------
 Aggregate  (cost=3519.05..3519.06 rows=1 width=8)
   ->  Gather Motion 2:1  (slice1; segments: 2)  (cost=3518.99..3519.03 rows=1 width=8)
         ->  Aggregate  (cost=3518.99..3519.00 rows=1 width=8)
               ->  Seq Scan on part  (cost=0.00..3018.79 rows=100040 width=1)
 Settings:  optimizer=off
 Optimizer status: legacy query optimizer
(5 rows)
```

