通过实时计算Flink版读取云原生数据仓库AnalyticDB PostgreSQL 
===============================================================

本文介绍如何通过阿里云实时计算Flink版实时读取云原生数据仓库AnalyticDB PostgreSQL（以下简称ADB PG版，原分析型数据库PostgreSQL版）数据，包括版本限制、语法示例、创建和运行Flink作业、WITH参数、CACHE参数、类型映射和参数支持等。

版本限制 
-------------------------

* 创建3.6.0及以上版本实时计算集群。

  

* 创建6.0版本ADB PG集群（实时计算集群和ADB PG版实例需要位于同一VPC下，且ADB PG版实例的白名单规则允许Flink集群网段访问）。

  




语法示例 
-------------------------

    CREATE TABLE dim_adbpg(
            id int,
            username varchar,
            INDEX(id)
    ) with(
            type='custom',
            tableFactoryClass='com.alibaba.blink.customersink.ADBPGCustomSourceFactory',
            url='jdbc:postgresql://内网连接串/databasename',
            tableName='tablename',
            userName='username',
            password='password',
            joinMaxRows='100',
            maxRetryTimes='1',
            connectionMaxActive='5',
            retryWaitTime='100',
            targetSchema='public',
            caseSensitive='0',
            cache='LRU',
            cacheSize='1000',
            cacheTTLMs='10000',
            cacheReloadTimeBlackList='2017-10-24 14:00 -> 2017-10-24 15:00',
            partitionedJoin='true'
    );
    
    -- join时需要指定在代码中加入维表标识 FOR SYSTEM_TIME AS OF PROCTIME()
    INSERT INTO print_sink
    SELECT R.c1, R.a2, R.a3, R.a4, R.a5, R.a6, R.a6, R.a8, R.a9, R.a10, R.a11, R.a13, T.username
    FROM s_member_cart_view AS R
    left join
    dim_adbpg FOR SYSTEM_TIME AS OF PROCTIME() AS T
    on R.c1 = T.id;



WITH参数 
---------------------------



|         参数名         |       参数含义       |                                                      备注                                                       |
|---------------------|------------------|---------------------------------------------------------------------------------------------------------------|
| url                 | ADBPG连接地址        | 必填，需要填写格式为jdbc:postgresql://\<ADBPG内网连接串\>/databaseName 的内网连接地址。                                              |
| type                | 表类型              | 必填。                                                                                                           |
| tableName           | ADBPG源表名         | 必填，填写维表对应的ADBPG数据仓库中的表名。                                                                                      |
| userName            | ADBPG用户名         | 必填。                                                                                                           |
| password            | ADBPG密码          | 必填。                                                                                                           |
| joinMaxRows         | 左表一条记录连接右表的最大记录数 | 非必填，表示在一对多连接时，左表一条记录连接右表的最大记录数（默认值为1024）。在一对多连接的记录数过多时，可能会极大地影响流任务的性能，因此您需要增大Cache的内存（cacheSize限制的是左表key的个数）。 |
| maxRetryTimes       | 单次SQL失败后重试次数     | 非必填，实际执行时，可能会因为各种因素造成执行失败，比如网络或者IO不稳定，超时等原因，ADBPG维表支持SQL执行失败后自动重试，用maxRetryTimes参数可以设定重试次数。默认值为3。             |
| connectionMaxActive | 连接池最大连接数         | 非必填，ADBPG维表中内置连接池，设置合理的连接池最大连接数可以兼顾效率和安全性，默认值为5。                                                              |
| retryWaitTime       | 重试休眠时间           | 非必填，每次SQL失败重试之间的sleep间隔，单位ms，默认值100。                                                                          |
| targetSchema        | 查询的ADBPG schema  | 非必填，默认值public。                                                                                                |
| caseSensitive       | 是否大小写敏感          | 非必填，默认值0，即不敏感；填1可以设置为敏感。                                                                                      |



CACHE参数 
----------------------------



|           参数名            |                                                                                                                            参数含义                                                                                                                             |                                                                                                                                                                             备注                                                                                                                                                                              |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| cache                    | 缓存策略                                                                                                                                                                                                                                                        | 目前ADB PG版支持以下三种缓存策略： * None（默认值）：无缓存。   * LRU：缓存维表里的部分数据。源表来一条数据，系统会先查找Cache，如果没有找到，则去物理维表中查询。   * ALL：缓存维表里的所有数据。在Job运行前，系统会将维表中所有数据加载到Cache中，之后所有的维表查询都会通过Cache进行。如果在Cache中无法找到数据，则KEY不存在，并在Cache过期后重新加载一遍全量Cache。    |
| cacheSize                | 设置LRU缓存的最大行数                                                                                                                                                                                                                                                | 非必填，默认为10000行。                                                                                                                                                                                                                                                                                                                                              |
| cacheTTLMs               | 缓存更新时间间隔。系统会根据您设置的缓存更新时间间隔，重新加载一次维表中的最新数据，保证源表能JOIN到维表的最新数据。                                                                                                                                                                                                | 非必填，单位为毫秒。默认不设置此参数，表示不重新加载维表中的新数据。                                                                                                                                                                                                                                                                                                                          |
| cacheReloadTimeBlackList | 更新时间黑名单。在缓存策略选择为ALL时，启用更新时间黑名单，防止在此时间内做Cache更新（例如双11场景）。                                                                                                                                                                                                    | 非必填，默认空，格式为 **'2017-10-24 14:00 -\> 2017-10-24 15:00, 2017-11-10 23:30 -\> 2017-11-11 08:00'** 。其中分割符使用情况如下： * 用逗号（,）来分隔多个黑名单。   * 用箭头（-\>）来分割黑名单的起始结束时间。                                                                                                |
| partitionedJoin          | 是否开启partitionedJoin。在开启partitionedJoin优化时，主表会在关联维表前，先按照Join KEY进行Shuffle，这样做有以下优点： * 在缓存策略为LRU时，可以提高缓存命中率。   * 在缓存策略为ALL时，节省内存资源，因为每个并发只缓存自己并发所需要的数据。    | 非必填，默认情况下为false，表示不开启partitionedJoin。                                                                                                                                                                                                                                                                                                                       |



类型映射 
-------------------------



| 实时计算字段类型  |   ADB PG版字段类型    |
|-----------|------------------|
| BOOLEAN   | BOOLEAN          |
| TINYINT   | SMALLINT         |
| SMALLINT  | SMALLINT         |
| INT       | INT              |
| BIGINT    | BIGINT           |
| DOUBLE    | DOUBLE PRECISION |
| VARCHAR   | TEXT             |
| DATETIME  | TIMESTAMP        |
| DATE      | DATE             |
| FLOAT     | REAL             |
| DECIMAL   | DOUBLE PRECISION |
| TIME      | TIME             |
| TIMESTAMP | TIMESTAMP        |



创建和运行Flink作业 
---------------------------------

1. 登录[实时计算控制台](https://realtime-compute.console.aliyun.com/regions/cn-shanghai)，在页面顶部菜单栏上，鼠标悬停在用户头像上，单击项目管理。在项目管理\>项目列表页面，单击项目名进入自己创建的项目。![4-1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1833472061/p173474.png)

   

2. 单击开发\>新建作业，创建数据写入的Flink SQL作业。![4-2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1833472061/p173475.png)![4-3](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1833472061/p173476.png)

   

3. 目前采用Flink自定义维表的方式支持读取ADB PG版目标表数据，使用自定义维表功能上线前需要在资源引用界面上传及引用.jar包，编写完作业后点击资源引用\>新建资源\>上传JAR包\>更多\>引用。![3](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1833472061/p173497.png)通过以下链接下载jar包：[下载JAR包。](https://adbpg-public.oss-cn-beijing.aliyuncs.com/blink-customerdim-adbpg-0909.jar)

   

4. 完成作业开发后，依次点击保存、上线，即可上线该任务。![4-4](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1833472061/p173511.png)

   

5. 继续点击运维，启动对应项目即可启动任务。![18609001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1459730261/p271545.png)

   




代码示例 
-------------------------

这里给出读取ADB PG版数据打印到Flink日志中的Flink SQL示例：

    --SQL
    --********************************************************************--
    --Author: zihua
    --CreateTime: 2019-09-07 10:34:34
    --********************************************************************--
    
    CREATE TABLE s_member_cart
    (
            a1 int,
            a2 tinyint    ,
            a3 smallint  ,
            a4 int,
            a5 boolean,
            a6 FLOAT     ,
            a7 DECIMAL   ,
            a8 double,
            a9 date   ,
            a10 time      ,
            a11 timestamp ,
            a12 tinyint
    ) WITH (
            type='random'
    );
    
    CREATE VIEW s_member_cart_view AS
    SELECT MOD(a1, 10) c1, a2, a3, a4, a5, a6, a6, a8, a9, a10, a11, case when a12 >0 then 'test1' else 'test5' end as b12,'{ "customer": "中文56", "items": {"product": "Beer","qty": 6}}' a13
    FROM s_member_cart;
    
    --adbpg dim index
    CREATE TABLE dim_adbpg(
            id int,
            username varchar,
            INDEX(id)
    ) with(
            type='custom',
            tableFactoryClass='com.alibaba.blink.customersink.ADBPGCustomSourceFactory',
            url='jdbc:postgresql://内网连接串/databasename',
            tableName='tablename',
            userName='username',
            password='password',
            joinMaxRows='100',
            maxRetryTimes='1',
            connectionMaxActive='5',
            retryWaitTime='100',
            targetSchema='public',
            caseSensitive='0',
            cache='LRU',
            cacheSize='1000',
            cacheTTLMs='10000',
            cacheReloadTimeBlackList='2017-10-24 14:00 -> 2017-10-24 15:00',
            partitionedJoin='true'
    );
    
    -- ads sink.
    CREATE TABLE print_sink (
            B1 int,
            B2 tinyint  ,
            B3 smallint  ,
            B4 int,
            B5 boolean,
            B6 FLOAT     ,
            B7 FLOAT   ,
            B8 double,
            B9 date   ,
            B10 time      ,
            B11 timestamp ,
            B12 varchar,
            B15 varchar,
            PRIMARY KEY(B1)
    ) with (
            type='print'
    );
    
    
    INSERT INTO print_sink
    SELECT R.c1, R.a2, R.a3, R.a4, R.a5, R.a6, R.a6, R.a8, R.a9, R.a10, R.a11, R.a13, T.username
    FROM s_member_cart_view AS R
    left join
    dim_adbpg FOR SYSTEM_TIME AS OF PROCTIME() AS T
    on R.c1 = T.id;









