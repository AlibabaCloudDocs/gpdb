Use OSS foreign tables to access OSS data 
==============================================================

This topic describes how to use OSS foreign tables to access OSS data. OSS foreign tables are designed based on the PostgreSQL Foreign Data Wrapper (FDW) framework to access OSS data for data analysis.

OSS foreign tables allow you to perform the following operations:

* Import OSS data to the internal row-oriented tables and column-oriented tables of the AnalyticDB for PostgreSQL instance for accelerated data analysis.

  

* Query and analyze large amounts of OSS data.

  

* Join OSS foreign tables and internal tables for data analysis.

  




OSS foreign tables allow you to access data files in the formats of ORC, Parquet, JSON, JSON Lines, and CSV. Some of the files can be compressed by using GZIP or standard Snappy. You can partition an OSS foreign table based on one or more columns to filter out undesired partitions when you query a specific partition.

The sources of OSS data include business applications, log archives of Alibaba Cloud Log Service, and extract-load-transform (ELT) operations of Data Lake Analytics.

Use an OSS foreign table 
---------------------------------------------

To use an OSS foreign table, perform the following operations:

* Create a 

  ***user mapping*** 

  to a foreign server. For more information, see 

  [CREATE USER MAPPING](https://www.postgresql.org/docs/9.4/sql-createusermapping.html)

  .
  

* Create an 

  ***OSS*** 

  ***foreign server*** 

  . For more information, see 

  [CREATE SERVER](https://www.postgresql.org/docs/9.4/sql-createserver.html)

  .
  

* Create an 

  **OSS** 

  ***foreign table*** 

  . For more information, see 

  [CREATE FOREIGN TABLE](https://www.postgresql.org/docs/9.4/sql-createforeigntable.html)

  .
  




For example, you can use the [ossutil command line tool](https://www.alibabacloud.com/help/zh/doc-detail/50452.htm) to view the following information about a TPC-H lineitem table in an OSS bucket.

    [adbpgadmin@localhost]$ ossutil ls oss://adbpg-tpch/data/tpch_data_10x/lineitem.tbl
    LastModifiedTime                   Size(B)  StorageClass   ETAG                                  ObjectName
    2020-03-12 09:29:48 +0800 CST    144144997      Standard   1F426F2FFC70A0262D2D69183BC3A0BD-57   oss://adbpg-tpch/data/tpch_data_10x/lineitem.tbl.1
    2020-03-12 09:29:58 +0800 CST    145177420      Standard   CFE2CFF1C8059547DC9F1711E77F74DD-57   oss://adbpg-tpch/data/tpch_data_10x/lineitem.tbl.10
    2020-03-10 21:23:24 +0800 CST    145355168      Standard   35C6227D1C29F1236A92A4D5D7922625-57   oss://adbpg-tpch/data/tpch_data_10x/lineitem.tbl.11
    ... ...



In the preceding information:

* **adbpg-tpch** 

  is the name of the OSS bucket.
  

* **data/tpch_data_10x/...** 

  is the path of the file relative to the OSS bucket.
  




The following section describes how to create and use an OSS foreign table in detail.

1. Create an OSS server 
--------------------------------------------

To create an OSS server, specify the OSS endpoint that is used to access the OSS server.

1.1 Example

    CREATE SERVER oss_serv              -- The name of the OSS server.
        FOREIGN DATA WRAPPER oss_fdw
        OPTIONS (
            endpoint '<Oss endpoint>',  -- The endpoint of the OSS server.
            bucket '<Oss bucket>'       -- The bucket where the data file is located.
      );



For more information about valid parameter values, see 1.2 Parameter description. For more information, see [CREATE SERVER](https://www.postgresql.org/docs/9.4/sql-createserver.html).

1.2 Parameter description


|     Parameter     |  Type   |  Unit   | Required | Default value |                                                                                                                                                Description                                                                                                                                                |
|-------------------|---------|---------|----------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| endpoint          | String  |         | Yes      |               | The endpoint of the OSS server. **Note** If you access your AnalyticDB for PostgreSQL instance from an Alibaba Cloud server, use an internal endpoint to avoid incurring Internet traffic. An internal endpoint contains keyword internal.                                                |
| bucket            | String  |         | No       |               | Specifies the bucket that contains the data file. You must create an OSS bucket before data import. **Note** You must specify a bucket for either an OSS server or an OSS table. If you specify a bucket for both, the bucket value for the OSS table overwrites that for the OSS server. |
| **Note** * The following fault tolerance parameters can be used when you access OSS. You can retain the default values.   * If you retain the default values for the following parameters, a timeout is triggered after the transmission rate remains lower than 1 KB/s for 1,500 consecutive seconds. For more information, see [Error handling](https://www.alibabacloud.com/help/doc-detail/32141.htm).    ||||||
| speed_limit       | Numeric | byte/s  | No       | 1024          | Specifies the minimum transmission rate.                                                                                                                                                                                                                                                                  |
| speed_time        | Numeric | seconds | No       | 1500          | Specifies the maximum duration for maintaining the minimum transmission rate.                                                                                                                                                                                                                             |
| connect_timeout   | Numeric | seconds | No       | 10            | Specifies the connection timeout period.                                                                                                                                                                                                                                                                  |
| dns_cache_timeout | Numeric | seconds | No       | 60            | Specifies the timeout period for DNS resolution.                                                                                                                                                                                                                                                          |





2. Create a user mapping to the OSS server 
---------------------------------------------------------------

After you create an OSS server, you must create a user mapping from your AnalyticDB for PostgreSQL instance to the OSS server.

2.1 Example

    CREATE USER MAPPING FOR PUBLIC -- Create a user mapping to the OSS server for all users. For more information, see 2.2 Syntax.
        SERVER oss_serv                         -- Specify the OSS server that you want to access.
        OPTIONS ( 
          id '<oss access id>',         -- Specify the AccessKey ID of the OSS account.
          key '<oss access key>'        -- Specify the AccessKey secret of the OSS account.
        );



2.2 Syntax

    -- Create a user mapping.
    CREATE USER MAPPING FOR { username | USER | CURRENT_USER | PUBLIC }
        SERVER servername
        [ OPTIONS ( option 'value' [, ... ] ) ]
    
    -- Delete a user mapping.
    DROP USER MAPPING [ IF EXISTS ] FOR { user_name | USER | CURRENT_USER | PUBLIC }
        SERVER server_name


**Note**

* `username`: The name of an existing user that is mapped to the OSS server.

  

* `CURRENT_USER` or `USER`: Both parameters specify the name of the current user.

  

* `PUBLIC`: All users, including those that might be created later, are mapped to the OSS server.

  






For more information about valid parameter values, see 2.3 Parameter description. For more information about how to create a user mapping, see [CREATE USER MAPPING](https://www.postgresql.org/docs/9.4/sql-createusermapping.html).

2.3 Parameter description


| Parameter | Required | Default value |               Description                |
|-----------|----------|---------------|------------------------------------------|
| id        | Yes      |               | The AccessKey ID of the OSS account.     |
| key       | Yes      |               | The AccessKey secret of the OSS account. |





3. Create an OSS foreign table 
------------------------------------------------

After you create an OSS bucket and an account that is used to access the OSS bucket, you can create an OSS foreign table. OSS foreign tables allow you to access data files in multiple formats to meet the requirements of different business scenarios:

* Uncompressed text files in CSV, TXT, JSON, and JSON Lines formats.

  

* GZIP or standard Snappy-compressed text files in CSV and TXT formats. GZIP-compressed text files in JSON and JSON Lines formats.

  

* Binary files in ORC format. For more information about the data type mappings between ORC files and AnalyticDB for PostgreSQL files, see 

  [Data type mappings between ORC files and AnalyticDB for PostgreSQL files.](#section-3dg-wt5-9gy)
  

* Binary files in Parquet format. For more information about the data type mappings between the Parquet files and the AnalyticDB for PostgreSQL files, see 

  [Data type mappings between Parquet files and AnalyticDB for PostgreSQL files.](#section-3dg-wx5-9zy)
  




3.1 Example

    CREATE FOREIGN TABLE x(i int, j int)
    SERVER oss_serv OPTIONS (format 'jsonline')
    PARTITION BY LIST (j) (        
        VALUES('20181218'),        
        VALUES('20190101')
    );


**Note**

After you create an OSS foreign table, you can use one of the following statements to check whether OSS objects that match the table meet expectations:

* Statement 1: `explain verbose select * from <OSS foreign table>;`

  

* Statement 2: `select * from get_oss_table_meta('<OSS foreign table>');`

  




3.2 Syntax

    -- Create an OSS foreign table.
    CREATE FOREIGN TABLE [ IF NOT EXISTS ] table_name ( [
        column_name data_type [ OPTIONS ( option 'value' [, ... ] ) ] [ COLLATE collation ] [ column_constraint [ ... ] ]
          [, ... ]
    ] )
        SERVER server_name
      [ OPTIONS ( option 'value' [, ... ] ) ]
    
    -- Delete an OSS foreign table.
    DROP FOREIGN TABLE [ IF EXISTS ] name [, ...] [ CASCADE | RESTRICT ]



For more information about valid parameter values, see 3.3 Parameter description. For more information about how to create a foreign table, see [CREATE FOREIGN TABLE](https://www.postgresql.org/docs/9.4/sql-createforeigntable.html).

3.3 Parameter description

3.3.1 Common parameters


| Parameter |  Type  | Unit |                                                             Required                                                             | Default value |                                                                                                                                                                                                                                                                                                                                                                                            Description                                                                                                                                                                                                                                                                                                                                                                                             |
|-----------|--------|------|----------------------------------------------------------------------------------------------------------------------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| filepath  | String |      | Yes. Select one of the three parameters. **Note** The three parameters specify paths relative to the OSS bucket. |               | This path directs to a single file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| prefix    | String |      | Yes. Select one of the three parameters. **Note** The three parameters specify paths relative to the OSS bucket. |               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| dir       | String |      | Yes. Select one of the three parameters. **Note** The three parameters specify paths relative to the OSS bucket. |               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| bucket    | String |      | No                                                                                                                               |               | You must specify a bucket for either an OSS server or an OSS table. If you specify a bucket for both, the bucket value for the OSS table overwrites that for the OSS server.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| format    | String |      | Yes                                                                                                                              |               | Specifies the file format. Valid values: * csv   * text   * orc   * parquet   * JSON. For more information, see [Introducing JSON](https://www.json.org/json-en.html) .   * JSON Lines. For more information, see [JSON Lines](http://jsonlines.org/). JSON Lines is newline-delimited JSON. All the data that can be read in JSON Lines format can be read in JSON format. However, not all the data that can be read in JSON format can be read in JSON Lines format. We recommend that you use the JSON Lines format.    |



3.3.2 Parameters for CSV and TXT files

Note: The following parameters are applicable to only CSV and TXT formats and are invalid for ORC and Parquet formats.


|      Parameter       |  Type   | Unit | Required |                                                                                                   Default value                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                            Description                                                                                                                                                                                                                                                                                                                                                                                            |
|----------------------|---------|------|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| filetype             | String  |      | No       | plain                                                                                                                                                                                                             | Valid values: * plain: reads only the raw binary data.   * gzip: reads the raw binary data and decompresses the package by using GZIP.   * standard snappy: reads raw binary data and decompresses the package by using standard Snappy. You can read only standard Snappy-compressed files. You cannot read hadoop-snappy-compressed files. For more information, see [snappy/format_description.txt](https://github.com/google/snappy/blob/master/format_description.txt). You can also use this parameter to specify the compression type of the input file in JSON or JSON Lines format. Valid values: plain and gzip.    |
| log_errors           | Boolean |      | No       | false                                                                                                                                                                                                             | Specifies whether to record errors in log files. For more examples, see 3.4 Fault tolerance mechanism.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| segment_reject_limit | Numeric |      | No       |                                                                                                                                                                                                                   | Specifies the maximum number of error lines before the execution is aborted. If the value contains a percent sign (%), it indicates the percentage of error lines. Otherwise, the value indicates the number of error lines. Examples: * segment_reject_limit = '10 indicates that execution is aborted when the number of error lines in a segment exceeds 10.   * segment_reject_limit = '10 indicates that execution is aborted when the number of error lines in a segment is more than 10% of the processed lines.    For more information, see 3.4 Fault tolerance mechanism.                                                                                            |
| The following table lists the parameters used in formatting. For more information, see [COPY](https://www.postgresql.org/docs/current/sql-copy.html).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               ||||||
| header               | Boolean |      | No       | false                                                                                                                                                                                                             | Specifies whether the source file contains the header row. This parameter is valid for CSV files only.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| delimiter            | String  |      | No       | * Default value for TXT files: the Tab key.   * Default value for CSV files: a comma (,).                                      | The column separator. Only single-byte characters are allowed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| quote                | String  |      | No       | Double quotation marks (").                                                                                                                                                                                       | The column quotation marks. * This parameter is valid for CSV files only.   * Only single-byte characters are allowed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| escape               | String  |      | No       | By default, the value is the same as the value of the quote parameter.                                                                                                                                            | Specifies the character that appears before a character that is the same as the value of the quote parameter. * Only single-byte characters are allowed.   * This parameter is valid for CSV files only.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| null                 | String  |      |          | * Default value for TXT files: \\N   * Default value for CSV files: spaces that are not enclosed in double quotation marks.    | Specifies the empty string for a file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| encoding             | String  |      | No       | By default, the encoding format on the client is used.                                                                                                                                                            | Specifies the encoding format of the data file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| force_not_null       | Boolean |      | No       | false                                                                                                                                                                                                             | If the parameter is set to true, the values of specified columns are not matched against the empty string.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| force_null           | Boolean |      | No       | false                                                                                                                                                                                                             | * If this parameter is set to true, the column values that match the empty string are returned as NULL even if the values are quoted.   * If this parameter is not specified, only unquoted column values that match the empty string are returned as NULL.                                                                                                                                                                                                                                                                                                                                                                                                                                                    |



3.4 Fault tolerance mechanism

When you create an OSS foreign table, you can set the `log_errors` and `segment_reject_limit` parameters to avoid unexpected exits due to error lines in the raw data during a scan of the OSS foreign table. The following list describes these parameters:

* `log_errors`: Specifies whether to record the information of error lines.

  

* `segment_reject_limit`: Specifies the maximum allowable percentage of error lines in all parsed lines. 



**Note**

Only OSS foreign tables in CSV and TXT formats support the fault tolerance mechanism.

1. Create a FDW-based OSS foreign table that supports fault tolerance. 

       create foreign table oss_error_sales (id int, value float8, x text)
           server oss_serv
           options (log_errors 'true',         -- Record the information of error lines.
                    segment_reject_limit '10', -- The number of error lines can be up to 10. Otherwise, the system returns an error and exits.
                    dir 'error_sales/',        -- Specify the OSS directory that the foreign table matches.
                    format 'csv',              -- Parse files in the CSV format.
                    encoding 'utf8');          -- Specify the encoding format. 
                                       

   

2. Scan the foreign table.

   Three error lines are added to an OSS file to demonstrate the effect of fault tolerance.

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

   

3. View the log of error lines.

       postgres=# select * from gp_read_error_log('oss_error_sales');
                  cmdtime            |     relname     |        filename         | linenum | bytenum |                          errmsg                           | rawdata | rawbytes
       ------------------------------+-----------------+-------------------------+---------+---------+-----------------------------------------------------------+---------+----------
        2020-04-22 19:37:35.21125+08 | oss_error_sales | error_sales/sales.2.csv |       2 |         | invalid byte sequence for encoding "UTF8": 0xed 0xab 0xad |         | \x
        2020-04-22 19:37:35.21125+08 | oss_error_sales | error_sales/sales.2.csv |       3 |         | invalid byte sequence for encoding "UTF8": 0xed 0xab 0xad |         | \x
        2020-04-22 19:37:35.21125+08 | oss_error_sales | error_sales/sales.3.csv |       2 |         | invalid byte sequence for encoding "UTF8": 0xed 0xab 0xad |         | \x
       (3 rows)                         

   

   

4. Delete the log of error lines.

       postgres=# select gp_truncate_error_log('oss_error_sales');
        gp_truncate_error_log
       -----------------------
        t
       (1 row)

   

   




4. Use the OSS foreign table 
----------------------------------------------

4.1 Import OSS data to an internal table.

Perform the following steps to import data:

1. Distribute data evenly into multiple files in OSS.

   **Note**

   
   * All compute nodes of an AnalyticDB for PostgreSQL instance read data in parallel from the data files stored in OSS based on the round-robin algorithm.

     
   
   * We recommend that you use the same data encoding formats for data files and databases to simplify the encoding process and improve efficiency. The default database encoding format is UTF-8.

     
   
   * Multiple CSV and TXT files can be read in parallel. By default, four files are read in parallel.

     
   
   * To maximize the read efficiency, we recommend that you set the number of files that are read in parallel to an integer multiple of the number of compute node cores. The number of compute node cores is the product of the number of compute nodes and the number of cores per compute node.

     
   
   * If the number of files is small, we recommend that you split the source files into multiple files so that multiple nodes can scan the files in parallel. For more information, see [Split files](#section-hvx-cxk-mp0).

     
   

   
   

2. Create an OSS foreign table for each database in your AnalyticDB for PostgreSQL instance.

   

3. Execute one of the following statements to import data to the new table:

       -- INSERT statement
       INSERT INTO <Destination internal table> SELECT * FROM <OSS foreign table>;
       
       -- CREATE TABLE AS statement
       CREATE TABLE <Destination internal table> AS SELECT * FROM <OSS foreign table>;

   




* Example 1: Use the INSERT statement to import the oss_lineitem_orc data to an internal AOCS table

      -- Create an internal AOCS table.
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
      
      -- Import the oss_lineitem_orc data to the internal AOCS table.
      INSERT INTO aocs_lineitem SELECT * FROM oss_lineitem_orc;

  

* Example 2: Use the CREATE TABLE AS statement to import the oss_lineitem_orc data to an internal heap table

      create table heap_lineitem as select * from oss_lineitem_orc distributed by (l_orderkey);

  






4.2 Query and analyze OSS data

You can query an OSS foreign table in the same way as you query an internal table. The following list describes the common query scenarios:

* Data filtering based on key-value pairs

  




    select * from oss_lineitem_orc where l_orderkey = 14062498;



* Data aggregation

  




    select count(*) from oss_lineitem_orc where l_orderkey = 14062498;



* A combination of the filter, group, and limit clauses

  




    select l_partkey, sum(l_suppkey)
      from oss_lineitem_orc
     group by l_partkey
     order by l_partkey
     limit 10;



4.3 Join OSS foreign tables and internal tables for data analytics

* Example - Join internal AOCS table aocs_lineitem and OSS foreign tables for a TPC-H Q3 query

  




    select l_orderkey,
           sum(l_extendedprice * (1 - l_discount)) as revenue,
           o_orderdate,
           o_shippriority 
      from oss_customer,                                    -- OSS foreign table
           oss_orders,                                      -- OSS foreign table
           aocs_lineitem                                    -- Internal  AOCS table
     where c_mktsegment = 'furniture'
       and c_custkey = o_custkey
       and l_orderkey = o_orderkey
       and o_orderdate < date '1995-03-29'
       and l_shipdate > date '1995-03-29'
     group by l_orderkey, o_orderdate, o_shippriority
     order by revenue desc, o_orderdate
     limit 10;



5. Use a partitioned OSS foreign table 
--------------------------------------------------------

AnalyticDB for PostgreSQL supports partitioned OSS foreign tables. If you use partition key columns in a WHERE clause to query data, you can reduce the amount of data to be pulled from OSS.

To use the partitioning function of FDW-based OSS foreign tables in AnalyticDB for PostgreSQL, make sure the following requirements are met: The data of a partition in a partitioned OSS foreign table must be stored in the `oss://bucket/partcol1=partval1/partcol2=partval2/` directory of the OSS server. In this directory, `partcol1` and `partcol2` indicate partition key columns, and `partval1` and `partval2` indicate the partition key column values that define the partition.

5.1 Example

The following SQL statements create the userlogin foreign table that has two partitions. The paths of the two partitions in OSS are `oss://bucket/userlogin/month=201812/` and `oss://bucket/userlogin/month=201901/`.

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



5.2 Syntax

5.2.1 Create a partitioned OSS foreign table

FDW-based OSS foreign tables in AnalyticDB for PostgreSQL and standard partitioned tables use the same syntax to perform partitioning. For more information about the syntax of partitioning standard partitioned tables, see [Table partitioning](/intl.en-US/Data/Define table partitioning.md). FDW-based OSS foreign tables in AnalyticDB for PostgreSQL support only list partitioning.

5.2.2 Delete a partitioned OSS foreign table

You can execute the DROP FOREIGN TABLE statement to delete a regular foreign table. You can also execute this statement to delete a partitioned OSS foreign table.

5.2.3 Modify the structure of a partitioned OSS foreign table

You can execute the ALTER TABLE statement to modify the structure of a partitioned foreign table. You can add and delete partitions. For more information about the syntax, see [ALTER TABLE](https://gpdb.docs.pivotal.io/6-3/ref_guide/sql_commands/ALTER_TABLE.html). The following content uses examples to describe how to add or delete a partition. Execute the following statements to create the ossfdw_parttable table:




    CREATE FOREIGN TABLE ossfdw_parttable(            
      key text,
      value bigint,
      pt text,                                        -- The partition key of the foreign table.
      region text                                     -- The subpartition key of the foreign table.
    ) 
    SERVER oss_serv
    OPTIONS (dir 'PartationDataDirInOss/', format 'jsonline')
    PARTITION BY LIST (pt)                            -- Use the pt column as the partition key.
    SUBPARTITION BY LIST (region)                     -- Use the region column as the subpartition key.
        SUBPARTITION TEMPLATE (                       -- The template of the subpartition.
           SUBPARTITION hangzhou VALUES ('hangzhou'),
           SUBPARTITION shanghai VALUES ('shanghai')
        )
    ( PARTITION "20170601" VALUES ('20170601'), 
      PARTITION "20170602" VALUES ('20170602'));



Execute the ALTER TABLE statement to add a partition to the table. After you perform this operation, a subpartition is automatically created for the new partition based on the subpartition template that is defined in the preceding code block.




    ALTER TABLE ossfdw_parttable ADD PARTITION VALUES ('20170603');



![](../images/p130512.png)

You can also create a subpartition for an existing partition.




    ALTER TABLE ossfdw_parttable ALTER PARTITION FOR ('20170603') ADD PARTITION VALUES('nanjing');



![](../images/p130540.png)

You can delete a partition.



    ALTER TABLE ossfdw_parttable DROP PARTITION FOR ('20170601');



You can delete a subpartition of an existing partition.



    ALTER TABLE ossfdw_parttable ALTER PARTITION FOR ('20170602') DROP PARTITION FOR ('hangzhou');



When you create or add a foreign table partition, you can customize the partition instead of using a template. The following code block shows an example:



    CREATE FOREIGN TABLE ossfdw_parttable(            
      key text,
      value bigint,
      pt text,                                        -- The partition key of the foreign table.
      region text                                     -- The subpartition key of the foreign table.
    ) 
    SERVER oss_serv
    OPTIONS (dir 'PartationDataDirInOss/', format 'jsonline')
    PARTITION BY LIST (pt)                            -- Use the pt column as the partition key.
    SUBPARTITION BY LIST (region)                     -- Use the region column as the subpartition key.
    (
        -- The following two partitions have different subpartitions.
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
    
    -- Add a partition to the table. You must specify subpartitions for the new partition because no subpartition templates are defined.
    ALTER TABLE ossfdw_parttable ADD PARTITION VALUES ('20181220')
    (
        VALUES('hefei'),
        VALUES('guangzhou') 
    );



5.3 Scenario

Partitioned foreign tables are commonly used to access logs shipped by Log Service. For more information, see [Use FDW-based OSS foreign tables to access logs shipped by Log Service](#section-juz-qpj-6yo). You can use a similar method to organize directories of the OSS server when you write data from business applications to OSS, and then create a partitioned OSS foreign table.

Split files 
--------------------------------

FDW-based OSS foreign tables support multi-node parallel scans. If the number of files is small, we recommend that you split the source files into multiple files so that multiple nodes can scan the files in parallel.

For example, in Linux, you can use the **split** command to split files in `TXT` or `CSV` format into multiple files.
**Note**

A row in a file cannot be split into different files. Therefore, files must be split by rows.

    # Query the number of rows in the current file.
    wc -l csv_file
    
    # Split the file into small files based on the specified number of rows. N specifies the number of rows in each small file.
    split -l N csv_file





Collect statistics of foreign tables 
------------------------------------------------------

Data of OSS foreign tables is stored in OSS. By default, the statistics of foreign tables are not collected. If no up-to-date statistics are available, the query optimizer may generate inefficient query plans for complex queries such as queries on joined tables. You can execute the ANALYZE statement to update statistics. 

    -- Execute the EXPLAIN statement to view the query plan before the ANALYZE statement is executed.
    EXPLAIN <Table name>;
    
    -- Use the ANALYZE statement to collect statistics of an OSS foreign table.
    ANALYZE <Table name>;
    
    -- Use the EXPLAIN statement to view the query plan after the ANALYZE statement is executed.
    EXPLAIN <Table name>;



Determine the number of files in the OSS foreign table based on the number of compute nodes 
----------------------------------------------------------------------------------------------------------------

* All compute nodes of an AnalyticDB for PostgreSQL instance read data in parallel from the data files stored in OSS by using a round-robin algorithm.

  

* We recommend that you use the same data encoding formats for data files and databases to simplify the encoding process and maximize efficiency. The default database encoding format is UTF-8.

  

* Multiple CSV and TXT files can be read in parallel. By default, four files are read in parallel.

  

* To maximize reading efficiency, we recommend that you set the number of files that are read in parallel as an integer multiple of the number of compute node cores. The number of compute node cores is the product of the number of compute nodes and the number of cores per compute node.

  




View the query plan 
----------------------------------------

Execute the following statements to view the query plan of an OSS foreign table:

    EXPLAIN SELECT COUNT(*) FROM oss_lineitem_orc WHERE l_orderkey > 14062498;


**Note**

You can execute the `EXPLAIN VERBOSE` statement to view more information.

View the file information of a specified OSS foreign table 
-------------------------------------------------------------------------------

You can execute the following statement to obtain the file information of a specified OSS foreign table:

    SELECT * FROM get_oss_table_meta('<OSS FOREIGN TABLE>');



Data type mappings between ORC files and AnalyticDB for PostgreSQL files 
---------------------------------------------------------------------------------------------

The following table lists the data type mappings between ORC files and AnalyticDB for PostgreSQL files.


| AnalyticDB for PostgreSQL data type | ORC data type |
|-------------------------------------|---------------|
| bool                                | BOOLEAN       |
| int2                                | SHORT         |
| int4                                | INT           |
| int8                                | LONG          |
| float4                              | FLOAT         |
| float8                              | DOUBLE        |
| numeric                             | DECIMAL       |
| char                                | CHAR          |
| text                                | STRING        |
| bytea                               | BINARY        |
| timestamp                           | TIMESTAMP     |
| date                                | DATE          |
| int2\[\]                            | LIST(SHORT)   |
| int4\[\]                            | LIST(INT)     |
| int8\[\]                            | LIST(LONG)    |
| float4\[\]                          | LIST(FLOAT)   |
| float8\[\]                          | LIST(DOUBLE)  |




**Note**

ORC data of the LIST type can be converted to only one-dimensional arrays in AnalyticDB for PostgreSQL.

Data type mappings between Parquet files and AnalyticDB for PostgreSQL files 
-------------------------------------------------------------------------------------------------

Files in the Parquet format support the following data types: primitive types and logical types. We recommend that you use the following data types supported by AnalyticDB for PostgreSQL to map the data types supported by Parquet files when you create a table. Otherwise, the data types are converted.
**Note**

Two nested data types in Parquet are not supported: ARRAY and MAP.

The following table lists the data type mappings that apply when Parquet files do not contain data of logical types.


| AnalyticDB for PostgreSQL data type |  Parquet data type   |
|-------------------------------------|----------------------|
| bool                                | BOOLEAN              |
| integer                             | INT32                |
| bigint                              | INT64                |
| timestamp                           | INT96                |
| float4                              | FLOAT                |
| float8                              | DOUBLE               |
| bytea                               | BYTE_ARRAY           |
| bytea                               | FIXED_LEN_BYTE_ARRAY |



The following table shows the data type mappings that apply when Parquet files contain data of logical types. 


| AnalyticDB for PostgreSQL data type | Parquet data type |
|-------------------------------------|-------------------|
| text                                | STRING            |
| date                                | DATE              |
| timestamp                           | TIMESTAMP         |
| time                                | TIME              |
| interval                            | INTERVAL          |
| numeric                             | DECIMAL           |
| smallint                            | INT(8)/INT(16)    |
| integer                             | INT(32)           |
| bigint                              | INT(64)           |
| bigint                              | UINT(8/16/32/64)  |
| json                                | JSON              |
| jsonb                               | BSON              |
| uuid                                | UUID              |
| text                                | ENUM              |





Use FDW-based OSS foreign tables to access logs shipped by Log Service 
-------------------------------------------------------------------------------------------

Partitioned FDW-based OSS foreign tables in AnalyticDB for PostgreSQL are widely used to access logs shipped by Log Service. Log Service is an end-to-end logging service developed by Alibaba Cloud and is widely used in big data scenarios. For more information about Log Service, see [What is Log Service?](/intl.en-US/Product Introduction/What is Log Service?.md)

To create a partitioned OSS foreign table based on the data shipped from Log Service to OSS, set Shard Format in the OSS LogShipper dialog box, as shown in the following figure. For more information about the parameters, see [Ship logs to OSS](/intl.en-US/Log consumption and shipping/Data shipping/Ship logs to OSS/Ship log data from Log Service to OSS.md).

![Use FDW-based OSS foreign tables to access logs shipped by Log Service](../images/p143566.png)

If the parameters are configured as shown in the preceding figure, logs generated within the same month are stored in the same OSS directory. The configuration generates the following directory structure in OSS:

    oss://oss-fdw-test/adbpgossfdw
    ├── date=202002
    │   ├── userlogin_1585617629106546791_647504382.csv
    │   └── userlogin_1585617849232201154_647507440.csv
    └── date=202003
        └── userlogin_1585617944247047796_647508762.csv



Then, create the following partitioned OSS foreign table based on the columns contained in the files shipped by Log Service:

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



Finally, implement the required analysis logic on the partitioned foreign table. For example, you can query the number of all user logons in February 2020.

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



In the preceding statements, the FDW-based OSS foreign table reads only data generated in February 2020 to reduce the amount of data to read and maximize query efficiency.

Troubleshooting 
------------------------------------

During a scan of an OSS foreign table, the following error message may appear:

oss server returned error: StatusCode=..., ErrorCode=..., ErrorMessage="...", RequestId=...

The following list describes the parameters in this message:

* StatusCode: the HTTP status code.

* ErrorCode: the error code returned by OSS.

  

* ErrorMessage: the error message returned by OSS.

* RequestId: the UUID of the request. If you need technical support, contact Customer Services and provide this ID.




For more information about error types and how to handle errors, see [Handle OSS errors](/intl.en-US/Developer Guide/Troubleshooting/Error responses.md).

Differences between OSS foreign tables and OSS external tables 
-----------------------------------------------------------------------------------

* AnalyticDB for PostgreSQL allows you to use OSS external tables to import and export data. However, OSS external tables cannot meet requirements for the analysis of large amounts of OSS data.

  

* OSS foreign tables are developed based on the PostgreSQL FDW framework and support ORC and CSV files. The CSV files can be compressed by using GZIP. OSS foreign tables can be partitioned based on one or more columns. You can collect the statistics of OSS foreign tables so that the optimizer can generate an optimal query plan.

  

* Foreign tables are superior to external tables in terms of performance, features, and stability. Therefore, the Greenplum community plans to replace external tables with foreign tables.

  




References 
-------------------------------

* [Get started with Object Storage Service](/intl.en-US/Quick Start/Get started with OSS.md)

  

* [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md)

  

* [Handle errors](/intl.en-US/SDK Reference/C/Error handling.md)

  

* [Handle OSS errors](/intl.en-US/Developer Guide/Troubleshooting/Error responses.md)

  




