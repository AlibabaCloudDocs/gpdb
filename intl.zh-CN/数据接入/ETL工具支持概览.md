# ETL工具支持概览

支持的ETL工具见下，可以同时参见更详细的同步方案列表[数据迁移及同步方案综述](/intl.zh-CN/数据接入/数据迁移及同步方案综述.md)：

-   [数据传输服务（DTS）](/intl.zh-CN/实例管理/升级实例配置.md)：阿里云提供的实时数据同步服务，可以将其他数据源（RDS MySQL，ECS自建MySQL，PolarDB等）实时同步数据到AnalyticDB PostgreSQL版，构建实时数据仓库解决方案。
-   [阿里云的数据集成服务\(Data Integration\)](https://www.aliyun.com/product/cdp/) ：阿里云提供的ETL工具。在数据集成服务中，将AnalyticDB PostgreSQL版配置为一个PostgreSQL数据库，即可实现其他数据源（RDS、MaxCompute、TableStore等）到AnalyticDB PostgreSQL版的数据同步。
    -   您可以直接从其他数据源读取数据，写入到AnalyticDB PostgreSQL版中。
    -   如果数据量较大，需要并发导入，则建议您先通过数据集成服务把数据从其他数据源导入到OSS，再通过OSS外部表导入AnalyticDB PostgreSQL版。
-   [Pentaho Kettle 数据集成软件](http://community.pentaho.com/projects/data-integration/)：开源的ETL工具。
    -   支持将数据先通过Kettle导入到本地磁盘，再通过COPY或OSS导入到AnalyticDB PostgreSQL版。
    -   也支持将OSS存储挂载为本地虚拟磁盘，通过Kettle导入到此磁盘，最后通过AnalyticDB PostgreSQL版的OSS外部表导入到AnalyticDB PostgreSQL版中。
-   [Informatica软件](https://www.informatica.com/cn/)：商业化的ETL工具。
-   [dbsync](https://github.com/aliyun/rds_dbsync/releases)：阿里云提供的开源数据库同步工具。
    -   支持从MySQL、PostgreSQL并发同步数据到AnalyticDB PostgreSQL版。
    -   支持简单的数据转换。
    -   支持通过解析Binlog，准实时地从MySQL同步数据到AnalyticDB PostgreSQL版。
-   其他支持Greenplum的ETL工具。

