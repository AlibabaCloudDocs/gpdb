# Synchronize data from other data sources to AnalyticDB for PostgreSQL

This topic describes the extract, transform, load \(ETL\) tools used by AnalyticDB for PostgreSQL to synchronize data from other data sources. For more information about data synchronization solutions, see [Introduction to data migration and synchronization solutions](/intl.en-US/Data Access/Introduction to data migration and synchronization solutions.md).

-   [Data Transmission Service \(DTS\)](/intl.en-US/Instance Management/Upgrade instance specification.md): a real-time data synchronization service provided by Alibaba Cloud. You can use it to synchronize data in real time from other data sources to AnalyticDB for PostgreSQL. Such data sources include ApsaraDB RDS for MySQL instances, user-created MySQL databases housed on ECS instances, and Apsara PolarDB clusters.
-   [Data Integration](https://www.aliyun.com/product/cdp/): an ETL tool provided by Alibaba Cloud. In Data Integration, you can configure your AnalyticDB for PostgreSQL instance as the destination to which you synchronize data from other data sources such as ApsaraDB for RDS, MaxCompute, and Tablestore.
    -   You also have the option to directly read data from other data sources and write the data into AnalyticDB for PostgreSQL.
    -   If you want to synchronize a large volume of data concurrently, we recommend that you first use Data Integration to import the data to Alibaba Cloud Object Storage Service \(OSS\) and then use an OSS external table to import the data to AnalyticDB for PostgreSQL.
-   [Pentaho Kettle](http://community.pentaho.com/projects/data-integration/): an open source ETL tool.
    -   You can use Kettle to import data to a local disk and then use a COPY statement or OSS to import the data from that disk to AnalyticDB for PostgreSQL.
    -   You can also mount an OSS bucket as a local virtual disk, use Kettle to import data to that disk, and then use an OSS external table to import the data from that disk to AnalyticDB for PostgreSQL.
-   [Informatica](https://www.informatica.com/cn/): an ETL tool for commercial use.
-   [dbsync](https://github.com/aliyun/rds_dbsync/releases): an open source database synchronization tool provided by Alibaba Cloud. This tool offers the following functions:
    -   Concurrent data synchronization from a MySQL or PostgreSQL database to an AnalyticDB for PostgreSQL database.
    -   Simple data conversion.
    -   Quasi-real-time data synchronization from a MySQL database to an AnalyticDB for PostgreSQL database based on binary log parsing.
-   Other ETL tools that support Greenplum.

