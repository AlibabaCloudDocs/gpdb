# Migrate data by using an OSS external table {#concept_ofw_3mr_52b .concept}

AnalyticDB for PostgreSQL can import or export data from or to OSS tables in parallel by using the OSS external table function, gpossext. AnalyticDB for PostgreSQL also supports gzip file compression for the OSS external tables to reduce file size and storage costs.

Currently, gpossext can read from and write to TEXT and CSV files as well as gzip compressed TEXT and CSV files.

This topic includes the following contents:

-   [Operation instructions](#)
-   [Parameter description](#)
-   [Examples](#)
-   [Precautions](#)
-   [TEXT and CSV format description](#)
-   [SDK error handling](#)
-   [FAQ](#)
-   [References](#)

## Operation instructions {#section_cpw_pyr_52b .section}

Accessing and editing OSS external tables through AnalyticDB for PostgreSQL consists of the following operations:

-   [Create an OSS external table plug-in \(oss\_ext\)](#)
-   [Import data in parallel](#)
-   [Export data in parallel](#)
-   [Create OSS external table syntax](#)

**Create an OSS external table plug-in \(oss\_ext\)**

To use an OSS external table, you must first create an OSS external table plug-in in AnalyticDB for PostgreSQL. You must create a plug-in for each database that you want to access.

-   Creation statement: `CREATE EXTENSION IF NOT EXISTS oss_ext;`
-   Deletion statement: `DROP EXTENSION IF EXISTS oss_ext;`

**Import data in parallel**

Perform the following procedure to import data:

1.  Distribute data evenly among multiple OSS files for storage. We recommend that the number of OSS files be an integer multiple of the number of segments in AnalyticDB for PostgreSQL.

2.  Create a READABLE external table in AnalyticDB for PostgreSQL.

3.  Execute the following statement to import data in parallel:

```
INSERT INTO <target table> SELECT * FROM <external table>
```


**Export data in parallel**

Perform the following procedure to export data:

1.  Create a WRITABLE external table in AnalyticDB for PostgreSQL.

2.  Execute the following statement to export data to OSS in parallel:

```
INSERT INTO <external table> SELECT * FROM <source table>
```


**Create OSS external table syntax**

Execute the following statements to create OSS external table syntax:

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

## Parameter description {#section_fqw_pyr_52b .section}

This section provides definitions of parameters used in various operations, including:

-   [Common parameters](#)
-   [Import mode parameters](#)
-   [Export mode parameters](#)
-   [Other common parameters](#)

**Common parameters**

-   Protocol and endpoint: indicates the communication protocol and endpoint address in the "protocol name://oss\_endpoint" format. The protocol name indicates oss and oss\_endpoint indicates the domain name of the OSS region.

    **Note:** 

    You can access the database from an Alibaba Cloud host by using an internal endpoint containing "internal" in the name in order not to generate public traffic.

-   id: the AccessKey ID of the OSS account.

-   key: the AccessKey Secret of the OSS account.

-   bucket: the bucket that contains the data file to be operated. It must be an existing bucket in OSS.

-   prefix: the prefix of the path name corresponding to the data file. Prefixes are directly matched and cannot be controlled by regular expressions. Only one parameter among prefix, filepath, and dir can be specified at a time as they are mutually exclusive.

    -   All OSS files containing the specified prefix will be imported if you create a READABLE external table for data import.

        -   The following files will be imported if you set prefix to test/filename:
            -   test/filename
            -   test/filenamexxx
            -   test/filename/aa
            -   test/filenameyyy/aa
            -   test/filenameyyy/bb/aa
        -   Only the following file out of the preceding files will be imported if you set prefix to test/filename/:
            -   test/filename/aa
    -   The exported files are uniquely named based on this parameter if you create a READABLE external table for data import.

        **Note:** 

        One or more files can be exported for each data node. The names of exported files are in the `prefix_tablename_uuid.x` format. uuid indicates a timestamp in microseconds as an int64 value. x indicates the node ID. You can use an external table for multiple export operations. The files exported at a time have a unique uuid value.

-   dir: the virtual folder path in OSS. Only one parameter among prefix, filepath, and dir can be specified at a time as they are mutually exclusive.

    -   A folder path must end with "/" such as `test/mydir/`.
    -   If you use this parameter to create an external table for data import, all files under the specified virtual directory will be imported, excluding its subdirectories and files in those subdirectories. Unlike filepath, dir does not need to specify the names of files in the directory.
    -   All data will be exported to multiple files in the specified directory if this parameter is used to create an external table for data export. The names of exported files are in the `filename.x` format, where x is a number. The values of x may be inconsecutive.
-   filepath: the name of the OSS file, which contains the path. Only one parameter among prefix, filepath, and dir can be specified at a time as they are mutually exclusive. This parameter can be specified only when you are creating a READABLE external table for data import.

    -   The file specified by filepath must include the file name and path, but cannot include the bucket.
    -   The file name specified for data import must be in the `filename` or `filename.x` format. The values of x must be consecutive numbers starting from 1. For example, filepath is set to filename and OSS contains the following files:

        ```
        filename
        filename. 1
        filename. 2
        filename. 4
        ```

        The files named filename, filename.1, and filename.2 will be imported because they are numbered consecutively. filename.4 will not be imported because it is not consecutive in the file order.


**Import mode parameters**

-   async: indicates whether to enable asynchronous data import.

    -   You can enable an auxiliary thread to import data from OSS for better performance.

    -   Asynchronous data import is enabled by default. You can set async to false or f to disable asynchronous data import.

    -   Asynchronous data import consumes more hardware resources than normal data import.

-   compressiontype: indicates the format to be used to compress imported files.

    -   If this parameter is set to none \(default\), imported files are not compressed.

    -   If this parameter is set to gzip, files are imported in the GZIP format. Currently, only the GZIP format is supported.

-   compressionlevel: indicates the degree to which files written to OSS are compressed. Valid values: 1 to 9. Default value: 6.


**Export mode parameters**

-   oss\_flush\_block\_size: indicates the buffer size for a single data flush to OSS. Default value: 32 MB. Valid values: 1 MB to 128 MB.

-   oss\_file\_max\_size: indicates the maximum size of files written to OSS. If a file reaches the maximum size, the remaining data is written to another file. Default value: 1024 MB. Valid values: 8 MB to 4000 MB.

-   num\_parallel\_worker: indicates the number of active parallel compression threads when data is written to OSS. Valid values: 1 to 8. Default value: 3.

-   compressiontype: indicates the compression format of exported files.

    -   If this parameter is set to none \(default\), exported files are not compressed.

    -   If this parameter is set to gzip, exported files are in the GZIP format. Currently, only the GZIP format is supported.


Take note of the following characteristics of export mode:

-   WRITABLE is the keyword of the external table for data export. You must specify this keyword when creating an external table.

-   Only the prefix and dir parameters are supported for data export. The filepath parameter is not supported.

-   You can use the DISTRIBUTED BY clause to write data from segments to OSS based on specified distribution keys.


**Other common parameters**

The following error-tolerance parameters can be used for data import and export:

-   oss\_connect\_timeout: indicates the connection timeout period in seconds. Default value: 10 seconds.

-   oss\_dns\_cache\_timeout: indicates the DNS timeout period in seconds. Default value: 60 seconds.

-   oss\_speed\_limit: indicates the minimum data transmission rate. Default value: 1 Kbit/s.

-   oss\_speed\_time: indicates the timeout period when the data transmission rate is lower than the minimum value. Default value: 15 seconds.


When the default values are used for the preceding parameters, a timeout is triggered if the transmission rate is lower than 1 Kbit/s for 15 consecutive seconds. For more information, see [OSS SDK error handling](https://www.alibabacloud.com/help/zh/doc-detail/32141.html).

The other parameters are compatible with the original external table syntax of Greenplum. For more information about the syntax, see [Greenplum official documentation on external table syntax](http://gpdb.docs.pivotal.io/4380/ref_guide/sql_commands/CREATE_EXTERNAL_TABLE.html). These parameters include:

-   FORMAT: indicates the supported file format, such as TEXT and CSV.

-   ENCODING: indicates the data encoding format of a file, such as UTF8.

-   LOG ERRORS: indicates that the clause can ignore imported erroneous data and write the data to error\_table. You can also use the count parameter to specify the error reporting threshold.


## Examples {#section_crw_pyr_52b .section}

```
# Create an OSS external table for data import.
create readable external table ossexample
        (date text, time text, open float, high float,
        low float, volume int)
        location('oss://oss-cn-hangzhou.aliyuncs.com
        prefix=osstest/example id=XXX
        key=XXX bucket=testbucket compressiontype=gzip')
        FORMAT 'csv' (QUOTE '''' DELIMITER E'\t')
        ENCODING 'utf8'
        LOG ERRORS INTO my_error_rows SEGMENT REJECT LIMIT 5;
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
# Create an OSS external table for data export.
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
# Create a heap table for data loading.
create table example
        (date text, time text, open float,
         high float, low float, volume int)
         DISTRIBUTED BY (date);
# Load data into example from ossexample in parallel.
insert into example select * from ossexample;
# Export data from example to OSS in parallel
insert into ossexample_exp select * from example;
# As shown in the following execution plan, all segments are involved in the task.
# Each segment pulls data from OSS in parallel. The redistribution motion node calculates the hash value of the data and distributes the hash value to the corresponding segment. That segment then imports the data to the database through the insert node.
explain insert into example select * from ossexample;
                                            QUERY PLAN
-----------------------------------------------------------------------------------------------
 Insert (slice0; segments: 4)  (rows=250000 width=92)
   ->  Redistribute Motion 4:4  (slice1; segments: 4)  (cost=0.00.. 11000.00 rows=250000 width=92)
         Hash Key: ossexample.date
         ->  External Scan on ossexample  (cost=0.00.. 11000.00 rows=250000 width=92)
(4 rows)
# As shown in the following query plan, each segment exports local data directly to OSS without redistributing the data.
explain insert into ossexample_exp select * from example;
                          QUERY PLAN
---------------------------------------------------------------
 Insert (slice0; segments: 3)  (rows=1 width=92)
   ->  Seq Scan on example  (cost=0.00.. 0.00 rows=1 width=92)
(2 rows)
```

## Precautions {#section_ssw_pyr_52b .section}

-   The syntax for creating and using external tables, except for the syntax of location-related parameters, is the same as that for Greenplum.

-   Data import performance depends on the OSS performance as well as available resources of the AnalyticDB for PostgreSQL cluster, such as CPU, I/O, memory, and network resources. We recommend that you use column-based storage and compression when creating a table for the best import performance. For example, you can specify the following clause: WITH \(APPENDONLY=true, ORIENTATION=column, COMPRESSTYPE=zlib, COMPRESSLEVEL=5, BLOCKSIZE=1048576\). For more information, see [Greenplum official documentation on database table creation syntax](http://gpdb.docs.pivotal.io/4350/ref_guide/sql_commands/CREATE_TABLE.html).

-   We recommend that OSS and AnalyticDB for PostgreSQL instances be in the same region for the best import performance.


## TEXT/CSV format description {#section_usw_pyr_52b .section}

The following parameters specify the formats of files read from and written to OSS. They can be specified in the external DDL parameters.

-   The string used as a line delimiter or line break for TEXT and CVS files is '\\n.'
-   DELIMITER: indicates the string used to delimit columns.
    -   If the DELIMITER parameter is specified, the QUOTE parameter must also be specified.
    -   Recommended column delimiters include ',', '\\t', '|', and certain uncommon characters.
-   QUOTE: indicates a pair of characters that enclose a string that contains special characters.
    -   QUOTE characters are used to differentiate user data strings that contain special characters from code for the machine.
    -   For the sake of efficient coding, it is unnecessary to enclose data such as integers in QUOTE characters.
    -   QUOTE cannot be the same string specified in DELIMITER. The default value of QUOTE is a pair of double quotation marks \("\).
    -   User data that contains QUOTE characters must also contain ESCAPE characters to differentiate the user data from code for the machine.
-   ESCAPE: indicates the escape character.
    -   Escape characters are placed before characters that otherwise have a special meaning.
    -   If ESCAPE is not specified, its default value is the same as QUOTE.
    -   You can also use other characters as ESCAPE characters such as the '\\' used by MySQL by default.

**Default control characters for TEXT and CSV files**

|Control character|TEXT|CSV|
|-----------------|----|---|
|DELIMITER|\\t \(tab\)|, \(comma\)|
|QUOTE|" \(double quotation mark\)|" \(double quotation mark\)|
|ESCAPE|N/A|Same as QUOTE|
|NULL|\\N \(backslash-N\)|\(Empty string without quotation marks\)|

**Note:** 

All control characters must be single-byte characters.

## SDK error handling {#section_dtw_pyr_52b .section}

If an error occurs during the import or export process, the error log contains the following information:

-   code: the HTTP status code of the error request.

-   error\_code: the OSS error code.

-   error\_msg: the OSS error message.

-   req\_id: the UUID that identifies the request. If you require assistance in solving a problem, you can submit a ticket with the req\_id of the failed request to OSS development engineers.


For more information, see [OSS API error responses](https://www.alibabacloud.com/help/zh/doc-detail/32005.html). Timeout errors can be handled by using oss\_ext related parameters.

## FAQ {#section_ftw_pyr_52b .section}

If the import process is taking an abnormally long time, see the descriptions on import performance in the “Precautions” section.

## References {#section_msd_qzr_52b .section}

-   [OSS endpoint information](https://www.alibabacloud.com/help/zh/doc-detail/31834.html)

-   [OSS help](https://www.alibabacloud.com/help/zh/product/31815.htm)

-   [OSS SDK error handling](https://www.alibabacloud.com/help/zh/doc-detail/32141.html)

-   [OSS API error responses](https://www.alibabacloud.com/help/zh/doc-detail/32005.html) 

-   [Greenplum official documentation on database external table syntax](http://gpdb.docs.pivotal.io/4380/ref_guide/sql_commands/CREATE_EXTERNAL_TABLE.html)

-   [Greenplum official documentation on database table creation syntax](http://gpdb.docs.pivotal.io/4350/ref_guide/sql_commands/CREATE_TABLE.html)


