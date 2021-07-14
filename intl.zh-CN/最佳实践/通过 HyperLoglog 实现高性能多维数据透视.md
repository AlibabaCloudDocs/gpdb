# 通过 HyperLoglog 实现高性能多维数据透视

本文通过电商类数据透视示例，介绍了使用AnalyticDB PostgreSQL通过HLL预计算，实现毫秒级多维数据透视的方法。关于HyperLogLog的用法，请参考[使用HLL](/intl.zh-CN/开发进阶/高级扩展插件使用/HyperLogLog 的使用.md)。

## 实践总结

本文介绍的操作方法，涉及以下最佳实践。如您已了解操作方法，可以直接参考实践总结来应用。

-   对于透视分析需求，使用倒转的方法，将数据按查询需求进行预计算，得到统计结果；从而在透视时仅需查询计算结果，可以做到任意维度透视，都可以在100毫秒以内响应。

-   使用GROUPING SETS，对多个标签维度进行一次性统计，降低数据重复扫描和重复运算，大幅提升处理效率。

-   使用数组，记录每个透视维度的UID，不仅支持透视，也可以满足圈人的需求，同时支持未来更加复杂的透视需求。

-   使用HLL类型来存储估算值，在进行复杂透视时，可以使用HLL。例如，多个HLL的值可以UNION，可以求唯一值个数，通常用于评估UV，新增UV等。

-   使用流计算。如果数据需要实时的统计，那么可以使用pipelineDB进行流式分析，实时计算统计结果。

-   与阿里云云端组件结合，使用OSS对象存储过渡数据（原始数据）。使用OSS\_FDW外部表对接OSS，因此过渡数据可以不入库，仅仅用于预计算。大幅降低数据库的写入需求、空间需求。

-   使用Greenplum的一级、二级分区，将透视数据的访问需求打散到更小的单位，然后使用标签索引，再次降低数据搜索的范围，从而做到任意数据量，任意维度透视请求100毫秒以内响应。

-   使用列存储，提升压缩比，节省统计数据的空间占用。


## 背景

典型的电商类数据透视业务会使用一些用户的标签数据作为透视的语料。例如，包含品牌的ID，销售区域的ID，品牌对应用户的ID，以及若干用户标签字段，时间字段等。在作分析时，标签可能会按不同的维度进行归类。例如，tag1 性别，tag2 年龄段, tag3 兴趣爱好等等。

业务方较多的需求往往是对自有品牌的用户进行透视。例如，一个非常典型的数据透视需求就是统计不同的销售区域（渠道）、时间段、标签维度下的用户数。

## 准备

作为示例，定义以下数据结构。

-   t1：每天所在区域、销售渠道的活跃用户ID。

    ```
    t1 (
    uid,       -- 用户 ID
    groupid,   -- 销售渠道、区域 ID
    day        -- 日期
    )
    ```

-   t2：每个品牌的自有用户（维护增量）。

    ```
    t2 (
    uid,    -- 用户 ID
    brand   -- 品牌
    )
    ```

-   t3：用户标签（维护增量）。

    ```
    t3 (
    uid,    -- 用户 ID
    tag1,   -- 标签1，如兴趣
    tag2,   -- 标签2，如性别
    tag3,   -- 标签3，如年龄段
    ... ,
    )
    ```


基于已定义的数据结构，可以按照品牌、销售区域、标签、日期进行透视。例如，

```
select
  '兴趣' as tag,
  t3.tag1 as tag_value,
  count(1) as cnt
from
  t1,
  t2,
  t3
where
  t1.uid = t3.uid
  and t1.uid = t2.uid
  and t2.brand = ?
  and t1.groupid = ?
  AND t1.day = '2017-06-25'
group by t3.tag1
```

可以看出，这类查询的运算量较大。而且，分析师可能需要对不同的维度进行比对分析。因此，建议采用预计算的方法进行优化。

## 使用预计算优化检索

为实现快速检索，您可以使用以下优化方法：

-   对于Greenplum，使用列存储。
-   表分区按照day范围一级分区，按brand, groupid哈希进行二级分区。
-   数据分布策略选择随机分布。
-   针对每个`tag?`字段建立单独索引。

结合以上优化，不管数据量多大，单次透视请求的响应速度都可以控制在100毫秒以内。

通过预计算优化，希望得到以下结果：

```
t_result (
  day,      -- 日期
  brand,    -- 品牌 ID
  groupid,  -- 渠道、地区、门店 ID
  tag1,     -- 标签类型1
  tag2,     -- 标签类型2
  tag3,     -- 标签类型3
  ...       -- 标签类型n
  cnt,      -- 用户数
  uids,     -- 用户 ID 数组，这个为可选字段，如果不需要知道 ID 明细，则不需要保存
  hll_uids  -- 用户 HLL 估值
)
```

得到这份结果后，分析师的查询过程可以简化为以下内容：

```
select
  day, brand, groupid, 'tag?' as tag, cnt, uids, hll_uids
from t_result
where
  day =
  and brand =
  and groupid =
  and tag? = ?
```

其中，前三个条件（`day, brand, groupid`）通过分区过滤数据，最后根据`tag?`的索引快速得到结果。

可以看出，预计算后能够以少量的运算，实现更加复杂的维度分析。例如，可以分析出某两天的差异用户，多个TAG叠加的用户等。

## 使用预计算的方法

使用如下SQL来产生统计结果。

```
select
  t1.day,
  t2.brand,
  t1.groupid,
  t3.tag1,
  t3.tag2,
  t3.tag3,
  ...
  count(1) as cnt,
  array_agg(uid) as uids,
  ## 将 uid 聚合为数组。
  hll_add_agg(hll_hash_integer(uid)) as hll_uids
  ## 将 UID 转换为 hll hash val，并聚合为 HLL 类型。
from
  t1,
  t2,
  t3
where
  t1.uid = t3.uid
  and t1.uid = t2.uid
group by
  t1.day,
  t2.brand,
  t1.groupid,
  grouping sets (
  ## 为了按每个标签维度进行统计，使用多维分析语法 grouping sets，这样可以不必通过多条 SQL 来实现。结果是数据只扫描一遍，且按每个标签维度进行统计。
    (t3.tag1),
    (t3.tag2),
    (t3.tag3),
    (...),
    (t3.tagn)
  )
```

## 预计算结果透视查询

如果进行复杂透视，可以对分析结果的不同记录进行数组逻辑运算，得到UID集合结果。

**使用数组逻辑运算**

您可以使用以下数组逻辑运算。

-   统计在数组1但不在数组2的值。

    ```
    create or replace function arr_miner(anyarray, anyarray) returns anyarray as $$
    select array(select * from (select unnest($1) except select unnest($2)) t group by 1);
    $$ language sql strict;
    ```

-   统计数组1和数组2的交集。

    ```
    create or replace function arr_overlap(anyarray, anyarray) returns anyarray as $$
    select array(select * from (select unnest($1) intersect select unnest($2)) t group by 1);
    $$ language sql strict;
    ```

-   统计数组1和数组2的并集。

    ```
    create or replace function arr_merge(anyarray, anyarray) returns anyarray as $$
    select array(select unnest(array_cat($1,$2)) group by 1);
    $$ language sql strict;
    ```


**应用示例**

例如，假设促销活动前（2017-06-24）的用户集合为UID1\[\]，促销活动后（2017-06-25）的用户集合为UID2\[\]，可以使用以下命令得出促销活动中有哪些新增用户。

```
arr_miner(uid2[], uid1[])
```

## 使用HLL做数据逻辑计算

您可以使用HLL进行以下逻辑计算。

-   计算唯一值个数。

    ```
    hll_cardinality(users)
    ```

-   计算两个HLL的并集，得到一个HLL。

    ```
    hll_union()
    ```


**应用示例**

例如，假设在促销活动前（2017-06-24）的用户集合HLL为uid1\_hll，促销活动后（2017-06-25）的用户集合HLL为uid2\_hll，可以使用以下命令得出促销活动中有哪些新增用户。

```
hll_cardinality(uid2_hll) - hll_cardinality(uid1_hll)
```

## 预计算调度

在优化前，业务通过即时JOIN得到透视结果，而优化后使用事先统计的方法来获得透视结果，而事先统计本身需要调度。调度方法取决于数据的来源以及数据合并的方法（流式增量或批量增量）。

**按天统计数据**

历史统计数据无更新，只有增量。需要定时将统计结果写入并合并至`t_result`结果表中。

```
insert into t_result
select
  t1.day,
  t2.brand,
  t1.groupid,
  t3.tag1,
  t3.tag2,
  t3.tag3,
  ...
  count(1) as cnt,
  array_agg(uid) as uids,
  hll_add_agg(hll_hash_integer(uid)) as hll_uids
from
  t1,
  t2,
  t3
where
  t1.uid = t3.uid
  and t1.uid = t2.uid
group by
  t1.day,
  t2.brand,
  t1.groupid,
  grouping sets (
    (t3.tag1),
    (t3.tag2),
    (t3.tag3),
    (...),
    (t3.tagn)
  )
```

**合并统计维度数据**

数据结果按天进行统计，但如果要查询按月，或者按年的统计，则需要对按天统计的数据查询并汇聚。业务也能选择异步汇聚，最终用户查询到的是汇聚后的结果。

```
t_result_month (
  month,    -- yyyy-mm
  brand,    -- 品牌 ID
  groupid,  -- 渠道、地区、门店 ID
  tag1,     -- 标签类型1
  tag2,     -- 标签类型2
  tag3,     -- 标签类型3
  ...       -- 标签类型n
  cnt,      -- 用户数
  uids,     -- 用户 ID 数组，这个为可选字段，如果不需要知道 ID 明细，则不需要保存
  hll_uids  -- 用户 HLL 估值
)
```

array聚合需要自定义以下聚合函数：

```
postgres=# create aggregate arragg (anyarray) ( sfunc=arr_merge, stype=anyarray);
CREATE AGGREGATE
postgres=# select arragg(c1) from (values (array[1,2,3]),(array[2,5,6])) t (c1);
   arragg
-------------
 {6,3,2,1,5}
(1 row)
```

例如，您可以使用以下SQL，来按月汇聚数据：

```
select
  to_char(day, 'yyyy-mm'),
  brand,
  groupid,
  tag1,
  tag2,
  tag3,
  ...
  array_length(arragg(uid),1) as cnt,
  arragg(uid) as uids,
  hll_union_agg() as hll_uids
from t_result
group by
  to_char(day, 'yyyy-mm'),
  brand,
  groupid,
  tag1,
  tag2,
  tag3,
  ...
```

以此类推，可以得出按年汇聚的结果。

## 流式调度

如果业务方有实时统计的需求，那么可以使用流式计算的方法，实时进行以上聚合统计。如果数据量非常庞大，可以根据分区键，对数据进行分流，不同的数据落到不同的流计算节点，最后汇总流计算的结果到AnalyticDB PostgreSQL（base on GPDB）中。

