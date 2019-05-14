# 如何从其他数据源同步数据到 AnalyticDB for PostgreSQL？ {#concept_ujs_lgg_v2b .concept}

目前支持的 ETL 工具如下：

-   [阿里云的数据集成服务\(Data Integration\)](https://www.aliyun.com/product/cdp/) ：阿里云提供的 ETL 工具。在数据集成服务中，将 AnalyticDB for PostgreSQL 配置为一个 PostgreSQL 数据库，即可实现其他数据源（RDS、MaxCompute、TableStore 等）到 AnalyticDB for PostgreSQL 的数据同步。

    -   您可以直接从其他数据源读取数据，写入到 AnalyticDB for PostgreSQL 中。
    -   如果数据量较大，需要并发导入，则建议您先通过数据集成服务把数据从其他数据源导入到 OSS，再通过 OSS 外部表导入 AnalyticDB for PostgreSQL。
-   [Pentaho Kettle 数据集成软件](http://community.pentaho.com/projects/data-integration/)：开源的 ETL 工具。

    -   支持将数据先通过 Kettle 导入到本地磁盘，再通过 COPY 或 OSS 导入到 AnalyticDB for PostgreSQL。
    -   也支持将 OSS 存储挂载为本地虚拟磁盘，通过 Kettle 导入到此磁盘，最后通过 AnalyticDB for PostgreSQL 的 OSS 外部表导入到 AnalyticDB for PostgreSQL 中。
-   [Informatica软件](https://www.informatica.com/cn/)：商业化的 ETL 工具。

-   [彩虹桥](https://market.aliyun.com/products/56024006/cmjj011322.html)：阿里云云市场中的 ETL 工具，提供商业化服务。

-   [dbsync](https://github.com/aliyun/rds_dbsync/releases)：阿里云提供的开源数据库同步工具。

    -   支持从 MySQL、PostgreSQL 并发同步数据到 AnalyticDB for PostgreSQL。
    -   支持简单的数据转换。
    -   支持通过解析 Binlog，准实时地从 MySQL 同步数据到 AnalyticDB for PostgreSQL。
-   其他支持 Greenplum 的 ETL 工具。


