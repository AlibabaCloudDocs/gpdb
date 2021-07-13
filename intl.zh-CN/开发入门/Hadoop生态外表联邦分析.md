# Hadoop生态外表联邦分析

云原生数据仓库 AnalyticDB PostgreSQL （简称 ADB PG）支持访问 Hadoop 生态的外部数据源。

**说明：**

-   本特性只支持存储弹性模式实例，且需要ADB PG实例和目标访问的外部数据源处于同一个VPC网络。
-   2020年9月6日前申请的存量存储弹性模式实例，由于网络架构不同，无法与外部 Hadoop 生态的数据源网络打通，无法使用该特性。如需使用，请联系后台技术人员，重新申请实例，迁移数据。

![Hadoop生态外表联邦分析](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4359700061/p166346.png)

## 前提条件：配置SERVER端

由于不同用户的配置需求不尽相同，如果您需要访问 Hadoop 生态的外部数据源进行联邦分析，请[提交工单](https://workorder.console.aliyun.com/console.htm#/ticket/add?productCode=gpdb)由ADB PG后台技术人员进行配置。以下为提交工单时需要提交的对应文件。

|连接对象|提交工单内容|
|----|------|
|Hadoop\(HDFS, HIVE, HBase\)|core-site.xml 、hdfs-site.xml 、mapred-site.xml 、yarn-site.xml、hive-site.xml**说明：** Kerberos认证时还需提供 keytab、krb5.conf等配置文件 |

## 基本语法

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
    |path-to-data|外表访问的目录，文件名，表名例如 //data/pxf\_examples/pxf\_hdfs\_simple.txt|
    |PROFILE \[&<custom-option\>=<value\>\[...\]\]|PXF访问外部数据的配置。支持格式包括：Jdbc \| hdfs:text \| hdfs:text:multi \| hdfs:avro \| hdfs:json \| hdfs:parquet \| hdfs:AvroSequenceFile \| hdfs:SequenceFile \| HiveText \| HiveRC \| HiveORC \| HiveVectorizedORC \| HBase |
    |FORMAT '\[TEXT\|CSV\|CUSTOM\]'|读取数据的格式。取值范围：TEXT、CSV、CUSTOM。|
    |formatting-properties|与特定文件数据对应的格式化选项： formatter 或者 delimiter（分割符）    -   与CUSTOM搭配
        -   formatter='pxfwritable\_import'
        -   formatter='pxfwritable\_export'
    -   与TEXT\|CSV搭配

        -   delimiter=E'\\t'
        -   delimiter ':'
**说明：** escape 时需要加上 E。 |
    |SERVER|配置服务端文件的位置，该部分由后台技术人员操作后反馈给用户。    -   postgresql
    -   hdp3 |


## 访问HDFS数据

支持格式：

|数据格式|PROFILE|
|----|-------|
|text|hdfs:text|
|csv|hdfs:text:multi 、hdfs:text|
|Avro|hdfs:avro|
|JSON|hdfs:json|
|Parquet|hdfs:parquet|
|AvroSequenceFile|hdfs:AvroSequenceFile|
|SequenceFile|hdfs:SequenceFile|

`FORMAT`与`formatting-properties`请参见[创建外表](#dt_4co_gq6_9ra)部分 。

-   **示例 HDFS访问文件**

    HDFS构建测试数据文件 `pxf_hdfs_simple.txt`。

    ```
    echo 'Prague,Jan,101,4875.33
    Rome,Mar,87,1557.39
    Bangalore,May,317,8936.99
    Beijing,Jul,411,11600.67' > /tmp/pxf_hdfs_simple.txt
    
    # 构建目录
    hdfs dfs -mkdir -p /data/pxf_examples
    # 推文件至hdfs
    hdfs dfs -put /tmp/pxf_hdfs_simple.txt /data/pxf_examples/
    
    # 查看文件
    hdfs dfs -cat /data/pxf_examples/pxf_hdfs_simple.txt
    ```

    ADB PG实例创建外表并访问

    ```
    postgres=# CREATE EXTERNAL TABLE pxf_hdfs_textsimple(location text, month text, num_orders int, total_sales float8)
                LOCATION ('pxf://data/pxf_examples/pxf_hdfs_simple.txt?PROFILE=hdfs:text&SERVER=hdp3')
              FORMAT 'TEXT' (delimiter=E',');
    
    postgres=# select * from pxf_hdfs_textsimple;
     location  | month | num_orders |    total_sales
    -----------+-------+------------+--------------------
     Prague    | Jan   |        101 | 4875.3299999999999
     Rome      | Mar   |         87 | 1557.3900000000001
     Bangalore | May   |        317 | 8936.9899999999998
     Beijing   | Jul   |        411 |           11600.67
    (4 rows)
    ```

    LOCATION各字段含义说明：

    -   pxf:// ：pxf 协议，固定值。
    -   data/pxf\_examples/pxf\_hdfs\_simple.txt：代表HDFS中的 pxf\_hdfs\_simple.txt 文件。
    -   PROFILE=hdfs:text: 代表使用 PROFILE=hdfs:text 访问HDFS。
    -   SERVER=hdp3：后台技术人员会提供该选项。代表使用PXF\_SERVER/hdp3/下的配置文件来支持PXF访问 HDFS。
    -   FORMAT 'TEXT' \(delimiter=E','\) ：外部数据源格式配置项，代表以`,`分割符，格式为`TEXT`。
    详细字段说明请参见[基本语法](#section_prh_36f_6cz)。


-   **示例：向HDFS写入`(TEXT,CSV)`**

    前提条件：在HDFS上构建数据目录/data/pxf\_examples/pxfwritable\_hdfs\_textsimple1。

    ```
    hdfs dfs -mkdir -p /data/pxf_examples/pxfwritable_hdfs_textsimple1
    ```

    **说明：** 在ADB PG执行insert的用户需要有HDFS该目录的写入权限。

    1.  ADB PG实例上创建可写外表。

        ```
        CREATE WRITABLE EXTERNAL TABLE pxf_hdfs_writabletbl_1(location text, month text, num_orders int, total_sales float8)
                    LOCATION ('pxf://data/pxf_examples/pxfwritable_hdfs_textsimple1?PROFILE=hdfs:text&SERVER=hdp3')
                  FORMAT 'TEXT' (delimiter=',');
                  
        INSERT INTO pxf_hdfs_writabletbl_1 VALUES ( 'Frankfurt', 'Mar', 777, 3956.98 );
        INSERT INTO pxf_hdfs_writabletbl_1 VALUES ( 'Cleveland', 'Oct', 3812, 96645.37 );
        ```

    2.  HDFS查看。

        ```
        #查看文件
        hdfs dfs -ls /data/pxf_examples/pxfwritable_hdfs_textsimple1
        
        #查看数据
        hdfs dfs -cat /data/pxf_examples/pxfwritable_hdfs_textsimple1/*
        Frankfurt,Mar,777,3956.98
        Cleveland,Oct,3812,96645.37
        ```


## 访问Hive数据

|数据格式|PROFILE|
|----|-------|
|TextFile|Hive, HiveText|
|SequenceFile|Hive|
|RCFile|Hive, HiveRC|
|ORC|Hive, HiveORC, HiveVectorizedORC|
|Parquet|Hive|

FORMAT与 <formatting-properties\>请参见[创建外表](#dt_4co_gq6_9ra)部分。

-   **示例 Hive**

    1.  产生数据。

        ```
        echo 'Prague,Jan,101,4875.33
        Rome,Mar,87,1557.39
        Bangalore,May,317,8936.99
        Beijing,Jul,411,11600.67
        San Francisco,Sept,156,6846.34
        Paris,Nov,159,7134.56
        San Francisco,Jan,113,5397.89
        Prague,Dec,333,9894.77
        Bangalore,Jul,271,8320.55
        Beijing,Dec,100,4248.41' > /tmp/pxf_hive_datafile.txt
        ```

    2.  Hive 创建table。

        ```
        hive> CREATE TABLE sales_info (location string, month string,
                number_of_orders int, total_sales double)
                ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
                STORED AS textfile;
        
        hive> LOAD DATA LOCAL INPATH '/tmp/pxf_hive_datafile.txt'
                INTO TABLE sales_info;
        hive> SELECT * FROM sales_info;
        ```

    3.  ADB PG实例访问数据

        ```
        postgres=# create extension pxf;
        
        postgres=# CREATE EXTERNAL TABLE salesinfo_hiveprofile(location text, month text, num_orders int, total_sales float8)
        LOCATION ('pxf://default.sales_info?PROFILE=Hive&SERVER=hdp3')
        FORMAT 'custom' (formatter='pxfwritable_import');
        
        postgres=# SELECT * FROM salesinfo_hiveprofile;
           location    | month | num_orders | total_sales
        ---------------+-------+------------+-------------
         Prague        | Jan   |        101 |     4875.33
         Rome          | Mar   |         87 |     1557.39
         Bangalore     | May   |        317 |     8936.99
         Beijing       | Jul   |        411 |    11600.67
         San Francisco | Sept  |        156 |     6846.34
         Paris         | Nov   |        159 |     7134.56
        ......
        ```

    LOCATION各字段含义说明：

    -   pxf://：pxf 协议，固定值。
    -   default.sales\_info：代表 Hive中default数据库下的 sales\_info 表。
    -   PROFILE=Hive：代表使用PROFILE=Hive 访问 Hive。
    -   SERVER=hdp3：后台技术人员会提供该选项,代表使用PXF\_SERVER/hdp3/下的配置文件来支持PXF访问 Hive。
    -   FORMAT 'custom' \(formatter='pxfwritable\_import'\)：外部数据源格式配置项，读取Hive中sales\_info表时采用 custom ，并与 formatter='pxfwritable\_import' 搭配。
    详细字段说明请参见[基本语法](#section_prh_36f_6cz)。


-   **示例 HiveText**

    ```
    postgres=# CREATE EXTERNAL TABLE salesinfo_hivetextprofile(location text, month text, num_orders int, total_sales float8)
                 LOCATION ('pxf://default.sales_info?PROFILE=HiveText&SERVER=hdp3')
               FORMAT 'TEXT' (delimiter=E',');
    postgres=# select * from salesinfo_hivetextprofile;
       location    | month | num_orders | total_sales
    ---------------+-------+------------+-------------
     Prague        | Jan   |        101 |     4875.33
     Rome          | Mar   |         87 |     1557.39
     Bangalore     | May   |        317 |     8936.99
     ......
    ```


-   **示例 HiveRC**

    1.  Hive创建table。

        ```
        hive> CREATE TABLE sales_info_rcfile (location string, month string,
                     number_of_orders int, total_sales double)
                   ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
                   STORED AS rcfile;
        OK
        ##导入数据
        hive> INSERT INTO TABLE sales_info_rcfile SELECT * FROM sales_info;
        ##查看
        hive> SELECT * FROM sales_info_rcfile;
        ```

    2.  ADB PG 实例访问数据。

        ```
        postgres=# CREATE EXTERNAL TABLE salesinfo_hivercprofile(location text, month text, num_orders int, total_sales float8)
                     LOCATION ('pxf://default.sales_info_rcfile?PROFILE=HiveRC&SERVER=hdp3')
                   FORMAT 'TEXT' (delimiter=E',');
        postgres=# SELECT location, total_sales FROM salesinfo_hivercprofile;
           location    | total_sales
        ---------------+-------------
         Prague        |     4875.33
         Rome          |     1557.39
         Bangalore     |     8936.99
         ......
        ```


-   **示例 HiveORC**

    ORC可以有两种配置 HiveORC 和 HiveVectorizedORC 。

    -   一次读取一行数据。
    -   支持列投影。
    -   支持复杂类型, 可以访问由数组、映射、结构和联合数据类型组成的Hive表。
    1.  Hive创建table。

        ```
        hive> CREATE TABLE sales_info_ORC (location string, month string,
                number_of_orders int, total_sales double)
              STORED AS ORC;
        hive> INSERT INTO TABLE sales_info_ORC SELECT * FROM sales_info;
        hive> SELECT * FROM sales_info_ORC;
        ```

    2.  ADB PG 实例访问数据。

        ```
        postgres=# CREATE EXTERNAL TABLE salesinfo_hiveORCprofile(location text, month text, num_orders int, total_sales float8)
                     LOCATION ('pxf://default.sales_info_ORC?PROFILE=HiveORC&SERVER=hdp3')
                     FORMAT 'CUSTOM' (FORMATTER='pxfwritable_import');
                     
        postgres=# SELECT * FROM salesinfo_hiveORCprofile;
        ......
         Prague        | Dec   |        333 |     9894.77
         Bangalore     | Jul   |        271 |     8320.55
         Beijing       | Dec   |        100 |     4248.41
        (60 rows)
        Time: 420.920 ms
        ```


-   **示例 HiveVectorizedORC**

    -   一次最多读取1024行数据。
    -   不支持列投影。
    -   不支持复杂类型或时间戳数据类型。
    ADB PG 实例访问数据

    ```
    CREATE EXTERNAL TABLE salesinfo_hiveVectORC(location text, month text, num_orders int, total_sales float8)
                 LOCATION ('pxf://default.sales_info_ORC?PROFILE=HiveVectorizedORC&SERVER=hdp3')
                 FORMAT 'CUSTOM' (FORMATTER='pxfwritable_import');
                 
    select * from salesinfo_hiveVectORC;
       location    | month | num_orders | total_sales
    ---------------+-------+------------+-------------
     Prague        | Jan   |        101 |     4875.33
     Rome          | Mar   |         87 |     1557.39
     Bangalore     | May   |        317 |     8936.99
     Beijing       | Jul   |        411 |    11600.67
     San Francisco | Sept  |        156 |     6846.34
     ......
    ```


-   **示例 Parquet**

    1.  Hive创建table。

        ```
        hive> CREATE TABLE hive_parquet_table (location string, month string,
                    number_of_orders int, total_sales double)
                STORED AS parquet;
                
        INSERT INTO TABLE hive_parquet_table SELECT * FROM sales_info;
        
        select * from hive_parquet_table;
        ```

    2.  ADB PG 实例访问数据。

        ```
        postgres=# CREATE EXTERNAL TABLE pxf_parquet_table (location text, month text, number_of_orders int, total_sales double precision)
            LOCATION ('pxf://default.hive_parquet_table?profile=Hive&SERVER=hdp3')
            FORMAT 'CUSTOM' (FORMATTER='pxfwritable_import');
        postgres=# SELECT month, number_of_orders FROM pxf_parquet_table;
         month | number_of_orders
        -------+------------------
         Jan   |              101
         Mar   |               87
         May   |              317
         Jul   |              411
         Sept  |              156
         Nov   |              159
         ......
        ```


