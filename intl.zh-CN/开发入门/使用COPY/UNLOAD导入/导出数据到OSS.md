# 使用COPY/UNLOAD导入/导出数据到OSS

AnalyticDB PostgreSQL版支持COPY/UNLOAD命令，COPY表示从外表导入数据到本地表，UNLOAD表示从本地表导出数据到外表。

COPY和UNLOAD都是基于OSS Foreign Table来完成数据导入导出的，OSS Foreign Table的详细内容请参见[使用 OSS Foreign Table 访问 OSS 数据](/intl.zh-CN/开发入门/使用 OSS Foreign Table 访问 OSS 数据.md) 。

## 注意事项

在使用COPY/UNLOAD导出为CSV文件时，可能存在某些option会因关键字冲突而发生语法错误，需要将option选项用双引号引用，并写成小写形式。需要进行特殊处理的options：`delimiter`、`quote`、`null`、`header`、`escape`、`encoding`。示例如下：

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

-   **语法**

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

    -   table\_name：需要导入数据的本地表名，必须是已经存在的本地表。
    -   column-list：可选，表示需要写入的目标列列表。若不使用，则默认写入所有列。
    -   data\_source：表示OSS路径的字符串，如oss://bucket\_name/path\_prefix。
    -   <access-key-id\>：表示OSS账号ID。
    -   <secret-access-key\>：表示OSS账号secret key。
    -   \[ FORMAT \] \[ AS \] data\_format： 可选。若不使用，则默认FORMAT AS CSV。data\_format可以是BINARY，CSV，JSON，JSONLINE，ORC，PARQUET，TEXT（FORMAT与AS可缺省，即FORMAT AS CSV与FORMAT CSV和CSV等价）。
    -   \[ MANIFEST \] ：表示data\_source是manifest清单文件。manifest清单文件必须是JSON格式，由以下元素构成：
        -   entries：数组，表示清单内具体的OSS文件列表，可以位于不同的bucket或拥有不同的路径前缀，但需要能够使用相同的ACCESS\_KEY\_ID或SECRET\_ACCESS\_KEY访问。示例如下：

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

        -   url：一个OSS文件的完整路径。
        -   mandatory：OSS文件不存在时是否报错。
    -   \[ option \[ value \] \[ ... \] \]：选项列表，以key value的形式输入。选项说明见下表：

        |选项|类型|是否必选|备注|
        |--|--|----|--|
        |endpoint|字符串|是|指定OSS的endpoint。|
        |fdw|字符串|是|指定oss fdw插件名字。COPY命令在创建临时Server时需要用到。|
        |其他所有创建外表时用到的option，如format，filetype，delimiter，escape等|不涉及|不涉及|创建临时外表时用到的option，详细内容请参见[使用 OSS Foreign Table 访问 OSS 数据](/intl.zh-CN/开发入门/使用 OSS Foreign Table 访问 OSS 数据.md)。|

-   **示例1**

    1.  创建本地表。

        ```
         postgres=# create table local_t2 (a int, b float8, c text);
        ```

    2.  使用COPY命令导入数据，只写入a和c两列，b列全部写入NULL。

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

    3.  可以看到从外表导入的a和c列，与源数据表local\_t的a和c列数据相同。

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
        ENDPOINT '<endpoint>'
        FDW 'oss_fdw';
        
        -- 导入parquet格式数据
        COPY tp
        FROM  'oss://adbpg-regress/test_parquet/'
        ACCESS_KEY_ID 'id'
        SECRET_ACCESS_KEY 'key'
        FORMAT AS PARQUET
        ENDPOINT '<endpoint>'
        FDW 'oss_fdw';
        ```

-   **示例2**

    1.  创建本地表。

        ```
         create table local_manifest (a int, c text);
        ```

    2.  创建manifest文件，其中OSS文件列表可位于不同的bucket。

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

    3.  使用COPY命令从manifest清单中导入本地表。

        ```
        -- 从 manifest 清单中导入 OSS 文件
        COPY local_manifest
        FROM 'oss://adbpg-regress-2/unload_manifest/t_manifest'
        ACCESS_KEY_ID '<id>'
        SECRET_ACCESS_KEY '<key>'
        FORMAT AS CSV
        MANIFEST                       -- 表示从 manifest 文件中导入
        ENDPOINT '<endpoint>'
        FDW 'oss_fdw';
        ```

-   **示例3**

    COPY导入OSS数据时，可能会存在异常的数据行（无法正常COPY导入）。当遇到这种情况时，可以通过额外的option选项设置实现容错。

    -   log\_errors：表示是否记录错误行信息。
    -   segment\_reject\_limit：`segment_reject_limit '10'`表示最多容忍10行，大于等于10行时报错退出；`segment_reject_limit '10%'` 表示当前的错误总行数/当前总共已处理的行 \>= 10% 时，报错退出。
    1.  创建本地表。

        ```
         create table sales(id integer, value float8, x text) distributed by (id);
        ```

    2.  使用COPY导入OSS数据文件，文件中有3行数据存在编码问题。

        ```
        COPY sales
        FROM 'oss://adbpg-const/error_sales/'
        ACCESS_KEY_ID '<id>'
        SECRET_ACCESS_KEY '<key>'
        FORMAT AS csv
        log_errors 'true'                -- 表示需要记录错误行的信息
        segment_reject_limit '10'        -- 表示最多容忍10行错误记录，超过10行时报错退出
        endpoint '<endpoint>'
        FDW 'oss_fdw';
        NOTICE:  found 3 data formatting errors (3 or more input rows), rejected related input data
        COPY FOREIGN TABLE
        ```

    3.  查看具体的错误行信息。

        ```
        select * from gp_read_error_log('<COPY的目标表名>');
        ```

        例如，查看sales表的错误行信息：

        ```
        select * from gp_read_error_log('sales');
                    cmdtime            |                    relname                     |        filename         | linenum | bytenum |                          errmsg                           | rawdata | rawbytes
        -------------------------------+------------------------------------------------+-------------------------+---------+---------+-----------------------------------------------------------+---------+----------
         2021-02-08 14:24:04.225238+08 | adbpgforeigntabletmp_20210208142403_1936866966 | error_sales/sales.2.csv |       2 |         | invalid byte sequence for encoding "UTF8": 0xed 0xab 0xad |         | \x
         2021-02-08 14:24:04.225238+08 | adbpgforeigntabletmp_20210208142403_1936866966 | error_sales/sales.2.csv |       3 |         | invalid byte sequence for encoding "UTF8": 0xed 0xab 0xad |         | \x
         2021-02-08 14:24:04.225269+08 | adbpgforeigntabletmp_20210208142403_1936866966 | error_sales/sales.3.csv |       2 |         | invalid byte sequence for encoding "UTF8": 0xed 0xab 0xad |         | \x
        (3 rows)
        ```

        **说明：** 追加保存的错误行日志，需要占用一定的存储空间。删除错误行日志语法为：`select gp_truncate_error_log('<表名>')` 。


## UNLOAD

-   **语法**

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

    参数说明：

    -   select-statement：一个select查询语句，查询的结果数据会被写到OSS。
    -   destination\_url：OSS路径的字符串，如oss://bucket-name/path-prefix。
    -   <access-key-id\>： OSS账号ID。
    -   <secret-access-key\>： OSS账号KEY。
    -   \[ FORMAT \] \[ AS \] data\_format： 可选。若不使用，则默认FORMAT AS CSV。data\_format可以是BINARY，CSV，JSON，JSONLINE，ORC，PARQUET，TEXT（FORMAT与AS可缺省，即FORMAT AS CSV与FORMAT CSV和CSV等价）。
    -   MANIFEST：表示导出时需要生成导出清单文件。<manifest\_url\>可以指定与数据文件不同bucket的OSS完整路径，且路径需要以manifest为后缀。不指定<manifest\_url\>时，表示清单文件与导出的数据文件有相同的路径前缀。

        **说明：** 如果<manifest\_url\>指定的文件已经存在，则需要在\[ option \[ value \] \[ ... \] \]选项列表中指定allowoverwrite选项为true， 表示覆盖原manifest文件。

    -   PARALLEL ：表示是否多segment并行导出。默认多节点并行导出，每个节点生成独立的导出文件。值设置为OFF/FALSE时，表示关闭并行，导出数据大小不超过8GB时，仅导出一个文件。
    -   \[ option \[ value \] \[ ... \] \] ：选项列表，以key value的形式输入。选项说明见下表：

        |选项|类型|是否必选|备注|
        |--|--|----|--|
        |endpoint|字符串|是|指定OSS的endpoint。|
        |fdw|字符串|是|指定oss fdw插件名字。UNLOAD命令在创建临时Server时需要用到。|
        |allowoverwrite|布尔字符串|否|指定是否覆写已存在的manifest文件。**说明：** 仅覆写manifest文件，不会覆写数据文件。 |
        |其他所有创建外表时用到的option，如format，filetype，delimiter，escape等|不涉及|不涉及|创建临时外表时用到的option，详细内容请参见[使用 OSS Foreign Table 访问 OSS 数据](/intl.zh-CN/开发入门/使用 OSS Foreign Table 访问 OSS 数据.md)。|


-   **示例1**

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

    2.  使用UNLOAD，导出指定列数据到OSS。

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

    3.  查看OSS对应路径下已经写入的csv文件。

        ```
        $ ossutil --config hangzhou-zmf.config ls oss://adbpg-regress/local_t/
        LastModifiedTime                   Size(B)  StorageClass   ETAG                                  ObjectName
        2020-09-07 16:48:01 +0800 CST        12023      Standard   9F38B5407142C044C1F3555F00000000      oss://adbpg-regress/local_t/adbpgforeigntabletmp_20200907164801_1354519958_20200907164801_652261618_seg0_0.csv
        2020-09-07 16:48:01 +0800 CST        12469      Standard   807BA680A0DED49BC1F3555F00000000      oss://adbpg-regress/local_t/adbpgforeigntabletmp_20200907164801_1354519958_20200907164801_652261618_seg1_0.csv
        2020-09-07 16:48:01 +0800 CST        12401      Standard   3524F68F628CEB64C1F3555F00000000      oss://adbpg-regress/local_t/adbpgforeigntabletmp_20200907164801_1354519958_20200907164801_652261618_seg2_0.csv
        Object Number is: 3
        
        0.153414(s) elapsed
        ```

        查看文件数据，只写出了a和c两列的数据。

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

-   **示例2**

    1.  使用UNLOAD导出时自动生成manifest文件（manifest文件与数据文件有相同的路径前缀）。

        ```
        -- UNLOAD导出，同时生成manifest文件清单
        UNLOAD ('select * from local_t') TO 'oss://adbpg-regress/local_t/'
        ACCESS_KEY_ID '<id>'
        SECRET_ACCESS_KEY '<key>'
        FORMAT AS CSV
        MANIFEST    -- 表示UNLOAD导出时生成导出清单，清单文件与数据文件有相同的路径前缀
        ENDPOINT '<endpoint>'
        FDW 'oss_fdw';
        NOTICE:  OSS output prefix: "local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c".
        UNLOAD
        
        -- 查看导出的文件列表，除了数据文件外，还有一个清单文件
        ossutil ls -s oss://adbpg-regress/local_t/
        oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_162488956_seg1_0.csv
        oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_163756258_seg0_0.csv
        oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_1741120517_seg2_0.csv
        oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_manifest
        Object Number is: 4
        
        0.136180(s) elapsed
        
        -- 查看清单文件内容
        ossutil cat oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_manifest
        {
           "entries": [
              {"url": "oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_162488956_seg1_0.csv"},
              {"url": "oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_163756258_seg0_0.csv"},
              {"url": "oss://adbpg-regress/local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c_1741120517_seg2_0.csv"}
           ]
        }
        ```

    2.  使用UNLOAD导出时，生成指定manifest文件（manifest路径可与数据文件路径不同）。

        **说明：** ALLOWOVERWRITE为true时，会覆写已存在的manifest文件，但不会覆写数据文件，数据文件由客户按需自行删除。

        ```
        -- UNLOAD导出，同时生成指定的manifest文件清单，且manifest位于不同的bucket
        UNLOAD ('select * from local_t') TO 'oss://adbpg-regress/local_t/'
        ACCESS_KEY_ID '<id>'
        SECRET_ACCESS_KEY '<key>'
        FORMAT AS CSV
        MANIFEST 'oss://adbpg-regress-2/unload_manifest/t_manifest' -- 表示UNLOAD导出时生成指定路径的导出清单
        ALLOWOVERWRITE 'true' -- 覆写已存在的manifest文件
        ENDPOINT '<endpoint>'
        FDW 'oss_fdw';
        NOTICE:  OSS output prefix: "local_t/_20210114100329_3e9b07726306d88b3193dc95c10a5c5c".
        UNLOAD
        
        -- 查看导出的文件列表，导出路径下只有数据文件
        ossutil ls -s oss://adbpg-regress/local_t/
        oss://adbpg-regress/local_t/_20210114100956_4d3395a9501f6e22da724a2b6df1b6d3_1736161168_seg0_0.csv
        oss://adbpg-regress/local_t/_20210114100956_4d3395a9501f6e22da724a2b6df1b6d3_1925769064_seg2_0.csv
        oss://adbpg-regress/local_t/_20210114100956_4d3395a9501f6e22da724a2b6df1b6d3_644328153_seg1_0.csv
        Object Number is: 3
        
        0.118540(s) elapsed
        
        -- 查看清单文件内容，清单文件位于另一个bucket下
        ossutil cat oss://adbpg-regress-2/unload_manifest/t_manifest
        {
           "entries": [
              {"url": "oss://adbpg-regress/local_t/_20210114100956_4d3395a9501f6e22da724a2b6df1b6d3_1736161168_seg0_0.csv"},
              {"url": "oss://adbpg-regress/local_t/_20210114100956_4d3395a9501f6e22da724a2b6df1b6d3_1925769064_seg2_0.csv"},
              {"url": "oss://adbpg-regress/local_t/_20210114100956_4d3395a9501f6e22da724a2b6df1b6d3_644328153_seg1_0.csv"}
           ]
        }
        ```


