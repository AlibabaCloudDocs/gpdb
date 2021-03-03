# 列存表 MetaScan 加速查询性能

**说明：**

-   该特性目前仅支持创建实例时，数据库内核版本为20200826版本之后的存储预留模式实例。
-   如果您的实例是存储弹性模式实例，请暂时忽略这个特性。

AnlayticDB for PostgreSQL 支持列存储格式，具有较高的数据压缩能力，以及查询性能，但是当针对有较高过滤率的查询条件时，依然要做整列数据读取，或者建 B-Tree 索引，但是索引也有其的问题：一是列存表的索引无压缩，数据膨胀比较严重；二是结果集大的时候，索引代价比顺序扫描高，索引失效等问题。为此 AnlayticDB for PostgreSQL 针对此问题开发了MetaScan功能，具有很好的过滤性能，并且占用的存储空间也基本可以忽略不计。

## 开启MetaScan功能

MetaScan分为2部分：meta信息收集和MetaScan，由2个系统参数控制开启和关闭：

-   RDS\_ENABLE\_CS\_ENHANCEMENT

    控制MetaScan的meta信息收集功能，on表示开启meta信息收集，off表示关闭meta信息收集。系统默认为on，在列存表数据发生变化时，自动收集meta信息。该系统参数是实例级别的参数，如需要修改，可以提工单让技术支持人员修改。

-   RDS\_ENABLE\_COLUMN\_META\_SCAN

    控制查询是否使用MetaScan，on表示使用MetaScan，off表示不使用MetaScan。系统默认为off。该系统参数为session级别，可以通过下面sql 查看、修改：

    ```
    --- 查看MetaScan是否开启
    Show RDS_ENABLE_COLUMN_META_SCAN;
    --- 关闭MetaScan
    Set RDS_ENABLE_COLUMN_META_SCAN = OFF;
    --- 开启MetaScan
    Set RDS_ENABLE_COLUMN_META_SCAN = ON;
    ```


**说明：** RDS\_ENABLE\_CS\_ENHANCEMENT关闭后，所有列存表都将停止收集meta，如果需要开启RDS\_ENABLE\_CS\_ENHANCEMENT的话，则在开启后，要想使用MetaScan功能，必须重新收集表的meta信息，可以使用如下SQL：ALTER TABLE table\_name SET WITH（REORGANIZE=TRUE）。

## 如何查看query是否使用了MetaScan

可以使用explain查看select是否使用了MetaScan，如下图所示：

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2679219951/p65633.png)

Explain 输出中的”Append-only Columnar Meta Scan”节点即是MetaScan节点，如果explain中有该节点，则表示查询时使用了MetaScan。

## MetaScan支持的类型和操作符

目前版本，MetaScan支持如下数据类型：

-   INT2/INT4/INT8
-   FLOAT4/FLOAT8
-   TIME/TIMETZ/TIMESTAMP/TIMESTAMPTZ
-   VARCHAR/TEXT/BPCHAR
-   CASH

支持如下操作符：

-   =、<、<=、\>、\>=
-   逻辑运算符：and

## 已有实例升级

已有实例要使用MetaScan特性，需要做如下步骤升级AnlayticDB for PostgreSQL 版本和表的meta信息：

-   升级AnlayticDB for PostgreSQL 版本

    从控制台上点击”小版本升级“：

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2679219951/p65635.png)

-   升级表的meta 信息

    升级完AnlayticDB for PostgreSQL版本后，需要升级表的meta信息到最新版。 如果RDS\_ENABLE\_CS\_ENHANCEMENT是关闭的，请提工单联系技术支持来升级meta信息，否则可以按如下步骤升级表的meta版本：

    1.  使用管理员账号创建升级函数

        ```
        CREATE OR REPLACE FUNCTION UPGRADE_AOCS_TABLE_META(tname TEXT) RETURNS BOOL AS $$
        DECLARE
            tcount INT := 0;
        BEGIN
            -- CHECK TABLE NAME
            EXECUTE 'SELECT COUNT(1) FROM PG_AOCSMETA WHERE RELID = ''' || tname || '''::REGCLASS' INTO tcount;
            IF tcount IS NOT NULL THEN
                IF tcount > 1 THEN
                    RAISE EXCEPTION 'found more than one table of name %', tname;
                ELSEIF tcount = 0 THEN
                    RAISE EXCEPTION 'not found target table in pg_aocsmeta, table name:%', tname;
                END IF;
            END IF;
        
            EXECUTE 'ALTER TABLE ' || tname || ' SET WITH(REORGANIZE=TRUE)';
            RETURN TRUE;
        END;
        $$  LANGUAGE PLPGSQL;
        ```

    2.  使用管理员账号或者表的owner升级需要的表，执行如下sql

        ```
        SELECT UPGRADE_AOCS_TABLE_META(table_name);
        ```

    3.  检查表的meta version, SQL如下

        ```
        SELECT version = 4 AS is_latest_version  FROM pg_appendonly WHERE relid = 'test'::REGCLASS
        ```


## 使用SortKey提升MetaScan性能

SortKey 是AnlayticDB for PostgreSQL的另一个特性，可以让表按指定的列排序。把MetaScan与SortKey结合使用，可以有效的提高MetaScan执行的性能。 列存表是通过block为单位来存储数据的，MetaScan是通过meta信息判断block是否满足条件，跳过不满足条件的block，从而减少IO，提升扫描性能的。如果过滤列的数值分布的非常散，例如每个block都有，这样的话，即使过滤率很好也需要扫描所有的block，扫描性能低下。如果在表上以过滤列创建SortKey，则可以把列上相同的值集中到连续的block内，这样MetaScan就可以快速过滤掉不需要的block，从而提升扫描性能。

SortKey的创建参考 [列存表使用排序键和粗糙集索引加速查询](/cn.zh-CN/开发入门/列存表使用排序键和粗糙集索引加速查询.md)

## MetaScan的限制

目前版本MetaScan与ORCA优化器不兼容，在开启ORCA优化器时，无法使用MetaScan。查看当前优化器的方式是：

```
SHOW OPTIMIZER;
```

on表示是ORCA优化器。优化器相关信息可以参考[两种优化器的选择](/cn.zh-CN/最佳实践/查询性能优化指导.md)

