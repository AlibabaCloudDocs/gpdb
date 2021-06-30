# AnalyticDB for PostgreSQL overview

AnalyticDB for PostgreSQL allows you to perform real-time interactive analysis, extract, transform, load \(ETL\) and extract, load, transform \(ELT\) operations, and business intelligence \(BI\) report visualization on petabytes of data. AnalyticDB for PostgreSQL also allows you to write data in real time and import data in batches with high throughput and provides atomicity, consistency, isolation, durability \(ACID\) guarantee and standard transaction isolation. AnalyticDB for PostgreSQL is a cost-effective cloud native database warehouse that uses the massively parallel processing \(MPP\) architecture to provide public cloud and hybrid cloud services based on the Alibaba Cloud ecosystem.

## Overview

AnalyticDB for PostgreSQL supports JDBC and ODBC connections and is fully compatible with the SQL:2003 syntax standard, PostgreSQL, and Greenplum, and partially compatible with Oracle syntax. AnalyticDB for PostgreSQL provides PL/pgSQL stored procedures and user-defined functions \(UDFs\) in Java or Python. AnalyticDB for PostgreSQL supports Apache MADLib for machine learning and PostGIS for geometry analysis in SQL standards. You can use standard SQL to perform hybrid analysis on semi-structured data in the JSON or JSON-B format, unstructured data such as images and audios, and structured data.

AnalyticDB for PostgreSQL supports various deployment modes. AnalyticDB for PostgreSQL provides Alibaba Cloud public cloud services on a pay-as-you-go basis, supports vertical and horizontal scaling, and allows you to separately scale up storage capacity online. AnalyticDB for PostgreSQL also provides Apsara Stack Enterprise and Apsara Stack Agility for hybrid cloud deployment by using DBStack. AnalyticDB for PostgreSQL supports x86 platforms and the ARM platform.

AnalyticDB for PostgreSQL ranks first in the TPC Benchmark-H \(TPC-H\) 30 TB cost-effectiveness list. AnalyticDB for PostgreSQL passes the TPC Benchmark-C \(TPC-C\) transactional performance test and the TPC Benchmark-DS \(TPC-DS\) 100 TB 640-node analytical performance test that are organized by the China Academy of Information and Communications Technology \(CAICT\).

## Architecture

The architecture of AnalyticDB for PostgreSQL consists of coordinator nodes and compute nodes. The nodes are interconnected for data transmission. The following figure shows the architecture.

![21107201](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2837273261/p276620.png)

Coordinator nodes and compute nodes provide multiple replicas to ensure high availability of services and high reliability of data. You can scale out coordinator nodes or compute nodes of an instance to improve the concurrency and throughput of read and write queries.

## Module components

-   **Coordinator node**

    Coordinator nodes establish connections with clients over access protocols, perform authentication and authorization, and assume roles of parsers, rewriters, optimizers, and dispatchers.

    Coordinator nodes also assume the role of the global transaction manager \(GTM\) to generate snapshots and global transaction IDs and manage distributed transactions. Coordinator nodes provide a global catalog to record the metadata of database objects such as users, databases, tables, views, indexes, and distributed partitions.

-   **Compute Node**

    A compute node consists of a set of segments. A compute node can be a physical machine, virtual machine \(VM\), or container.

-   **Segment**

    Segments are responsible for specific SQL execution and data storage. The local catalog of segments is synchronized with the global catalog of coordinator nodes to accelerate statement execution. This way, segments do not need to frequently access coordinator nodes to obtain metadata. The local transaction manager of a segment is used to coordinate transactions. The buffer pool for a segment caches reads and writes to improve read and write performance.

    The query executor leverages a vectorized execution engine and just-in-time \(JIT\) compilation to improve calculation performance by multiple times compared with the volcano model that executes calculation by row.

    AnalyticDB for PostgreSQL supports row store tables, column store tables, external tables, and corresponding indexes.

    -   Row store tables: Data is stored by row. Rows can be ordered by a primary key. B-tree, bitmap, and GIN indexes are supported. Row store tables are suitable for live data write, update, and delete operations. They are also suitable for point queries and range queries. AnalyticDB for PostgreSQL manages transactions in the row-store format by using the Multi-Version Concurrency Control \(MVCC\) model.
    -   Column store tables: Data is stored by column in a high compression ratio. Column stores are suitable for scenarios in which a small amount of data requires to be updated or deleted. Column stores support B-tree indexes for efficient point queries and provide lightweight block range indexes by using min and max values. Data can be sorted in multiple columns by multiple dimensions. Sorted columns can be filtered based on composite conditions for efficient analysis.
    -   External tables: You can use external tables to query metadata that is stored in local system tables and data that is stored in Object Storage Service \(OSS\). External tables can be partitioned and support various data formats such as ORC, Parquet, CSV, and JSON. ORC and Parquets data files support column filtering and predicate pushdown, which improves analysis performance. External tables can also query data from Hadoop Distributed File System \(HDFS\) and Apache Hive.
    ![21107201](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2837273261/p276623.png)


## Data models

Ideally, tabular data is evenly distributed across nodes. Even data distribution makes the most advantage of I/O performance of an instance, improves storage capacity, and optimizes efficiency in computing and network transmission. Hash distribution is the default table distribution option. AnalyticDB for PostgreSQL also supports replication distribution and random distribution. Replication distribution generates a full copy of the table accessible on each compute node. Typically, replication distribution is used for small tables on which associated queries are frequently performed. When queries are executed on replicated tables, data is not required to be broadcasted or redistributed. This way, query performance is improved. Random distribution is suitable for scenarios where fields in a large table are inappropriate to be hash columns. For example, hash distribution may cause data skew among nodes and replication distribution is not suitable for a large table. In this case, random distribution evenly distributes data across compute nodes.

After tabular data is distributed across compute nodes, you can partition the tabular data on a single compute node for specific queries to narrow down the scope of query and data processing. AnalyticDB for PostgreSQL supports the range and list partitioning types and allows you to create multi-level partitions. In the following figure, data in a hash table is distributed across three nodes based on the id column. On each node, range partitioning is performed based on the date column and then list partitioning is performed based on the city column. Each partition stores data and indexes of the table, as shown in the rightmost part of the following figure. Partitioned tables can be row store tables, column store tables, or external tables. For example, you can use row store tables for the Mar partition to which data needs to be written, column store tables for the Feb partition in which data is archived, and OSS external tables for the Jan partition on which few queries are performed.

![21107203](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2837273261/p276624.png)

## Database objects

AnalyticDB for PostgreSQL is an object-relational database service that can be used to customize objects and object properties including data types, functions, operators, domains, and indexes. Complex data structures can be created, stored, and retrieved. Database objects typically include tables, views, functions, sequences, indexes, partitioned child tables, and external tables. The objects are divided into different schemas by logic. After a database is created, the public schema is created as the default schema. All users or roles can access the public schema. By default, objects created for this database are included in the public schema.

A database is a physical collection of database objects. A schema is a logical collection that is used to organize and manage database objects in a database. A schema contains objects that applications can access, such as tables, indexes, data types, functions, and operators. You can use schemas to organize database objects into logical groups for ease of management. This way, multiple users or roles can use the same database without interfering with each other.

Users or roles control the global permissions of an instance and manage the permissions of all objects for an instance. A user is not specific to an individual database. To log on to the Data Management \(DMS\) console, a user must connect to a database. A user can own various database objects.

By default, a user cannot view the objects that are not owned by the user. To view the objects, the user must be granted the required permissions. If the required permissions are granted, the user can create objects in other schemas. By default, each user has the permissions to create objects in the public schema, such as the permissions to create a table and read data from and write data to the table.

AnalyticDB for PostgreSQL stores objects as system metadata on servers of both coordinator and compute nodes.

![21107204](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2837273261/p276625.png)

