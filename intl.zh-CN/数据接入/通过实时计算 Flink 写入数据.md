# 通过实时计算 Flink 写入数据

Blink 3.6.0版本开始支持通过Blink connector将数据写入云原生数据仓库PostgreSQL版（以下简称ADB PG版），本文将为您介绍使用的必要条件、操作流程、字段映射和参数支持。

## 前提条件

-   实时计算集群和ADB PG版实例位于同一VPC下，且ADB PG实例的白名单规则允许Blink集群网段访问。
-   实时计算集群为3.6.0及以上版本，可按以下步骤创建。
    1.  开通阿里云实时计算服务和项目，请参见[开通服务和创建项目](/intl.zh-CN/独享模式/准备工作/开通服务和创建项目.md)。

        **说明：** 开通的实时计算集群与目标ADB PG版集群必须在同一VPC下。

    2.  确认并安装实时计算集群3.6.0及以上版本，请参见[管理独享集群Blink版本](/intl.zh-CN/独享模式/Flink SQL开发指南/管理独享集群Blink版本.md)。

        ![2-2-4 ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7340594951/p127364.png)

-   设置6.0版本ADB PG版实例。
    1.  [创建实例](/intl.zh-CN/快速入门/创建实例.md)。

        **说明：** 开通的ADB PG版实例与实时计算集群必须在同一VPC下。

    2.  设置ADB PG版实例白名单。
        1.  在[VPC控制台](https://vpc.console.aliyun.com/)找到对应网段的ip地址。

            ![3-2-1 ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7340594951/p127366.png)

        2.  在[ADB PG版控制台](https://gpdbnext.console.aliyun.com/)点击目标ADB PG版实例ID，在实例详情页面，单击**数据安全性**\>**添加白名单分组**。

            ![白名单](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4093274951/p129715.png)

        3.  将对应的VPC网段添加进ADB PG版实例白名单，单击**确定**。

            ![添加白名单](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4093274951/p129707.png)

    3.  创建ADB PG版目标表。

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


## 创建数据写入任务

为了方便介绍，本节的数据源采用随机数据源（random），实际使用中可以根据实际情况创建数据源。

1.  在实时计算控制台上，点击**项目管理**\>**项目列表**，单击项目名进入目标项目。

    ![创建数据写入任务1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8340594951/p128426.png)

2.  点击**开发**\>**新建作业**，创建数据写入的Flink SQL作业。

    ![创建数据写入任务2](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8340594951/p128428.png)

    ![创建数据写入任务3](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8340594951/p128429.png)


写入ADB PG的作业举例。

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

参数说明

|参数|参数说明|是否必填|备注|
|--|----|----|--|
|type|源表类型|是|固定值：adbpg。|
|url|JDBC连接地址|是|分析型数据库PostgreSQL版数据库的JDBC连接地址 。格式为‘jdbc:postgresql://<yourNetworkAddress\>:<PortId\>/<yourDatabaseName\>’，其中yourNetworkAddress：内网地址。PortId：连接端口。yourDatabaseName：连接的数据库名称。示例：url='jdbc:postgresql://gp-xxxxxx.gpdb.cn-chengdu.rds.aliyuncs.com:3432/postgres'|
|tableName|表名|是|无。|
|username|账号|是|无。|
|password|密码|是|无。|
|maxRetryTimes|写入重试次数|否|默认为3。|
|useCopy|是否采用copy API写入数据|否|参数取值如下 -   1：采用copy API方式写入数据。
-   0：采用其他方式写入数据，例如BATCH INSERT或BATCH UPSERT。

blink 3.6.0 版本默认为0，3.6.4及以上版本默认值为1；当取值为0时，会根据writeMode字段选择数据写入方式。|
|batchSize|一次批量写入的条数|否|默认值为5000。|
|exceptionMode|数据写入过程中出现异常时的处理策略|否|支持以下两种处理策略 -   ignore（默认值）：忽略出现异常时写入的数据。
-   strict：数据写入异常时，Failover报错。 |
|conflictMode|当出现主键冲突或者唯一索引冲突时的处理策略|否|支持以下三种处理策略 -   ignore （默认值）：忽略主键冲突，保留之前的数据。
-   strict：主键冲突时，Failover报错。
-   update：主键冲突时，更新新到的数据。
-   upsert：主键冲突时，采用upsert方式写入数据。 |
|targetSchema|Schema名称|否|默认值为public。|
|writeMode|在useCopy字段基础上，更细分的写入方式|否|blink 3.6.4 以后版本开始支持，在useCopy字段为0的场景下，可以设定writeMode字段采用其他写入方式，参数取值如下：-   0 ：采用BATCH INSERT方式写入数据。
-   1（默认值）：采用COPY API写入数据。
-   2：采用BATCH UPSERT方式写入数据。upsert含义见[INSERT ON CONFLICT覆盖写入](/intl.zh-CN/开发入门/INSERT ON CONFLICT覆盖写入.md)。 |

类型映射

|实时计算字段类型|分析型数据库PostgreSQL版字段类型|
|--------|---------------------|
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

## 启动导入任务

1.  在开发作业页面的右下角确认当前作业版本为3.6.0及以上，如果不符请点击切换版本。

    ![启动导入任务](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4093274951/p129709.png)

2.  完成作业开发后，依次点击**保存**、**上线**，即可上线该任务。

    ![保存上线](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4093274951/p129710.png)

3.  点击**运维**，在运维页面点击目标项目操作栏中的**启动**即可开始导入。

    ![启动任务3](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8340594951/p128436.png)


连接对应ADBPG实例，发现数据已经写入了目标表。

![add4](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5093274951/p129687.png)

## 版本变更记录

blink 3.6.4版本：

-   默认写入方式由BATCH INSERT变为BATCH COPY，以提高写入性能。
-   增加writeMode字段
    -   在3.6.4版本以后，如果不设置useCopy字段为1，则writeMode字段无论为何值均采用BATCH COPY方式写入。
    -   例如：采用BATCH INSERT方式写入，需要设定useCopy=0, writeMode=0; 采用BATCH UPSERT方式写入，需要设置useCopy=0,writeMode=2。
    -   在以后的迭代中，会逐步放弃useCopy字段，请尽量采用writeMode字段配置写入方式。
-   conflictMode字段增加upsert取值，通过insert on conflict的方式处理主键冲突。

blink 3.6.0 版本：

-   blink 3.6.0版本开始支持6.0版本ADB PG版结果表

