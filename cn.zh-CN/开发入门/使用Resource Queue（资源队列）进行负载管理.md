# 使用Resource Queue（资源队列）进行负载管理

使用Resource Queue可以对云原生数据仓库PostgreSQL版的系统资源进行管理或隔离，本文将介绍如何在云原生数据仓库PostgreSQL版中创建和使用Resource Queue。

## Resource Queue介绍

一个数据库实例的CPU资源和内存资源是有限的，这些资源影响着数据库的查询性能，当数据库负载达到一定程度，各个查询会竞争CPU资源和内存资源，造成整体的查询性能低下。对于那些延迟敏感的业务，是不可忍受的情况。

云原生数据仓库PostgreSQL版提供了系统资源负载管理工具——Resource Queue。您可以根据自身业务的情况，指定数据库可运行的并发查询数、每个查询可以使用的内存大小、以及可使用的CPU资源。这样可以保证执行查询时有预期的系统资源，从而得到符合预期的查询性能。

## 创建Resource Queue

您可以使用如下语法创建Resource Queue：

```
CREATE RESOURCE QUEUE name WITH (queue_attribute=value [, ... ])

其中queue_attribute为Resource Queue的属性，可以取如下值：

    ACTIVE_STATEMENTS=integer
        [ MAX_COST=float [COST_OVERCOMMIT={TRUE|FALSE}] ]
        [ MIN_COST=float ]
        [ PRIORITY={MIN|LOW|MEDIUM|HIGH|MAX} ]
        [ MEMORY_LIMIT='memory_units' ]

    MAX_COST=float [ COST_OVERCOMMIT={TRUE|FALSE} ]
        [ ACTIVE_STATEMENTS=integer ]
        [ MIN_COST=float ]
        [ PRIORITY={MIN|LOW|MEDIUM|HIGH|MAX} ]
        [ MEMORY_LIMIT='memory_units' ]
```

|参数|描述|
|--|--|
|ACTIVE\_STATEMENTS|用于指定Resource Queue在某个时间点最多的活跃查询（正在执行的查询）数量。|
|MEMORY\_LIMIT|用于指定在单个计算节点（segment）上Resource Queue所有查询最多可以使用的内存。 -   单位可以为KB、MB、GB。
-   默认值为-1，表示没有限制。 |
|MAX\_COST|用于指定Resource Queue查询代价的最大值，默认值为-1，表示没有限制。 **说明：** 这里的查询代价是指云原生数据仓库PostgreSQL版优化器估算出来的查询代价。 |
|COST\_OVERCOMMIT|该参数需要设置MAX\_COST参数。 -   当COST\_OVERCOMMIT为true，查询代价大于MAX\_COST的查询可以在系统空闲的时候执行。
-   当COST\_OVERCOMMIT为false，查询代价大于MAX\_COST的查询将会被拒绝执行。 |
|MIN\_COST|用于指定Resource Queue最小的查询代价，当查询代价小于MIN\_COST的查询，将不会排队等待而是会立即被执行。|
|PRIORITY|用于指定Resource Queue的优先级，优先级高的查询将会被分配更多的CPU资源用于执行。 -   MIN
-   LOW
-   MEDIUM
-   HIGH
-   MAX

 默认值为MEDIUM。|

**说明：** 创建Resource Queue时，`ACTIVE_STATEMENTS`和`MAX_COST`两个属性中必须指定一个，否则创建无法成功。

```
postgres=>  CREATE RESOURCE QUEUE adhoc2 WITH (MEMORY_LIMIT='2000MB');
ERROR:  at least one threshold ("ACTIVE_STATEMENTS", "MAX_COST") must be specified
```

-   指定并发查询数量

    在创建Resource Queue时，使用如下语法指定Resource Queue里用户可以并发执行的查询数量：

    ```
    CREATE RESOURCE QUEUE adhoc WITH (ACTIVE_STATEMENTS=3);
    ```

    这里创建了一个名为`adhoc`的Resource Queue，这个Resource Queue的用户在指定时间点，最多执行3个查询操作。如果此时队列中执行的查询数量为3个，同时有新的查询进入，那么新的查询将会处于等待状态，直到前面有查询执行完毕。

-   指定使用的内存上限

    在创建Resource Queue时，使用如下语法指定Resource Queue里查询使用的内存上限：

    ```
    CREATE RESOURCE QUEUE myqueue WITH (ACTIVE_STATEMENTS=20, MEMORY_LIMIT='2000MB');
    ```

    这里创建了一个名为`myqueue`的Resource Queue，在这个Resource Queue中，所有查询最多能用2000MB的内存。执行查询时，会在计算节点（segment）占用MEMORY\_LIMIT/ACTIVE\_STATEMENTS的内存，对于`myqueue`来说每个查询会占用2000MB/20=100MB的内存。如果查询需要单独可用内存，可以使用系统参数statement\_mem来设置，但是需要保证不超过Resource Queue设定的MEMORY\_LIMIT和系统参数max\_statement\_mem。示例如下：

    ```
    SET statement_mem='1GB';
    SELECT * FROM test_adbpg WHERE col='adb4pg' ORDER BY id;
    RESET statement_mem;
    ```

-   设置队列优先级

    给不同的Resource Queue设置优先级可以控制Resource Queue中的查询对CPU资源的使用。例如，系统中有大规模并发查询时，高优先级Resource Queue中的查询会比低优先级Resource Queue中的查询使用更多的CPU资源，从而保证高优先级的查询有更充分的CPU资源来执行。

    Resource Queue优先级的设置可以在创建时设置，示例如下：

    ```
    CREATE RESOURCE QUEUE executive WITH (ACTIVE_STATEMENTS=3, PRIORITY=MAX);
    ```

    Resource Queue优先级也可以在创建完成之后进行修改，具体操作请参见[修改Resource Queue的配置](#section_hmc_28o_d74)。

    **说明：** 优先级与ACTIVE\_STATEMENTS/MEMORY\_LIMIT的区别：

    -   ACTIVE\_STATEMENTS/MEMORY\_LIMIT会在查询执行之前判断其是否允许被执行。
    -   优先级机制是在一个查询开始执行后，根据系统的运动状态以及其所在队列的优先级，动态地给这个对应的查询分配可用的CPU资源。
    例如，云原生数据仓库PostgreSQL版在执行一些低优先级的查询，然后一个高优先级的查询进入并准备执行。那么云原生数据仓库PostgreSQL版会为这个查询分配更多的CPU资源，并减少低优先级查询的CPU资源。CPU资源分配的规则如下：

    -   相同优先级的查询所能被分配到的CPU资源一致。
    -   当系统中同时有高、中、低三个优先级别的查询的时候，高优先级的查询会分配得到系统90%的CPU资源，剩下的10%会留给中和低优先级的查询去分配，在这10%的系统CPU资源中，中优先级的查询会得到其中90%的CPU资源，低优先级查询则得到剩余的10%。

Resource Queue被创建完毕之后，可以使用`gp_toolkit.gp_resqueue_status`系统视图查看限制设置和当前资源队列的状态，如下所示：

```
postgres=> SELECT * from gp_toolkit.gp_resqueue_status WHERE
postgres->   rsqname='adhoc';
 queueid | rsqname | rsqcountlimit | rsqcountvalue | rsqcostlimit | rsqcostvalue | rsqmemorylimit | rsqmemoryvalue | rsqwaiters | rsqholders
---------+---------+---------------+---------------+--------------+--------------+----------------+----------------+------------+------------
   19283 | adhoc   |             3 |             0 |           -1 |            0 |             -1 |              0 |          0 |          0
(1 行记录)
```

Resource Queue的创建不能在事务块内进行，如下所示：

```
postgres=> begin;
BEGIN
postgres=> CREATE RESOURCE QUEUE test_q WITH (ACTIVE_STATEMENTS=3, PRIORITY=MAX);
ERROR:  CREATE RESOURCE QUEUE cannot run inside a transaction block
```

**说明：** 并非所有的SQL都会受到Resource Queue的限制：

-   当resource\_select\_only为on，SELECT、 SELECT INTO、CREATE TABLE AS SELECT、DECLARE CURSOR会受到约束。
-   当resource\_select\_only为off，INSERT、UPDATE、DELETE也会被资源队列管理起来。
-   在云原生数据仓库PostgreSQL版中，resource\_select\_only默认为off。

## 分配用户到Resource Queue

创建Resource Queue完成后，需要将一个或多个用户分配给这个Resource Queue，完成分配后Resource Queue就会对队列中用户的查询进行资源管理。

**说明：**

-   如果某个用户没有分配到Resource Queue，云原生数据仓库PostgreSQL版会将该用户分配给pg\_default资源队列。
-   pg\_default可以同时运行500个活跃的查询语句，没有cost limit的限制，优先级为MEDIUM。

```
postgres=> SELECT * from gp_toolkit.gp_resqueue_status WHERE rsqname='pg_default';
 queueid |  rsqname   | rsqcountlimit | rsqcountvalue | rsqcostlimit | rsqcostvalue | rsqmemorylimit | rsqmemoryvalue | rsqwaiters | rsqholders
---------+------------+---------------+---------------+--------------+--------------+----------------+----------------+------------+------------
    6055 | pg_default |           500 |             1 |           -1 |          126 |             -1 |   2.096128e+09 |          0 |          1
(1 行记录)
```

将用户分配给指定Resource Queue，语法如下：

```
ALTER ROLE name RESOURCE QUEUE queue_name;
CREATE ROLE name WITH LOGIN RESOURCE QUEUE queue_name;
```

您可以在用户创建之后修改其所属的Resource Queue，也可以在用户创建之时为其指定Resource Queue。

**说明：** 在任意时刻一个用户只能归属于一个Resource Queue。

## 删除Resource Queue中的用户

如果您需要把某个role移除出指定的Resource Queue，可以使用如下指令：

```
ALTER ROLE role_name RESOURCE QUEUE none;
```

## 修改Resource Queue的配置

您可以使用如下语句对Resource Queue的资源配置进行修改：

-   修改活跃的查询数：

    ```
    ALTER RESOURCE QUEUE adhoc WITH (ACTIVE_STATEMENTS=5);
    ```

-   修改Resource Queue内存和查询代价约束：

    ```
    ALTER RESOURCE QUEUE adhoc WITH (MAX_COST=-1.0, MEMORY_LIMIT='2GB');
    ```

-   修改Resource Queue优先级：

    ```
    ALTER RESOURCE QUEUE adhoc WITH (PRIORITY=LOW);
    ALTER RESOURCE QUEUE reporting WITH (PRIORITY=HIGH);
    ```


## 删除Resource Queue

您可以使用如下语句删除Resource Queue：

```
DROP RESOURCE QUEUE name;
```

**说明：** 删除Resource Queue前请检查如下项目，否则会导致删除失败：

-   Resource Queue没有被分配的用户。
-   Resource Queue中没有任何查询处于waiting状态。

