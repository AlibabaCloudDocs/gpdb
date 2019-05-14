# What ETL tools does AnalyticDB for PostgreSQL currently support? How to synchronize data from other data sources to AnalyticDB for PostgreSQL on a regular basis? {#concept_ujs_lgg_v2b .concept}

The following ETL tools are supported:

-   [Alibaba Cloud Data Integration](https://www.aliyun.com/product/cdp/): an ETL tool provided by Alibaba Cloud. In Data Integration, you can configure AnalyticDB for PostgreSQL as a PostgreSQL database to synchronize data from other data sources \(RDS, MaxCompute, and TableStore\) to AnalyticDB for PostgreSQL.

    -   You can directly read data from other data sources, and write data to AnalyticDB for PostgreSQL.
    -   If the data size is big and concurrent imports are required, we recommend that you first import the data to OSS from other data sources by using Data Integration, and then import the data by using the OSS external table to AnalyticDB for PostgreSQL.
-   [Pentaho Kettle](http://community.pentaho.com/projects/data-integration/): an open-source ETL tool.

    -   You can import data through Kettle to an ephemeral disk first, and then import the data to AnalyticDB for PostgreSQL by using COPY or OSS.
    -   You can also mount an OSS instance as a virtual ephemeral disk, import data through Kettle to that disk, and then import data to AnalyticDB for PostgreSQL by using the OSS external table.
-   [Informatica](https://www.informatica.com/cn/): a commercial ETL tool.

-   [Bifrost](https://market.aliyun.com/products/56024006/cmjj011322.html): an ETL tool in Alibaba Cloud marketplace. It now provides commercial services.

-   [dbsync](https://github.com/aliyun/rds_dbsync/releases): an open-source database synchronization tool provided by Alibaba Cloud.

    -   Supports synchronizing data concurrently from MySQL and PostgreSQL to AnalyticDB for PostgreSQL.
    -   Supports simple data conversion.
    -   Supports parsing binlog to synchronize data from MySQL to AnalyticDB for PostgreSQL in a quasi-real-time manner.
-   Other ETL tools supporting Greenplum.


