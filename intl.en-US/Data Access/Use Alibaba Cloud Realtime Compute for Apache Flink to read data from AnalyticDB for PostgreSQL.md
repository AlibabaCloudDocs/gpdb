Use Alibaba Cloud Realtime Compute for Apache Flink to read data from AnalyticDB for PostgreSQL 
====================================================================================================================

Alibaba Cloud Realtime Compute for Apache Flink allows you to read data from AnalyticDB for PostgreSQL instances. This topic describes the prerequisites, syntax, parameters in the WITH and CACHE clauses, data type mapping, and how to create and run a Flink job.

Prerequisites 
----------------------------------

* The version of the Realtime Compute cluster is 3.6.0 or later.

  

* An AnalyticDB for PostgreSQL V6.0 instance is created. The Realtime Compute cluster and the AnalyticDB for PostgreSQL instance exist within the same VPC. The CIDR block of the cluster is added to the whitelist of the AnalyticDB for PostgreSQL instance.

  




Syntax 
---------------------------

    CREATE TABLE dim_adbpg(
            id int,
            username varchar,
            INDEX(id)
    ) with(
            type='custom',
            tableFactoryClass='com.alibaba.blink.customersink.ADBPGCustomSourceFactory',
            url='jdbc:postgresql://Internal endpoint/databasename',
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
    
    -- You must include FOR SYSTEM_TIME AS OF PROCTIME() in the INSERT INTO statement when you join a dimension table with another table.
    INSERT INTO print_sink
    SELECT R.c1, R.a2, R.a3, R.a4, R.a5, R.a6, R.a6, R.a8, R.a9, R.a10, R.a11, R.a13, T.username
    FROM s_member_cart_view AS R
    left join
    dim_adbpg FOR SYSTEM_TIME AS OF PROCTIME() AS T
    on R.c1 = T.id;



Parameters in the WITH clause 
--------------------------------------------------



|      Parameter      |                                          Description                                           |                                                                                                                                                                                           Constraint                                                                                                                                                                                           |
|---------------------|------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| url                 | The JDBC URL used to access the database.                                                      | Required. Specify the URL in the jdbc:postgresql://\<Internal endpoint\>/databaseName format.                                                                                                                                                                                                                                                                                                  |
| tableName           | The name of the source table in the database.                                                  | Required. Set the tableName parameter to the name of the source table that is joined with the dimension table.                                                                                                                                                                                                                                                                                 |
| userName            | The username that is used to log on to the database.                                           | Required.                                                                                                                                                                                                                                                                                                                                                                                      |
| password            | The password that is used to log on to the database.                                           | Required.                                                                                                                                                                                                                                                                                                                                                                                      |
| joinMaxRows         | The maximum number of rows in the right table that can be joined with a row in the left table. | Optional. If a row in the left table needs to be joined with multiple rows in the right table, specify this parameter. Default value: 1024. If one row is joined with a large number of rows, the performance of the streaming task may be compromised. In this case, you must increase the cache size. The cacheSize parameter is used to limit the number of keys in the left table.         |
| maxRetryTimes       | The maximum number of retries to write data when a statement fails to be executed.             | Optional. In actual scenarios, a statement may fail to be executed for various reasons, such as network jitter, I/O latency, and timeout. If an SQL statement to read data from an AnalyticDB for PostgreSQL dimension table fails to be executed, the statement is automatically retried. You can use the maxRetryTimes parameter to specify the maximum number of retries. Default value: 3. |
| connectionMaxActive | The maximum number of active connections that can be allocated in a pool at the same time.     | Optional. An AnalyticDB for PostgreSQL dimension table provides built-in connection pooling. For efficiency and security purposes, we recommend that you specify this parameter. Default value: 5.                                                                                                                                                                                             |
| retryWaitTime       | The wait interval between retries of failed SQL statements.                                    | Optional. Unit: milliseconds. Default value: 100.                                                                                                                                                                                                                                                                                                                                              |
| targetSchema        | The name of the schema to be queried.                                                          | Optional. Default value: public.                                                                                                                                                                                                                                                                                                                                                               |
| caseSensitive       | Specifies whether the dimension table name is case-sensitive.                                  | Optional. A value of 0 indicates that the table name is case-insensitive. A value of 1 indicates that the table name is case-sensitive.                                                                                                                                                                                                                                                        |



Parameters in the CACHE clause 
---------------------------------------------------



|        Parameter         |                                                                                                                                                                                                                                                                                     Description                                                                                                                                                                                                                                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     Constraint                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| cache                    | The policy that is used to cache data.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | An AnalyticDB for PostgreSQL dimension table supports the following cache policies: * None: indicates that no data is cached. This is the default value.   * LRU: indicates that partial data in the dimension table is cached. The system searches the cache each time it receives a data record. If the system does not find the record in the cache, the system searches for the data record in the physical dimension table.   * ALL: indicates that all data in the dimension table is cached. Before a Realtime Compute for Apache Flink job starts to run, the system loads all data in the dimension table to the cache, and then looks up the cache for all subsequent queries on the dimension table. If the system does not find the data record in the cache, the join key does not exist. The system reloads all data in the cache after cache entries expire.    |
| cacheSize                | The maximum number of rows that can be cached.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Optional. You can sepcify this parameter only when the cache parameter is set to LRU. Default value: 10000.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| cacheTTLMs               | The cache refresh interval. The system reloads the latest data in the dimension table based on the value of this parameter to ensure that the source table can be joined with the latest data of the dimension table.                                                                                                                                                                                                                                                                                                                                                               | Optional. Unit: milliseconds. By default, this parameter is empty, which indicates that the latest data in the dimension table is not reloaded.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| cacheReloadTimeBlackList | The time periods during which cache is not refreshed. This parameter takes effect when the cache parameter is set to ALL. The cache is not refreshed during the time periods that you specify for this parameter. This parameter is useful for large-scale online promotional events such as Double 11.                                                                                                                                                                                                                                                                             | Optional. This parameter is empty by default. Specify this parameter in the **'2017-10-24 14:00 -\> 2017-10-24 15:00, 2017-11-10 23:30 -\> 2017-11-11 08:00'** format. Make sure that the parameter conforms to the following rules of delimiters: * Separate multiple time periods with commas (,).   * Separate the start time and end time of each time period with a hyphen and a greater-than sign (-\>).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| partitionedJoin          | Specifies whether to enable partitionedJoin. If the partitionedJoin feature is enabled, the shuffle operation is performed based on JOIN keys before the source table is joined with the dimension table. This process provides the following benefits: * If you set the cache parameter to LRU, the cache hit rate increases.   * If you set the cache parameter to ALL, only the required data is cached for each concurrent job. This allows you to save memory resources.    | Optional. By default, this parameter is set to false, which indicates that the partitionedJoin feature is disabled.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |



Mapping between field data types 
-----------------------------------------------------



| Data type of Realtime Compute for Apache Flink | Data type of AnalyticDB for PostgreSQL |
|------------------------------------------------|----------------------------------------|
| BOOLEAN                                        | BOOLEAN                                |
| TINYINT                                        | SMALLINT                               |
| SMALLINT                                       | SMALLINT                               |
| INT                                            | INT                                    |
| BIGINT                                         | BIGINT                                 |
| DOUBLE                                         | DOUBLE PRECISION                       |
| VARCHAR                                        | TEXT                                   |
| DATETIME                                       | TIMESTAMP                              |
| DATE                                           | DATE                                   |
| FLOAT                                          | REAL                                   |
| DECIMAL                                        | DOUBLE PRECISION                       |
| TIME                                           | TIME                                   |
| TIMESTAMP                                      | TIMESTAMP                              |



Create and run a Flink job 
-----------------------------------------------

Log on to the Realtime Compute for Apache Flink console. In the left-navigation pane, choose Project Management \> Projects. Find the project that you created and click its name to go to the Overview page.

![4-1](../images/p173474.png)

In the top navigation bar, click Development. On the page that appears, click Create File to create a Flink SQL file in which to write data.

![4-2](../images/p173475.png)

![4-3](../images/p173476.png)

You can create dimension tables in Realtime Compute for Apache Flink to read data from tables of an AnalyticDB for PostgreSQL instance. Before the dimension table customization feature is released, you must upload and reference a JAR package. In the left-side navigation pane, click the Resources navigation tab, and then click Create Resource. In the Upload Resource dialog box, upload a JAR package from your computer and click OK. In the left-side navigation pane, choose More \> Reference in the Actions column corresponding to the JAR package name.

![3](../images/p173497.png)You can download the JAR package from the following link:

https://adbpg-public.oss-cn-beijing.aliyuncs.com/blink-customerdim-adbpg-0909.jar

After you create a file, click Save and then click Publish to publish the job.

![4-4](../images/p173511.png)

On the Administration page, find the job and click Start in the Actions column.

![4-5](../images/p173510.png)

Sample code 
--------------------------------

The following code demonstrates how to use a dimension table to read data from an AnalyticDB for PostgreSQL instance and use the Print connector to write the data to the standard SQL output of the Realtime Compute for Apache Flink cluster logs.

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
    SELECT MOD(a1, 10) c1, a2, a3, a4, a5, a6, a6, a8, a9, a10, a11, case when a12 >0 then 'test1' else 'test5' end as b12,'{ "customer": "English56", "items": {"product": "Beer","qty": 6}}' a13
    FROM s_member_cart;
    
    --adbpg dim index
    CREATE TABLE dim_adbpg(
            id int,
            username varchar,
            INDEX(id)
    ) with(
            type='custom',
            tableFactoryClass='com.alibaba.blink.customersink.ADBPGCustomSourceFactory',
            url='jdbc:postgresql://Internal endpoint/databasename',
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



