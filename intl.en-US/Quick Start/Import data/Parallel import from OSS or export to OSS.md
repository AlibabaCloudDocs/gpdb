# Parallel import from OSS or export to OSS {#concept_ofw_3mr_52b .concept}

HybridDB for PostgreSQL supports parallel import from OSS or export to OSS through external tables \(which is called the gpossext function\). It can also compress OSS external table files in gzip format to reduce the storage space and the costs.

The gpossext function can read or write text/csv files or text/csv files in gzip format.

## Create an extension for OSS external tables \(oss\_ext\) { .section}

Before using OSS external tables, create an oss\_ext extension for each database.

-   To create an oss\_ext, run the command: `CREATE EXTENSION IF NOT EXISTS oss_ext;`
-   To delete an oss\_ext, run the command: `DROP EXTENSION IF EXISTS oss_ext;`

## Import data in parallel { .section}

Follow these steps to import data:

1.  Distribute and store the data evenly in multiple OSS files. The number of files is preferably an integral multiple of the number of HybridDB for PostgreSQL data nodes \(number of Segments\).

2.  Create a READABLE external table in the HybridDB for PostgreSQL database.

3.  Run the following command to import data in parallel.

    ```
    INSERT INTO <target table> SELECT * FROM <external table>
    ```


## Export data in parallel { .section}

Follow these steps to export data:

1.  Create a WRITABLE external table in the HybridDB for PostgreSQL database.

2.  Run the following command to export data to OSS in parallel.

    ```
    INSERT INTO <external table> SELECT * FROM <source table>
    ```


## Syntax of creating OSS external tables { .section}

The syntax of creating OSS external tables is as follows:

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

## Parameters description { .section}

**Common parameters**

-   Protocol and endpoint: The format is `protocol name: //oss_endpoint`. The “protocol name” is OSS, and “oss\_endpoint” is the domain name of the corresponding OSS region.

    **Note:** If the access request is from an Alibaba Cloud host, use the intranet domain name \(that is, with “internal” in the domain name\) to avoid incurring public network traffic.

-   id: OSS account ID.

-   key: OSS account key.

-   bucket: Specifies the bucket where the data file is located. You need to create the bucket in OSS in advance.

-   prefix: Specifies the prefix of the corresponding path of the data file. The prefix does not support regular expressions and is only a matching prefix. In addition, it is mutually exclusive with filepath and dir. You can set only one of them.

    -   If you create a READABLE external table for data import, all the OSS files with this prefix are imported.

        -   If you have specified `prefix=test/filename`, all the following files are imported:

            -   test/filename
            -   test/filenamexxx
            -   test/filename/aa
            -   test/filenameyyy/aa
            -   test/filenameyyy/bb/aa
        -   If you have specified `prefix=test/filename/`, only the following files are imported \(other files precedingly listed are not imported\):

            -   test/filename/aa
    -   If you create a WRITABLE external table for data export, a unique file name is generated automatically based on the prefix to name the exported file.

        **Note:** 

        When more than one file are exported, every data node exports one or more files. The exported file name format is `prefix_tablename_uuid.x`. To be specific, the `uuid` is the generated int64 integer value \(time stamp in microsecond\), and the `x` is the node ID. HybridDB for PostgreSQL allows you to use the same external table to export data multiple times. The exported files from each export are identified by the UUID, and the exported files in the same export share the same UUID.

-   dir: The path of virtual folders in OSS. It is mutually exclusive with prefix and filepath, you can only set one of them.

    -   The folder path ends with "/". For example, test/mydir/.

    -   If you use this parameter to create an external table during data importing, all the files under the specified virtual directory are imported, excluding its subdirectories and files under the subdirectories. Unlike filepath, the dir directory has no naming requirements for files under it.

    -   If you use this parameter to create an external table during data exporting, all the data is exported to the multiple files under this directory. The output file names follow the format of `filename.x`. To be specific, the `x` is a number but may be discontinuous.

-   filepath: The file name that contains a path in OSS. It is mutually exclusive with prefix and dir, you can only set one of them. And you can **ONLY** specify filepath at the creation of a READABLE external table \(that is, only usable during data import\).

    -   The file name contains the file path, but does not contain the bucket name.

    -   The file naming rule must follow `filename` or `filename.x` during data import. `x` is required to start from 1 and be continuous. For example, if you specify `filepath = filename` and the OSS contains the following files:

        ```
        filename 
        filename.1 
        filename.2 
        filename.4,
        ```

        As a result, the imported files include filename, filename.1, and filename.2. Because filename.3 does not exist, filename.4 won’t be imported.


**Import mode parameters**

-   async: whether to enable asynchronous mode to import data or not.

    -   Enabling worker threads to import data from OSS can improve the import performance.
    -   Asynchronous mode is enabled by default, and it consumes more hardware resources than the normal mode. You can use `async=false` or `async=f` to disable it.
-   compressiontype: The compression format of the imported files.

    -   If specified to none \(default value\), it indicates that the imported files are not compressed.
    -   If specified to gzip, it indicates that the imported format is gzip. Only the gzip compression format is supported.

**Export mode parameters**

-   oss\_flush\_block\_size: The buffer size for a single data flushed to OSS, 32 MB by default. The value ranges from 1 MB to 128 MB.

-   oss\_file\_max\_size: The maximum size of the file written to OSS. When this limit is exceeded, the export switches to another file to continue data writing. The value is 1,024 MB by default and ranges from 8 MB to 4,000 MB.

-   compressiontype: The compression format of the exported files.

    -   If specified to none \(default value\), it indicates that the exported files are not compressed.
    -   If specified to gzip, it indicates that the exported format is gzip. Only the gzip compression format is supported.

In addition, pay attention to the following items for the export mode:

-   WRITABLE is the key word of the external table in the export mode. It must be explicitly specified when you create an external table.

-   The export mode only supports *prefix* and *dir* parameter modes, and does not support *filepath*.

-   The DISTRIBUTED BY clause in the export mode enables the data node \(Segment\) to write data to OSS according to the specified distribution key.


**Other general parameters**

The import and export modes also involve the following fault tolerance parameters:

-   oss\_connect\_timeout: Sets the connection timeout. The unit is second and the default value is 10 seconds.

-   oss\_dns\_cache\_timeout: Sets the DNS timeout. The unit is second and the default value is 60 seconds.

-   oss\_speed\_limit: Sets the minimum tolerable rate. The default value is 1,024 \(that is, 1 K\).

-   oss\_speed\_time: Sets the maximum tolerable duration. The default value is 15 seconds.


With all the preceding parameters set as default, timeout is triggered when the transmission speed is slower than 1 K for 15 consecutive seconds. For details, see [OSS SDK error handling](https://www.alibabacloud.com/help/doc-detail/32141.htm).

All other parameters are compatible with the legacy syntax of Greenplum EXTERNAL TABLE. For specific syntax explanations, see [Greenplum External Table Syntax Official Documentation](http://gpdb.docs.pivotal.io/4380/ref_guide/sql_commands/CREATE_EXTERNAL_TABLE.html). Such parameters mainly include:

-   FORMAT: The supported file formats, including text and csv.
-   ENCODING: The encoding format of the data in the file, such as UTF-8.
-   LOG ERRORS: Specifies that the clause can ignore erroneous data during the import and write the data into error\_table. You can also specify the error reporting threshold by using the count parameter.

## Examples { .section}

```
# Create an external table for OSS import
create readable external table ossexample
        (date text, time text, open float, high float,
        low float, volume int) 
        location('oss://oss-cn-hangzhou.aliyuncs.com
        prefix=osstest/example id=XXX
        key=XXX bucket=testbucket' compressiontype=gzip)
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
# Create an external table for OSS export
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
# Create a heap table to load data
create table example
        (date text, time text, open float,
         high float, low float, volume int)
         DISTRIBUTED BY (date);
# Load data from ossexample to example in parallel
insert into example select * from ossexample;
# Export data from example to OSS in parallel
insert into ossexample_exp select * from example;
# We can see from the following query plan that every segment participates in the work.
# They pull data from the OSS in parallel and then use the Redistribute Motion node to distribute the hashed data to corresponding segments. Data-receiving segments store the data to the database through the Insert node.
explain insert into example select * from ossexample;
                                            QUERY PLAN                                            
-----------------------------------------------------------------------------------------------
 Insert (slice0; segments: 4)  (rows=250000 width=92)
   ->  Redistribute Motion 4:4  (slice1; segments: 4)  (cost=0.00..11000.00 rows=250000 width=92)
         Hash Key: ossexample.date
         ->  External Scan on ossexample  (cost=0.00..11000.00 rows=250000 width=92)
(4 rows)
# We can see from the following query plan that the segment exports local data directly to the OSS without data redistribution.
explain insert into ossexample_exp select * from example;
                          QUERY PLAN                           
---------------------------------------------------------------
 Insert (slice0; segments: 3)  (rows=1 width=92)
   ->  Seq Scan on example  (cost=0.00..0.00 rows=1 width=92)
(2 rows)
```

**Attention**

-   Apart from the location related parameters, the rest part of the syntax for creating and using external tables is consistent with that of Greenplum.

-   The data importing performance is related with the HybridDB for PostgreSQL cluster resources \(CPU, IO, memory, network, and so on\) and OSS. We recommend that you use compressed column store when creating a table to achieve optimal importing performance. For example, you can specify the clause `WITH (APPENDONLY=true, ORIENTATION=column, COMPRESSTYPE=zlib, COMPRESSLEVEL=5, BLOCKSIZE=1048576)`. For details, see [Greenplum Database Tabulation Syntax Official Documentation](http://gpdb.docs.pivotal.io/4350/ref_guide/sql_commands/CREATE_TABLE.html).

-   The ossendpoint region must match the HybridDB for PostgreSQL region to ensure the data importing performance. We recommend that you configure the OSS and HybridDB for PostgreSQL instances in the same region to achieve the optimal performance. For related information, see [OSS Endpoint Information](https://www.alibabacloud.com/help/doc-detail/31834.htm).


## TEXT/CSV format description { .section}

You can specify the following parameters in the external table DDL parameters to specify the file format for read/write operations of OSS:

-   The TEXT/CSV row delimiter is ‘\\n’, which is a newline character.
-   DELIMITER is the delimeter used to define columns:
    -   If DELIMITER is included in the user data, the QUOTE parameter is required.
    -   Recommended column delimeters are ',', '\\t', '|' or some infrequent characters.
-   QUOTE is used to wrap user data that contains special characters in columns.
    -   Strings containing special characters are wrapped by QUOTE to distinguish user data from control characters.
    -   Sometimes you do not need to wrap data with QUOTE due to considerations of performance optimization, for example, in the case of integers.
    -   QUOTE cannot be the same as DELIMITER. The default QUOTE is double quotes.
    -   If the user data contains a QUOTE character, it needs to be distinguished by using the escape character ESCAPE.
-   ESCAPE is the espace character for special characters
    -   The escape character appears before certain special characters to indicate that they are not special characters.
    -   ESCAPE is the same as QUOTE by default, which is double quotes.
    -   It can also be specified as '\\' \(MySQL's default escape character\) or other characters.

|Control Character \\ Format|TEXT|CSV|
|---------------------------|----|---|
|DELIMITER \(Column delimiter\)|\\t \(tab\)|, \(comma\)|
|QUOTE \(quoted\)|" \(double-quote\)|" \(double-quote\)|
|ESCAPE \(escape\)|\(Not applicable\)|Same as QUOTE|
|NULL \(null\)|\\N \(backslash-N\)|\(empty string without quotes\)|

All control characters must be single-byte characters.

## SDK error handling { .section}

If the data import and export fails, the error log displays the following information:

-   code: The HTTP status code of the erroneous request.

-   error\_code: The error code of OSS.

-   error\_msg: The error message of OSS.

-   req\_id: The UUID of the request. If you cannot solve the problem, use the req\_id to ask OSS development engineers for help.


For details, see [OSS API Error Response](https://www.alibabacloud.com/help/doc-detail/32005.htm). Timeout-related errors can be handled by using oss\_ext related parameters.

## FAQs { .section}

If the import is too slow, see the import performance descriptions in the preceding [Attention](#attention) section.

## Reference { .section}

-   [OSS Endpoint Information](https://www.alibabacloud.com/help/doc-detail/31834.htm)
-   [OSS Help Page](https://www.alibabacloud.com/help/product/31815.htm)
-   [OSS SDK Error Handling](https://www.alibabacloud.com/help/doc-detail/32141.htm)
-   [OSS API Error Response](https://www.alibabacloud.com/help/doc-detail/32005.htm)
-   [Greenplum Database External Table Syntax Official Documentation](http://gpdb.docs.pivotal.io/4380/ref_guide/sql_commands/CREATE_EXTERNAL_TABLE.html)
-   [Greenplum Database Tabulation Syntax Official Documentation](http://gpdb.docs.pivotal.io/4350/ref_guide/sql_commands/CREATE_TABLE.html)

