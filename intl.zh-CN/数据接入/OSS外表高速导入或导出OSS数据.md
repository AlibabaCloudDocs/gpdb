# OSS外表高速导入或导出OSS数据

OSS为阿里云对象存储服务，云原生数据仓库AnalyticDB PostgreSQL版支持通过OSS外部表（即gpossext功能），将数据并行从OSS云存储 导入或导出到OSS云存储，并支持通过gzip进行OSS外部表文件压缩，大量节省存储空间及成本。

目前的gpossext支持读写text/csv格式的文件或者gzip 压缩格式的text/csv文件。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3459611951/p51404.png)

本文内容包括：

-   [操作说明](#section_cpw_pyr_52b)
-   [参数释义](#section_fqw_pyr_52b)
-   [使用示例](#section_crw_pyr_52b)
-   [注意事项](#section_ssw_pyr_52b)
-   [TEXT/CSV格式说明](#section_usw_pyr_52b)
-   [SDK错误处理](#section_dtw_pyr_52b)
-   [常见问题](#section_ftw_pyr_52b)
-   [参考文档](#section_msd_qzr_52b)

## 操作说明

通过AnalyticDB PostgreSQL使用OSS外部表，主要涉及以下操作。

-   [创建OSS外部表插件（oss\_ext）](#p_v4r_g1s_52b)
-   [并行导入数据](#p_rvb_h1s_52b)
-   [并行导出数据](#p_pql_h1s_52b)
-   [创建 OSS 外部表语法](#p_mjr_h1s_52b)

**创建 OSS 外部表插件（oss\_ext）**

使用OSS外部表时，需要在AnalyticDB PostgreSQL中先创建OSS外部表插件（每个数据库需要单独创建）。

-   创建命令为：`CREATE EXTENSION IF NOT EXISTS oss_ext;`
-   删除命令为：`DROP EXTENSION IF EXISTS oss_ext;`

**并行导入数据**

导入数据时，请执行如下步骤：

1.  将数据均匀分散存储在多个OSS文件中。

    **说明：** AnalyticDB PostgreSQL的每个数据分区（segment）将按轮询方式并行对OSS上的数据文件进行读取，文件的数目建议为数据节点数（Segment个数）的整数倍，从而提升读取效率。

2.  在AnalyticDB PostgreSQL中，创建READABLE外部表。
3.  执行如下操作，并行导入数据。

    ```
    INSERT INTO <目标表> SELECT * FROM <外部表>
    ```


**并行导出数据**

导出数据时，请执行如下步骤：

1.  在AnalyticDB PostgreSQL中，创建WRITABLE外部表。
2.  执行如下操作，并行把数据导出到OSS中。

    ```
    INSERT INTO <外部表> SELECT * FROM <源表>
    ```


**创建 OSS 外部表语法**

创建OSS外部表语法，请执行如下命令：

```
CREATE [READABLE] EXTERNAL TABLE tablename
( columnname datatype [, ...] | LIKE othertable )
LOCATION ('ossprotocol')
FORMAT 'TEXT'
            [( [HEADER]
               [DELIMITER [AS] 'delimiter' | 'OFF']
               [NULL [AS] 'null string']
               [ESCAPE [AS] 'escape' | 'OFF']
               [NEWLINE [ AS ] 'LF' | 'CR' | 'CRLF']
               [FILL MISSING FIELDS] )]
           | 'CSV'
            [( [HEADER]
               [QUOTE [AS] 'quote']
               [DELIMITER [AS] 'delimiter']
               [NULL [AS] 'null string']
               [FORCE NOT NULL column [, ...]]
               [ESCAPE [AS] 'escape']
               [NEWLINE [ AS ] 'LF' | 'CR' | 'CRLF']
               [FILL MISSING FIELDS] )]
[ ENCODING 'encoding' ]
[ [LOG ERRORS [INTO error_table]] SEGMENT REJECT LIMIT count
       [ROWS | PERCENT] ]
CREATE WRITABLE EXTERNAL TABLE table_name
( column_name data_type [, ...] | LIKE other_table )
LOCATION ('ossprotocol')
FORMAT 'TEXT'
               [( [DELIMITER [AS] 'delimiter']
               [NULL [AS] 'null string']
               [ESCAPE [AS] 'escape' | 'OFF'] )]
          | 'CSV'
               [([QUOTE [AS] 'quote']
               [DELIMITER [AS] 'delimiter']
               [NULL [AS] 'null string']
               [FORCE QUOTE column [, ...]] ]
               [ESCAPE [AS] 'escape'] )]
[ ENCODING 'encoding' ]
[ DISTRIBUTED BY (column, [ ... ] ) | DISTRIBUTED RANDOMLY ]
ossprotocol:
   oss://oss_endpoint prefix=prefix_name
    id=userossid key=userosskey bucket=ossbucket compressiontype=[none|gzip] async=[true|false]
ossprotocol:
   oss://oss_endpoint dir=[folder/[folder/]...]/file_name
    id=userossid key=userosskey bucket=ossbucket compressiontype=[none|gzip] async=[true|false]
ossprotocol:
   oss://oss_endpoint filepath=[folder/[folder/]...]/file_name
    id=userossid key=userosskey bucket=ossbucket compressiontype=[none|gzip] async=[true|false]
```

## 参数释义

该部分介绍各操作中用到的参数定义，涉及到参数包括：

-   [常用参数](#p_kt5_dzr_52b)
-   [导入模式参数](#p_xrm_fzr_52b)
-   [导出模式参数](#p_odc_3zr_52b)
-   [其他通用参数](#p_oqg_wzr_52b)

**常用参数**

-   协议和endpoint：格式为`协议名://oss_endpoint`，其中协议名为oss，oss\_endpoint为OSS对应区域的域名。

    **说明：** 如果是从阿里云的主机访问数据库，应该使用内网域名（即带有“internal”的域名），避免产生公网流量。

-   id：OSS 账号的 ID。
-   key：OSS 账号的 key。
-   bucket：指定数据文件所在的 bucket，需要通过 OSS 预先创建。
-   prefix：指定数据文件对应路径名的前缀，不支持正则表达式，仅是匹配前缀，且与 filepath、dir 互斥，三者只能设置其中一个。
    -   如果创建的是用于数据导入的READABLE外部表，则在导入时含有这一前缀的所有OSS文件都会被导入。
        -   如果指定 prefix=test/filename，以下文件都会被导入：
            -   test/filename
            -   test/filenamexxx
            -   test/filename/aa
            -   test/filenameyyy/aa
            -   test/filenameyyy/bb/aa
        -   如果指定 prefix=test/filename/，只有以下文件会被导入（上面列的其他文件不会被导入）：
            -   test/filename/aa
    -   如果创建的是用于数据导出的WRITABLE外部表，在导出数据时，将根据该前缀自动生成一个唯一的文件名来给导出文件命名。
-   dir：OSS中的虚拟文件夹路径，与prefix、filepath互斥，三者只能设置其中一个。
    -   文件夹路径需要以“/”结尾，如`test/mydir/`。
    -   在导入数据时，使用此参数创建外部表，会导入指定虚拟目录下的所有文件，但不包括它子目录和子目录下的文件。与 filepath 不同，dir 下的文件没有命名要求。
    -   在导出数据时，使用此参数创建外部表，所有数据会导出到此目录下的多个文件中，输出文件名的形式为`filename.x`，x 为数字，但可能不是连续的。
-   filepath：OSS中包含路径的文件名称，与prefix、dir互斥，三者只能设置其中一个，并且这个参数只能在创建READABLE外部表时指定（即只支持在导入数据时使用）。
    -   该文件名称包含该路径，但不包含bucket名。
    -   在导入数据时，文件命名方式必须为`filename`或`filename.x`，x要求从1开始，且是连续的。例如，如果指定filepath = filename，而OSS中含有如下文件：

        ```
        filename
        filename.1
        filename.2
        filename.4，
        ```

        则将被导入的文件有filename、filename.1和filename.2。而因为filename.3不存在，所以filename.4不会被导入。


**导入模式参数**

-   async：是否启用异步模式导入数据。
    -   开启辅助线程从 OSS 导入数据，加速导入性能。
    -   默认情况下异步模式是打开的，如果需要关掉，可以使用参数 async = false 或 async = f。
    -   异步模式和普通模式比，会消耗更多的硬件资源。
-   compressiontype：导入的文件的压缩格式。
    -   指定为 none（缺省值），说明导入的文件没经过压缩。
    -   指定为 gzip，则导入的格式为 gzip。目前仅支持 gzip 压缩格式。
-   compressionlevel：设置写入 OSS 的文件的压缩等级，取值范围为 1 - 9，默认值为 6

**导出模式参数**

-   oss\_flush\_block\_size：单次刷出数据到 OSS 的 buffer 大小，默认为 32 MB，可选范围是 1 到 128 MB。
-   oss\_file\_max\_size：设置写入到 OSS 的最大文件大小，超出之后会切换到另一个文件继续写。默认为 1024 MB，可选范围是 8 MB 到 4000 MB。
-   num\_parallel\_worker： 设置写入 OSS 的压缩数据的并行压缩线程个数，取值范围为 1 - 8，默认值为3。
-   compressiontype：导出文件的压缩格式。
    -   指定为 none（缺省值），说明导出的文件没经过压缩。
    -   指定为 gzip，则导出的格式为 gzip。目前仅支持 gzip 压缩格式。

另外，针对导出模式，有如下注意事项：

-   WRITABLE 是导出模式外部表的关键字，创建外部表时需要明确指明。
-   导出模式目前只支持 prefix 和 dir 参数模式，不支持 filepath。
-   导出模式的 DISTRIBUTED BY 子句可以使数据节点（Segment）按指定的分布键将数据写入 OSS。

**其他通用参数**

针对导入模式和导出模式，还有下列容错相关的参数：

-   oss\_connect\_timeout：设置链接超时，单位为秒，默认是 10 秒。
-   oss\_dns\_cache\_timeout：设置 DNS 超时，单位为秒，默认是 60 秒。
-   oss\_speed\_limit：设置能容忍的最小速率，默认是 1024，即 1 KB。
-   oss\_speed\_time：设置能容忍的最长时间，默认是 15 秒。

上述参数如果使用默认值，则如果连续 15 秒的传输速率小于 1 KB，就会触发超时。详细描述请参见 [OSS SDK 错误处理](https://www.alibabacloud.com/help/zh/doc-detail/32141.html)。

其他参数兼容Greenplum EXTERNAL TABLE的原有语法，具体语法解释请参见 [Greenplum 外部表语法官方文档](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_EXTERNAL_TABLE.html)。这部分参数主要有：

-   FORMAT：支持文件格式，支持 text、csv 等。
-   ENCODING：文件中数据的编码格式，如 utf8。
-   LOG ERRORS：指定该子句可以忽略掉导入中出错的数据，将这些数据写入error\_table，并可以使用count参数指定报错的阈值。

    **说明：**

    -   通过`LOG ERRORS`将错误行信息记录到内部关联文件。

        ```
        create readable external table ossexample
                (date text, time text, open float, high float,
                low float, volume int)
                location('oss://oss-cn-hangzhou.aliyuncs.com
                prefix=osstest/example id=XXX
                key=XXX bucket=testbucket compressiontype=gzip')
                FORMAT 'csv' (QUOTE '''' DELIMITER E'\t')
                ENCODING 'utf8'
                LOG ERRORS SEGMENT REJECT LIMIT 5;
        ```

    -   通过函数`gp_read_error_log('external_table_name')`可以读取错误行信息。

        ```
        select * from gp_read_error_log('external_table_name');
        ```

    -   内部文件随外表删除而删除，也可以通过函数`gp_truncate_error_log('external_table_name')`删除。

        ```
        select gp_truncate_error_log('external_table_name');
        ```

    -   同时4.3版本也支持通过`LOG ERRORS INTO error_table`语法指定错误表，其他版本不再支持。

        ```
        create readable external table ossexample
                (date text, time text, open float, high float,
                low float, volume int)
                location('oss://oss-cn-hangzhou.aliyuncs.com
                prefix=osstest/example id=XXX
                key=XXX bucket=testbucket compressiontype=gzip')
                FORMAT 'csv' (QUOTE '''' DELIMITER E'\t')
                ENCODING 'utf8'
                LOG ERRORS INTO my_error_rows SEGMENT REJECT LIMIT 5;
        ```


## 使用示例

```
#  创建 OSS 导入外部表
create readable external table ossexample
        (date text, time text, open float, high float,
        low float, volume int)
        location('oss://oss-cn-hangzhou.aliyuncs.com
        prefix=osstest/example id=XXX
        key=XXX bucket=testbucket compressiontype=gzip')
        FORMAT 'csv' (QUOTE '''' DELIMITER E'\t')
        ENCODING 'utf8'
        LOG ERRORS SEGMENT REJECT LIMIT 5;
create readable external table ossexample
        (date text, time text, open float, high float,
        low float, volume int)
        location('oss://oss-cn-hangzhou.aliyuncs.com
        dir=osstest/ id=XXX
        key=XXX bucket=testbucket')
        FORMAT 'csv'
        LOG ERRORS SEGMENT REJECT LIMIT 5;
create readable external table ossexample
        (date text, time text, open float, high float,
        low float, volume int)
        location('oss://oss-cn-hangzhou.aliyuncs.com
        filepath=osstest/example.csv id=XXX
        key=XXX bucket=testbucket')
        FORMAT 'csv'
        LOG ERRORS SEGMENT REJECT LIMIT 5;
# 创建 OSS 导出外部表
create WRITABLE external table ossexample_exp
        (date text, time text, open float, high float,
        low float, volume int)
        location('oss://oss-cn-hangzhou.aliyuncs.com
        prefix=osstest/exp/outfromhdb id=XXX
        key=XXX bucket=testbucket') FORMAT 'csv'
        DISTRIBUTED BY (date);
create WRITABLE external table ossexample_exp
        (date text, time text, open float, high float,
        low float, volume int)
        location('oss://oss-cn-hangzhou.aliyuncs.com
        dir=osstest/exp/ id=XXX
        key=XXX bucket=testbucket') FORMAT 'csv'
        DISTRIBUTED BY (date);
# 创建堆表，数据就装载到这张表中
create table example
        (date text, time text, open float,
         high float, low float, volume int)
         DISTRIBUTED BY (date);
# 数据并行地从 ossexample 装载到 example 中
insert into example select * from ossexample;
# 数据并行地从 example 导出到 oss
insert into ossexample_exp select * from example;
# 从下面的执行计划中可以看出，每个 Segment 都会参与工作。
# 每个 Segment 从 OSS 并行拉取数据，然后通过 Redistribution Motion 这个执行节点将拿到的数据 HASH 计算后分发给对应的 Segment，接受数据的 Segment 通过 Insert 执行节点进行入库。
explain insert into example select * from ossexample;
                                            QUERY PLAN
-----------------------------------------------------------------------------------------------
 Insert (slice0; segments: 4)  (rows=250000 width=92)
   ->  Redistribute Motion 4:4  (slice1; segments: 4)  (cost=0.00..11000.00 rows=250000 width=92)
         Hash Key: ossexample.date
         ->  External Scan on ossexample  (cost=0.00..11000.00 rows=250000 width=92)
(4 rows)
# 从下面的查询计划可以看到，Segment 把本地数据直接导出到 OSS ,没有进行数据重分布
explain insert into ossexample_exp select * from example;
                          QUERY PLAN
---------------------------------------------------------------
 Insert (slice0; segments: 3)  (rows=1 width=92)
   ->  Seq Scan on example  (cost=0.00..0.00 rows=1 width=92)
(2 rows)
```

## 注意事项

-   创建和使用外部表的语法，除了 location 相关的参数，其余部分和 Greenplum 相同。
-   数据导入的性能和 AnalyticDB for PostgreSQL 集群的资源（CPU、IO、内存、网络等）相关，也和 OSS 相关。为了获取最大的导入性能，建议在创建表时，使用列式存储 + 压缩功能。例如，指定子句“WITH \(APPENDONLY=true, ORIENTATION=column, COMPRESSTYPE=zlib, COMPRESSLEVEL=5, BLOCKSIZE=1048576\)”，详情请参见 [Greenplum Database 表创建语法官方文档](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_TABLE.html)。
-   为了保证数据导入的性能，ossendpoint Region 需要匹配 AnalyticDB for PostgreSQL 云上所在 Region，建议 OSS AnalyticDB for PostgreSQL 在同一个 Region 内以获得最好的性能。

## TEXT/CSV 格式说明

下列几个参数可以在外表 DDL 参数中指定，用于规定读写 OSS 的文件格式：

-   TEXT/CSV 行分割符号是 ‘\\n’ ，也就是换行符。
-   DELIMITER 用于定义列的分割符：
    -   当用户数据中包括 DELIMITER 时，则需要和 QUOTE 参数一同使用。
    -   推荐的列分割符有 ‘,’、‘\\t‘ 、‘\|’ 或一些不常出现的字符。
-   QUOTE 以列为单位包裹有特殊字符的用户数据。
    -   用户包含有特殊字符的字符串会被 QUOTE 包裹，用于区分用户数据和控制字符。
    -   如果不必要，例如整数，基于优化效率的考虑，不必使用 QUOTE 包裹数据。
    -   QUOTE 不能和 DELIMITER 相同，默认 QUOTE 是双引号。
    -   当用户数据中包含了 QUOTE 字符，则需要使用转义字符 ESCAPE 加以区分。
-   ESCAPE 特殊字符转义
    -   转义字符出现在需要转义的特殊字符前，表示它不是一个特殊字符。
    -   ESCAPE 默认和 QUOTE 相同，也就是双引号。
    -   也支持设置成 ‘\\’（MySQL默认的转义字符）或别的字符。

**典型的 TEXT/CSV 默认控制字符**

|控制字符 \\ 格式|TEXT|CSV|
|----------|----|---|
|DELIMITER（列分割符）|\\t （tab）|, （comma）|
|QUOTE（摘引）|" （double-quote）|"（double-quote）|
|ESCAPE（转义）|（不适用）|和 QUOTE 相同|
|NULL（空值）|\\N （backslash-N）|（无引号的空字符串）|

**说明：** 所有的控制字符都必须是单字节字符。

## SDK 错误处理

当导入或导出操作出错时，错误日志可能会出现如下信息：

-   code：出错请求的 HTTP 状态码。
-   error\_code：OSS 的错误码。
-   error\_msg：OSS 的错误信息。
-   req\_id：标识该次请求的 UUID。当您无法解决问题时，可以凭 req\_id 来请求 OSS 开发工程师的帮助。

详情请参见[OSS API 错误响应](https://www.alibabacloud.com/help/zh/doc-detail/32005.html)，超时相关的错误可以使用 oss\_ext 相关参数处理。

## 常见问题

如果导入过慢，请参见上面“注意事项”中关于导入性能的描述。

## 参考文档

-   [OSS endpoint 信息](https://www.alibabacloud.com/help/zh/doc-detail/31834.html)
-   [OSS help 页面](https://www.alibabacloud.com/help/zh/product/31815.htm)
-   [OSS SDK 错误处理](https://www.alibabacloud.com/help/zh/doc-detail/32141.html)
-   [OSS API 错误响应](https://www.alibabacloud.com/help/zh/doc-detail/32005.html)
-   [Greenplum Database 外部表语法官方文档](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_EXTERNAL_TABLE.html)
-   [Greenplum Database 表创建语法官方文档](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_TABLE.html)

