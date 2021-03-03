# AWS Redshift应用和数据迁移至AnalyticDB for PostgreSQL

本文描述从AWS Redshift迁移数据到AnalyticDB for PostgreSQL的整体过程。

## 总体步骤

从Redshift迁移数据到AnalyticDB for PostgreSQL包含如下步骤：

1.  资源和环境准备，执行操作前需提前准备Amazon Redshift、Amazon S3（Amazon Simple Storage Service）、AnalyticDB for PostgreSQL和阿里云对象存储服务（OSS）的相关资源。
2.  将Redshift的数据导入到Amazon S3中。
3.  使用OSSImport将Amazon S3中CSV格式的数据文件导入到OSS。
4.  在目标AnalyticDB for PostgreSQL中创建和源Redshift对应的对象，包括模式（Schema）、表（Table）、视图（View）和函数（Function）。
5.  使用OSS外部表将数据导入到AnalyticDB for PostgreSQL。

整体迁移路径如下：

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7844352951/p35299.png)

## AWS上的准备工作

**准备用户访问S3的安全凭证**

包括如下信息：

-   访问密钥ID（AccessKeyID）和秘密访问密钥（Secret AccessKey）。
-   S3的Endpoint，例如s3.ap-southeast-2.amazonaws.com。
-   S3的Bucket名称，例如alibaba-hybrid-export。

**导出数据格式约定**

-   导出文件为CSV格式。
-   导出文件不得大于50MB。
-   导出文件中列的顺序必须和建表语句中列的顺序一致。
-   导出文件的数量最好和AnalyticDB for PostgreSQL计算组的数量一致或者是计算组数量的整数倍。

**推荐的Redshift UNLOAD命令选项**

经过大量的实践，我们建议使用类似如下的Redshift UNLOAD选项将数据导入到S3中：

```
unload ('select * from test')
to 's3://xxx-poc/test_export_'
access_key_id '<Your access key id>'
secret_access_key '<Your access key secret>'
DELIMITER AS ','
ADDQUOTES
ESCAPE
NULL AS 'NULL'
MAXFILESIZE 50 mb;
			
```

在上述样例中，推荐使用如下选项：

```
DELIMITER AS ','
ADDQUOTES
ESCAPE
NULL AS 'NULL'
MAXFILESIZE 50 mb
			
```

**从Redshift导出DDL语句**

从Redshift导出所有的DDL语句，包括但不限于创建模式、创建表、创建视图和创建函数的语句。

## 阿里云上的准备工作

**准备阿里云RAM账户**

-   RAM账户ID
-   RAM账户密码
-   RAM账户AccessKeyID
-   RAM账户AccessKeySecret

**创建OSS存储空间（Bucket）**

在AWS S3 Bucket所在地域，比如华北2（北京），创建一个OSS存储空间。OSS存储空间创建完成后，可以从OSS的控制台获取存储空间的访问域名，本文中会使用到**ESC的VPC网络访问（内网）**的访问域名信息。使用内网传输，可以保障数据传输的速度和安全性。

**下载安装OSSImport**

1.  在AWS S3 Bucket所在地域，创建一个ECS实例。我们选择创建带宽为100Mbps，系统镜像为Windows X64的ECS实例。
2.  在ECS系统中下载并安装单机模式的OSSImport。OSSImport的最新版本，可从[说明及配置](/cn.zh-CN/常用工具/数据迁移工具ossimport/说明及配置.md)获取。
3.  单机模式的OSSImport软件包解压后，软件的文件结构如下：

```
ossimport
├── bin
│ └── ossimport2.jar  # 包括Master、Worker、Tracker、Console四个模块的总jar
├── conf
│ ├── local_job.cfg   # 单机Job配置文件
│ └── sys.properties  # 系统运行参数配置文件
├── console.bat         # Windows命令行，可以分布执行调入任务
├── console.sh          # Linux命令行，可以分布执行调入任务
├── import.bat          # Windows一键导入，执行配置文件为conf/local_job.cfg配置的数据迁移任务，包括启动、迁移、校验、重试
├── import.sh           # Linux一键导入，执行配置文件为conf/local_job.cfg配置的数据迁移任务，包括启动、迁移、校验、重试
├── logs                # 日志目录
└── README.md           # 说明文档，强烈建议使用前仔细阅读
			
```

## 使用OSSImport将数据从S3导入到OSS中

**配置OSSImport**

在本文中，我们采用单机模式的OSSImport。请参考如下样例修改配置文件conf/local\_job.cfg，请确保仅修改本样例提及的参数。关于OSSImport配置的详细信息，请参考[说明及配置](/cn.zh-CN/常用工具/数据迁移工具ossimport/说明及配置.md)。

```
srcType=s3
srcAccessKey="your AWS Access Key ID"
srcSecretKey="your AWS Access Key Secret"
srcDomain=s3.ap-southeast-2.amazonaws.com
srcBucket=alibaba-hybrid-export
srcBucket=
destAccessKey="your Alibaba Cloud Access Key ID"
destSecretKey="your Alibaba Cloud Access Key Secret"
destDomain=http://oss-ap-southeast-2-internal.aliyuncs.com
destBucket=alibaba-hybrid-export-1
destPrefix=
isSkipExistFile=true
			
```

**启动OSSImport迁移任务**

在单机模式的OSSImport中，执行import.bat批处理文件启动迁移任务。

**查看迁移任务的状态**

在数据迁移过程中，您可以通过命令执行窗口查看任务的执行状态。另外，你还可通过Windows系统的任务管理器查看带宽的占用情况。

在本样例中，ECS和OSS存储空间位于相同的地域，采用内网传输，不受网速的限制；S3到ECS采用外网传输，有网速限制。数据的上传速度受限于下载速度，因此从ECS到OSS存储空间的上传速度几乎和S3到ECS的下载速度相同。

**任务失败重试（可选）**

由于网络或者其他因素，迁移任务可能失败。在ECS Windows系统中的CMD命令窗口中执行concole.bat retry命令重试任务。失败任务重试仅仅重新执行失败的子任务，不会重试已成功的子任务。

**检查OSS存储空间的文件（可选）**

您可在OSS控制台检查导入的数据文件。我们推荐使用ossbrowser客户端工具来查看和管理OSS存储空间中的文件。ossbrowser可从[快速开始](/cn.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md)获取。

**整理CSV文件中的数据（可选）**

**说明：** 本操作仅仅提供一个参考样例，为可选步骤，您可以根据您的业务需求整理CSV文件。

-   将CSV文件中的`NULL`替换成空格。
-   将CSV文件中的`\,`替换成`,`。

推荐在本地进行数据整理。您先通过ossbrowser将文件下载到ECS中，进行数据整理。然后再将整理后的文件上传到一个新建的OSS存储空间中，以区别于原来从S3下载的CSV文件。无论是上传还是下载CSV文件，我们都推荐使用OSS的内网Endpoint，以降低内网流量的开销。

## 将Redshift的DDL语句转换成AnalyticDB for PostgreSQL的DDL语句

在创建AnalyticDB for PostgreSQL数据库对象之前，我们需要做一些必要的准备工作。主要是将上述步骤导出的Redshift DDL语句转换成AnalyticDB for PostgreSQL语法的DDL语句。下面我们将简要地介绍这些转换规则。

**CREATE SCHEMA**

按照AnalyticDB for PostgreSQL语法标准创建模式，将其保存为create\_schema.sql。如下为具体的样例：

```
CREATE SCHEMA schema1
  AUTHORIZATION xxxpoc;
GRANT ALL ON SCHEMA schema1 TO xxxpoc;
GRANT ALL ON SCHEMA schema1 TO public;
COMMENT ON SCHEMA model IS 'for xxx migration  poc test';

CREATE SCHEMA oss_external_table
  AUTHORIZATION xxxpoc;
			
```

**CREATE FUNCTION**

由于AnalyticDB for PostgreSQL不兼容Redshift的某些SQL函数，因此你需要定制或者重写这些函数。涉及的函数举例如下：

-   `CONVERT_TIMEZONE(a,b,c)`，使用如下语句替换：

    ```
    timezone(b, timezone(a,c))
    ```

-   `GETDATE()`，使用如下语句替换：

    ```
    current_timestamp(0):timestamp
    ```

-   替换或优化用户定义函数（UDF）。

    例如，Redshift的SQL函数如下：

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

    替换成如下AnalyticDB for PostgreSQL函数，可以提升性能。

    ```
    to_char(a - interval '4 hour', 'yyyy-mm-dd')
    ```

-   其他Redshift标准的函数。

    在具体的实践中，您可以在[Functions and Operators in PostgreSQL8.2](https://www.postgresql.org/docs/8.2/functions.html)中查询标准的PostgreSQL函数的用法，修改或者自行实现Redshift和AnalyticDB for PostgreSQL不兼容的函数。如下为一些相关资源：

    -   [ISNULL\(\)](https://docs.microsoft.com/en-us/sql/t-sql/functions/isnull-transact-sql?view=sql-server-2017)
    -   [DATEADD\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_DATEADD_function.html)
    -   [DATEDIFF\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_DATEDIFF_function.html)
    -   [REGEXP\_COUNT\(\)](https://docs.aws.amazon.com/redshift/latest/dg/REGEXP_COUNT.html)
    -   [LEFT\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_LEFT.html)
    -   [RIGHT\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_LEFT.html)

**CREATE TABLE**

-   修改压缩编码。AnalyticDB for PostgreSQL目前并不支持所有的[Redshift压缩编码](https://docs.aws.amazon.com/zh_cn/redshift/latest/dg/c_Compression_encodings.html)。**不**支持的压缩编码包括：

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
    -   ZSTD
    必须删除Redshift建表语句中的`ENCODE XXX`，用如下子句代替。

    ```
    with (COMPRESSTYPE={ZLIB|QUICKLZ|RLE_TYPE|NONE})
    ```

-   修改分布键。Redshift支持三种分布键（分配），具体请参见[分配方式](https://docs.aws.amazon.com/zh_cn/redshift/latest/dg/c_choosing_dist_sort.html)。您需按照如下规则修改分布键。
    -   EVEN分配（DISTSTYLE EVEN）：用`distributed randomly`代替。
    -   KEY分配（DISKEY）：用`distributed by (colname1,...)`代替。
    -   ALL分配（ALL）：不支持，直接删除。
-   修改排序键（SortKey）。删除Redshift的排序键子句`[ COMPOUND | INTERLEAVED ] SORTKEY (column_name [, ...] ) ]`中的COMPOUND或者INTERLEAVED选项，使用如下子句代替：

    ```
    with(APPENDONLY=true,ORIENTATION=column)
    sortkey (volume);
    					
    ```

    **样例1**

    Redshift的CREATE TABLE语句：

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

    转换成AnalyticDB for PostgreSQL的CREATE TABLE语句：

    ```
    CREATE TABLE schema1.table1
    (
        filed1 VARCHAR(100) ,
        filed3 INTEGER,
        filed5 INTEGER
    )
    WITH(APPENDONLY=true,ORIENTATION=column,COMPRESSTYPE=zlib)
    DISTRIBUTED BY (filed2)
    SORTKEY
    (
        filed1,
        filed2
    )
    					
    ```

    **样例2**

    Redshift的CREATE TABLE语句，包含ENCODE和SORTKEY选项：

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

    转换成AnalyticDB for PostgreSQL的CREATE TABLE语句：

    ```
    CREATE TABLE schema2.table2
    (
        filed1 VARCHAR(50),
        filed2 VARCHAR(50),
        filed3 VARCHAR(20),
    )
    WITH(APPENDONLY=true, ORIENTATION=column, COMPRESSTYPE=zlib)
    DISTRIBUTED randomly
    SORTKEY
    (
        filed1
    );
    					
    ```


**CREATE VIEW**

同样需要将Redshift的CREATE VIEW语句转换成符合AnalyticDB for PostgreSQL语法的SQL语句，转换规则和CREATE TABLE的转换规则类似。

## 创建和配置AnalyticDB for PostgreSQL实例

根据如下内容，创建并配置实例。

-   [创建实例](/cn.zh-CN/快速入门/创建实例.md)
-   [设置白名单](/cn.zh-CN/快速入门/设置白名单.md)
-   [t16843.md\#](/cn.zh-CN/快速入门/设置账号.md)

## 创建数据库对象

参考[客户端连接](/cn.zh-CN/快速入门/客户端连接.md)，使用psql或者pgAdmin III 1.6.3客户端，登录数据库。

按照上述转换规则，将Redshift的DDL语句转换成符合AnalyticDB for PostgreSQL语法规则的DDL语句。然后执行这些DDL语句创建数据库对象。

## CREATE EXTERNAL TABLE

AnalyticDB for PostgreSQL支持通过OSS外部表（即gpossext功能），将数据并行从OSS导入或导出到OSS，并支持通过gzip进行OSS外部表文件压缩，大量地节省存储空间及成本。请参考[OSS外表高速导入或导出OSS数据](/cn.zh-CN/数据接入/OSS外表高速导入或导出OSS数据.md)，创建OSS外部表。

## 使用INSERT INTO脚本导入数据

在完成OSS外部表和目标数据库各个对象的创建后，您需要准备`INSERT`脚本，用于将OSS外部表的数据插入到AnalyticDB for PostgreSQL目标表中。请将该脚本保存为insert.sql文件，并执行该脚本。

插入语句的格式为：`INSERT INTO <TABLE NAME> SELECT * FROM <OSS EXTERNAL TABLE NAME>;`

例如：

```
INSERT INTO schema1.table1 SELECT * FROM oss_external_table.table1;
```

导入数据后，请使用SELECT语句查询导入后的数据并和源数据进行对比分析，验证导入前后数据的一致性。

## 使用VACUUM脚本清理数据库

在将OSS外部表导入AnalyticDB for PostgreSQL后，您还需要使用`VACUUM`命令清理一下数据库，请将VACUUM脚本存储为vacuum.sql文件。关于VACUUM的用法，请参见[VACUUM](https://www.postgresql.org/docs/8.2/sql-vacuum.html)。

