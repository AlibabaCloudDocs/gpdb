# Database外表联邦分析

云原生数据仓库 AnalyticDB PostgreSQL （简称 ADB PG）支持通过JDBC的方式访问Oracle、PostgreSQL、MySQL外部数据源。

**说明：**

-   本特性只支持存储弹性模式实例，且需要ADB PG实例和目标访问的外部数据源处于同一个VPC网络。
-   2020年9月6日前申请的存量存储弹性模式实例，由于网络架构不同，无法与外部数据库网络打通，无法使用该特性。如需使用，请联系后台技术人员，重新申请实例，迁移数据。

## 前提条件：配置SEVER端

由于不同用户的需求配置不尽相同。如果您需要通过JDBC的方式访问外部数据源进行联邦分析，请[提交工单](https://workorder.console.aliyun.com/console.htm#/ticket/add?productCode=gpdb)由ADB PG后台技术人员进行配置。以下为提交工单时需要提交的对应文件。

|连接对象|提交工单内容|
|----|------|
|SQL Database\(PostgreSQL, Mysql, Oracle\)|JDBC的连接串、目标访问外部数据源的用户名和密码|

## 使用Database外表联邦分析

-   **创建扩展**

    ```
    CREATE extension pxf; 
    ```


-   **创建外表**

    ```
    CREATE EXTERNAL TABLE <table_name>
            ( <column_name> <data_type> [, ...] | LIKE <other_table> )
    LOCATION('pxf://<path-to-data>?PROFILE[&<custom-option>=<value>[...]]&[SERVER=value]')
    FORMAT '[TEXT|CSV|CUSTOM]' (<formatting-properties>);
    ```

    创建 EXTERNAL TABLE 语法请参见[CREATE EXTERNAL TABLE](/intl.zh-CN/开发入门/SQL语法.md)。

    |参数|说明|
    |--|--|
    |path-to-data|外表访问表名，例如 public.test\_a|
    |PROFILE \[&<custom-option\>=<value\>\[...\]\]|访问外部数据的配置。使用JDBC方式访问外部数据库时使用PROFILE=Jdbc |
    |FORMAT '\[TEXT\|CSV\|CUSTOM\]'|读取文件的格式。|
    |formatting-properties|与特定文件数据对应的格式化选项： formatter 或者 delimiter（分割符）    -   与CUSTOM搭配
        -   formatter='pxfwritable\_import'
        -   formatter='pxfwritable\_export'
    -   与TEXT\|CSV搭配

        -   delimiter=E'\\t'
        -   delimiter ':'
**说明：** escape 时需要加上 E |
    |SERVER|配置服务端文件的位置，该部分由后台技术人员操作后反馈给用户。|


## 示例 访问 postgresql 数据库

ADB PG后台技术人员配置完成后，您可以在ADB PG数据库中采用以下SQL语句创建外表并查询。

```
postgres=# CREATE EXTERNAL TABLE pxf_ext_pg(a int, b int)
  LOCATION ('pxf://public.t?PROFILE=Jdbc&SERVER=postgresql')
FORMAT 'CUSTOM' (FORMATTER='pxfwritable_import')
ENCODING 'UTF8';

postgres=# select * from pxf_ext_pg;
   a   |   b
-------+-------
     1 |     2
     2 |     4
     3 |     6
     4 |     8
     5 |    10
     6 |    12
     7 |    14
--more--      
```

LOCATION各字段含义说明：

-   pxf:// ：pxf 协议，固定值
-   public.t：代表数据库public下的表名为 t 的表。
-   PROFILE=Jdbc: 代表使用Jdbc访问外部数据源
-   SERVER=postgresql：ADB PG的后台技术人员将提供该选项。后台人员会根据您提交的工单要求进行相对应配置。此例中SERVER=postgresql 代表使用PXF\_SERVER/postgresql/下的配置文件来支持连接。
-   FORMAT 'CUSTOM' \(FORMATTER='pxfwritable\_import'\) ：外部数据源格式配置项。 查询外部数据源 table 时使用 CUSTOM ，并与 FORMATTER='pxfwritable\_import' 搭配。

```
CREATE EXTERNAL TABLE pxf_ext_test_a( id int,name varchar)
  LOCATION ('pxf://public.test_a?PROFILE=Jdbc&server=postgresql')
FORMAT 'CUSTOM' (FORMATTER='pxfwritable_import')
ENCODING 'UTF8';
```

## 支持的数据类型

-   INTEGER, BIGINT, SMALLINT
-   REAL, FLOAT8
-   NUMERIC
-   BOOLEAN
-   VARCHAR, BPCHAR, TEXT
-   DATE
-   TIMESTAMP
-   BYTEA

