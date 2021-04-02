# Overview

AnalyticDB for PostgreSQL is a massively parallel processing \(MPP\) data warehousing service that is designed to analyze large volumes of data online.

AnalyticDB for PostgreSQL is developed based on the open source Greenplum Database project and is enhanced with in-depth extensions by Alibaba Cloud. AnalyticDB for PostgreSQL is compatible with the ANSI SQL 2003 syntax and the PostgreSQL and Oracle database ecosystems. AnalyticDB for PostgreSQL also supports row store and column store. AnalyticDB for PostgreSQL processes petabytes of data offline at a high performance level and supports highly concurrent online queries. This way, AnalyticDB for PostgreSQL can provide a competitive data warehousing solution in various industries.

## Features

-   Adaptable to variable workloads with no tuning required.

    AnalyticDB for PostgreSQL is fully compatible with SQL 2003 syntax and partially compatible with Oracle syntax. This service also supports PL/SQL stored procedures. AnalyticDB for PostgreSQL offers new-generation query optimizers to eliminate the need to tune complex SQL statements.

-   Analyzes petabytes of data within seconds.

    AnalyticDB for PostgreSQL uses an MPP scale-out architecture to respond to queries for petabytes of data within seconds. AnalyticDB for PostgreSQL supports vector computing and intelligent column store indexing. The performance of AnalyticDB for PostgreSQL is ten times better than that of a traditional database engine.

-   Provides high availability and always-on connectivity.

    AnalyticDB for PostgreSQL supports distributed transactions, redundancy for all nodes and data, automatic monitoring and failover for hardware faults, and atomicity, consistency, isolation, durability \(ACID\).

-   Compatible with a wide variety of ecosystems.

    AnalyticDB for PostgreSQL supports mainstream business intelligence \(BI\) and extract, transform, load \(ETL\) tools. For example, AnalyticDB for PostgreSQL is integrated with the PostGIS extension to analyze geographic data and with the MADlib library to provide more than 300 built-in machine learning algorithms.

-   Enables data synchronization.

    AnalyticDB for PostgreSQL can synchronize data with various data sources by using tools such as Data Transmission Service \(DTS\) and DataWorks. AnalyticDB for PostgreSQL supports highly parallel access to Object Storage Service \(OSS\) and Data Lake Analytics \(DLA\).


## Architecture

AnalyticDB for PostgreSQL uses the MPP architecture. In this architecture, an instance is composed of multiple [compute nodes](/intl.en-US/Product Introduction/Terms.md). The storage type of the disk can be SSD or enhanced SSD \(ESSD\). Computing is decoupled from storage. You can add compute nodes to an instance to linearly scale out its storage capacity and maintain a stable response time. Each instance is composed of one coordinator node and multiple compute nodes.

-   Coordinator node
    -   Receives query requests and determines distributed query plans.
-   Compute node
    -   Provides MPP.
    -   Stores data in dual copies on each partition.
    -   Automatically backs up data to OSS on a regular basis.

Starting from February 8, 2021, you are allowed to configure multiple coordinator nodes in AnalyticDB for PostgreSQL. Greenplum does not support this feature. You can add multiple coordinator nodes to an instance to push beyond the limits of the original architecture in which an instance has a single coordinator node. If compute nodes permit, the number of connections and the I/O capabilities can linearly increase with the number of coordinator nodes. This way, the overall performance of the system is enhanced to better meet the requirements of business scenarios such as real-time data warehousing and hybrid transaction/analytical processing \(HTAP\). For an instance that has multiple coordinator nodes, AnalyticDB for PostgreSQL provides instance endpoints in addition to the primary coordinator node endpoints. For more information, see [t2043839.md\#]().

## References

-   To learn more about AnalyticDB for PostgreSQL, you can go to the Alibaba Cloud Yunqi community and join the AnalyticDB for PostgreSQL technical support group on DingTalk. For more information, see [Get started with AnalyticDB for PostgreSQL](https://yq.aliyun.com/articles/696196).
-   If you require technical assistance, [submit a ticket](https://workorder.console.aliyun.com/console.htm#/ticket/add?productCode=gpdb).
-   For information about the open source Greenplum Database project, visit [http://www.greenplum.org](http://www.greenplum.org/).

**Note:** Since August 23, 2019, the basic building block of an AnalyticDB for PostgreSQL instance has been changed from compute group to compute node. A compute group contains multiple partitions, whereas a compute node is equivalent to an MPP partition. This simplifies the definition of instance specifications and complies with the naming conventions for cluster databases. For information about the mappings between compute groups and compute nodes, see [Mappings between compute node types and compute group types](https://www.alibabacloud.com/product/hybriddb-postgresql/pricing).

