# Amazon Redshift应用和数据迁移至AnalyticDB PostgreSQL

本文介绍从Amazon Redshift迁移数据到云原生数据仓库AnalyticDB PostgreSQL版的过程。

## 准备工作

-   需要迁移的Amazon Redshift实例。
-   用于导出Amazon Redshift数据的Amazon S3服务。
-   已开通阿里云对象存储服务（OSS），OSS的详细信息，请参见[什么是对象存储OSS](/intl.zh-CN/产品简介/什么是对象存储OSS.md)。
-   已创建云原生数据仓库AnalyticDB PostgreSQL版实例，如何选择实例规格，请参见[规格选型](#section_xc8_nll_5uq)。

## 迁移大纲

![AWS Redshift迁移到ADB PG步骤](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2243442261/p278851.png)

具体迁移步骤，请参见[数据迁移步骤](#section_alq_ohh_nja)。

## 规格选型

以下内容将为您介绍如何选择根据原有Amazon Redshift实例判断需要的云原生数据仓库AnalyticDB PostgreSQL版的规格。

Amazon Redshift实例由**Leader Node**和多个**Compute Node**组成。

-   **Leader Node**：相当于AnalyticDB PostgreSQL版的Master节点，负责与客户端通信，分析和制定执行计划，实施数据库操作。
-   **Compute Node**：相当于AnalyticDB PostgreSQL版的一个计算组，由多个Node Slices组成。每个Node Slices都相当于AnalyticDB PostgreSQL版的单个Segment节点，负责实际的数据存储和查询计算。

因此，当您创建AnalyticDB PostgreSQL版实例时，如果不知道如何选择规格，可以通过Amazon Redshift中每个Node Slice的规格来选择合适的AnalyticDB PostgreSQL版节点规格。

示例如下：

Amazon Redshift实例包含4个计算节点，每个节点规格为2核16 GB，存储为1 TB；每个节点下包含两个Node Slices。

根据源Amazon Redshift实例规格，您在创建AnalyticDB PostgreSQL版实例时，可以选择创建8个Segment节点，节点规格为2C16GB；每个Segment节点的存储容量选择1000 GB。

**说明：**

-   创建AnalyticDB PostgreSQL版实例具体操作，请参见[创建实例](/intl.zh-CN/快速入门/创建实例.md)。
-   存储磁盘类型建议选择**ESSD云盘**，I/O性能比高效云盘更佳。
-   按量付费实例在创建后支持升降配。

## 数据迁移步骤

1.  将Amazon Redshift的数据导出到Amazon S3中。您可以使用UNLOAD命令进行导出，如何导出，请参见[UNLOAD](https://docs.aws.amazon.com/zh_cn/redshift/latest/dg/t_Unloading_tables.html)。

    ```
    UNLOAD ('select-statement')
    TO 's3://object-path/name-prefix'
    authorization
    [ option [ ... ] ]
    
    where option is
    { [ FORMAT [ AS ] ] CSV | PARQUET
    | PARTITION BY ( column_name [, ... ] ) [ INCLUDE ]
    | MANIFEST [ VERBOSE ] 
    | HEADER           
    | DELIMITER [ AS ] 'delimiter-char' 
    | FIXEDWIDTH [ AS ] 'fixedwidth-spec'   
    | ENCRYPTED [ AUTO ]
    | BZIP2  
    | GZIP 
    | ZSTD
    | ADDQUOTES 
    | NULL [ AS ] 'null-string'
    | ESCAPE
    | ALLOWOVERWRITE
    | CLEANPATH
    | PARALLEL [ { ON | TRUE } | { OFF | FALSE } ]
    | MAXFILESIZE [AS] max-size [ MB | GB ] 
    | REGION [AS] 'aws-region' }
    ```

    **说明：**

    -   导出数据时，建议使用FORMAT AS PARQUET或CSV格式。
    -   导出数据时，建议使用并行导出（PARALLEL ON），提高导出效率，生成更多文件分片。
    -   导出数据时，建议通过MAXFILESIZE参数设置合适的文件段大小，尽可能生成多个文件段，推荐数量为AnalyticDB PostgreSQL版实例的节点个数的整数倍，便于后续OSS外表并行导入AnalyticDB PostgreSQL版，提高效率。
2.  将Amazon S3数据同步到OSS中。
    1.  创建OSS存储空间，请参见[创建存储空间](/intl.zh-CN/开发指南/存储空间（Bucket）/创建存储空间.md)。

        **说明：** 建议OSS存储空间与AnalyticDB PostgreSQL版实例在同一地域，便于后续数据导入数据库中。

    2.  下载并安装单机模式的ossimport，如何下载并安装，请参见[说明及配置](/intl.zh-CN/常用工具/数据迁移工具ossimport/说明及配置.md)。

        单机模式的ossimport软件的文件结构如下：

        ```
        ossimport
        ├── bin
        │   └── ossimport2.jar  # 包括Master、Worker、TaskTracker、Console四个模块的总jar
        ├── conf
        │   ├── local_job.cfg   # Job配置文件
        │   └── sys.properties  # 系统运行参数配置文件
        ├── console.bat         # Windows命令行，可以分布执行调入任务
        ├── console.sh          # Linux命令行，可以分布执行调入任务
        ├── import.bat          # Windows一键导入，执行配置文件为conf/local_job.cfg配置的数据迁移任务，包括启动、迁移、校验、重试
        ├── import.sh           # Linux一键导入，执行配置文件为conf/local_job.cfg配置的数据迁移任务，包括启动、迁移、校验、重试
        ├── logs                # 日志目录
        └── README.md           # 说明文档，强烈建议使用前仔细阅读
        ```

    3.  配置单机模式的ossimport，请参考如下示例修改配置文件conf/local\_job.cfg，关于ossimport配置的详细信息，请参见[说明及配置](/intl.zh-CN/常用工具/数据迁移工具ossimport/说明及配置.md)。

        ```
        srcType=s3
        srcAccessKey="your AWS Access Key ID"
        srcSecretKey="your AWS Access Key Secret"
        srcDomain=s3.ap-southeast-2.amazonaws.com
        srcBucket=s3-export-bucket
        destAccessKey="your Alibaba Cloud Access Key ID"
        destSecretKey="your Alibaba Cloud Access Key Secret"
        destDomain=http://oss-ap-southeast-2-internal.aliyuncs.com
        destBucket=oss-export-bucket
        destPrefix=
        isSkipExistFile=true
        ```

        **说明：** 仅需修改以上示例中的参数。

    4.  运行ossimport将数据同步至OSS，使用单机版ossimport的具体操作，请参见[单机部署](/intl.zh-CN/常用工具/数据迁移工具ossimport/单机部署.md)。
3.  将OSS数据导入AnalyticDB PostgreSQL版实例。
    1.  修改DDL定义，修改SCHEMA、TABLE、FUNCTION、VIEW等对象信息，具体信息，请参见[DDL语法转换](#section_9hd_lrd_dh7)。
    2.  使用COPY FROM OSS命令将数据导入到AnalyticDB PostgreSQL版实例，使用方法，请参见[使用COPY/UNLOAD导入/导出数据到OSS](/intl.zh-CN/开发入门/使用COPY/UNLOAD导入/导出数据到OSS.md)。

        ```
        COPY table_name [ ( column_name [, ...] ) ]
            FROM 'data_source_url'
            ACCESS_KEY_ID 'access_key_id'
            SECRET_ACCESS_KEY 'secret_access_key'
            [ [ FORMAT ] [ AS ] data_format ]
            [ MANIFEST ]
            [ [ option value]  ...  ]
        
        where data_source_url should be in format:
        
            oss://{bucket_name}/{path_prefix}
        ```

        -   示例一：使用COPY命令从OSS上导入PARQUET格式文件

            ```
            COPY tp
            FROM  'oss://adbpg-regress/test_parquet/'
            ACCESS_KEY_ID 'id'
            SECRET_ACCESS_KEY 'key'
            FORMAT AS PARQUET
            ENDPOINT 'oss-****.aliyuncs.com'
            FDW 'oss_fdw';
            ```

        -   示例二：使用COPY命令从OSS上导入CSV格式文件，只导入a和c两列，b列默认为NULL。

            ```
            COPY local_t2 (a, c)
            FROM 'oss://adbpg-regress/local_t/'
            ACCESS_KEY_ID 'id'
            SECRET_ACCESS_KEY 'key'
            FORMAT AS CSV
            ENDPOINT 'oss-****.aliyuncs.com'
            FDW 'oss_fdw';
            ```


## DDL语法转换

Amazon Redshift DDL语法与AnalyticDB PostgreSQL版DDL语法略有不同，迁移前您需要对这些语法进行转换。

-   **CREATE SCHEMA**

    按照AnalyticDB PostgreSQL版语法标准创建Schema，示例如下：

    ```
    CREATE SCHEMA schema1 AUTHORIZATION xxxpoc;
    GRANT ALL ON SCHEMA schema1 TO xxxpoc;
    GRANT ALL ON SCHEMA schema1 TO public;
    COMMENT ON SCHEMA model IS 'for xxx migration  poc test';
    
    CREATE SCHEMA oss_external_table AUTHORIZATION xxxpoc;
    ```

-   **CREATE FUNCTION**

    由于AnalyticDB PostgreSQL版不兼容Amazon Redshift的部分SQL函数，因此您需要定制或者重写这些函数。示例如下：

    -   `CONVERT_TIMEZONE(a,b,c)`，使用如下语句替换：

        ```
        timezone(b, timezone(a,c))
        ```

    -   `GETDATE()`，使用如下语句替换：

        ```
        current_timestamp(0):timestamp
        ```

    -   用户定义函数（UDF）替换或优化。Redshift示例函数如下：

        ```
        CREATE OR REPLACE FUNCTION public.f_jdate(dt timestamp without time zone)
        RETURNS character varying AS
        '      from datetime import timedelta, datetime
               if dt.hour < 4:
                      d = timedelta(days=-1)
                      dt = dt + d
               return str(dt.date())'
        LANGUAGE plpythonu IMMUTABLE;
        COMMIT;
        ```

        使用如下语句替换：

        ```
        to_char(a - interval '4 hour', 'yyyy-mm-dd')
        ```

    -   其他Amazon Redshift标准的函数。

        您可以在[Functions and Operators](https://www.postgresql.org/docs/9.4/functions.html)中查看标准的PostgreSQL函数的用法，修改或自行实现Amazon Redshift和AnalyticDB PostgreSQL版不兼容的函数，例如：

        -   [DATEADD\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_DATEADD_function.html)
        -   [DATEDIFF\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_DATEDIFF_function.html)
        -   [REGEXP\_COUNT\(\)](https://docs.aws.amazon.com/redshift/latest/dg/REGEXP_COUNT.html)
-   **CREATE TABLE**

    Amazon Redshift的表名最大有效长度为127个字节，而AnalyticDB PostgreSQL版的表名默认最大有效长度为63字节。若表、函数、视图等对象名称超过63个有效字符时，需要裁剪名称。

    -   修改压缩编码

        您需要删除Amazon Redshift建表语句中的`ENCODE XXX`，用如下子句代替。

        ```
        with (COMPRESSTYPE={ZLIB|ZSTD|RLE_TYPE|NONE})
        ```

        AnalyticDB PostgreSQL版目前并不支持所有的[Redshift压缩编码](https://docs.aws.amazon.com/zh_cn/redshift/latest/dg/c_Compression_encodings.html)。不支持的压缩编码包括：

        -   BYTEDICT
        -   DELTA
        -   DELTA32K
        -   LZO
        -   MOSTLY8
        -   MOSTLY16
        -   MOSTLY32
        -   RAW \(no compression\)
        -   RUNLENGTH
        -   TEXT255
        -   TEXT32K
    -   修改分布键

        Amazon Redshift支持[三种分布键（分配）](https://docs.aws.amazon.com/zh_cn/redshift/latest/dg/c_choosing_dist_sort.html)。您需按照如下规则修改分布键：

        -   EVEN分配（DISTSTYLE EVEN）：可用`DISTRIBUTED RANDOMLY`代替。
        -   KEY分配（DISKEY）：可用`DISTRIBUTED BY (column, [ ... ] )`代替。
        -   ALL分配（ALL）：可用`DISTRIBUTED REPLICATED`代替。
    -   修改排序键（SortKey）

        删除Amazon Redshift的排序键子句`[ COMPOUND | INTERLEAVED ] SORTKEY (column_name [, ...] ) ]`中的COMPOUND或者INTERLEAVED选项，使用如下子句代替。

        ```
        order by (column, [ ... ])
        ```

    示例如下：

    -   示例一：

        Amazon Redshift的CREATE TABLE语句：

        ```
        CREATE TABLE schema1.table1
        (
            filed1 VARCHAR(100) ENCODE lzo,
            filed2 INTEGER DISTKEY,
            filed3 INTEGER,
            filed4 BIGINT ENCODE lzo,
            filed5 INTEGER,
        )
        INTERLEAVED SORTKEY
        (
            filed1,
            filed2
        );
        ```

        转换成AnalyticDB PostgreSQL版的CREATE TABLE语句：

        ```
        CREATE TABLE schema1.table1
        (
            filed1 VARCHAR(100) ,
            filed3 INTEGER,
            filed5 INTEGER
        )
        WITH(APPENDONLY=true,ORIENTATION=column,COMPRESSTYPE=zstd)
        DISTRIBUTED BY (filed2)
        ORDER BY (filed1, filed2);
        
        -- 排序
        SORT schema2.table1;
        MULTISORT schema2.table1;
        ```

    -   示例二：

        Amazon Redshift的CREATE TABLE语句，包含ENCODE和SORTKEY选项：

        ```
        CREATE TABLE schema2.table2
        (
            filed1 VARCHAR(50) ENCODE lzo,
            filed2 VARCHAR(50) ENCODE lzo,
            filed3 VARCHAR(20) ENCODE lzo,
        )
        DISTSTYLE EVEN
        INTERLEAVED SORTKEY
        (
            filed1
        );
        ```

        转换成AnalyticDB PostgreSQL版的CREATE TABLE语句：

        ```
        CREATE TABLE schema2.table2
        (
            filed1 VARCHAR(50),
            filed2 VARCHAR(50),
            filed3 VARCHAR(20),
        )
        WITH(APPENDONLY=true, ORIENTATION=column, COMPRESSTYPE=zstd)
        DISTRIBUTED randomly
        ORDER BY (filed1);
        
        -- 排序
        SORT schema2.table2;
        MULTISORT schema2.table2;
        ```

        **说明：** 更多sortkey信息，请参见[列存表使用排序键和粗糙集索引加速查询](/intl.zh-CN/开发入门/列存表使用排序键和粗糙集索引加速查询.md)。

-   **CREATE VIEW**

    您需要将Amazon Redshift的CREATE VIEW语句转换成符合AnalyticDB PostgreSQL版语法的SQL语句。CREATE VIEW与CREATE TABLE的转换规则基本一致。

    **说明：** 不支持WITH NO SCHEMA BINDING子句。


