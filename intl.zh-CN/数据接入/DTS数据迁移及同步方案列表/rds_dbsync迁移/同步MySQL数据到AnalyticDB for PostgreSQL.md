# rds\_dbsync迁移/同步MySQL数据到AnalyticDB for PostgreSQL

rds\_dbsync为开源的数据同步迁移工具 ，其mysql2pgsql功能支持不落地的把MySQL中的表迁移到AnalyticDB PostgreSQL/Greenplum Database/PostgreSQL/PPAS。此工具的原理是，同时连接源端MySQL数据库和目的端数据库，从MySQL库中通过查询得到要导出的数据，然后通过 COPY命令导入到目的端。此工具支持多线程导入（每个工作线程负责导入一部分数据库表）。

## 参数配置

修改配置文件my.cfg、配置源和目的库连接信息。

-   源库MySQL的连接信息如下：

    **说明：** 源库MySQL的连接信息中，用户需要有对所有用户表的读权限。

    ```
    [src.mysql]
    host = "192.168.1.1"
    port = "3306"
    user = "test"
    password = "test"
    db = "test"
    encodingdir = "share"
    encoding = "utf8"
    ```

-   目的库pgsql（包括Postgresql、PPAS和AnalyticDB PostgreSQL）的连接信息如下：

    **说明：** 目的库pgsql的连接信息，用户需要对目标表有写的权限。

    ```
    [desc.pgsql]
    connect_string = "host=192.168.1.2 dbname=test port=3432  user=test password=pgsql"
    ```


## mysql2pgsql用法

mysql2pgsql的用法如下所示：

```
./mysql2pgsql -l <tables_list_file> -d -n -j <number of threads> -s <schema of target table>
```

参数说明：

-   -l：可选参数，指定一个文本文件，文件中含有需要同步的表；如果不指定此参数，则同步配置文件中指定数据库下的所有表。`<tables_list_file>`为一个文件名，里面含有需要同步的表集合以及表上查询的条件，其内容格式示例如下：

    ```
    table1 : select * from table_big where column1 < '2016-08-05'
    table2 : 
    table3
    table4: select column1, column2 from tableX where column1 != 10
    table5: select * from table_big where column1 >= '2016-08-05'
    ```

-   -d：可选参数，表示只生成目的表的建表DDL语句，不实际进行数据同步。
-   -n：可选参数，需要与-d一起使用，指定在DDL语句中不包含表分区定义。
-   -j：可选参数，指定使用多少线程进行数据同步；如果不指定此参数，会使用5个线程并发。
-   -s：可选参数，指定目标表的schema，目前仅支持设定为public。

## 典型用法

**全库迁移**

全库迁移的操作步骤如下所示：

1.  通过如下命令，获取目的端对应表的DDL。

    ```
    ./mysql2pgsql -d
    ```

2.  根据这些DDL，再加入Distribution Key等信息，在目的端创建表。
3.  执行如下命令，同步所有表：

    ```
    ./mysql2pgsql
    ```

    此命令会把配置文件中所指定数据库中的所有MySQL 表数据迁移到目的端。过程中使用5个线程（即缺省线程数为5），读取和导入所有涉及的表数据。


**部分表迁移**

1.  编辑一个新文件tab\_list.txt，放入如下内容：

    ```
    t1
    t2 : select * from t2 where c1 > 138888
    ```

2.  执行如下命令，同步指定的t1和t2表（注意t2表只迁移符合c1 \> 138888条件的数据）：

    ```
    ./mysql2pgsql -l tab_list.txt
    ```


## 下载与说明

-   下载[mysql2pgsql](https://github.com/aliyun/rds_dbsync/releases)二进制安装包。

-   查看[mysql2pgsql](https://github.com/aliyun/rds_dbsync/blob/master/README.md)源码编译说明。


