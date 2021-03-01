# Use a Realtime Compute for Apache Flink cluster to write data to an AnalyticDB for PostgreSQL instance

Blink 3.6.0 and later allow you to use Blink connectors to write data to an AnalyticDB for PostgreSQL instance. This topic describes the related prerequisites, procedures, field mappings, and parameters.

## Prerequisites

-   The Realtime Compute for Apache Flink cluster and the AnalyticDB for PostgreSQL instance exist within the same virtual private cloud \(VPC\). The CIDR block of the cluster is added to the whitelist of the AnalyticDB for PostgreSQL instance.
-   The Blink version of the Realtime Compute for Apache Flink cluster is 3.6.0 or later. The following steps describe how to create the cluster:
    1.  Activate Realtime Compute for Apache Flink and create a project. For more information, see [Activate Realtime Compute for Apache Flink and create a project](https://help.aliyun.com/document_detail/62458.html?spm=a2c4g.11186623.2.6.52bd6ad4j7vRFx).

        **Note:** The created Realtime Compute for Apache Flink cluster and the destination AnalyticDB for PostgreSQL instance must reside within the same VPC.

    2.  Install Blink V3.6.0 or later for Realtime Compute for Apache Flink cluster. For more information, see [Manage Blink versions of a Realtime Compute for Apache Flink cluster deployed in exclusive mode](/intl.en-US/Exclusive Mode/Flink SQL Development Guide/Manage Blink versions of a Realtime Compute for Apache Flink cluster deployed in exclusive
         mode.md).
-   Configure an AnalyticDB for PostgreSQL V6.0 instance.
    1.  [Create an instance](/intl.en-US/Quick Start/Create an instance.md).

        **Note:** The created AnalyticDB for PostgreSQL instance and the Realtime Compute for Apache Flink cluster must reside within the same VPC.

    2.  Configure the whitelist of the AnalyticDB for PostgreSQL instance.
        1.  In the [VPC console](https://vpc.console.aliyun.com/), find the CIDR block of the cluster.
        2.  In the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/), find the AnalyticDB for PostgreSQL instance and click its ID. In the left-side navigation pane, click **Security Controls**. On the Security Controls page, click **Create Whitelist**.
        3.  Set Whitelist Name and IP Addresses, and click **OK**. The specified CIDR block in the VPC is allowed to access the AnalyticDB for PostgreSQL instance.
    3.  Create a destination table in the AnalyticDB for PostgreSQL instance.

        ```
        create table test15(                  
        b1 bigint,
        b2 smallint,
        b3 smallint,
        b4 int,
        b5 boolean,
        b6 real,                      
        b7 double precision,          
        b8 double precision,          
        b9 date,
        b10 time with time zone,
        b11 timestamp with time zone,
        b12 text,
        b15 json
        );
        ```


## Create a file in which to write data

Random data sources are used in this section. You can create your data sources in actual scenarios.

1.  Log on to the Realtime Compute Development Platform. In the left-side navigation pane, choose **Project Management** \> **Projects**. Find the desired project and click its name.
2.  In the left-side navigation pane, click **Development**. In the top navigation bar, click **Create File** to create a Flink SQL file in which to write data.

The following sample SQL statements show how to write data to the AnalyticDB for PostgreSQL instance:

```
--SQL
--********************************************************************--
--Author: sqream_test
--CreateTime: 2020-04-27 19:13:44
--********************************************************************--

CREATE TABLE s_member_cart
(
a1 bigint   ,
a2 tinyint      ,
a3 smallint  ,
a4 int       ,
a5 boolean   ,
a6 FLOAT     ,
a7 DECIMAL   ,
a8 double    ,
a9 date      ,
a10 time      ,
a11 timestamp    ,
a12 tinyint


) WITH (
    type='random'
);

-- ads sink.
CREATE TABLE adsSink (
    `B1` bigint   ,
    `B2` tinyint  ,
    `B3` smallint  ,
    `B4` int       ,
    `B5` boolean,
    `B6` FLOAT     ,
    `B7` FLOAT   ,
    `B8` double    ,
    `B9` date      ,
    `B10` time      ,
    `B11` timestamp    ,
    `B12` varchar,
    `B15` varchar 
    --PRIMARY KEY(b1)
) with (  
    --type='print'
    type='adbpg',
    version='1.1',
    url='jdbc:postgresql://gp-xxxx:3432/testblink',
    tableName='test',
    userName='xxxx',
    password='xxxxxx',
    timeZone='Asia/Shanghai',
    useCopy='0'
);



INSERT INTO adsSink
SELECT a1,a2,a3,a4,a5,a6,a6,a8,a9,a10,a11, case when a12 >0 then 'value1' else 'value2' end as b12,'{ "customer": "value", "items": {"product": "Beer","qty": 6}}'
     from s_member_cart;

--insert into adsSink2 select a2, sum(a4) from s_member_cart group by a2;
```

Parameters

|Parameter|Description|Required|Constraint|
|---------|-----------|--------|----------|
|type|The type of the source table.|Yes|Set the value to adbpg.|
|url|The JDBC URL.|Yes|The JDBC URL is used to connect to the AnalyticDB for PostgreSQL database. The URL must be in the following format: jdbc:postgresql://<yourNetworkAddress\>:<PortId\>/<yourDatabaseName\>. yourNetworkAddress indicates the internal URL of the database. PortId indicates the ID of the port used to connect to the database. yourDatabaseName indicates the name of the database. Example: url='jdbc:postgresql://gp-xxxxxx.gpdb.cn-chengdu.rds.aliyuncs.com:3432/postgres'|
|tableName|The name of the table.|Yes|None|
|username|The username that is used to connect to the database.|Yes|None|
|password|The password that is used to connect to the database.|Yes|None|
|maxRetryTimes|The maximum number of retries for writing data to the table.|No|Default value: 3.|
|useCopy|Specifies whether to copy the API to write data.|No|Valid values: -   1: The API is copied to write data.
-   0: The API is not copied to write data. Instead, an SQL statement such as BATCH INSERT or BATCH UPSERT is used.

In Blink 3.6.0, the default value is 0. In Blink 3.6.4 and later, the default value is 1. A value of 0 indicates that data is written based on the writeMode field.|
|batchSize|The number of data records that can be written at a time.|No|Default value: 5000.|
|exceptionMode|The policy that is used to handle exceptions when data is written to the table.|No|Default value: ignore. Valid values: -   ignore: The system ignores the data that is written when exceptions occur.
-   strict: When primary key conflicts occur, the system performs failovers. |
|conflictMode|The policy that is used to handle primary key conflicts or unique index conflicts.|No|Default value:ignore. Valid values: -   ignore: When primary key conflicts occur, the system ignores the primary key conflicts and retains the existing data.
-   strict: When primary key conflicts occur, the system performs failovers.
-   update: When primary key conflicts occur, the system updates data.
-   upsert: When primary key conflicts occur, the system uses the UPSERT statement to write data. |
|targetSchema|The name of the schema.|No|Default value: public.|
|writeMode|The fine-grained policy for data writing based on the useCopy parameter.|No|Blink of the versions later than 3.6.4 support this parameter. You can specify this parameter when you set the useCopy parameter. Default value: 1. Valid values:-   0: The BATCH INSERT statement is used to write data.
-   1: The API is copied to write data.
-   2: The BATCH UPSERT statement is used to write data. For more information about UPSERT, see [Use INSERT ON CONFLICT to overwrite data](https://help.aliyun.com/document_detail/162380.html?spm=5176.11065259.1996646101.searchclickresult.49752818hBAUaZ). |

Field type mapping

|Realtime Compute for Apache Flink field type|AnalyticDB for PostgreSQL field type|
|--------------------------------------------|------------------------------------|
|BOOLEAN|BOOLEAN|
|TINYINT|SAMLLINT|
|SAMLLINT|SAMLLINT|
|INT|INT|
|BIGINT|BIGINT|
|DOUBLE|DOUBLE PRECISION|
|VARCHAR|TEXT|
|DATETIME|TIMESTAMP|
|DATE|DATE|
|FLOAT|REAL|
|DECIMAL|DOUBLE PRECISION|
|TIME|TIME|
|TIMESTAMP|TIMESTAMP|

## Start a job

1.  In the lower-right corner of the Development tab, check whether Blink 3.6.0 or later is used. If Blink 3.6.0 or later is not used, change the version.
2.  After you create a file, click **Save** and **Publish** in sequence to publish the job.
3.  Click the **Administration** tab, and click **Start** in the Actions column corresponding to the job to write data.

Connect to the AnalyticDB for PostgreSQL instance and check whether the data is written to the destination table.

## Data writing policies in different Blink versions

Blink 3.6.4:

-   By default, the BATCH COPY statement is used to write data. The BATCH COPY statement provides higher write performance than the BATCH INSERT statement.
-   The writeMode parameter is supported.
    -   In Blink of the versions later than 3.6.4, if the useCopy field is not set to 1, the BATCH COPY statement is used to write data regardless of the writeMode value.
    -   For example, if you want to use the BATCH INSERT statement to write data to the AnalyticDB for PostgreSQL instance, you must set the useCopy parameter to 0 and the writeMode parameter to 0. If you want to use the BATCH UPSERT statement to write data to the AnalyticDB for PostgreSQL instance, you must set the useCopy parameter to 0 and the writeMode parameter to 2.
    -   In later updates, the useCopy parameter will be removed. We recommend that you use the writeMode parameter to configure the data writing policy.
-   You can set the conflictMode field to upsert to handle primary key conflicts by using the INSERT ON CONFLICT statement.

Blink 3.6.0:

-   Blink 3.6.0 supports result tables of AnalyticDB for PostgreSQL V6.0.

