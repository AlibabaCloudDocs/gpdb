# Use external tables for federated analytics of Hadoop data sources

AnalyticDB for PostgreSQL allows you to query external Hadoop data sources.

**Note:**

-   This feature is available only for AnalyticDB for PostgreSQL instances in elastic storage mode. The AnalyticDB for PostgreSQL instances must be in the same virtual private cloud \(VPC\) as the external data sources.
-   This feature is unavailable for AnalyticDB for PostgreSQL instances in elastic storage mode that were created before September 6, 2020. This is because these instances cannot be connected to external Hadoop data sources over different network architectures. If you want to apply this feature to the existing instances, we recommend that you contact Alibaba Cloud technical support to apply for new instances and migrate data.

## Configure a server

Server configurations vary based on user requirements. To query external Hadoop data sources for federated analytics, you must [submit a ticket](https://workorder.console.aliyun.com/console.htm#/ticket/add?productCode=gpdb) to request technical support to configure a server. The following table describes the information that is required when you submit a ticket.

|External data source|Required information|
|--------------------|--------------------|
|Hadoop \(HDFS, Hive, and HBase\)|core-site.xml, hdfs-site.xml, mapred-site.xml, yarn-site.xml, and hive-site.xml**Note:** Profiles such as keytab and krb5.conf are also required for Kerberos authentication. |

## Syntax

-   **Create an extension**

    ```
    CREATE extension pxf; 
    ```


-   **Create an external table**

    ```
    CREATE EXTERNAL TABLE <table_name>
            ( <column_name> <data_type> [, ...] | LIKE <other_table> )
    LOCATION('pxf://<path-to-data>?PROFILE[&<custom-option>=<value>[...]]&[SERVER=value]')
    FORMAT '[TEXT|CSV|CUSTOM]' (<formatting-properties>);
    ```

    For more information about the syntax that is used to create an external table, see [CREATE EXTERNAL TABLE](/intl.en-US/Beginner Developer Guide/SQL statements.md).

    |Parameter|Description|
    |---------|-----------|
    |path-to-data|The path to the data to be queried. Example: //data/pxf\_examples/pxf\_hdfs\_simple.txt.|
    |PROFILE \[&<custom-option\>=<value\>\[...\]\]|The profile that is used to query external data based on the Platform Extension Framework \(PXF\). Valid values: Jdbc, hdfs:text, hdfs:text:multi, hdfs:avro, hdfs:json, hdfs:parquet, hdfs:AvroSequenceFile, hdfs:SequenceFile, HiveText, HiveRC, HiveORC, HiveVectorizedORC, and HBase. |
    |FORMAT '\[TEXT\|CSV\|CUSTOM\]'|The file format of the data to be queried. Valid values: TEXT, CSV, and CUSTOM.|
    |formatting-properties|The formatting option of the specified file format. Set this parameter to formatter or delimiter.    -   Valid values when you set FORMAT to CUSTOM:
        -   formatter='pxfwritable\_import'
        -   formatter='pxfwritable\_export'
    -   Valid values when you set FORMAT to TEXT or CSV:

        -   delimiter=E'\\t'
        -   delimiter ':'
**Note:** E is used to escape possible special characters. |
    |SERVER|The location of the profile on the server, which is provided by the technical support of AnalyticDB for PostgreSQL.     -   postgresql
    -   hdp3 |


## Query Hadoop Distributed File System \(HDFS\) data

The following table describes the supported data formats.

|Data format|Profile|
|-----------|-------|
|text|hdfs:text|
|csv|hdfs:text:multi and hdfs:text|
|Avro|hdfs:avro|
|JSON|hdfs:json|
|Parquet|hdfs:parquet|
|AvroSequenceFile|hdfs:AvroSequenceFile|
|SequenceFile|hdfs:SequenceFile|

For more information about `FORMAT` and `formatting-properties`, see the "[Create an external table](#dt_4co_gq6_9ra)" section of this topic.

-   **Example: Query an HDFS file**

    Create a test data file named `pxf_hdfs_simple.txt` on HDFS.

    ```
    echo 'Prague,Jan,101,4875.33
    Rome,Mar,87,1557.39
    Bangalore,May,317,8936.99
    Beijing,Jul,411,11600.67' > /tmp/pxf_hdfs_simple.txt
    
    # Create a directory.
    hdfs dfs -mkdir -p /data/pxf_examples
    # Put the file into the directory on HDFS.
    hdfs dfs -put /tmp/pxf_hdfs_simple.txt /data/pxf_examples/
    
    # View the file.
    hdfs dfs -cat /data/pxf_examples/pxf_hdfs_simple.txt
    ```

    Create an external table and query it on an AnalyticDB for PostgreSQL instance.

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

    The following list describes the parameters in the LOCATION clause:

    -   pxf:// : the pxf protocol. It cannot be modified.
    -   data/pxf\_examples/pxf\_hdfs\_simple.txt: the pxf\_hdfs\_simple.txt file on HDFS.
    -   PROFILE=hdfs:text: the profile that is used to query HDFS data. In this example, hdfs:text is used.
    -   SERVER=hdp3: the location of the profile used to query HDFS data based on PXF. In this example, PXF\_SERVER/hdp3/ is used. This value is provided by the technical support of AnalyticDB for PostgreSQL.
    -   FORMAT 'TEXT' \(delimiter=E','\): the file format and formatting option of the external data source. In this example, the file format is set to `TEXT`, and the delimiter is set to commas \(`,`\).
    For more information about parameter descriptions, see the "[Syntax](#section_prh_36f_6cz)" section of this topic.


-   **Example: Write data to HDFS in the TEXT or CSV format**

    The /data/pxf\_examples/pxfwritable\_hdfs\_textsimple1 directory is created on HDFS by using the following command:

    ```
    hdfs dfs -mkdir -p /data/pxf_examples/pxfwritable_hdfs_textsimple1
    ```

    **Note:** To execute INSERT statements on AnalyticDB for PostgreSQL, you must have write permissions on the preceding directory.

    1.  Create a writable external table on an AnalyticDB for PostgreSQL instance.

        ```
        CREATE WRITABLE EXTERNAL TABLE pxf_hdfs_writabletbl_1(location text, month text, num_orders int, total_sales float8)
                    LOCATION ('pxf://data/pxf_examples/pxfwritable_hdfs_textsimple1?PROFILE=hdfs:text&SERVER=hdp3')
                  FORMAT 'TEXT' (delimiter=',');
                  
        INSERT INTO pxf_hdfs_writabletbl_1 VALUES ( 'Frankfurt', 'Mar', 777, 3956.98 );
        INSERT INTO pxf_hdfs_writabletbl_1 VALUES ( 'Cleveland', 'Oct', 3812, 96645.37 );
        ```

    2.  View information on HDFS.

        ```
        # View the directory.
        hdfs dfs -ls /data/pxf_examples/pxfwritable_hdfs_textsimple1
        
        # View data in the directory.
        hdfs dfs -cat /data/pxf_examples/pxfwritable_hdfs_textsimple1/*
        Frankfurt,Mar,777,3956.98
        Cleveland,Oct,3812,96645.37
        ```


## Query Hive data

|Data format|Profile|
|-----------|-------|
|TextFile|Hive and HiveText|
|SequenceFile|Hive|
|RCFile|Hive and HiveRC|
|Optimized Row Columnar \(ORC\)|Hive, HiveORC, and HiveVectorizedORC|
|Parquet|Hive|

For more information about FORMAT and formatting-properties, see the "[Create an external table](#dt_4co_gq6_9ra)" section of this topic.

-   **Example: Use the Hive profile**

    1.  Generate data.

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

    2.  Create a Hive table.

        ```
        hive> CREATE TABLE sales_info (location string, month string,
                number_of_orders int, total_sales double)
                ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
                STORED AS textfile;
        
        hive> LOAD DATA LOCAL INPATH '/tmp/pxf_hive_datafile.txt'
                INTO TABLE sales_info;
        hive> SELECT * FROM sales_info;
        ```

    3.  Create an external table and query it on an AnalyticDB for PostgreSQL instance.

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

    The following list describes the parameters in the LOCATION clause:

    -   pxf://: the pxf protocol. It cannot be modified.
    -   default.sales\_info: the sales\_info table that is stored in the default database on Hive.
    -   PROFILE=Hive: the profile that is used to query Hive data. In this example, Hive is used.
    -   SERVER=hdp3: the location of the profile used to query Hive data based on PXF. In this example, PXF\_SERVER/hdp3/ is used. This value is provided by the technical support of AnalyticDB for PostgreSQL.
    -   FORMAT 'custom' \(formatter='pxfwritable\_import'\): the file format and formatting option of the external data source. In this example, the file format is set to custom, and the formatter is set to pxfwritable\_import.
    For more information about parameter descriptions, see the "[Syntax](#section_prh_36f_6cz)" section of this topic.


-   **Example: Use the HiveText profile**

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


-   **Example: Use the HiveRC profile**

    1.  Create a Hive table.

        ```
        hive> CREATE TABLE sales_info_rcfile (location string, month string,
                     number_of_orders int, total_sales double)
                   ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
                   STORED AS rcfile;
        OK
        ## Import data.
        hive> INSERT INTO TABLE sales_info_rcfile SELECT * FROM sales_info;
        ## View data.
        hive> SELECT * FROM sales_info_rcfile;
        ```

    2.  Create an external table and query it on an AnalyticDB for PostgreSQL instance.

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


-   **Example: Use the HiveORC profile**

    ORC-supporting profiles, HiveORC and HiveVectorizedORC, provide the following features:

    -   Read a single row of data at a time.
    -   Support column projection.
    -   Support complex types. You can query Hive tables that are composed of array, map, struct, and union data types.
    1.  Create a Hive table.

        ```
        hive> CREATE TABLE sales_info_ORC (location string, month string,
                number_of_orders int, total_sales double)
              STORED AS ORC;
        hive> INSERT INTO TABLE sales_info_ORC SELECT * FROM sales_info;
        hive> SELECT * FROM sales_info_ORC;
        ```

    2.  Create an external table and query it on an AnalyticDB for PostgreSQL instance.

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


-   **Example: Use the HiveVectorizedORC profile**

    This profile provides the following features:

    -   Reads up to 1,024 rows of data at a time.
    -   Does not support column projection.
    -   Does not support complex types or the timestamp data type.
    Create an external table and query it on an AnalyticDB for PostgreSQL instance.

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


-   **Example: Query a Parquet-format Hive table**

    1.  Create a Hive table.

        ```
        hive> CREATE TABLE hive_parquet_table (location string, month string,
                    number_of_orders int, total_sales double)
                STORED AS parquet;
                
        INSERT INTO TABLE hive_parquet_table SELECT * FROM sales_info;
        
        select * from hive_parquet_table;
        ```

    2.  Create an external table and query it on an AnalyticDB for PostgreSQL instance.

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


