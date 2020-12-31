使用 ODPS Foreign Table 访问 MaxCompute 数据 
===========================================================

ODPS FDW 是 AnalyticDB PostgreSQL 版（简称 ADB PG）基于 PostgreSQL Foreign Data Wrapper（简称 PG FDW）框架开发的用于访问大数据计算服务MaxCompute（原名 ODPS）的外部数据访问方案。整理概览如下图：

![ODPS概览](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6923588951/p141763.png)

ODPS FDW 模块的加入，填补了当前 ADB PG 与 MaxCompute 的数据同步链路的缺失。用户通过 ODPS FDW 可以创建三种类型的 ODPS 外表。

1. ODPS 非分区外表，映射 MaxCompute 非分区表。

   

2. ODPS 末级分区外表，映射 MaxCompute 末级分区表。

   

3. ODPS 分区外表，映射 MaxCompute 分区表。

   




下面详细说明。

开始使用 ODPS FDW 
----------------------------------

1. 新建实例默认创建 ODPS FDW extension， **可** **跳过步骤2** 。

   

2. 既存实例，可以使用 **初始账号** ，连接指定 database手动执行如下命令，创建 ODPS FDW extension。

   




    -- 创建ODPS FDW
    CREATE EXTENSION odps_fdw ;
    -- 将使用权赋权给所有用户
    GRANT USAGE ON FOREIGN DATA WRAPPER odps_fdw TO PUBLIC;



开始使用 ODPS Foreign Table 
--------------------------------------------

使用 ODPS 外表需要以下三要素，缺一不可，包括：

* ODPS Server - 定义 MaxCompute 的访问端点。

  

* ODPS User Mapping - 定义 MaxCompute 的访问账户。

  

* ODPS Foreign Table - 定义 MaxCompute 的访问对象。

  




1. 创建 ODPS Server 
--------------------------------------

1.1 语法示例

    CREATE SERVER odps_serv                        -- ODPS Server 名称
      FOREIGN DATA WRAPPER odps_fdw
      OPTIONS (
        tunnel_endpoint '<odps tunnel endpoint>'   -- ODPS Tunnel Endpoint
      );



1.2 参数选项

在 ADB PG 中定义 ODPS Server 只需要指定 tunnel_endpoint 或 odps_endpoint 即可。其中：


|       选项        |     是否必选      |             备注              |
|-----------------|---------------|-----------------------------|
| tunnel_endpoint | 可选，优先推荐设置此选项。 | 指 ODPS tunnel 服务的 Endpoint。 |
| odps_endpoint   | 可选            | 指 MaxCompute 服务的 Endpoint。  |


**说明**

* 创建时，可以设置其中任意一个，也可以都设置，优先选择使用 Tunnel Endpoint，当缺少 Tunnel Endpoint时，则通过 ODPS Endpoint 路由到对应的 Tunnel Endpoint。

  

* 推荐配置阿里云经典网络或VPC网络的 Tunnel Endpoint。

  

* 通过配置外网 Tunnel Endpoint 地址访问 MaxCompute 数据，价格为0.8元/GB。

  




关于 ODPS Endpoint，请参见[配置Endpoint](/cn.zh-CN/准备工作/配置Endpoint.md)。

2. 创建 ODPS User Mapping 
--------------------------------------------

2.1 语法示例

    CREATE USER MAPPING FOR { username | USER | CURRENT_USER | PUBLIC }
      SERVER odps_serv                                  -- ODPS Server 名称
      OPTIONS (
        id '<odps access id>',                                -- ODPS Account ID
        key '<odps access key>'                               -- ODPS Account Key
      );


**说明**



* **username** The name of an existing user that is mapped to foreign server.

  

* **CURRENT_USER** and **USER** match the name of the current user.

  

* **PUBLIC** all roles, including those that might be created later.

  




2.2 参数选项

在 ADB PG 中定义访问 ODPS Server 的账户，需要指定账户类型 TYPE，ID 和 KEY。


| 选项  | 是否必选 |   备注    |
|-----|------|---------|
| id  | 必选   | 指定账户ID  |
| key | 必选   | 指定账户KEY |





3. 创建 ODPS Foreign Table 
---------------------------------------------

3.1 语法示例

    CREATE FOREIGN TABLE IF NOT EXISTS table_name ( -- ODPS 外表名称
        column_name data_type [, ... ]
    )
      SERVER odps_serv                              -- ODPS Server 名称
      OPTIONS (
        project '<odps project>',                   -- ODPS 项目空间
        table '<odps table>'                        -- ODPS 表名称
    );



3.2 参数选项

定义了 ODPS Server 和 ODPS User Mapping 后，就可以创建 ODPS Foreign Table。参数选项包括：


|    选项     | 是否必选 |                                                                                                   备注                                                                                                   |
|-----------|------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| project   | 必选   | 即项目/项目空间。项目空间（Project）是 MaxCompute 的基本组织单元，它类似于传统数据库的 Database 或 Schema 的概念，是进行多用户隔离和访问控制的主要边界。详情请参见[项目](/cn.zh-CN/产品简介/基本概念/项目.md)。                                                   |
| table     | 必选   | 即 ODPS 表。表是 MaxCompute 的数据存储单元，详情请参见[表](/cn.zh-CN/产品简介/基本概念/表.md)。                                                                                                                     |
| partition | 可选   | 即用于定义 MaxCompute 的末级分区表。分区 partition 是指一张表下，根据分区字段（一个或多个字段的组合）对数据存储进行划分。也就是说，如果表没有分区，数据是直接放在表所在的目录下。如果表有分区，每个分区对应表下的一个目录，数据是分别存储在不同的分区目录下。关于分区的更多介绍请参见[分区](/cn.zh-CN/产品简介/基本概念/分区.md)。 |





3.3 外表分类

根据 MaxCompute 的表分类，ODPS FDW 支持定义以下三种类型的 ODPS 外表。

* 非分区外表

  非分区 ODPS 外表映射的是 MaxCompute 的非分区表。用户创建外表时，只需要指定有效的 **project** 和 **table** 属性即可，无需指定 **partition** 属性或者指定 **partition** 属性为"空"。例如：

      CREATE FOREIGN TABLE odps_lineitem (              -- ODPS 外表名称
          l_orderkey      bigint,
          l_partkey       bigint,
          l_suppkey       bigint,
          l_linenumber    bigint,
          l_quantity      double precision,
          l_extendedprice double precision,
          l_discount      double precision,
          l_tax           double precision,
          l_returnflag    char(1),
          l_linestatus    char(1),
          l_shipdate      date,
          l_commitdate    date,
          l_receiptdate   date,
          l_shipinstruct  char(25),
          l_shipmode      char(10),
          l_comment       varchar(44)
      ) SERVER odps_serv                              -- ODPS Server 名称
      OPTIONS (
        project 'odps_fdw',                           -- ODPS 项目空间
        table 'lineitem_big'                          -- ODPS 表名称
      );

  

* 末级分区外表

  相对于非分区外表，末级分区外表，映射的是 MaxCompute 的末级分区表，需要设置正确的 **partition** 属性，多级分区时，末级分区外表只支持末级分区表，即 **partition** 属性需要包含多级分区完整路径。

  举例说明：在 MaxCompute 上创建一个二级分区表，如下：

      --创建一个二级分区表，以日期为一级分区，地域为二级分区
      CREATE TABLE src (key string, value bigint) PARTITIONED BY (pt string,region string);

  

  ![ODPS分区 ](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6923588951/p141722.png)

  当我们需要在 ADB PG 上定义一个末级分区外表，映射 MaxCompute 上一级分区为（20170601）二级分区为（hangzhou）的末级分区表时，需要设置 partition 为"pt=20170601,region=hangzhou"。

      CREATE FOREIGN TABLE odps_src_20170601_hangzhou (   -- ODPS 外表
        key string,
        value bigint
      ) SERVER odps_serv                                  -- ODPS Server 名称
      OPTIONS (
        project 'odps_fdw',                               -- ODPS 项目空间 
        table 'src',                                      -- ODPS 表名称
        partition 'pt=20170601,region=hangzhou'           -- 末级分区完整路径
      );

  
  **说明**

  
  1. 分区的 partition specification 需按照 key=value 的方式设置；多级分区时，以逗号分隔，且不能包含额外的空格。

     
  
  2. 不支持映射非末级分区的表，如，不支持仅设置一级分区路径：`partition 'pt=20170601'`。

     
  
  3. 一定要设置完整的多级分区路径，如，不支持仅设置末级分区路径：`partition '`

     `region=hangzhou'`​。
     
  

  
  

* 分区外表

  ODPS 分区外表，映射的是 MaxCompute 的分区表。同样，以上述 MaxCompute 二级分区表 src 为例。我们可以按照如下方法创建对应的分区外表。更多[表分区定义](/cn.zh-CN/数据管理/表分区定义.md)。

      CREATE FOREIGN TABLE odps_src(                    -- ODPS 外表名称
        key text,
        value bigint,
        pt text,                                        -- ODPS 一级分区键
        region text                                     -- ODPS 二级分区键
      ) SERVER odps_serv
      OPTIONS (
        project 'odps_fdw',                             -- ODPS 项目空间
        table 'src'                                     -- ODPS 表名称
      )
      PARTITION BY LIST (pt)                            -- 一级分区以"pt"字段为分区键
      SUBPARTITION BY LIST (region)                     -- 二级分区以"region"字段为分区键
          SUBPARTITION TEMPLATE (                       -- 二级分区模板
             SUBPARTITION hangzhou VALUES ('hangzhou'),
             SUBPARTITION shanghai VALUES ('shanghai')
          )
      ( PARTITION "20170601" VALUES ('20170601'), 
        PARTITION "20170602" VALUES ('20170602'));

  
  **说明**

  与 MaxCompute 分区表定义不同，ADB PG 的分区外表定义时：
  1. 需要将分区键以字段方式定义在其他字段尾部，多级分区时，分区字段定义顺序、分区键层级、MaxCompute 分区表层级三者需保持一致。

     
  
  2. 分区表定义需要指定分区键值，请使用 **LIST** 方式分区。

     
  
  3. 分区外表定义时，不需要指定 **partition** 属性，这是因为 **partition** 属性标记的是末级分区外表，与分区外表在逻辑上冲突。

     
  
  4. 当出现 MaxCompute 上不存在的分区外表，查询外表时，会告警显示，用户可以参考本章的3.5节 **如何删除子分区外表** 删除相应子分区。

     ![不存在的分析外表](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6923588951/p141731.png)
     
  

  
  




3.4 如何添加子分区外表

以上述 odps_src 分区外表为例。

* 添加一级子分区，效果如下图。

  




    -- 添加一级子分区（自动创建二级子分区）
    alter table odps_src add partition "20170603" values(20170603);



![添加一级子分区](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6923588951/p141742.png)

* 添加二级子分区，效果如下图。

  




    -- 添加二级子分区
    alter table odps_src alter partition "20170603" add partition "nanjing" values('nanjing');



![](https://intranetproxy.alipay.com/skylark/lark/0/2020/png/155907/1594086347653-a67dd3f4-b18f-4293-9711-fb2daf3f5d29.png?x-oss-process=image%2Fresize%2Cw_1492)

3.5 如何删除子分区外表

以上述 odps_src 分区外表为例。

* 删除一级子分区，效果如下图。

  




    -- 删除一级子分区（级联删除二级子分区）
    alter table odps_src drop partition "20170602";



![](https://intranetproxy.alipay.com/skylark/lark/0/2020/png/155907/1594086369069-3bca1b71-bc26-4ae5-bee5-9a978a8fdff3.png?x-oss-process=image%2Fresize%2Cw_1492)

* 删除二级子分区

  




    -- 删除二级子分区
    alter table odps_src alter partition "20170601" drop partition "hangzhou";



![删除二级子分区](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6923588951/p141744.png)

ODPS 外表数据类型 
--------------------------------

目前 MaxCompute 数据类型与 ADB PG 数据类型的对应关系如下，建议按照此类型对照表来定义 ADB PG 外表的字段类型。
**说明**

目前暂不支持 MaxCompute 的STRUCT/MAP/ARRAY类型。


|   ODPS类型   |  ADB PG类型  |
|------------|------------|
| BOOLEAN    | bool       |
| TINYINT    | int2       |
| SMALLINT   | int2       |
| INTEGER    | int4       |
| BIGINT     | int8       |
| FLOAT      | float4     |
| DOUBLE     | float8     |
| DECIMAL    | numeric    |
| BINARY     | bytea      |
| VARCHAR(n) | varchar(n) |
| CHAR(n)    | char(n)    |
| STRING     | text       |
| DATE       | date       |
| DATETIME   | timestamp  |
| TIMESTAMP  | timestamp  |





ODPS 外表使用场景 
--------------------------------

ODPS 外表扫描，实现了 ADB PG 的 Foreign Scan 算子。因此，表查询的使用方法上，对 ODPS 外表的查询与对普通表的查询基本一致。我们以TPCH Query为例，举例说明常见的使用场景。

ODPS 外表查询分析 
--------------------------------

TPCH Query Q1 是典型的单表聚集过滤场景，我们定义 odps_lineitem 为 ODPS 外表，对其执行 Q1 查询。

    -- 定义 ODPS 外表 odps_lineitem
    CREATE FOREIGN TABLE odps_lineitem (
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
    ) server odps_serv
        options (
            project 'odps_fdw', table 'lineitem'
        );
    
    -- TPCH Q1
    select
        l_returnflag,
        l_linestatus,
        sum(l_quantity) as sum_qty,
        sum(l_extendedprice) as sum_base_price,
        sum(l_extendedprice * (1 - l_discount)) as sum_disc_price,
        sum(l_extendedprice * (1 - l_discount) * (1 + l_tax)) as sum_charge,
        avg(l_quantity) as avg_qty,
        avg(l_extendedprice) as avg_price,
        avg(l_discount) as avg_disc,
        count(*) as count_order
    from
        odps_lineitem
    where
        l_shipdate <= date '1998-12-01' - interval '88' day --(3)
    group by
        l_returnflag,
        l_linestatus
    order by
        l_returnflag,
        l_linestatus;



ODPS 数据导入本地表 
---------------------------------

导入数据时，请执行如下步骤：

1. 在 ADB PG 中，创建 ODPS 外表。

   

2. 执行如下操作，并行导入数据。

   




    -- INSERT 方式
    INSERT INTO <本地目标表> SELECT * FROM <ODPS 外表>;
    
    -- CREATE TABLE AS 方式
    CREATE TABLE <本地目标表> AS SELECT * FROM <ODPS 外表>;



* **示例1** - INSERT 方式将 odps_lineitem 数据导入到本地 AOCS 表。

  




    -- 创建本地 AOCS 表
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
    ) WITH (APPENDONLY=TRUE, ORIENTATION=COLUMN, COMPRESSTYPE=ZSTD, COMPRESSLEVEL=5)
    DISTRIBUTED BY (l_orderkey);
    
    -- 将 odps_lineitem 数据导入到 AOCS 本地表
    INSERT INTO aocs_lineitem SELECT * FROM odps_lineitem;



* **示例2** - CREATE TABLE AS 方式将 odps_lineitem 导入到本地 heap 表。

  




    create table heap_lineitem as select * from odps_lineitem distributed by (l_orderkey);



ODPS 外表与本地表关联 
----------------------------------

以TPCH Query Q19 为例，使用本地列存表 aocs_lineitem 与 ODPS 外表 odps_part 关联查询。

    -- TPCH Q19
    select
        sum(l_extendedprice* (1 - l_discount)) as revenue
    from
        aocs_lineitem,          -- 本地 AOCS 列存表
        odps_part               -- ODPS 外表
    where
        (
            p_partkey = l_partkey
            and p_brand = 'Brand#32'
            and p_container in ('SM CASE', 'SM BOX', 'SM PACK', 'SM PKG')
            and l_quantity >= 8 and l_quantity <= 8 + 10
            and p_size between 1 and 5
            and l_shipmode in ('AIR', 'AIR REG')
            and l_shipinstruct = 'DELIVER IN PERSON'
        )
        or
        (
            p_partkey = l_partkey
            and p_brand = 'Brand#41'
            and p_container in ('MED BAG', 'MED BOX', 'MED PKG', 'MED PACK')
            and l_quantity >= 15 and l_quantity <= 15 + 10
            and p_size between 1 and 10
            and l_shipmode in ('AIR', 'AIR REG')
            and l_shipinstruct = 'DELIVER IN PERSON'
        )
        or
        (
            p_partkey = l_partkey
            and p_brand = 'Brand#44'
            and p_container in ('LG CASE', 'LG BOX', 'LG PACK', 'LG PKG')
            and l_quantity >= 22 and l_quantity <= 22 + 10
            and p_size between 1 and 15
            and l_shipmode in ('AIR', 'AIR REG')
            and l_shipinstruct = 'DELIVER IN PERSON'
        );



ODPS 外表使用常见错误 
----------------------------------

[Tunnel常见错误](/cn.zh-CN/错误码附录/Tunnel常见错误.md)

ODPS 外表使用建议 
--------------------------------

ODPS 外表通过网络访问 MaxCompute，使用瓶颈除了机器自身资源外，还受限于 MaxCompute Tunnel 对外吞吐的网络带宽 。因此，建议用户

1. 建议纯外表使用的并发数不超过5个。

   

2. 多张ODPS外表关联使用时，建议将大表导入本地后再和小的外表关联，性能更佳。

   



