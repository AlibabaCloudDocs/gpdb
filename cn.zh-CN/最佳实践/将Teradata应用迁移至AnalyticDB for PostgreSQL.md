# 将Teradata应用迁移至AnalyticDB for PostgreSQL {#concept_1580306 .concept}

分析型数据库PostgreSQL版对Teradata语法有着很好的兼容，仅需进行少量修改即可将Teradata应用迁移到分析型数据库PostgreSQL版。本文介绍如何将Teradata应用迁移到分析型数据库PostgreSQL版。

## 数据类型 {#section_mc7_a09_cnv .section}

分析型数据库PostgreSQL版和Teradata的核心数据类型是互相兼容的，仅部分数据类型需要进行修改。详情请参见下表：

|Teradata|分析型数据库PostgreSQL版|
|--------|-----------------|
|char|char|
|varchar|varchar|
|long varchar|varchar\(64000\)|
|varbyte\(size\)|bytea|
|byteint|无，可用bytea替代|
|smallint|smallint|
|integer|integer|
|decimal\(size,dec\)|decimal\(size,dec\)|
|numeric\(precision,dec\)|numeric\(precision,dec\)|
|float|float|
|real|real|
|double precision|double precision|
|date|date|
|time|time|
|timestamp|timestamp|

## 建表语句 {#section_rpg_qcx_jg5 .section}

我们通过一个建表语句的例子来比较分析型数据库PostgreSQL版和Teradata。

Teradata建表SQL语句如下：

``` {#codeblock_8wb_ibb_ebr}
CREATE MULTISET TABLE test_table,NO FALLBACK ,
     NO BEFORE JOURNAL,
     NO AFTER JOURNAL,
     CHECKSUM = DEFAULT,
     DEFAULT MERGEBLOCKRATIO
     (
      first_column DATE FORMAT 'YYYYMMDD' TITLE '第一列' NOT NULL,
      second_column INTEGER TITLE '第二列' NOT NULL ,
      third_column CHAR(6) CHARACTER SET LATIN CASESPECIFIC TITLE '第三列' NOT NULL ,
      fourth_column CHAR(20) CHARACTER SET LATIN CASESPECIFIC TITLE '第四列' NOT NULL,
      fifth_column CHAR(1) CHARACTER SET LATIN CASESPECIFIC TITLE '第五列' NOT NULL,
      sixth_column CHAR(24) CHARACTER SET LATIN CASESPECIFIC TITLE '第六列' NOT NULL,
      seventh_column VARCHAR(18) CHARACTER SET LATIN CASESPECIFIC TITLE '第七列' NOT NULL,
      eighth_column DECIMAL(18,0) TITLE '第八列' NOT NULL ,
      nineth_column DECIMAL(18,6) TITLE '第九列' NOT NULL )
PRIMARY INDEX ( first_column ,fourth_column )
PARTITION BY RANGE_N(first_column  BETWEEN DATE '1999-01-01' AND DATE '2050-12-31' EACH INTERVAL '1' DAY );

CREATE INDEX test_index (first_column, fourth_column) ON test_table;
```

分析型数据库PostgreSQL版的建表语句如下：

``` {#codeblock_aml_29t_g7k}
CREATE TABLE test_table
     (
      first_column DATE NOT NULL,
      second_column INTEGER NOT NULL ,
      third_column CHAR(6) NOT NULL ,
      fourth_column CHAR(20) NOT NULL,
      fifth_column CHAR(1) NOT NULL,
      sixth_column CHAR(24) NOT NULL,
      seventh_column VARCHAR(18) NOT NULL,
      eighth_column DECIMAL(18,0) NOT NULL ,
      nineth_column DECIMAL(18,6) NOT NULL )
DISTRIBUTED BY ( first_column ,fourth_column )
PARTITION BY RANGE(first_column) 
(START (DATE '1999-01-01')  INCLUSIVE
END (DATE '2050-12-31')  INCLUSIVE
EVERY (INTERVAL '1 DAY' ) );

create index test_index on test_table(first_column, fourth_column);
```

通过以上例子，我们可以清晰地分析分析型数据库PostgreSQL版和Teradata建表语句的异同：

-   核心数据类型互相兼容，数据类型无需修改。
-   均支持分布列，但语法不同，Teradata使用的是primary index，分析分析型数据库PostgreSQL版使用的是distributed by。
-   均支持PARTITION BY二级分区，语义相同但语法不同。
-   均支持对表创建索引，但语法不同。
-   分析型数据库PostgreSQL版不支持TITLE关键字，但是支持单独对列添加注释COMMENT，语法为`COMMENT ON COLUMN table_name.column_name IS 'XXX';`
-   分析型数据库PostgreSQL版不支持在定义char/varchar时声明编码类型，可以在连接数据库时，通过执行`SET client_encoding = latin1;`来声明编码类型。

## 导入导出数据格式 {#section_83b_1rv_o3k .section}

分析型数据库PostgreSQL版和Teradata均支持txt、csv格式的数据导入导出，与Teradata的区别在于数据文件的分隔符。

-   Teradata支持双分隔符。
-   分析型数据库PostgreSQL版支持单分隔符。

## SQL语句 {#section_r0s_omt_4l3 .section}

分析型数据库PostgreSQL版和Teradata的大部分SQL语法都是兼容的，仅有部分Teradata语法需要进行修改。需要修改的语法如下所示：

-   **cast** 

    Teradata支持如下的cast语法：

    ``` {#codeblock_rss_5vl_3jm}
    cast(XXX as int format '999999')
    cast(XXX as date format 'YYYYMMDD')
    ```

    而分析型数据库PostgreSQL版支持如下cast语法：

    ``` {#codeblock_flm_r0t_ja4}
    cast(XXX as int)
    cast(XXX as date)
    ```

    分析型数据库PostgreSQL版不支持在cast中声明format。

    -   对于`cast(XXX as int format '999999')`，需要编写函数来实现相同功能。
    -   对于`cast(XXX as date format 'YYYYMMDD')`，分析型数据库PostgreSQL版支持date的显示格式为`'YYYY-MM-DD'`，不影响正常使用。
-   **qualify** 

    Teradata的qualify关键字，用与根据用户的条件，进一步过滤前序排序计算函数得到的结果。

    例如，Teradata的qualify关键字如下所示：

    ``` {#codeblock_3qq_9lw_bpx}
    SELECT itemid, sumprice, RANK() OVER (ORDER BY sumprice DESC)
         FROM (SELECT a1.item_id, SUM(a1.sale)
               FROM sales AS a1 
               GROUP BY a1.itemID) AS t1 (itemid, sumprice) 
         QUALIFY RANK() OVER (ORDER BY sum_price DESC) <=100;
    ```

    而分析型数据库PostgreSQL版不支持qualify关键字，需要将带qualify的SQL语句，修改为嵌套子查询：

    ``` {#codeblock_y42_vkt_zck}
    SELECT itemid, sumprice, rank from 
    (SELECT itemid, sumprice, RANK() OVER (ORDER BY sumprice DESC) as rank
         FROM (SELECT a1.item_id, SUM(a1.sale)
               FROM sales AS a1 
               GROUP BY a1.itemID) AS t1 (itemid,sumprice)
    )  AS a
    where rank <=100;
    ```

-   **macro** 

    Teradata通过macro来执行一组SQL语句，如下所示：

    ``` {#codeblock_cz8_rkr_2z4}
    CREATE MACRO Get_Emp_Salary(EmployeeNo INTEGER) AS ( 
       SELECT 
       EmployeeNo, 
       NetPay 
       FROM  
       Salary 
       WHERE EmployeeNo = :EmployeeNo; 
    );
    ```

    分析型数据库PostgreSQL版不支持macro，但是可以使用function语句来完成Teradata的macro功能：

    ``` {#codeblock_nuy_l18_mui}
    CREATE OR REPLACE FUNCTION Get_Emp_Salary(
            EmployeeNo INTEGER,
            OUT EmployeeNo INTEGER,
            OUT NetPay FLOAT
    ) returns setof record AS 
    $$
    
            SELECT EmployeeNo,NetPay 
            FROM Salary
            WHERE EmployeeNo = $1
    
    $$
     LANGUAGE SQL;
    ```


