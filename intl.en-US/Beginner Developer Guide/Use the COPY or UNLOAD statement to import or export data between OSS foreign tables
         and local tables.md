# Use the COPY or UNLOAD statement to import or export data between OSS foreign tables and local tables

This topic describes how to use the COPY or UNLOAD statement to import or export data between OSS foreign tables and local tables. AnalyticDB for PostgreSQL allows you to use the COPY statement to import data from OSS foreign tables to local tables. You can also use the UNLOAD statement to export data from local tables to OSS foreign tables. The COPY or UNLOAD statement is used to import or export data based on the OSS foreign tables that contain user mappings from your AnalyticDB for PostgreSQL instance. For more information, see [Use OSS foreign tables to access OSS data](/intl.en-US/Beginner Developer Guide/Use OSS foreign tables to access OSS data.md).

## UNLOAD

-   **Syntax**

    ```
    UNLOAD ('select-statement')
    TO destination_url
    ACCESS_KEY_ID '<access-key-id>'
    SECRET_ACCESS_KEY '<secret-access-key>'
    [ [ FORMAT ] [ AS ] data_format ] 
    [ [option value]  ...  ]
    ```

    -   select-statement: the SELECT query statement. The query results are written to OSS.
    -   destination\_url: the URL of the OSS bucket in which the query result data is stored. Example: oss://bucket-name/path-prefix.
    -   <access-key-id\>: the AccessKey ID of the OSS account.
    -   <secret-access-key\>: the AccessKey secret of the OSS account.
    -   \[ FORMAT \] \[ AS \] data\_format: the file format in which the exported data is stored. This parameter is optional. The default format is CSV. You can set data\_format to CSV, ORC, or TEXT.
    -   \[ option \[ value \] \[, ... \] \]: a list of one or more options. Specify each option in the key-value pair format. The following table describes the available parameters.

        |Option|Type|Unit|Required|Default value|Description|
        |------|----|----|--------|-------------|-----------|
        |endpoint|String|None|Yes|None|The endpoint for the specified OSS region.|
        |fdw|String|None|Yes|None|The name of the oss\_fdw plug-in. The oss\_fdw plug-in is required when you create a temporary OSS server for the UNLOAD statement.|
        |Other options that are used to create an OSS foreign table, including format, filetype, delimiter, and escape.|N/A|None| | |The options that are used to create a temporary OSS foreign table. For more information, see [Use OSS foreign tables to access OSS data](/intl.en-US/Beginner Developer Guide/Use OSS foreign tables to access OSS data.md).|


-   **Examples**

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
        -- Use the UNLOAD statement to export columns a and c from the local_t table to the OSS foreign table in the oss://adbpg-regress/local_t/ path.
        UNLOAD ('select a, c from local_t') TO 'oss://adbpg-regress/local_t/'
        ACCESS_KEY_ID 'id'
        SECRET_ACCESS_KEY 'key'
        FORMAT AS CSV
        ENDPOINT 'oss-endpoint'
        FDW 'oss_fdw';
        ```

    3.  Check whether the CSV file is written to the specified path.

        ```
        $ ossutil ls oss://adbpg-regress/local_t/
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


## COPY

-   **Syntax**

    ```
    COPY table_name [ ( column_name [, ...] ) ]
    FROM data_source
    ACCESS_KEY_ID '<access-key-id>'
    SECRET_ACCESS_KEY '<secret-access-key>'
    [ [ FORMAT ] [ AS ] data_format ] 
    [ [ option value]  ...  ]
    ```

    -   table\_name: the name of the local table in which the imported data is stored. The local table must exist in the AnalyticDB for PostgreSQL instance.
    -   data\_source: the URL of the OSS bucket from which data is obtained. Example: oss://bucket\_name/path\_prefix.
    -   <access-key-id\>: the AccessKey ID of the OSS account.
    -   <secret-access-key\>: the AccessKey secret of the OSS account.
    -   \[ FORMAT \] \[ AS \] data\_format: the file format in which the imported data is stored. This parameter is optional. The default format is CSV. You can set data\_format to CSV, JSON, JSONLINE, ORC, PARQUET, or TEXT.
    -   \[ option \[ value \] \[, ... \] \]: a list of one or more options. Specify each option in the key-value pair format. The following table describes the available parameters.

        |Option|Type|Unit|Required|Default value|Description|
        |------|----|----|--------|-------------|-----------|
        |endpoint|String|N/A|Yes|None|The endpoint for the specified OSS region.|
        |fdw|String|N/A|Yes|None|The name of the oss\_fdw plug-in. The oss\_fdw plug-in is required when you create a temporary OSS server for the COPY statement.|
        |Other options that are used to create an OSS foreign table, including format, filetype, delimiter, and escape.|N/A|N/A|No|None|The options that are used to create a temporary OSS foreign table. For more information, see [Use OSS foreign tables to access OSS data](/intl.en-US/Beginner Developer Guide/Use OSS foreign tables to access OSS data.md).|


-   **Examples**

    1.  Create a local table.

        ```
        create table local_t2 (a int, b float8, c text);
        ```

    2.  Use the COPY statement to import the data from the oss://adbpg-regress/local\_t/ path to columns a and c of the local table and save the data in the CSV format. By default, column b is NULL because column b is not specified in the COPY statement.

        ```
        postgres=# COPY local_t2 (a, c)
        FROM 'oss://adbpg-regress/local_t/'
        ACCESS_KEY_ID 'id'
        SECRET_ACCESS_KEY 'key'
        FORMAT AS CSV
        ENDPOINT 'oss_endpoint'
        FDW 'oss_fdw';
        COPY FOREIGN TABLE
        
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
        ENDPOINT 'oss-cn-hangzhou-zmf.aliyuncs.com'
        FDW 'oss_fdw';
        
        -- Save the data in the PARQUET format.
        COPY tp
        FROM  'oss://adbpg-regress/test_parquet/'
        ACCESS_KEY_ID 'id'
        SECRET_ACCESS_KEY 'key'
        FORMAT AS PARQUET
        ENDPOINT 'oss-cn-hangzhou-zmf.aliyuncs.com'
        FDW 'oss_fdw';
        ```


**Note:** When you use the COPY or UNLOAD statement to export data and store the data in a CSV file, you must specify some options in a particular way. Otherwise, syntax errors may occur because some options may be the same as keywords.

-   You must specify the following options in a particular way: `delimiter`, `quote`, `null`, `header`, `escape`, and `encoding`.
-   Method: Enclose the parameters in quotation marks \("\) and write the options in lowercase letters. Example:

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


