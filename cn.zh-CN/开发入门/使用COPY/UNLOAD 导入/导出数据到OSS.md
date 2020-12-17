# 使用COPY/UNLOAD 导入/导出数据到OSS

ADB PG 中可以使用COPY/UNLOAD命令，用于便捷地从外表导入数据到本地表\(COPY\)和从本地表导出数据到外表\(UNLOAD\)。COPY和UNLOAD都是基于ADB PG映射OSS外表（请参见[使用 OSS Foreign Table 访问 OSS 数据](/cn.zh-CN/开发入门/使用 OSS Foreign Table 访问 OSS 数据.md)）完成数据导出。

## UNLOAD

-   **语法**

    ```
    UNLOAD ('select-statement')
    TO destination_url
    ACCESS_KEY_ID '<access-key-id>'
    SECRET_ACCESS_KEY '<secret-access-key>'
    [ [ FORMAT ] [ AS ] data_format ] 
    [ [option value]  ...  ]
    ```

    -   select-statement：一个 select 查询语句，查询的结果数据会被写到OSS。
    -   destination\_url：表示OSS路径的字符串，如oss://bucket-name/path-prefix。
    -   <access-key-id\>： 表示OSS账号ID。
    -   <secret-access-key\>： 表示OSS账号KEY。
    -   \[ FORMAT \] \[ AS \] data\_format： 表示导出的数据格式。可选，默认格式为CSV。 data\_format 可以是CSV, ORC, TEXT。
    -   \[ option \[ value \] \[, ... \] \] ：选项列表, 以key value的形式输入。见下表：

        |选项|类型|单位|是否必选|默认值|备注|
        |--|--|--|----|---|--|
        |endpoint|字符串|无|是|无|指定oss的endpoint|
        |fdw|字符串|无|是|无|指定oss fdw插件名字。COPY命令在创建临时Server时需要用到。|
        |其他所有创建外表时用到的option，如format, filetype, delimiter,escape等| |无| | |剩下的option是创建临时外表时用到的option，请参见[使用 OSS Foreign Table 访问 OSS 数据](/cn.zh-CN/开发入门/使用 OSS Foreign Table 访问 OSS 数据.md)。|


-   **示例**

    1.  创建本地表并写入测试数据。

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

    2.  使用 UNLOAD，导出指定列数据到OSS。

        ```
        -- 使用UNLOAD将local_t的a,c两列导出到“oss://adbpg-regress/local_t/”路径
        UNLOAD ('select a, c from local_t') TO 'oss://adbpg-regress/local_t/'
        ACCESS_KEY_ID 'id'
        SECRET_ACCESS_KEY 'key'
        FORMAT AS CSV
        ENDPOINT 'oss-endpoint'
        FDW 'oss_fdw';
        ```

    3.  可以看到OSS对应路径下已经写入csv文件。

        ```
        $ ossutil ls oss://adbpg-regress/local_t/
        LastModifiedTime                   Size(B)  StorageClass   ETAG                                  ObjectName
        2020-09-07 16:48:01 +0800 CST        12023      Standard   9F38B5407142C044C1F3555F00000000      oss://adbpg-regress/local_t/adbpgforeigntabletmp_20200907164801_1354519958_20200907164801_652261618_seg0_0.csv
        2020-09-07 16:48:01 +0800 CST        12469      Standard   807BA680A0DED49BC1F3555F00000000      oss://adbpg-regress/local_t/adbpgforeigntabletmp_20200907164801_1354519958_20200907164801_652261618_seg1_0.csv
        2020-09-07 16:48:01 +0800 CST        12401      Standard   3524F68F628CEB64C1F3555F00000000      oss://adbpg-regress/local_t/adbpgforeigntabletmp_20200907164801_1354519958_20200907164801_652261618_seg2_0.csv
        Object Number is: 3
        
        0.153414(s) elapsed
        ```

        查看文件数据，只写出了a,c 两列的数据。

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

-   **语法**

    ```
    COPY table_name [ ( column_name [, ...] ) ]
    FROM data_source
    ACCESS_KEY_ID '<access-key-id>'
    SECRET_ACCESS_KEY '<secret-access-key>'
    [ [ FORMAT ] [ AS ] data_format ] 
    [ [ option value]  ...  ]
    ```

    -   table\_name：需要导入数据的本地表名，必须是已经存在的本地表。
    -   data\_source：表示OSS路径的字符串，如oss://bucket\_name/path\_prefix。
    -   <access-key-id\>：表示OSS账号ID。
    -   <secret-access-key\>：表示OSS账号KEY。
    -   \[ FORMAT \] \[ AS \] data\_format：表示按照data\_format格式解析读取OSS文件。可选，默认为CSV格式。data\_format 可以是CSV, JSON, JSONLINE, ORC, PARQUET, TEXT。
    -   \[ option \[ value \] \[, ... \] \] 选项列表, 以key value的形式输入。见下表：

        |选项|类型|单位|是否必选|默认值|备注|
        |--|--|--|----|---|--|
        |endpoint|字符串|无|是|无|指定oss的endpoint|
        |fdw|字符串|无|是|无|指定oss fdw插件名字。COPY命令在创建临时Server时需要用到。|
        |其他所有创建外表时用到的option，如format, filetype, delimiter,escape等|不涉及|无|否|无|剩下的option是创建临时外表时用到的option，请参见[使用 OSS Foreign Table 访问 OSS 数据](/cn.zh-CN/开发入门/使用 OSS Foreign Table 访问 OSS 数据.md)。|


-   **使用示例**

    1.  创建本地表。

        ```
        create table local_t2 (a int, b float8, c text);
        ```

    2.  使用COPY命令从oss://adbpg-regress/local\_t/导入a,c两列。由于未指定b列，则b列默认为NULL。

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

    3.  可以看到从外表导入的a和c列 与源数据表local\_t的a和c列数据相同

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

    4.  导入其他格式示例。

        ```
        -- 导入ORC格式数据
        COPY tt
        FROM 'oss://adbpg-regress/q_oss_orc_list/'
        ACCESS_KEY_ID 'id'
        SECRET_ACCESS_KEY 'key'
        FORMAT AS ORC
        ENDPOINT 'oss-cn-hangzhou-zmf.aliyuncs.com'
        FDW 'oss_fdw';
        
        -- 导入parquet格式数据
        COPY tp
        FROM  'oss://adbpg-regress/test_parquet/'
        ACCESS_KEY_ID 'id'
        SECRET_ACCESS_KEY 'key'
        FORMAT AS PARQUET
        ENDPOINT 'oss-cn-hangzhou-zmf.aliyuncs.com'
        FDW 'oss_fdw';
        ```


**说明：** 在使用UNLOAD/COPY 导出为CSV文件时，可能存在某些option会因关键字冲突而发生语法错误，需要将option选项进行特殊处理。

-   需要进行特殊处理的options：`delimiter`、`quote`、`null`、`header`、`escape`、`encoding`。
-   处理方式：将option选项用双引号引用，并写成小写形式。示例如下：

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


