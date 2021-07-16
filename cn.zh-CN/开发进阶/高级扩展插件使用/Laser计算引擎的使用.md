# Laser计算引擎的使用

Laser计算引擎是阿里巴巴自研的AnalyticDB PostgreSQL计算引擎，对客户透明，可以提升复杂计算的性能，经实测，1 GB、100 GB、1 TB、10 TB数据规模下，TPCH 22条SQL的测试性能是原生引擎的2倍以上。

## 功能限制

-   建议使用ORCA优化器。
-   仅如下支持 AnalyticDB PostgreSQL 6.0版及以上版本。

## 开启或关闭Laser

Laser计算引擎可以通过GUC参数**laser.enable**开启或关闭，on表示开启；off表示关闭。该参数可以设置Session级别、库级别和集群级别，Session结束后恢复到默认状态，库级别设置后立即生效，集群级别设置后重启生效，以下内容将为您介绍如何查看或修改Laser计算引擎状态：

-   查看Laser计算引擎的状态，示例如下：

    ```
    show laser.enable;
    ```

    返回示例如下：

    ```
     laser.enable
    --------------
     on
    (1 row)
    ```

-   开启Session级别Laser，示例如下：

    ```
    set laser.enable = on;
    ```

-   关闭Session级别Laser，示例如下：

    ```
    set laser.enable = off;
    ```

-   开启库级别Laser，示例如下：

    ```
    alter database ${DBNAME} set laser.enable = on;
    ```

-   关闭库级别Laser，示例如下：

    ```
    onalter database ${DBNAME} set laser.enable = off;
    ```


**说明：**

-   目前不支持修改集群级别的Laser，建议您使用库级别或Session级别的设置。如果需要修改集群级别Laser，请[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex)联系技术支持进行修改。
-   内核版本为6.3.4.0以前版本，Laser计算引擎状态默认为关闭；6.3.4.0及以后版本，Laser计算引擎状态默认为开启。如何查看和升级内核小版本，请参见[查看内核小版本]()和[版本升级](/cn.zh-CN/实例管理/版本管理/版本升级.md)。

## 支持的数据类型和操作

Laser支持的数据类型如下：

-   INT2、INT4、INT8
-   FLOAT4、FLOAT8、NUMERIC
-   DATE、TIME、TIMETZ、TIMESTAMP、TIMESTAMPTZ
-   VARCHAR、TEXT、BPCHAR

Laser支持的操作符如下：

-   =、<、<=、\>、\>=、<\> or !=、BETWEEN、 IS NOT NULL、 IS NULL、LIKE
-   逻辑运算符：AND、OR、NOT

