# Scenarios

This topic describes the common scenarios and benefits of AnalyticDB for PostgreSQL.

## Scenarios

-   **Data warehousing service**

    You can use Data Transmission Service \(DTS\) or Data Integration to synchronize large amounts of data from Alibaba Cloud database services such as ApsaraDB RDS and PolarDB or self-managed databases to AnalyticDB for PostgreSQL. AnalyticDB for PostgreSQL supports Extract, Transform, and Load \(ETL\) operations on large amounts of data. You can also use DataWorks to schedule these tasks. AnalyticDB for PostgreSQL also provides high-performance online analysis capabilities. You can use Business Intelligence \(BI\) tools such as Quick BI, DataV, Tableau, and FineReport to query real-time data that is stored in AnalyticDB for PostgreSQL and present the data in reports.

-   **Big data analytics platform**

    You can use Data Integration or Object Storage Service \(OSS\) to import large amounts of data from MaxCompute, Hadoop, and Spark to AnalyticDB for PostgreSQL for high-performance analysis, processing, and online data exploration.

-   **Data lake analytics**

    AnalyticDB for PostgreSQL allows you to use OSS foreign tables to directly query large amounts of data in OSS in parallel and build an Alibaba Cloud data lake analytics platform.


## Benefits

AnalyticDB for PostgreSQL provides the following benefits for online analytical processing \(OLAP\) services:

-   **ETL for offline data processing**

    AnalyticDB for PostgreSQL provides the following benefits that make it ideal for optimizing complex SQL queries as well as aggregating and analyzing large amounts of data:

    -   Supports standard SQL syntax, OLAP window functions, and stored procedures.
    -   Provides the ORCA query optimizer to enable complex queries without the need for tuning.
    -   Uses the massively parallel processing \(MPP\) architecture that can analyze and process petabytes of data in seconds.
    -   Provides column store-based high-performance scanning of large tables at a high compression ratio.
-   **Online high-performance query**

    AnalyticDB for PostgreSQL provides the following benefits for real-time exploration, warehousing, and updating of data:

    -   Allows you to process high-throughput data by performing operations such as INSERT, UPDATE, and DELETE.
    -   Allows you to obtain results from point query within milliseconds based on row store and multiple indexes such as B-tree and bitmap indexes.
    -   Supports distributed transactions, standard database isolation levels, and hybrid transaction/analytical processing \(HTAP\).
-   **Multi-modal data analysis**

    AnalyticDB for PostgreSQL provides the following benefits for processing unstructured data from a variety of sources:

    -   Supports the PostGIS extension for geographic data analysis and processing.
    -   Uses the MADlib library of in-database machine learning algorithms to implement AI-native databases.
    -   Provides high-performance retrieval and analysis of unstructured data such as images, audios, and text based on vector retrieval
    -   Supports formats such as JSON and can analyze and process semi-structured data such as logs.

