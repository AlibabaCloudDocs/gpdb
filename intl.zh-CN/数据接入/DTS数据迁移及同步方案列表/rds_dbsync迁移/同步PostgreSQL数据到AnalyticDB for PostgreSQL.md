# rds\_dbsync迁移/同步PostgreSQL数据到AnalyticDB for PostgreSQL

开源工具 rds\_dbsync的pgsql2pgsql功能，支持把AnalyticDB for PostgreSQL/Greenplum Database/PostgreSQL/PPAS中的表迁AnalyticDB for PostgreSQL/Greenplum Database/PostgreSQL/PPAS。

## pgsql2pgsql支持的功能

pgsql2pgsql支持如下功能：

-   PostgreSQL/PPAS/Greenplum Database/AnalyticDB for PostgreSQL全量数据迁移到PostgreSQL/PPAS/Greenplum Database/AnalyticDB for PostgreSQL。

-   PostgreSQL/PPAS（版本大于9.4）全量+增量迁移到PostgreSQL/PPAS。


## 参数配置

修改配置文件postgresql.conf、配置源和目的库连接信息。

-   源库pgsql连接信息如下所示：

    **说明：** 源库pgsql的连接信息中，用户最好是对应DB的owner。

    ```
    [src.pgsql]
    connect_string = "host=192.168.1.1 dbname=test port=3432 user=test password=pgsql"
    ```

-   本地临时Database pgsql连接信息如下所示：

    ```
    [local.pgsql]
    connect_string = "host=192.168.1.2 dbname=test port=3432 user=test2 password=pgsql"
    ```

-   目的库pgsql连接信息如下所示：

    **说明：** 目的库pgsql的连接信息，用户需要对目标表有写权限。

    ```
    [desc.pgsql]
    connect_string = "host=192.168.1.3 dbname=test port=3432 user=test3 password=pgsql"
    ```


**说明：**

-   如果要做增量数据同步，连接源库需要有创建replication slot的权限。
-   由于PostgreSQL 9.4及以上版本支持逻辑流复制，所以支持作为数据源的增量迁移。打开下列内核参数才能让内核支持逻辑流复制功能。

    ```
    wal_level = logical
    max_wal_senders = 6
    max_replication_slots = 6
    ```


## pgsql2pgsql用法

**全库迁移**

进行全库迁移，请执行如下命令：

```
./pgsql2pgsql
```

迁移程序会默认把对应pgsql库中所有用户的表数据将迁移到pgsql。

**状态信息查询**

连接本地临时Database，可以查看到单次迁移过程中的状态信息。这些信息被放在表db\_sync\_status中，包括全量迁移的开始和结束时间、增量迁移的开始时间和增量同步的数据情况。

## 下载与说明

-   下载rds\_dbsync二进制安装包，请单击[这里](https://github.com/aliyun/rds_dbsync/releases)。
-   查看rds\_dbsync源码编译说明，请单击[这里](https://github.com/aliyun/rds_dbsync/blob/master/doc/design.md)。

