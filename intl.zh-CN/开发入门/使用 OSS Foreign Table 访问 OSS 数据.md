使用 OSS Foreign Table 访问 OSS 数据 
===================================================

OSS Foreign Table是基于 PostgreSQL Foreign Data Wrapper（简称PG FDW）框架开发，用于访问 OSS 数据的数据分析方案。

![概览 ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8738398951/p101830.png)

通过 OSS Foreign Table，可以：

* 导入 OSS 数据到本地表（行存表/列存表）进行分析加速。

  

* 直接查询分析 OSS 海量数据。

  

* OSS 外表与本地表关联分析。

  




目前OSS Foreign Table 支持orc，parquet，json，jsonline，csv（支持 gzip、标准 snappy 压缩）文件格式，同时支持按一个或多个字段进行分区，查询特定分区时起到过滤效果。

OSS 上的数据，可以来自业务应用 APP 的写入，阿里云 SLS 的日志归档，阿里云 DLA 的 ETL 输出等。

开始使用 OSS Foreign Table 
-------------------------------------------

如何使用一个 OSS Foreign Table，简单的说就是：

* 以 ***USER MAPPING*** 用户（[CREATE USER MAPPING](https://www.postgresql.org/docs/9.4/sql-createusermapping.html)）

  

* 操作在 ***OSS*** ***FOREIGN SERVER*** （[CREATE SERVER](https://www.postgresql.org/docs/9.4/sql-createserver.html)）上

  

* 定义的 **OSS** ***FOREIGN TABLE*** （[CREATE FOREIGN TABLE](https://www.postgresql.org/docs/9.4/sql-createforeigntable.html)）

  




例如，我们通过[命令行工具ossutil](https://www.alibabacloud.com/help/zh/doc-detail/50452.htm)查看 OSS 域上 TPCH lineitem 表相关列表如下：

    [adbpgadmin@localhost]$ ossutil ls oss://adbpg-tpch/data/tpch_data_10x/lineitem.tbl
    LastModifiedTime                   Size(B)  StorageClass   ETAG                                  ObjectName
    2020-03-12 09:29:48 +0800 CST    144144997      Standard   1F426F2FFC70A0262D2D69183BC3A0BD-57   oss://adbpg-tpch/data/tpch_data_10x/lineitem.tbl.1
    2020-03-12 09:29:58 +0800 CST    145177420      Standard   CFE2CFF1C8059547DC9F1711E77F74DD-57   oss://adbpg-tpch/data/tpch_data_10x/lineitem.tbl.10
    2020-03-10 21:23:24 +0800 CST    145355168      Standard   35C6227D1C29F1236A92A4D5D7922625-57   oss://adbpg-tpch/data/tpch_data_10x/lineitem.tbl.11
    ... ...



其中：

* **adbpg-tpch** 为 OSS bucket 名称。

  

* **data/tpch_data_10x/...** 为文件相对于 bucket 的路径。

  




下面详细讲解如何创建并使用 OSS Foreign Table。

1. 创建 OSS Server 
-------------------------------------

创建 OSS Server，即定义需要访问的 OSS 服务端，需要指定\<oss endpoint\>。

1.1 示例

    CREATE SERVER oss_serv              -- OSS server 名称
        FOREIGN DATA WRAPPER oss_fdw
        OPTIONS (
            endpoint '<oss endpoint>',  -- OSS server 域名
            bucket '<oss bucket>'       -- 指定数据文件所在的 OSS bucket
      );



其中，OPTIONS 的取值请参见1.2参数选项。了解更多请参见 [CREATE SERVER](https://www.postgresql.org/docs/9.4/sql-createserver.html)。

1.2 参数选项


|        选项         | 类型  |  单位  | 是否必选 | 默认值  |                                                             备注                                                              |
|-------------------|-----|------|------|------|-----------------------------------------------------------------------------------------------------------------------------|
| endpoint          | 字符串 |      | 必选   |      | OSS 对应区域的域名。 **说明** 如果是从阿里云的主机访问数据库，应该使用内网域名（即带有"internal"的域名），避免产生公网流量。                                    |
| bucket            | 字符串 |      | 可选   |      | 指定数据文件所在的 bucket，需要通过 OSS 预先创建。 **说明** OSS Server 和 OSS Table 必须有一个设置该选项且 OSS Table 的优先级高，会覆盖OSS Server 的值。 |
| **说明** * 以下为 OSS 访问时容错相关参数，通常不需要额外设置，使用默认值即可。   * 下列参数如果使用默认值，表示如果连续 1500 秒的传输速率小于 1 KB，就会触发超时。详细描述请参见[错误处理](https://www.alibabacloud.com/help/doc-detail/32141.htm)。    ||||||
| speed_limit       | 数值  | 字节/秒 | 可选   | 1024 | 设置能容忍的最小速率                                                                                                                  |
| speed_time        | 数值  | 秒    | 可选   | 1500 | 设置能容忍speed_limit的最长时间                                                                                                       |
| connect_timeout   | 数值  | 秒    | 可选   | 10   | 设置链接超时时间                                                                                                                    |
| dns_cache_timeout | 数值  | 秒    | 可选   | 60   | 设置 DNS 超时时间                                                                                                                 |



2. 创建 OSS User Mapping 
-------------------------------------------

创建 OSS Server 后，还需要创建访问 OSS Server 的用户。通过创建 OSS User Mapping，定义 ADB PG 数据库用户到访问 OSS Server 用户的映射关系。

2.1 示例

    CREATE USER MAPPING FOR PUBLIC  -- 为所有用户创建到 OSS server 用户的映射，了解更多请参见2.2语法部分
        SERVER oss_serv                 -- 指定需要访问的 OSS server
        OPTIONS ( 
          id '<oss access id>',         -- 指定 OSS 账号 ID
          key '<oss access key>'        -- 指定 OSS 账号 KEY
        );



2.2 语法

    -- 创建用户映射
    CREATE USER MAPPING FOR { username | USER | CURRENT_USER | PUBLIC }
        SERVER servername
        [ OPTIONS ( option 'value' [, ... ] ) ]
    
    -- 删除用户映射
    DROP USER MAPPING [ IF EXISTS ] FOR { user_name | USER | CURRENT_USER | PUBLIC }
        SERVER server_name


**说明**

* `username`：The name of an existing user that is mapped to foreign server.

  

* `CURRENT_USER` and `USER` ：match the name of the current user.

  

* `PUBLIC`：all roles, including those that might be created later.

  






其中，OPTIONS 详见2.3参数选项。了解更多请参见[CREATE USER MAPPING](https://www.postgresql.org/docs/9.4/sql-createusermapping.html)。

2.3 参数选项


| 选项  | 是否必选 | 默认值 |     备注     |
|-----|------|-----|------------|
| id  | 必选   |     | OSS 账号 ID  |
| key | 必选   |     | OSS 账号 KEY |





3. 创建 OSS Foreign Table 
--------------------------------------------

有了 OSS 域和访问 OSS 域的用户之后，接下来就可以定义 OSS Foreign Table 了。目前，OSS Foreign Table 支持多种格式的数据文件，适应不同的业务场景。

* 支持访问 csv、text、json、jsonline 格式的非压缩文本文件。

  

* 支持访问 csv、text 格式的 gzip 压缩、标准 snappy 压缩文本文件。支持访问 json、jsonline 格式的 gzip 压缩文本文件。

  

* 支持访问 orc 格式的二进制文件。ORC数据类型到ADBPG数据类型的映射关系请参见 

  [ORC - ADB PG 数据类型对照表](#section-3dg-wt5-9gy)
  

* 支持访问 parquet 格式的二进制文件。PARQUET数据类型到ADBPG数据类型的映射关系详见 

  [PARQUET - ADB PG 数据类型对照表](#section-3dg-wx5-9zy)
  




3.1 示例

    CREATE FOREIGN TABLE x(i int, j int)
    SERVER oss_serv OPTIONS (format 'jsonline')
    PARTITION BY LIST (j) ( 
        VALUES('20181218'), 
        VALUES('20190101')
    );


**说明**

创建 OSS Foreign Table 后，可以通过如下方式查看，OSS 外表匹配的 OSS 文件列表是否符合预期，借以校正。

* 方式一： `explain verbose select * from OSS外表;`

  

* 方式二： `select * from get_oss_table_meta('OSS外表');`

  




3.2 语法

    -- 创建 OSS Foreign Table
    CREATE FOREIGN TABLE [ IF NOT EXISTS ] table_name ( [
        column_name data_type [ OPTIONS ( option 'value' [, ... ] ) ] [ COLLATE collation ] [ column_constraint [ ... ] ]
          [, ... ]
    ] )
        SERVER server_name
      [ OPTIONS ( option 'value' [, ... ] ) ]
    
    -- 删除 OSS Foreign Table
    DROP FOREIGN TABLE [ IF EXISTS ] name [, ...] [ CASCADE | RESTRICT ]



其中，OPTIONS 见3.3参数选项。了解更多见[CREATE FOREIGN TABLE](https://www.postgresql.org/docs/9.4/sql-createforeigntable.html)。

3.3 参数选项

3.3.1通用选项


|    选项    | 类型  | 单位 |                      是否必选                      | 默认值 |                                                                                                                                                                                                                                                                           备注                                                                                                                                                                                                                                                                           |
|----------|-----|----|------------------------------------------------|-----|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| filepath | 字符串 |    | 必选，三选一。 **说明** 均为相对于bucket的路径。 |     | 只匹配一个文件。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| prefix   | 字符串 |    | 必选，三选一。 **说明** 均为相对于bucket的路径。 |     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| dir      | 字符串 |    | 必选，三选一。 **说明** 均为相对于bucket的路径。 |     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| bucket   | 字符串 |    | 可选                                             |     | Oss Server 和 Oss Table 必须有一个设置该选项且 Oss Table 的优先级高，会覆盖Oss Server 的值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| format   | 字符串 |    | 必选                                             |     | 指定文件格式，有效值如下： * csv   * text   * orc   * parquet   * json，参考[JSON 规范](https://www.json.org/json-en.html)了解当前支持的 json 规范。   * jsonline，参考[jsonline](http://jsonlines.org/)了解 jsonline 规范，简单来说就是以换行符分隔的 json。这里所有能被 jsonline 读取的数据一定可以用 json 读取，但反之则不一定。在可行的情况，更推荐使用 jsonline。    |



3.3.2 csv/text 格式选项

注意：以下选项若不特殊说明仅适用 csv/text 格式，orc/parquet 等格式设置无效。


|          选项          | 类型  | 单位 | 是否必选 |                                                                      默认值                                                                      |                                                                                                                                                                                                                         备注                                                                                                                                                                                                                          |
|----------------------|-----|----|------|-----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| filetype             | 字符串 |    | 可选   | plain                                                                                                                                         | 有效值如下： * plain -  按字节二进制读取，不做额外处理。   * gzip  - 读取原始二进制数据并gzip解压缩。   * snappy - 读取原始二进制数据并snappy解压缩。（只支持[标准格式](https://github.com/google/snappy/blob/master/format_description.txt)的snappy压缩文件，暂不支持 hadoop-snappy压缩文件）。 json/jsonline 中也可以使用这个字段指定输入文件的压缩类型，当前仅支持 plain，gzip。    |
| log_errors           | 布尔型 |    | 可选   | false                                                                                                                                         | 是否将错误记录到日志文件； 更多示例请参见3.4容错机制。                                                                                                                                                                                                                                                                                                                                                                                                       |
| segment_reject_limit | 数值  |    | 可选   |                                                                                                                                               | 设置error abort的上限数值。当包含%号时表示错误行百分比，否则表示错误行数。 如： * segment_reject_limit = '10' 表示当segment上错误的行数达超过10行时，error abort；   * segment_reject_limit = '10%' 表示当segment上错误的行数超过已处理行数的10%时，error abort。    请参见3.4容错机制                                                                                                                       |
| 以下为格式化选项，参考[COPY命令](https://www.postgresql.org/docs/current/sql-copy.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              ||||||
| header               | 布尔型 |    | 可选   | false                                                                                                                                         | 源文件是否包含字段名 header 行。仅适用于 csv 格式                                                                                                                                                                                                                                                                                                                                                                                                                     |
| delimiter            | 字符串 |    | 可选   | * text 格式默认tab键。   * csv 格式默认逗号。                           | 字段分隔符。只允许单字节字符。                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| quote                | 字符串 |    | 可选   | 默认双引号                                                                                                                                         | 字段引号 * 仅适用于 csv 格式。   * 只允许单字节字符。                                                                                                                                                                                                                                                                                                                |
| escape               | 字符串 |    | 可选   | 默认与QUOTE值相同                                                                                                                                   | 声明应该出现在一个匹配 QUOTE 值的数据字符之前的字符 * 只允许单字节的字符。   * 仅适用于 csv 格式。                                                                                                                                                                                                                                                                                      |
| null                 | 字符串 |    |      | * text 格式默认\\N (backslash-N)。   * csv 格式默认为未被引号引用的空白字符。    | 指定文件的 NULL 字符串。                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| encoding             | 字符串 |    | 可选   | 未指定时，默认为客户端编码。                                                                                                                                | 指定数据文件编码。                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| force_not_null       | 布尔型 |    | 可选   | false                                                                                                                                         | 如果为真，则声明字段的值不匹配空字符串。                                                                                                                                                                                                                                                                                                                                                                                                                                |
| force_null           | 布尔型 |    | 可选   | false                                                                                                                                         | * 如果为真，它声明匹配空字符串的字段的值作为NULL 返回，即使该值加了引号。   * 没有这个选项，只有未加引号的空字符串的字段的值作为 NULL返回。                                                                                                                                                                                                                                                                                   |





3.4 容错机制

创建 OSS Foreign 表时，通过设置参数`log_errors`和`segment_reject_limit`，可以使得 OSS 外表扫描时不因原始数据错误行而退出，具有一定的容错机制。其中：

* `log_errors`：表示是否记录错误行信息。

  

* segment_reject_limit：表示容错比例，即错误行在已解析行数中的占比。

  



**说明**

：当前只有 CSV/TEXT 格式的OSS外表支持容错机制

1. 创建容错 OSS FDW 外表。 

       create foreign table oss_error_sales (id int, value float8, x text)
           server oss_serv
           options (log_errors 'true',         -- 记录错误行信息
                    segment_reject_limit '10', -- 错误行数不得超过10行，否则会报错退出。
                    dir 'error_sales/',        -- 指定外表匹配的 OSS 文件目录
                    format 'csv',              -- 指定按 csv 格式解析文件
                    encoding 'utf8');          -- 指定文件编码 
                                       

   

2. 扫描该外表

   为显示容错效果，我们故意在OSS 文件中添加3行错误记录。

       postgres=# select * from oss_error_sales ;
       NOTICE:  found 3 data formatting errors (3 or more input rows), rejected related input data
        id |     value     |     x
       ----+---------------+-----------
         1 |  0.1102213212 | abcdefg
         1 |  0.1102213212 | abcdefg
         2 | 0.92983182312 | mmsmda123
         3 |     0.1123123 | abbs
         1 |  0.1102213212 | abcdefg
         2 | 0.92983182312 | mmsmda123
         3 |     0.1123123 | abbs
         1 |  0.1102213212 | abcdefg
         1 |  0.1102213212 | abcdefg
         2 | 0.92983182312 | mmsmda123
         3 |     0.1123123 | abbs
         3 |     0.1123123 | abdsa
         1 |  0.1102213212 | abcdefg
         2 | 0.92983182312 | mmsmda123
         3 |     0.1123123 | abbs
         1 |  0.1102213212 | abcdefg
         2 | 0.92983182312 | mmsmda123
         3 |     0.1123123 | abbs
       (18 rows)

   

3. 查看错误行日志

       postgres=# select * from gp_read_error_log('oss_error_sales');
                  cmdtime            |     relname     |        filename         | linenum | bytenum |                          errmsg                           | rawdata | rawbytes
       ------------------------------+-----------------+-------------------------+---------+---------+-----------------------------------------------------------+---------+----------
        2020-04-22 19:37:35.21125+08 | oss_error_sales | error_sales/sales.2.csv |       2 |         | invalid byte sequence for encoding "UTF8": 0xed 0xab 0xad |         | \x
        2020-04-22 19:37:35.21125+08 | oss_error_sales | error_sales/sales.2.csv |       3 |         | invalid byte sequence for encoding "UTF8": 0xed 0xab 0xad |         | \x
        2020-04-22 19:37:35.21125+08 | oss_error_sales | error_sales/sales.3.csv |       2 |         | invalid byte sequence for encoding "UTF8": 0xed 0xab 0xad |         | \x
       (3 rows)                         

   

   

4. 删除错误行日志

       postgres=# select gp_truncate_error_log('oss_error_sales');
        gp_truncate_error_log
       -----------------------
        t
       (1 row)

   

   




4. 使用 OSS Foreign Table 
-----------------------------------------

4.1 导入 OSS 数据到本地表

导入数据时，请执行如下步骤：

1. 将数据均匀分散存储在多个 OSS 文件中。

   **说明**

   
   * AnalyticDB for PostgreSQL的每个数据分区（Segment）将按轮询方式并行对 OSS 上的数据文件进行读取。

     
   
   * 推荐数据文件的编码和数据库编码保持一致，减少编码转换，提高效率。数据库编码默认UTF-8。

     
   
   * 对于 csv/text 格式的文本文件，支持多文件并行读取，默认并行数4。

     
   
   * 文件的数目建议为 数据节点数（Segment 个数 × 单个Segment 核数）的整数倍，从而提升读取效率。

     
   
   * 当文件数过少时，推荐拆分多个源文件，以便于使用多节点并行扫描功能，请参见[大文件切分](#section-hvx-cxk-mp0)。

     
   

   
   

2. 在 AnalyticDB for PostgreSQL 中，创建 OSS Foreign 外表。

   

3. 执行如下操作，并行导入数据。

       -- INSERT 方式
       INSERT INTO <本地目标表> SELECT * FROM <OSS Foreign 外表>;
       
       -- CREATE TABLE AS 方式
       CREATE TABLE <本地目标表> AS SELECT * FROM <OSS Foreign 外表>;

   




* 示例1：INSERT 方式将oss_lineitem_orc数据导入到本地AOCS表

      -- 创建本地AOCS表
      CREATE TABLE aocs_lineitem (
          l_orderkey bigint,
          l_partkey bigint,
          l_suppkey bigint,
          l_linenumber bigint,
          l_quantity double precision,
          l_extendedprice double precision,
          l_discount double precision,
          l_tax double precision,
          l_returnflag CHAR(1),
          l_linestatus CHAR(1),
          l_shipdate DATE,
          l_commitdate DATE,
          l_receiptdate DATE,
          l_shipinstruct CHAR(25),
          l_shipmode CHAR(10),
          l_comment VARCHAR(44)
      ) WITH (APPENDONLY=TRUE, ORIENTATION=COLUMN, COMPRESSTYPE=ZSTD, COMPRESSLEVEL=5);
      
      -- 将oss_lineitem数据导入到AOCS本地表
      INSERT INTO aocs_lineitem SELECT * FROM oss_lineitem_orc;

  

* 示例2：CREATE TABLE AS 方式将oss_lineitem_orc导入到本地heap表

      create table heap_lineitem as select * from oss_lineitem_orc distributed by (l_orderkey);

  






4.2 查询分析 OSS 数据

OSS Foreign 表创建后，可以像本地表一样查询。常见的查询场景如下：

* 键值过滤查询

  




    select * from oss_lineitem_orc where l_orderkey = 14062498;



* 聚集查询

  




    select count(*) from oss_lineitem_orc where l_orderkey = 14062498;



* 过滤+分组+LIMIT

  




    select l_partkey, sum(l_suppkey)
      from oss_lineitem_orc
     group by l_partkey
     order by l_partkey
     limit 10;



4.3 OSS 外表与本地表关联分析

* 示例 - 本地AOCS表 aocs_lineitem 与 OSS 外表关联执行TPCH Q3

  




    select l_orderkey,
           sum(l_extendedprice * (1 - l_discount)) as revenue,
           o_orderdate,
           o_shippriority 
      from oss_customer,                                    -- OSS 外表
           oss_orders,                                      -- OSS 外表
           aocs_lineitem                                    -- 本地 AOCS 表
     where c_mktsegment = 'furniture'
       and c_custkey = o_custkey
       and l_orderkey = o_orderkey
       and o_orderdate < date '1995-03-29'
       and l_shipdate > date '1995-03-29'
     group by l_orderkey, o_orderdate, o_shippriority
     order by revenue desc, o_orderdate
     limit 10;



5. 使用 OSS 外表分区表 
---------------------------------

在 OSS 外表基础上，ADB PG 为其加入了分区功能支持，利用分区，当分区列出现在查询 WHERE 条件时，可以显著降低从远端拖取的数据量。

ADB PG OSS FDW 分区功能的使用对数据文件组织有一定的要求。具体来说，对于分区外表下一个特定的分区，该分区在 OSS 上的数据文件要位于`oss://bucket/partcol1=partval1/partcol2=partval2/` 这样的目录下，其中`partcol1`、`partcol2`为分区列，`partval1`、`partval2`定义了该分区，为该分区对应的分区列值。

5.1 示例

如下 SQL 创建的外表 userlogin 具有两个分区，这两个分区在 OSS 文件路径分别为`oss://bucket/userlogin/month=201812/`与`oss://bucket/userlogin/month=201901/`

    CREATE FOREIGN TABLE userlogin (
            uid integer,
            name character varying,
            source integer,
            logindate timestamp without time zone,
            month int
    ) SERVER oss_serv OPTIONS (
        dir 'userlogin/',
        format 'text'
    )
    PARTITION BY LIST (month)
    (
            VALUES ('201812'), 
            VALUES ('201901')       
    )



5.2 语法

5.2.1 创建分区外表

ADB PG OSS FDW 分区语法与定义普通分区表时采用的语法完全一致。关于普通分区表定义语法可参考[表分区定义](/intl.zh-CN/数据管理/表分区定义.md)。当前 ADB PG OSS FDW 只支持值（LIST）分区。

5.2.2 删除分区外表

与常规的外表一样，可通过 DROP FOREIGN TABLE 来删除一个分区外表。

5.2.3 分区外表结构调整

用户可通过 ALTER TABLE 命令完成一个已有外表分区的结构调整工作。当前支持：增加分区，删除原有分区。请参见[文档](https://gpdb.docs.pivotal.io/6-3/ref_guide/sql_commands/ALTER_TABLE.html)了解详细的语法定义，如下会通过例子来演示如何删除/新增分区：




    CREATE FOREIGN TABLE ossfdw_parttable(            
      key text,
      value bigint,
      pt text,                                        -- 一级分区键
      region text                                     -- 二级分区键
    ) 
    SERVER oss_serv
    OPTIONS (dir 'PartationDataDirInOss/', format 'jsonline')
    PARTITION BY LIST (pt)                            -- 一级分区以"pt"字段为分区键
    SUBPARTITION BY LIST (region)                     -- 二级分区以"region"字段为分区键
        SUBPARTITION TEMPLATE (                       -- 二级分区模板
           SUBPARTITION hangzhou VALUES ('hangzhou'),
           SUBPARTITION shanghai VALUES ('shanghai')
        )
    ( PARTITION "20170601" VALUES ('20170601'), 
      PARTITION "20170602" VALUES ('20170602'));



在如上外表分区的基础上运行如下 ALTER TABLE 命令会为 ossfdw_parttable 新增一个一级分区，再结合 ossfdw_parttable 中分区模板的定义，会自动为这个新增一级分区创建对应的子分区。




    ALTER TABLE ossfdw_parttable ADD PARTITION VALUES ('20170603');



![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9738398951/p130512.png)

也可以直接在一个已有一级分区下二级子分区。




    ALTER TABLE ossfdw_parttable ALTER PARTITION FOR ('20170603') ADD PARTITION VALUES('nanjing');



![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9738398951/p130540.png)

之后可以通过命令删除一个已有的一级分区。



    ALTER TABLE ossfdw_parttable DROP PARTITION FOR ('20170601');



或者删除一个已有二级分区。



    ALTER TABLE ossfdw_parttable ALTER PARTITION FOR ('20170602') DROP PARTITION FOR ('hangzhou');



在创建或者新增外表分区时，也可以不使用模板来灵活地定义分区结构，如下所示：



    CREATE FOREIGN TABLE ossfdw_parttable(            
      key text,
      value bigint,
      pt text,                                        -- 一级分区键
      region text                                     -- 二级分区键
    ) 
    SERVER oss_serv
    OPTIONS (dir 'PartationDataDirInOss/', format 'jsonline')
    PARTITION BY LIST (pt)                            -- 一级分区以"pt"字段为分区键
    SUBPARTITION BY LIST (region)                     -- 二级分区以"region"字段为分区键
    (
        -- 如下两个一级分区下面可以挂着不同的二级分区.
        VALUES('20181218')
        (
            VALUES('hangzhou'),
            VALUES('shanghai') 
        ),
        VALUES('20181219')
        (
            VALUES('nantong'),
            VALUES('anhui') 
        )    
    );
    
    -- 新增一级分区, 此时由于没有模板, 所以也需要详细指定一级分区下的二级分区.
    ALTER TABLE ossfdw_parttable ADD PARTITION VALUES ('20181220')
    (
        VALUES('hefei'),
        VALUES('guangzhou') 
    );



5.3 使用场景

外表分区表的一个典型场景是[OSS FDW 访问 SLS 投递数据](#section-juz-qpj-6yo)。业务APP写入数据到OSS时也可以使用类似目录组织，然后在定义外表时使用分区表。

6.使用 OSS Foreign Table 导出数据 
------------------------------------------------

ADB PG 支持通过外表向OSS导出数据。目前，OSS外表导出多种格式的数据文件，适用于不同的业务场景。

* 支持导出 csv、text格式的非压缩文本文件。

  

* 支持导出 csv、text格式的 gzip 压缩文件。

  

* 支持导出 orc 格式的二进制文件。ORC数据类型与ADBPG数据类型的映射关系请参见[ORC - ADB PG 数据类型对照表](#section-3dg-wt5-9gy)。

  

* **暂不支持对分区外表导出。**

  




6.1 导出文件命名规则 
------------------------------

导出时，多个segment并行将数据写出到相同的目录下，因此，OSS外表导出文件命名规则如下：

{tablename \| prefix } _{timestamp}_{random_key}_{seg}{segment_id}_{fileno}.{ext}\[.gz\]

* {tablename \| prefix } - 导出前缀，`prefix`时以设置的`prefix`为前缀；`dir` option时，以OSS外表名称为默认前缀。

  

* {timestamp} - 导出时的时间戳，格式如`YYYYMMDDHH24MISS`。

  

* {random_key} - 随机键值。

  

* {seg}{segment_id} - "seg"+ "segment节点号"。如："seg1"表明该文件由segment 1导出。

  

* {fileno} - 文件段序号，从0开始。

  

* {ext} - 表示导出的文件格式。 如 ".csv", ".txt", ".orc" 分别对应`format `option 设置为 "csv", "text", "orc"时导出的文件格式。

  

* \[.gz\] - 表示导出文件为gzip压缩文件。

  




    -- 创建外表, 指定格式为CSV, 用gzip压缩, 并用dir指定路径
    CREATE FOREIGN TABLE fdw_t_out_1(a int)
    SERVER oss_serv
    OPTIONS (format 'csv', filetype 'gzip', dir 'test/');
    
    -- 导出的OSS文件名示例如：
    fdw_t_out_1_20200805110207_1718599661_seg-1_0.csv.gz
    
    -- 创建外表, 指定格式为orc, 并用prefix指定前缀路径
    CREATE FOREIGN TABLE fdw_t_out_2(a int)
    SERVER oss_serv
    OPTIONS (format 'orc', prefix 'test/my_orc_test');
    
    -- 导出的OSS文件名示例如：
    my_orc_test_20200924153043_1737154096_seg0_0.orc



6.2 示例 
------------------------

1. 创建外表

       -- 创建一个外表，格式为CSV，导出目录为tt_csv
       CREATE FOREIGN TABLE foreign_x (i int, j int)
       SERVER oss_serv
       OPTIONS (format 'csv', dir 'tt_csv/');

   

2. 导出数据

   向外表写入数据和向普通表写入数据一样，可以使用INSERT INTO语句进行写入。

       -- 批量导出数据到OSS外表（推荐）
       INSERT INTO foreign_x SELECT * FROM local_x;
       
       -- 插入值列表（不推荐）
       INSERT INTO foreign_x VALUES (1,1), (2,2), (3,3);x;

   




6.3 参数选项 
--------------------------

使用ADB PG外表进行数据导出时，外表的option可以参考[3. 创建 OSS Foreign Table](#section-2jc-50d-3zp)章节中的参数选项。

除此之外，用外表进行导出时，可以使用特有的option，如下所示：

**通用选项** 


|      选项       | 类型 | 单位 | 是否必选 | 默认值 |                                       备注                                       |
|---------------|----|----|------|-----|--------------------------------------------------------------------------------|
| fragment_size | 数值 | MB | 否    | 256 | 指定导出数据到OSS时，切分文件段的大小。当写入文件中的数据量大小超过表定义的fragment_size时，则会切分到新的文件段，向新文件段中继续写入数据。 |


**说明**

* fragment指的是导出时的一个文件分片，每个fragment都是完整且独立的文件，可以被ADB PG外表正常解析。同一行记录不会跨fragment存储。

  

* fragment文件大小不会严格按照fragment_size设置切分，一般略大于fragment_size设定。

  

* **若无特殊需求，直接使用默认值即可，无需修改fragment_size。**

  




fragment_size使用示例

    -- 创建一个外表，格式为CSV，导出目录为test/lineitem/, 指定framgment_size为512MB
    CREATE FOREIGN TABLE oss_lineitem (
        l_orderkey bigint,
        l_partkey bigint,
        l_suppkey bigint,
        l_linenumber bigint,
        l_quantity double precision,
        l_extendedprice double precision,
        l_discount double precision,
        l_tax double precision,
        l_returnflag CHAR(1),
        l_linestatus CHAR(1),
        l_shipdate DATE,
        l_commitdate DATE,
        l_receiptdate DATE,
        l_shipinstruct CHAR(25),
        l_shipmode CHAR(10),
        l_comment VARCHAR(44)
    ) server oss_serv
        options (
            dir 'test/lineitem/',
            format 'csv',
            fragment_size '512' -- 每当超过512MB时，切分新的文件段
        );
    
    -- 从本地表数据写入到外表中
    INSERT INTO oss_lineitem SELECT * FROM lineitem;



导出结束后，查看OSS上的文件，可以看到大部分文件分片大小略大于512MB。

    $ ossutil -e endpoint -i id -k key ls oss://bucket/test/lineitem
    LastModifiedTime                   Size(B)  StorageClass   ETAG                                  ObjectName
    2020-09-24 14:12:01 +0800 CST    536875660      Standard   ED6C68093E738D09B1386C5F00000000      oss://adbpg-tpch/test/lineitem/oss_lineitem2_20200924140843_1702924182_seg15_7.csv
    2020-09-24 14:12:27 +0800 CST    536875604      Standard   FD25FA7C7109ABCDCB386C5F00000000      oss://adbpg-tpch/test/lineitem/oss_lineitem2_20200924140843_1702924182_seg15_8.csv
    2020-09-24 14:12:53 +0800 CST    536875486      Standard   7C3EDE6AFE354190E5386C5F00000000      oss://adbpg-tpch/test/lineitem/oss_lineitem2_20200924140843_1702924182_seg15_9.csv
    2020-09-24 14:09:07 +0800 CST    536875626      Standard   48B38E65A5BB8B5B03386C5F00000000      oss://adbpg-tpch/test/lineitem/oss_lineitem2_20200924140843_1702924182_seg1_0.csv
    2020-09-24 14:09:32 +0800 CST    536875858      Standard   AF5525D81166F02D1C386C5F00000000      oss://adbpg-tpch/test/lineitem/oss_lineitem2_20200924140843_1702924182_seg1_1.csv
    2020-09-24 14:13:08 +0800 CST    235457368      Standard   BF1FC0B81376AE14F4386C5F00000000      oss://adbpg-tpch/test/lineitem/oss_lineitem2_20200924140843_1702924182_seg1_10.csv
    2020-09-24 14:09:56 +0800 CST    536875899      Standard   20C824EBCAE2C5DB34386C5F00000000      oss://adbpg-tpch/test/lineitem/oss_lineitem2_20200924140843_1702924182_seg1_2.csv
    ...
    Object Number is: 176
    $ ossutil -e endpoint -i id -k key du oss://adbpg-tpch/test/lineitem/
    storage class   object count            sum size(byte)
    ----------------------------------------------------------
    Standard       176                  89686183118
    ----------------------------------------------------------
    total object count: 176                  total object sum size: 89686183118
    total part count:   0                           total part sum size:   0
    
    total du size(byte):89686183118
    
    0.051899(s) elapsed



**csv/text选项** 
**注意**

以下选项若不特殊说明仅适用csv/text格式导出时使用，orc格式设置无效。


|       选项        | 类型  | 单位 | 是否必选 |  默认值  |                                 备注                                 |
|-----------------|-----|----|------|-------|--------------------------------------------------------------------|
| gzip_level      | 数值  | 无  | 否    | 1     | 指定导出CSV/TEXT数据到OSS时，使用的gzip压缩级别，最高压缩级别为9。                          |
| force_quote_all | 布尔型 | 无  | 否    | false | 指定导出CSV数据时，是否将所有字段都强制引用。 force_quote_all只对CSV格式有效。 |


**说明**

* gzip_level 仅在filetype设置为`gzip`时生效。

  

* zip_level 压缩级别越高，导出的数据文件越小，但导出时间越长。

  

* **根据测试结果，gzip_level设置越大，文件并没有明显降低，但导出时间明显增加，因此若无特殊需求，推荐gzip_level使用默认值1。**

  




gzip_level使用示例

    -- 创建一个外表，格式为CSV，导出目录为test/lineitem2/, 指定gzip_level为9
    CREATE FOREIGN TABLE oss_lineitem3 (
        l_orderkey bigint,
        l_partkey bigint,
        l_suppkey bigint,
        l_linenumber bigint,
        l_quantity double precision,
        l_extendedprice double precision,
        l_discount double precision,
        l_tax double precision,
        l_returnflag CHAR(1),
        l_linestatus CHAR(1),
        l_shipdate DATE,
        l_commitdate DATE,
        l_receiptdate DATE,
        l_shipinstruct CHAR(25),
        l_shipmode CHAR(10),
        l_comment VARCHAR(44)
    ) server oss_serv
        options (
            dir 'test/lineitem2/',
            format 'csv', filetype 'gzip' ,gzip_level '9'
        );
        
     -- 从本地表数据写入到外表中
    INSERT INTO oss_lineitem SELECT * FROM lineitem;



导出结束后，查看OSS上的文件，可以看到文件总大小为23GB (25141481880Byte)左右，远小于fragment_size使用示例中的83GB(89686183118 Byte)。

    $ ossutil -e endpoint -i id -k key ls oss://bucket/test/lineitem2
    2020-09-24 15:07:49 +0800 CST    270338060      Standard   9B1F7D7CB0748391C5456C5F00000000      oss://adbpg-tpch/test/lineitem2/oss_lineitem3_20200924145900_1220405754_seg8_3.csv.gz
    2020-09-24 15:10:08 +0800 CST    270467652      Standard   17DE92CAE57F64F550466C5F00000000      oss://adbpg-tpch/test/lineitem2/oss_lineitem3_20200924145900_1220405754_seg8_4.csv.gz
    2020-09-24 15:12:00 +0800 CST    219497966      Standard   FCFE4E457C0F6942C0466C5F00000000      oss://adbpg-tpch/test/lineitem2/oss_lineitem3_20200924145900_1220405754_seg8_5.csv.gz
    2020-09-24 15:01:10 +0800 CST    270617343      Standard   13AD24EF528ECDF836446C5F00000000      oss://adbpg-tpch/test/lineitem2/oss_lineitem3_20200924145900_1220405754_seg9_0.csv.gz
    2020-09-24 15:03:18 +0800 CST    270377032      Standard   759DBF9B999F8609B6446C5F00000000      oss://adbpg-tpch/test/lineitem2/oss_lineitem3_20200924145900_1220405754_seg9_1.csv.gz
    2020-09-24 15:05:28 +0800 CST    270284091      Standard   F9896A5CFF554F2838456C5F00000000      oss://adbpg-tpch/test/lineitem2/oss_lineitem3_20200924145900_1220405754_seg9_2.csv.gz
    2020-09-24 15:07:43 +0800 CST    270350284      Standard   C120BE98B47DAD5EBF456C5F00000000      oss://adbpg-tpch/test/lineitem2/oss_lineitem3_20200924145900_1220405754_seg9_3.csv.gz
    2020-09-24 15:10:04 +0800 CST    270477777      Standard   69B9B1E854B626364C466C5F00000000      oss://adbpg-tpch/test/lineitem2/oss_lineitem3_20200924145900_1220405754_seg9_4.csv.gz
    2020-09-24 15:12:00 +0800 CST    219358236      Standard   A4EB5DFFBD67AF6BC0466C5F00000000      oss://adbpg-tpch/test/lineitem2/oss_lineitem3_20200924145900_1220405754_seg9_5.csv.gz
    ....
    
    Object Number is: 96
    
    $ ossutil -e endpoint -i id -k key du oss://adbpg-tpch/test/lineitem/
    storage class   object count            sum size(byte)
    ----------------------------------------------------------
    Standard       96                   25141481880
    ----------------------------------------------------------
    total object count: 96                   total object sum size: 25141481880
    total part count:   0                           total part sum size:   0
    
    total du size(byte):25141481880
    
    0.037620(s) elapsed



force_quote_all使用示例

    -- 创建一个外表，格式为CSV，导出目录为test/lineitem/, 指定force_quote_all为true
    CREATE FOREIGN TABLE foreign_x (
      a int, b text
    ) server oss_serv
        options (
            dir 'test/x/',
            format 'csv', force_quote_all 'true'
        );
        
     -- 从本地表数据写入到外表中
    INSERT INTO foreign_x values(1, 'a'), (2, 'b');



导出完成后，查看OSS上的文件内容，可以看到每一列的值都被双引号引用。

    $ cat foreign_x_20200923173618_447789894_seg4_0.csv
    "1","a"
    $ cat foreign_x_20200923173618_447789894_seg5_0.csv
    "2","b"



**orc 选项** 
**注意**

以下选项若不特殊说明仅适用orc格式导出时使用，csv/text格式设置无效。


|       选项        | 类型 | 单位 | 是否必选 | 默认值 |                备注                 |
|-----------------|----|----|------|-----|-----------------------------------|
| orc_stripe_size | 数值 | MB | 否    | 64  | 指定导出ORC格式数据时，单个ORC文件中每个Stripe的大小。 |


**说明**

* orc_stripe_size越小，同等数据量导出生成的ORC文件越大，且导出时间越长。

  

* **如无特殊需求，一般不需要更改** **orc_stripe_size，直接使用默认值64MB即可。**

  




orc_stripe_size 使用示例

    -- 创建一个外表，格式为ORC，导出目录为test/lineitem/, orc_stripe_size为128MB
    CREATE FOREIGN TABLE oss_lineitem (
        l_orderkey bigint,
        l_partkey bigint,
        l_suppkey bigint,
        l_linenumber bigint,
        l_quantity double precision,
        l_extendedprice double precision,
        l_discount double precision,
        l_tax double precision,
        l_returnflag CHAR(1),
        l_linestatus CHAR(1),
        l_shipdate DATE,
        l_commitdate DATE,
        l_receiptdate DATE,
        l_shipinstruct CHAR(25),
        l_shipmode CHAR(10),
        l_comment VARCHAR(44)
    ) server oss_serv
    options ( dir 'test/lineitem/', format 'orc', orc_stripe_size '128');
    
     -- 从本地表数据写入到外表中
    INSERT INTO oss_lineitem SELECT * FROM lineitem;



![OSS外表导出数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9891382061/p174207.png)

导出完成后，查看导出的某个ORC文件的meta信息，可以看到ORC文件中的Stripe大致为128MB左右。

大文件切分 
--------------------------

OSS FDW支持多节点并行扫描。因此，当文件数过少时，推荐拆分多个源文件，以便于使用多节点并行扫描功能。

例如：Linux平台下，可以使用 **split** 命令对 `text `或 `csv `格式的文件按行拆分成多个小文件。
**说明**

切分后的小文件需要保证行完整性，即同一行记录只允许存在同一个文件内，不允许跨文件存放。因此，只能通过行切分。

    # 获取当前文件行数
    wc -l csv_file
    
    # 按照指定行数切分成多个小文件，其中，N表示切分后小文件的行数。
    split -l N csv_file





外表统计信息收集 
--------------------------

OSS 外表实际数据存储在OSS上，默认不进行数据的统计信息收集。因此针对复杂场景下的查询SQL（比如多张表关联操作）如果统计信息不存在或者信息过时，优化器可能会生成低效的查询计划。使用ANALYZE语句可以更新统计信息。执行方式如下：

    -- 使用EXPLAIN查看ANALYZE前的执行计划
    EXPLAIN 表名;
    
    -- 使用ANALYZE触发收集统计信息
    ANALYZE 表名;
    
    -- 使用EXPLAIN查看ANALYZE后的执行计划
    EXPLAIN 表名;



OSS 外表文件数与集群 Segment 节点数对应关系 
-------------------------------------------------

* AnalyticDB for PostgreSQL的每个数据分区（Segment）将按轮询方式并行对 OSS 上的数据文件进行读取。

  

* 推荐源数据文件的编码和数据库编码保持一致，减少编码转换，提高效率。数据库编码默认UTF-8。

  

* 对于 csv/text 格式的文本文件，支持多文件并行读取，默认并行数4。

  

* 文件的数目建议为数据节点核数（Segment 个数 × 单个Segment 核数）的整数倍，从而提升读取效率。

  




查看执行计划 
---------------------------

执行如下操作，查看OSS Foreign 表的执行计划。

    EXPLAIN SELECT COUNT(*) FROM oss_lineitem_orc WHERE l_orderkey > 14062498;


**说明**

`EXPLAIN VERBOSE`可查看更多信息。

查看 OSS 文件信息 
--------------------------------

执行如下操作，获取指定 OSS Foreign 表的文件信息。

    SELECT * FROM get_oss_table_meta('<OSS FOREIGN TABLE>');



ORC - ADB PG 数据类型对照表 
-----------------------------------------

ORC - ADB for PG 数据类型对照如下：


| ADB PG数据类型 |   ORC数据类型    |
|------------|--------------|
| bool       | BOOLEAN      |
| int2       | SHORT        |
| int4       | INT          |
| int8       | LONG         |
| float4     | FLOAT        |
| float8     | DOUBLE       |
| numeric    | DECIMAL      |
| char       | CHAR         |
| text       | STRING       |
| bytea      | BINARY       |
| timestamp  | TIMESTAMP    |
| date       | DATE         |
| int2\[\]   | LIST(SHORT)  |
| int4\[\]   | LIST(INT)    |
| int8\[\]   | LIST(LONG)   |
| float4\[\] | LIST(FLOAT)  |
| float8\[\] | LIST(DOUBLE) |




**说明**

对于ORC的List类型，目前只支持转换为ADB for PG 的一维数组。

PARQUET - ADB PG 数据类型对照表 
---------------------------------------------

PARQUET格式文件数据类型包含primitive types和logical types，针对这两种情况，建表时推荐使用下面列举的ADB PG类型对应到PARQUET类型，如果不对应会尝试转换。
**说明**

不支持PARQUET中的嵌套类型：ARRAY、MAP。

在PARQUET文件中没有提供logical types情况下，数据类型对照如下：


| ADB PG数据类型 |     PARQUER数据类型      |
|------------|----------------------|
| bool       | BOOLEAN              |
| integer    | INT32                |
| bigint     | INT64                |
| timestamp  | INT96                |
| float4     | FLOAT                |
| float8     | DOUBLE               |
| bytea      | BYTE_ARRAY           |
| bytea      | FIXED_LEN_BYTE_ARRAY |



在PARQUET文件中提供了logical types情况下，数据类型对照如下： 


| ADB PG类型  |    PARQUET类型     |
|-----------|------------------|
| text      | STRING           |
| date      | DATE             |
| timestamp | TIMESTAMP        |
| time      | TIME             |
| interval  | INTERVAL         |
| numeric   | DECIMAL          |
| smallint  | INT(8)/INT(16)   |
| integer   | INT(32)          |
| bigint    | INT(64)          |
| bigint    | UINT(8/16/32/64) |
| json      | JSON             |
| jsonb     | BSON             |
| uuid      | UUID             |
| text      | ENUM             |





OSS FDW 访问 SLS 投递数据 
----------------------------------------

ADB PG OSS FDW 分区表更常见的一种使用场景是搭配 SLS 一起使用。SLS 是阿里巴巴集团经历大量大数据场景锤炼而成，针对日志类数据的一站式服务，关于 SLS 的更多介绍可参考 [什么是日志服务](/intl.zh-CN/产品简介/什么是日志服务.md)。

如果想基于 SLS 投递到 OSS 上的数据建立 OSS 分区外表，那么只需要在 SLS 投递 OSS 相关配置中按照 SLS 文档设置好"分区格式"这一配置项即可，如下图所示。可参考 [将日志服务数据投递到OSS](/intl.zh-CN/消费与投递/数据投递/投递日志到OSS/将日志服务数据投递到OSS.md) 了解这里各个配置项的语义。

![OSS FDW 访问 SLS 投递数据](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9738398951/p143566.png)

图中配置会将归属于同一月的日志都放在 OSS 同一目录下。该配置生成的 OSS 目录布局如下所示：

    oss://oss-fdw-test/adbpgossfdw
    ├── date=202002
    │   ├── userlogin_1585617629106546791_647504382.csv
    │   └── userlogin_1585617849232201154_647507440.csv
    └── date=202003
        └── userlogin_1585617944247047796_647508762.csv



之后再结合 SLS 投递上来的文件中包含的字段建立如下 OSS 分区外表：

    CREATE FOREIGN TABLE userlogin (
            uid integer,
            name character varying,
            source integer,
            logindate timestamp without time zone,
            "date" int
    ) SERVER oss_serv OPTIONS (
        dir 'adbpgossfdw/',
        format 'text'
    )
    PARTITION BY LIST ("date")
    (
            VALUES ('202002'), 
            VALUES ('202003')       
    )



最后便可以在此基础之上做一下业务所需的分析逻辑，如：分析2020年2月份，所有用户的登录次数。

    adbpg=# explain select uid, count(uid) from userlogin where "date" = 202002 group by uid;
                                                                                QUERY PLAN                                                                             
    -------------------------------------------------------------------------------------------------------------------------------------------------------------------
     Gather Motion 3:1  (slice2; segments: 3)  (cost=5135.10..5145.10 rows=1000 width=12)
       ->  HashAggregate  (cost=5135.10..5145.10 rows=334 width=12)
             Group Key: userlogin_1_prt_1.uid
             ->  Redistribute Motion 3:3  (slice1; segments: 3)  (cost=5100.10..5120.10 rows=334 width=12)
                   Hash Key: userlogin_1_prt_1.uid
                   -> HashAggregate  (cost=5100.10..5100.10 rows=334 width=12)
                         Group Key: userlogin_1_prt_1.uid
                         ->t;  Append  (cost=0.00..100.10 rows=333334 width=4)
                               ->  Foreign Scan on userlogin_1_prt_1  (cost=0.00..100.10 rows=333334 width=4)
                                     Filter: (date = 202002)
                                     Oss Url: endpoint=oss-cn-hangzhou-zmf-internal.aliyuncs.com bucket=adbpg-regress dir=adbpgossfdw/date=202002/ filetype=plain|text
                                     Oss Parallel (Max 4) Get: total 0 file(s) with 0 bytes byte(s).
     Optimizer: Postgres query optimizer
    (13 rows)



可以看到这里 OSS FDW 只会从 OSS 处取出 date=202002 对应的数据，而不会下载 date=202003 月份的数据，需要拉取数据量的减小可以大幅提升查询的执行效率。

常见错误处理 
---------------------------

Oss Foreign 外表扫描过程中，当出现形如：

"oss server returned error: StatusCode=...，ErrorCode=...，ErrorMessage="..."，RequestId=..."

的错误信息提示时，其中：

* StatusCode：出错请求的HTTP状态码。

  

* ErrorCode：OSS的错误码。

  

* ErrorMessage：OSS的错误信息。

  

* RequestId：标识该次请求的UUID。当您无法解决问题时，可以提供RequestId寻求OSS开发工程师帮助。

  




请参见[OSS错误响应](/intl.zh-CN/开发指南/错误处理/错误响应.md)中的文档了解和处理各类错误。

与 OSS External Table 区别 
--------------------------------------------

* ADB PG之前已支持OSS External Table用于数据导入导出，但是用于OSS数据分析的话在大数据量场景下性能无法达到预期。

  

* OSS Foreign Table基于PostgreSQL Foreign Data Wrapper（简称PG FDW）框架开发，支持orc，csv （支持gzip压缩）文件格式，同时支持按一个或多个字段进行分区，支持外表统计信息收集帮助优化器生成最优执行计划。

  

* Foreign Table整体在性能，功能和稳定性上都优于External Table。同时Greenplum社区本身接下来也计划用Foreign Table代替External Table。

  




参考文档 
-------------------------

* [开始使用阿里云OSS](/intl.zh-CN/快速入门/开始使用OSS.md)

  

* [OSS访问域名使用规则](/intl.zh-CN/开发指南/访问域名（Endpoint）/OSS访问域名使用规则.md)

  

* [错误处理](/intl.zh-CN/SDK 示例/C/错误处理.md)

  

* [OSS错误响应](/intl.zh-CN/开发指南/错误处理/错误响应.md)

  




