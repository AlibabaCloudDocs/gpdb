# Use the COPY or UNLOAD statement to import or export data between OSS foreign tables and local tables

This topic describes how to use the COPY or UNLOAD statement to import or export data between Object Storage Service \(OSS\) foreign tables and local tables. AnalyticDB for PostgreSQL allows you to use the COPY statement to import data from OSS foreign tables to local tables. You can also use the UNLOAD statement to export data from local tables to OSS foreign tables.

The COPY or UNLOAD statement is used to import or export data based on OSS foreign tables. For more information, see [Use OSS foreign tables to access OSS data](/intl.en-US/Beginner Developer Guide/Use OSS foreign tables to access OSS data.md).

## Precautions

When you use the COPY or UNLOAD statement to export data and store the data in a CSV file, you must enclose some options in quotation marks \("\) and write the options in lowercase letters. If you do not follow this requirement, some options may be the same as keywords. This may result in syntax errors. You must specify the following options in a particular way: `delimiter`, `quote`, `null`, `header`, `escape`, and `encoding`. Example:

```
UNLOAD ('select * from table') 
TO 'path'
ACCESS_KEY_ID 'id'
SECRET_ACCESS_KEY 'key'
FORMAT csv
"delimiter" '|'
"quote" '"'
"null" ''
"header" 'true'
"escape" 'E'
"encoding" 'utf-8'
FDW 'oss_fdw'
ENDPOINT 'endpoint';
```

## COPY

-   **Syntax**

    ```
    COPY table-name 
    [ column-list ]
    FROM data_source
    ACCESS_KEY_ID '<access-key-id>'
    SECRET_ACCESS_KEY '<secret-access-key>'
    [ [ FORMAT ] [ AS ] data_format ]
    [ MANIFEST ]
    [ option 'value' [ ... ] ]
    ```

    -   table\_name: the name of the local table in which the imported data is stored. The local table must exist in the AnalyticDB for PostgreSQL instance.
    -   column-list: optional. The list of columns to which you want to write data. If you do not specify this parameter, data is written to all columns.
    -   data\_source: the URL of the OSS bucket from which data is obtained. Example: oss://bucket\_name/path\_prefix.
    -   <access-key-id\>: the AccessKey ID of the OSS account.
    -   <secret-access-key\>: the AccessKey secret of the OSS account.
    -   \[ FORMAT \] \[ AS \] data\_format: optional. The file format in which the imported data is stored. By default, FORMAT AS CSV is used. You can set data\_format to BINARY, CSV, JSON, JSONLINE, ORC, PARQUET, or TEXT. In the parameter name, FORMAT and AS can be omitted. For example, FORMAT AS CSV or FORMAT CSV is equivalent to CSV.
    -   \[ MANIFEST \]: specifies that the data source is a manifest file. The manifest file must be in the JSON format and consist of the following elements:
        -   entries: an array of the OSS objects in the manifest file. The OSS objects can be in different buckets or paths. The OSS objects must be accessible by using the same ACCESS\_KEY\_ID or SECRET\_ACCESS\_KEY. Example:

            ```
            {   "entries": [   
                  {"url": "oss://adbpg-regress/local_t/_seg2_0.csv", "mandatory":true},
                  {"url": "oss://adbpg-regress/local_t/_seg1_0.csv", "mandatory":true},
                  {"url": "oss://adbpg-regress/local_t/_seg0_0.csv", "mandatory":true}, 
                  {"url": "oss://adbpg-regress-2/local_t/_seg1_0.csv", "mandatory":true}, 
                  {"url": "oss://adbpg-regress-2/local_t/_seg2_0.csv", "mandatory":true},  
                  {"url": "oss://adbpg-regress-2/local_t/_seg0_0.csv", "mandatory":true} 
                ]
            }
            ```

        -   url: the full path of an OSS object.
        -   mandatory: specifies whether to report an error when an OSS object is not found.
    -   \[ option \[ value \] \[ ... \] \]: a list of one or more options. Specify each option in the key-value pair format. The following table describes the available options.

        |Option|Type|Required|Description|
        |------|----|--------|-----------|
        |endpoint|String|Yes|The endpoint of the OSS bucket.|
        |fdw|String|Yes|The name of the oss\_fdw plug-in. The oss\_fdw plug-in is required when you create a temporary OSS server for the COPY statement.|
        |Other options that are used to create an OSS foreign table, including format, filetype, delimiter, and escape|N/A|N/A|The options that are used to create a temporary OSS foreign table. For more information, see [Use OSS foreign tables to access OSS data](/intl.en-US/Beginner Developer Guide/Use OSS foreign tables to access OSS data.md).|

-   **Example 1**

    1.  Create a local table.

        ```
         postgres=# create table local_t2 (a int, b float8, c text);
        ```

    2.  Use the COPY statement to import data to columns a and c. Column b is assigned NULL.

        ```
        postgres=# COPY local_t2 (a, c)
        FROM 'oss://adbpg-regress/local_t/'
        ACCESS_KEY_ID 'id'
        SECRET_ACCESS_KEY 'key'
        FORMAT AS CSV
        ENDPOINT '<endpoint>'
        FDW 'oss_fdw';
        
        postgres=# select * from local_t2 limit 10;
         a  | b |                c
        ----+---+----------------------------------
         12 |   | a24cba6ebdc5e0c485cd88ef60b72fea
         15 |   | c4d3028f5205fab98e5f43c7945db4ba
         20 |   | 769884311db01f400e21a903a3f1cb50
         26 |   | 7d12c981d262e0067ea1a04368f32f2a
         30 |   | 4e64bda52d54d263d16f42771b1d0225
         35 |   | b70c976d4c04568bd497b42a7d2e451d
         40 |   | d07ce2948b8618b47c351b6e222182f6
         46 |   | c2234393f878f5557776b7e778299564
         47 |   | cde904b2331fa274cd8d9266aa858342
         50 |   | 1235b900fb644bb36440a274314e4b6b
        (10 rows)
        ```

    3.  Check whether the data in columns a and c of the local\_t2 table is the same as that of the local\_t table.

        ```
        postgres=# select sum(hashtext(t.a::text)) as col_a_hash, sum(hashtext(t.c::text)) as col_c_hash from local_t2 t;
         col_a_hash  | col_c_hash
        -------------+-------------
         23725368368 | 13447976580
        (1 row)
        
        postgres=# select sum(hashtext(t.a::text)) as col_a_hash, sum(hashtext(t.c::text)) as col_c_hash from local_t t;
         col_a_hash  | col_c_hash
        -------------+-------------
         23725368368 | 13447976580
        (1 row)
        ```

    4.  Save the data in a format other than CSV.

        ```
        -- Save the data in the ORC format.
        COPY tt
        FROM 'oss://adbpg-regress/q_oss_orc_list/'
        ACCESS_KEY_ID 'id'
        SECRET_ACCESS_KEY 'key'
        FORMAT AS ORC
        ENDPOINT '<endpoint>'
        FDW 'oss_fdw';
        
        -- Save the data in the PARQUET format.
        COPY tp
        FROM  'oss://adbpg-regress/test_parquet/'
        ACCESS_KEY_ID 'id'
        SECRET_ACCESS_KEY 'key'
        FORMAT AS PARQUET
        ENDPOINT '<endpoint>'
        FDW 'oss_fdw';
        ```

-   **Example 2**

    1.  Create a local table.

        ```
         create table local_manifest (a int, c text);
        ```

    2.  Create a manifest file, in which the OSS objects can be in different buckets.

        ```
        {
           "entries": [
              {"url": "oss://adbpg-regress/local_t/_20210114103840_83f407434beccbd4eb2a0ce45ef39568_1450404435_seg2_0.csv", "mandatory":true},
              {"url": "oss://adbpg-regress/local_t/_20210114103840_83f407434beccbd4eb2a0ce45ef39568_1856683967_seg1_0.csv", "mandatory":true},
              {"url": "oss://adbpg-regress/local_t/_20210114103840_83f407434beccbd4eb2a0ce45ef39568_1880804901_seg0_0.csv", "mandatory":true},
              {"url": "oss://adbpg-regress-2/local_t/_20210114103849_67100080728ef95228e662bc02cb99d1_1008521914_seg1_0.csv", "mandatory":true},
              {"url": "oss://adbpg-regress-2/local_t/_20210114103849_67100080728ef95228e662bc02cb99d1_1234881553_seg2_0.csv", "mandatory":true},
              {"url": "oss://adbpg-regress-2/local_t/_20210114103849_67100080728ef95228e662bc02cb99d1_1711667760_seg0_0.csv", "mandatory":true}
           ]
        }
        ```

    3.  Use the COPY statement to import the data from the manifest file to the local table.

        ```
        -- Import an OSS object from the manifest file.
        COPY local_manifest
        FROM 'oss://adbpg-regress-2/unload_manifest/t_manifest'
        ACCESS_KEY_ID '<id>'
        SECRET_ACCESS_KEY '<key>'
        FORMAT AS CSV
        MANIFEST                       -- Specifies that the data source is a manifest file.
        ENDPOINT '<endpoint>'
        FDW 'oss_fdw';
        ```

-   **Example 3**

    When you use the COPY statement to import data from OSS, error lines may be returned. In this case, you can set the following options to implement fault tolerance:

    -   log\_errors: specifies whether to record information of the error lines in log files.
    -   segment\_reject\_limit: Up to 10 error lines are allowed. If the number of error lines exceeds 10, the system returns an error and exits.
    1.  Create a local table.

        ```
         create table sales(id integer, value float8, x text) distributed by (id);
        ```

    2.  Use the COPY statement to import data from an OSS object that has three error lines.

        ```
        COPY sales
        FROM 'oss://adbpg-const/error_sales/'
        ACCESS_KEY_ID '<id>'
        SECRET_ACCESS_KEY '<key>'
        FORMAT AS csv
        log_errors 'true'                -- The information of the error lines is recorded in log files.
        segment_reject_limit '10'        -- Up to 10 error lines are allowed. If the number of error lines exceeds 10, the system returns an error and exits.
        endpoint '<endpoint>'
        FDW 'oss_fdw';
        NOTICE:  found 3 data formatting errors (3 or more input rows), rejected related input data
        COPY FOREIGN TABLE
        ```

    3.  Execute the following statement to query the error line details:

        ```
        select * from gp_read_error_log('<Name of the source table on which the COPY statement is executed>');
        ```

        In the following example, the error line details of the sales table are queried:

        ```
        select * from gp_read_error_log('sales');
                    cmdtime            |                    relname                     |        filename         | linenum | bytenum |                          errmsg                           | rawdata | rawbytes
        -------------------------------+------------------------------------------------+-------------------------+---------+---------+-----------------------------------------------------------+---------+----------
         2021-02-08 14:24:04.225238+08 | adbpgforeigntabletmp_20210208142403_1936866966 | error_sales/sales.2.csv |       2 |         | invalid byte sequence for encoding "UTF8": 0xed 0xab 0xad |         | \x
         2021-02-08 14:24:04.225238+08 | adbpgforeigntabletmp_20210208142403_1936866966 | error_sales/sales.2.csv |       3 |         | invalid byte sequence for encoding "UTF8": 0xed 0xab 0xad |         | \x
         2021-02-08 14:24:04.225269+08 | adbpgforeigntabletmp_20210208142403_1936866966 | error_sales/sales.3.csv |       2 |         | invalid byte sequence for encoding "UTF8": 0xed 0xab 0xad |         | \x
        (3 rows)
        ```

        **Note:** The additionally saved error line logs occupy storage space. You can use the following syntax to delete the error line logs: `select gp_truncate_error_log('<Name of the table>')`.


## UNLOAD

-   **Syntax**

    ```
    UNLOAD ('select-statement')
    TO destination_url
    ACCESS_KEY_ID '<access-key-id>'
    SECRET_ACCESS_KEY '<secret-access-key>'
    [ [ FORMAT ] [ AS ] data_format ]
    [ MANIFEST [ '<manifest_url>' ] ]
    [ PARALLEL [ { ON | TRUE } | { OFF | FALSE } ] ]
    [ option 'value' [ ... ] ]
    ```

    The following section describes the parameters:

    -   select-statement: the SELECT query statement. The query result data is written to OSS.
    -   destination\_url: the URL of the OSS bucket in which the query result data is stored. Example: oss://bucket-name/path-prefix.
    -   <access-key-id\>: the AccessKey ID of the OSS account.
    -   <secret-access-key\>: the AccessKey secret of the OSS account.
    -   \[ FORMAT \] \[ AS \] data\_format: optional. The file format in which the exported data is stored. By default, FORMAT AS CSV is used. You can set data\_format to BINARY, CSV, JSON, JSONLINE, ORC, PARQUET, or TEXT. In the parameter name, FORMAT and AS can be omitted. For example, FORMAT AS CSV or FORMAT CSV is equivalent to CSV.
    -   MANIFEST: specifies to generate a manifest file when data is imported. If you specify <manifest\_url\>, the full path of the manifest file is different from that of the data files that are located in a different bucket. The path must end with the manifest suffix. If you do not specify <manifest\_url\>, the path of the manifest file is the same as that of the data files.

        **Note:** If the file that is specified by <manifest\_url\> already exists, you must set the allowoverwrite option to true in \[ option \[ value \] \[ ...\] \] to overwrite the manifest file.

    -   PARALLEL: specifies whether to export data from multiple compute nodes in parallel. By default, data is exported from multiple compute nodes in parallel. A separate exported file is generated for each compute node. If you set this parameter to OFF/FALSE, the data is not exported in parallel. If the size of the exported data does not exceed 8 GB, the exported data is saved in a single file.
    -   \[ option \[ value \] \[ ... \] \]: a list of one or more options. Specify each option in the key-value pair format. The following table describes the available options.

        |Option|Type|Required|Description|
        |------|----|--------|-----------|
        |endpoint|String|Yes|The endpoint of the OSS bucket.|
        |fdw|String|Yes|The name of the oss\_fdw plug-in. The oss\_fdw plug-in is required when you create a temporary OSS server for the UNLOAD statement.|
        |allowoverwrite|Boolean|No|Specifies whether to overwrite the existing manifest file. **Note:** Only the manifest file can be overwritten. The data files cannot be overwritten. |
        |Other options that are used to create an OSS foreign table, including format, filetype, delimiter, and escape|N/A|N/A|The options that are used to create a temporary OSS foreign table. For more information, see [Use OSS foreign tables to access OSS data](/intl.en-US/Beginner Developer Guide/Use OSS foreign tables to access OSS data.md).|


-   **Example 1**

    1.  Create a local table and insert test data into the table.

        ```
        postgres=# create table local_t (a int, b float8, c text);
        postgres=# insert into local_t select r, random() * 1000, md5(random()::text) from generate_series(1,1000)r;
        INSERT 0 1000
        postgres=# select * from local_t limit 5;
         a  |        b         |                c
        ----+------------------+----------------------------------
          5 |  550.81393988803 | 8009fa725372e996786849213a695ce0
          6 | 95.8335199393332 | ce7952c6728cdffdee06cc5b502d6457
          9 | 421.379795763642 | d3260ccbf6b9c03f3658d96bb7678b4d
         10 | 362.347379792482 | 2bbbf89d23a2f83b089b589f55b5c4fc
         11 | 800.203878898174 | a52994c5573e6b36d8a1c357bf800ce5
        (5 rows)
        ```

    2.  Use the UNLOAD statement to export the data from the specified columns of the local table to OSS and save the data in the CSV format.

        ```
        postgres=# UNLOAD ('select a, c from local_t') TO 'oss://adbpg-regress/local_t/'
        ACCESS_KEY_ID '<id>'
        SECRET_ACCESS_KEY '<key>'
        FORMAT AS CSV
        ENDPOINT '<endpoint>'
        FDW 'oss_fdw';
        NOTICE:  OSS output prefix: "local_t/adbpgforeigntabletmp_20200907164801_1354519958_20200907164801_652261618".
        UNLOAD
        ```

    3.  Check whether the CSV file is written to the specified path.

        ```
        $ ossutil --config hangzhou-zmf.config ls oss://adbpg-regress/local_t/
        LastModifiedTime                   Size(B)  StorageClass   ETAG                                  ObjectName
        2020-09-07 16:48:01 +0800 CST        12023      Standard   9F38B5407142C044C1F3555F00000000      oss://adbpg-regress/local_t/adbpgforeigntabletmp_20200907164801_1354519958_20200907164801_652261618_seg0_0.csv
        2020-09-07 16:48:01 +0800 CST        12469      Standard   807BA680A0DED49BC1F3555F00000000      oss://adbpg-regress/local_t/adbpgforeigntabletmp_20200907164801_1354519958_20200907164801_652261618_seg1_0.csv
        2020-09-07 16:48:01 +0800 CST        12401      Standard   3524F68F628CEB64C1F3555F00000000      oss://adbpg-regress/local_t/adbpgforeigntabletmp_20200907164801_1354519958_20200907164801_652261618_seg2_0.csv
        Object Number is: 3
        
        0.153414(s) elapsed
        ```

        Check whether the CSV file contains only the data in columns a and c of the local\_t table.

        ```
        $ head -n 10 adbpgforeigntabletmp_20200907164801_1354519958_20200907164801_652261618_seg2_0.csv
        7,1225341d0d367a69b1b345536b21ef73
        19,424a7a5c36066842f4de8c8a8341fc89
        27,c214432e9928e4a6f7bef7bd815424c0
        29,ade5d636e2b5d2a606a02e79255da4bd
        37,85660e60ede47b68493f6295620db568
        77,e1be448ba2b08f0a2ca05b7ed812abfd
        80,5e85d597a3b0f2f9736a728724a0f9e0
        92,dc23f76f0b1446504b8f1c2274521d2f
        94,50304822488d55a500e3a71bcf40890f
        97,e970fde8cd0df9c6b610925a488f6042
        ```

-   **Example 2**

    1.  Use the UNLOAD statement to export data and generate a manifest file. The path of the manifest file is the same as that of the data files.

        ```
        -- Use the UNLOAD statement to export data and generate a manifest file.
        UNLOAD ('select * from local_t') TO 'oss://adbpg-regress/local_t/'
        ACCESS_KEY_ID '<id>'
        SECRET_ACCESS_KEY '<key>'
        FORMAT AS CSV
        MANIFEST    -- Specifies to generate a manifest file when the UNLOAD statement is used to export data. The path of the manifest file is the same as that of the data files.
        ENDPOINT '<endpoint>'
        FDW 'oss_fdw';
        NOTICE:  OSS output prefix: "local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c".
        UNLOAD
        
        -- View the list of exported files. The list includes several data files and a manifest file.
        ossutil ls -s oss://adbpg-regress/local_t/
        oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_162488956_seg1_0.csv
        oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_163756258_seg0_0.csv
        oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_1741120517_seg2_0.csv
        oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_manifest
        Object Number is: 4
        
        0.136180(s) elapsed
        
        -- View the content of the manifest file.
        ossutil cat oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_manifest
        {
           "entries": [
              {"url": "oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_162488956_seg1_0.csv"},
              {"url": "oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_163756258_seg0_0.csv"},
              {"url": "oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_1741120517_seg2_0.csv"}
           ]
        }
        ```

    2.  Use the UNLOAD statement to export data and generate a specified manifest file. The path of the manifest file may be different from that of the data files.

        **Note:** If you set the ALLOWOVERWRITE option to true, the existing manifest file is overwritten. However, the data files are not overwritten. The data files can be manually deleted.

        ```
        -- Use the UNLOAD statement to export data and generate a specified manifest file. The manifest file is stored in a bucket that is different from that of the data files.
        UNLOAD ('select * from local_t') TO 'oss://adbpg-regress/local_t/'
        ACCESS_KEY_ID '<id>'
        SECRET_ACCESS_KEY '<key>'
        FORMAT AS CSV
        MANIFEST 'oss://adbpg-regress-2/unload_manifest/t_manifest' -- Use the UNLOAD statement to export data and generate a specified manifest file.
        ALLOWOVERWRITE 'true' -- Overwrite the existing manifest file.
        ENDPOINT '<endpoint>'
        FDW 'oss_fdw';
        NOTICE:  OSS output prefix: "local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c".
        UNLOAD
        
        -- View the list of exported files. Only data files are included in the list.
        ossutil ls -s oss://adbpg-regress/local_t/
        oss://adbpg-regress/local_t/_20210114100956_4d3395a9501f6e22da724a2b6df1b6d3_1736161168_seg0_0.csv
        oss://adbpg-regress/local_t/_20210114100956_4d3395a9501f6e22da724a2b6df1b6d3_1925769064_seg2_0.csv
        oss://adbpg-regress/local_t/_20210114100956_4d3395a9501f6e22da724a2b6df1b6d3_644328153_seg1_0.csv
        Object Number is: 3
        
        0.118540(s) elapsed
        
        -- View the content of the manifest file that is stored in a bucket that is different from that of the data files.
        ossutil cat oss://adbpg-regress-2/unload_manifest/t_manifest
        {
           "entries": [
              {"url": "oss://adbpg-regress/local_t/_20210114100956_4d3395a9501f6e22da724a2b6df1b6d3_1736161168_seg0_0.csv"},
              {"url": "oss://adbpg-regress/local_t/_20210114100956_4d3395a9501f6e22da724a2b6df1b6d3_1925769064_seg2_0.csv"},
              {"url": "oss://adbpg-regress/local_t/_20210114100956_4d3395a9501f6e22da724a2b6df1b6d3_644328153_seg1_0.csv"}
           ]
        }
        ```


