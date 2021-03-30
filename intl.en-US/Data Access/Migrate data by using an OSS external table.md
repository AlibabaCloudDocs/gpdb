# Migrate data by using an OSS external table

OSS is an Alibaba Cloud Object Storage Service. AnalyticDB for PostgreSQL allows you to use the OSS external table \(gpossext feature\) to import or export data from OSS to OSS. OSS also supports gzip for compressing OSS external table files, saving a lot of storage space and costs.

Currently, gpossext can read and write data to both gzip-compressed and decompressed TEXT and CSV files.

![OSS ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0431882951/p111598.jpg)

This topic includes the following contents:

-   [Restart a Logstash cluster or node](#section_cpw_pyr_52b)
-   [Parameters](#section_fqw_pyr_52b)
-   [Examples](#section_crw_pyr_52b)
-   [Precautions](#section_ssw_pyr_52b)
-   [TEXT and CSV formats](#section_usw_pyr_52b)
-   [SDK errors](#section_dtw_pyr_52b)
-   [FAQ](#section_ftw_pyr_52b)
-   [Topic](#section_msd_qzr_52b)

## Restart a Logstash cluster or node

When you use OSS external tables in AnalyticDB for PostgreSQL, you may need to perform the following operations:

-   [Create the OSS external table plug-in \(oss\_ext\)](#p_v4r_g1s_52b)
-   [Import data in parallel](#p_rvb_h1s_52b)
-   [Export data in parallel](#p_pql_h1s_52b)
-   [Create the OSS external table syntax](#p_mjr_h1s_52b)

**Create the OSS external table plug-in \(oss\_ext\)**

To use an OSS external table, you must first create the OSS external table plug-in for every database in your AnalyticDB for PostgreSQL instance.

-   To create the plug-in, execute the `CREATE EXTENSION IF NOT EXISTS oss_ext;` statement.
-   Deletion statement: `DROP EXTENSION IF EXISTS oss_ext;`

**Import data in parallel**

Follow these steps:

1.  Distribute data evenly among multiple files in OSS.

    **Note:**

    All compute nodes of your AnalyticDB for PostgreSQL instance read data in parallel from the data files stored in OSS by using a round-robin algorithm. To increase read efficiency, we recommend that the number of data files in OSS be an integer multiple of the number of compute nodes in your AnalyticDB for PostgreSQL instance.

2.  Create a readable external table for every database in your AnalyticDB for PostgreSQL instance.

3.  Execute the following statement to import data from OSS in parallel:

```
INSERT INTO <Destination table> SELECT * FROM <External table>
```


**Export data in parallel**

Follow these steps:

1.  Create a writable external table for every database in your AnalyticDB for PostgreSQL instance.

2.  Execute the following statement to export data to OSS in parallel:

```
INSERT INTO <External table> SELECT * FROM <Source table>
```


**Create the OSS external table syntax**

Execute the following statements to create the OSS external table syntax:

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

## Parameters

This section describes the parameters used in various operations, such as:

-   [Common parameters](#p_kt5_dzr_52b)
-   [Import mode parameters](#p_xrm_fzr_52b)
-   [Export mode parameters](#p_odc_3zr_52b)
-   [Other parameters](#p_oqg_wzr_52b)

**Common options**

-   Protocol and endpoint: the protocol and endpoint in the "Protocol name://oss\_endpoint" format. Protocol name is oss, and oss\_endpoint is the domain of the region to which the specified OSS bucket belongs.

    **Note:**

    If you access your AnalyticDB for PostgreSQL instance from a host that is housed on Alibaba Cloud, we recommend that you enter a domain name that contains the keyword internal to bypass Internet traffic.

-   id: the AccessKey ID of the OSS account.

-   key: the AccessKey secret of the OSS account.

-   bucket: the bucket where the data files you want to access are stored. It must be an existing bucket in OSS.

-   prefix: the prefix in the name of the directory used to store the data files you want to access. The prefix is directly matched and cannot be controlled by a regular expression. The prefix, filepath, and dir parameters are mutually exclusive, so only one of them can be specified at a time.

    -   If you specify the prefix parameter when you create a readable external file for data import, all data files with the specified prefix in their names are imported from OSS.

        -   If you set the prefix parameter to test/filename, all the following data files from OSS are imported:
            -   test/filename
            -   test/filenamexxx
            -   test/filename/aa
            -   test/filenameyyy/aa
            -   test/filenameyyy/bb/aa
        -   If you set prefix to test/filename/, only the following file out of the preceding files will be imported:
            -   test/filename/aa
    -   If you specify the prefix parameter when you create a writable external table for data export, a unique name with the specified prefix is generated for each data file exported to OSS.

-   dir: the name of a virtual directory in OSS. The prefix, filepath, and dir parameters are mutually exclusive, so only one of them can be specified at a time.

    -   The name of the specified directory must end with a forward slash \(/\), for example, `test/mydir/`.
    -   If you specify this parameter when you create an external table for data import, all data files stored in the specified directory of OSS are imported. However, the data files stored in the other levels of subdirectories are not imported. Unlike the filepath parameter, the dir parameter does not require the data files stored in it to follow specific naming conventions.
    -   If you specify this parameter when you create an external table for data export, all data is exported as multiple files to the specified directory of OSS. The exported data files are named in the `filename.x` format, where x is a number. The values of x may be nonconsecutive.
-   filepath: the file name used to filter the data files you want to import from OSS. The file name contains the directory name. The prefix, filepath, and dir parameters are mutually exclusive, so only one of them can be specified at a time. In addition, you can only specify the filepath parameter when you create a readable external table for data import.

    -   The file name specified by this parameter contains the directory name but not the bucket name.
    -   Only the files named as `filename` or in the `filename.x` format are imported. In addition, the values of x must be consecutive numbers starting from 1. Assume that the following data files are stored in OSS and you set the filepath parameter to filename:

        ```
        filename
        filename.1
        filename.2
        filename.4,
        ```

        Then the filename, filename.1, and filename.2 files are imported. The filename.4 file is not imported because there is not a file named filename.3.


**Import mode parameters**

-   async: specifies whether to enable the asynchronous mode for data import.

    -   You can enable an auxiliary thread to expedite the import of data from OSS.

    -   The asynchronous mode is enabled by default. If you want to disable it, set the async parameter to false or f.

    -   The asynchronous mode consumes more hardware resources than the normal data import mode.

-   compressiontype: specifies the format used to compress imported data files.

    -   If you retain the default value none, imported data files are not compressed.

    -   If you set this parameter to gzip, imported data files are compressed in the GZIP format. Currently, only the GZIP compression format is supported.

-   compressionlevel: specifies the degree to which data files written to OSS are compressed. Valid values: 1 to 9. Default value: 6.


**Export mode parameters**

-   oss\_flush\_block\_size: the buffer size for a single data flush to OSS. Unit: MB. Value range: 1 to 128. Default value: 32.

-   oss\_file\_max\_size: the maximum size of a data file allowed to be written to OSS. If a data file reaches the maximum size, the data that remains is written to another data file. Unit: MB. Valid values: 8 to 4000. Default value: 1024.

-   num\_parallel\_worker: the maximum number of threads that are allowed to run in parallel to compress the data written to OSS. Valid values: 1 to 8. Default value: 3.

-   compressiontype: the compression format of exported data files.

    -   If you retain the default value none, exported data files are not compressed.

    -   If you set this parameter to gzip, exported data files are compressed in the GZIP format. Currently, only the GZIP compression format is supported.


Note the following when you configure export mode parameters:

-   You must specify the WRITABLE keyword for the external table you want to create.

-   Only the prefix and dir parameters are supported. The filepath parameter is not supported.

-   You can use the DISTRIBUTED BY clause to write data from compute nodes to OSS based on the specified distribution key.


**Other parameters**

The following error-tolerance parameters can be used for data import and export:

-   oss\_connect\_timeout: the connection timeout period. Unit: seconds. Default value: 10.

-   oss\_dns\_cache\_timeout: the DNS timeout period. Unit: seconds. Default value: 60.

-   oss\_speed\_limit: the minimum data transmission rate. Unit: byte/s. Default value: 1024.

-   oss\_speed\_time: the maximum wait period during which the data transmission rate is lower than its minimum value. Unit: seconds. Default value: 15.


If you retain the default values of the preceding parameters, a timeout is triggered after the transmission rate remains lower than 1024 bytes/s for 15 consecutive seconds. For more information, see [Error handling](https://www.alibabacloud.com/help/doc-detail/32141.html).

AnalyticDB for PostgreSQL also supports parameters that are compatible with the Greenplum external table syntax. For more information, visit [Greenplum official documentation on external table syntax](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_EXTERNAL_TABLE.html). These parameters are as follows:

-   FORMAT: specifies the input file format, such as TEXT or CSV.

-   ENCODING: specifies the data encoding format, such as UTF-8.

-   LOG ERRORS: specifies to ignore the rows that fail to be imported and write them to a table named error\_table. In the LOG ERRORS clause, you can specify an error reporting threshold by using the count parameter.

    **Note:**

    -   By `LOG ERRORS` log the error line information to the internal associated file.

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

    -   You can call the `gp_read_error_log('external_table_name')` function to obtain information about the rows that fail to be imported.

        ```
        select * from gp_read_error_log('external_table_name ');
        ```

    -   After the external table is deleted, the internal file is deleted too. You also have the option to call the `gp_truncate_error_log('external_table_name')` function to delete the internal file.

        ```
        select gp_truncate_error_log('external_table_name ');
        ```

    -   Version 4.3 also supports `LOG ERRORS INTO error_table` incorrect table specified by the syntax.

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


## Examples

```
# Create a READABLE external table of OSS.
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
# Create a WRITABLE external table of OSS.
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
# Create a heap table named example to which you want to import data.
create table example
        (date text, time text, open float,
         high float, low float, volume int)
         DISTRIBUTED BY (date);
# Import data to the example heap table from the ossexample table in parallel.
insert into example select * from ossexample;
# Export data from example to OSS in parallel
insert into ossexample_exp select * from example;
# As shown in the following execution plan, all compute nodes are involved in the task.
# All compute nodes read data from OSS in parallel. AnalyticDB for PostgreSQL performs a redistribution motion operation to compute the data by using a hash algorithm, and then distributes the data to its compute nodes after computing. After a compute node receives data, it performs an insert operation to add the data to AnalyticDB for PostgreSQL.
explain insert into example select * from ossexample;
                                            QUERY PLAN
-----------------------------------------------------------------------------------------------
 Insert (slice0; segments: 4)  (rows=250000 width=92)
   ->  Redistribute Motion 4:4  (slice1; segments: 4)  (cost=0.00..11000.00 rows=250000 width=92)
         Hash Key: ossexample.date
         ->  External Scan on ossexample  (cost=0.00..11000.00 rows=250000 width=92)
(4 rows)
# As shown in the following query plan, each compute node exports local data directly to OSS without redistributing the data.
explain insert into ossexample_exp select * from example;
                          QUERY PLAN
---------------------------------------------------------------
 Insert (slice0; segments: 3)  (rows=1 width=92)
   ->  Seq Scan on example  (cost=0.00..0.00 rows=1 width=92)
(2 rows)
```

## Precautions

-   AnalyticDB for PostgreSQL and Greenplum use the same syntax to create and use external tables, except for their difference in location-related parameters.

-   Data import performance varies based on OSS performance and the resources such as CPU, I/O, memory, and network resources available to your AnalyticDB for PostgreSQL instance. To achieve the best import performance, we recommend that you enable column-oriented storage and compression when you create a table. For example, specify the following clause: WITH \(APPENDONLY=true, ORIENTATION=column, COMPRESSTYPE=zlib, COMPRESSLEVEL=5, BLOCKSIZE=1048576\). For more information, visit [Greenplum official documentation on database table creation syntax](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_TABLE.html).

-   To achieve the best import performance, we recommend that OSS and AnalyticDB for PostgreSQL instances reside in the same region.


## TEXT/CSV formats

The following parameters specify the formats of files read from and written to OSS. They can be specified in the external DDL parameters.

-   \\n: the string used to delimit or break lines in a TEXT or CVS file.
-   DELIMITER: the string used to delimit columns.
    -   If you specify the DELIMITER parameter, you must also specify the QUOTE parameter.
    -   We recommend that you use a comma \(,\), tab \(\\t\), vertical bar \(\|\), or another uncommon character such as a column delimiter.
-   QUOTE: a pair of characters used to enclose user data.
    -   The pair of characters specified by the QUOTE parameter is used to distinguish user data from control characters.
    -   For efficient coding purposes, you do not need to enclose user data such as an integer in the pair of characters specified by the QUOTE parameter.
    -   The QUOTE and DELIMITER parameters cannot specify the same characters. The default value of the QUOTE parameter is a pair of double quotation marks \(""\).
    -   If user data is enclosed in the pair of characters specified by the QUOTE parameter, you must specify an escape character to distinguish user data.
-   ESCAPE: the escape character used to distinguish data.
    -   The escape character precedes a character that otherwise has a special meaning.
    -   The default value of the ESCAPE parameter is the same as that of the QUOTE parameter.
    -   You can set the ESCAPE parameter to the default escape character, backslash \(\\\), or another character you want.

**Default control characters for TEXT and CSV files**

|Control character|TEXT|CSV|
|-----------------|----|---|
|DELIMITER|Tab \(\\t\)|Comma \(,\)|
|QUOTE|Double quotation mark \("\)|Double quotation mark \("\)|
|ESCAPE|N/A|Same as the QUOTE parameter|
|NULL|Backslash plus N \(\\N\)|\(Empty string without quotation marks\)|

**Note:**

All control characters must be single-byte characters.

## SDK errors

If an error occurs during the import or export process, the error log contains the following information:

-   code: the HTTP status code of the request.

-   error\_code: the OSS error code.

-   error\_msg: the OSS error message.

-   req\_id: the UUID that identifies the request. If you require assistance, submit a ticket with the req\_id of the failed request to OSS R&D engineers.


For more information, visit [OSS error response](https://www.alibabacloud.com/help/doc-detail/32005.html). Timeout errors can be handled based on oss\_ext related parameters.

## FAQ

If the data import process takes an abnormally long time, see the descriptions on import performance in the "Precautions" section.

## Topic

-   [Endpoints](https://www.alibabacloud.com/help/doc-detail/31834.html)

-   [Object Storage Service](https://www.alibabacloud.com/help/product/31815.htm)

-   [Error handling](https://www.alibabacloud.com/help/doc-detail/32141.html)

-   [OSS error response](https://www.alibabacloud.com/help/doc-detail/32005.html)

-   [Greenplum official documentation on database external table syntax](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_EXTERNAL_TABLE.html)

-   [Greenplum official documentation on database table creation syntax](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_TABLE.html)


