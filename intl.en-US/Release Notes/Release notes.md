# Release notes

You can upgrade the kernel of your AnalyticDB for PostgreSQL instance to the latest minor version in the AnalyticDB for PostgreSQL console. For more information about how to update the kernel of an instance, see [Update the kernel of an AnalyticDB for PostgreSQL instance](/intl.en-US/Instances/Update the kernel of an AnalyticDB for PostgreSQL instance.md).

## AnalyticDB for PostgreSQL V6.0

-   May 6, 2020
    1.  New feature: FDW-based OSS foreign tables in the ORC format can be scanned.
    2.  New feature: Real-time transfer process of gptransfer can be written to logs.
    3.  New feature: The orafce extension is available to implement syntax and functions from Oracle in AnalyticDB for PostgreSQL.
    4.  New feature: Uppercase field names are supported.
    5.  New feature: Enhanced features of gptransfer are supported. With the features, partitions can be exchanged with tables. The upper limit of a row can be automatically modified for retrying if the size of the row exceeds the original upper limit during transmission. Tables with the SERIAL field are supported. Double quotation marks \("\) can be added to table names. External tables can be partitioned.
    6.  Bug fix: Errors occur for the plancache direct dispatch function when the ORCA query optimizer is enabled. This bug is fixed.
    7.  Bug fix: gptransfer may not correctly inherit tables. This bug is fixed.
-   April 17, 2020
    1.  New feature: The intarray extension is supported.
    2.  New feature: gptransfer can be used to store index data in individual databases or tables separately.
    3.  New feature: Integrated queries of vectors and structured data are supported.
    4.  New feature: Temporary authorization tokens can be created to pull data from OSS by using the CREATE LIBRARY statement.
    5.  New feature: The Odyssey compute engine is available for AOCS tables.
    6.  New feature: The HyperLogLog extension is supported.
    7.  Bug fix: gptransfer fails to migrate temporary tables and extension data tables. This bug is fixed.
    8.  Bug fix: Errors occur for VACUUM FULL operations on the pg\_class table. This bug is fixed.
-   March 27, 2020
    1.  New feature: Lineage analysis is available to link Database Management \(DMS\).
    2.  New feature: The fast decimal function is supported.
    3.  New feature: OSS foreign data wrappers are supported.
    4.  New feature: The pg\_trgm extension is supported.
    5.  Bug fix: The problems caused by invalidity of the OSS account are fixed.
    6.  Bug fix: The problems caused by version incompatibility of the MADlib dynamic library are fixed.
    7.  Bug fix: The memory usage of coordinator nodes is increasing when a large number of transactions are executed by Data Transmission Service \(DTS\). This bug is fixed.
-   February 26, 2020
    1.  New feature: The Odyssey vector compute engine is released. You can set the session variable enable\_odyssey to true or off to specify whether to accelerate queries by using the Odyssey compute engine.
    2.  New feature: The PostGIS extension is available to analyze spatial data.
    3.  New feature: Accelerated analysis of foreign tables is implemented based on the PostgreSQL Foreign Data Wrapper \(FDW\) framework.
    4.  New feature: The method for importing SDK code for OSS external tables is optimized so that the OSS SDK versions can be managed in a centralized manner.
    5.  New feature: The FastANN extension is available for vector queries.
    6.  Bug fix: Errors occasionally occur in core dumps \(segmentation fault\) due to CANCEL operations when OSS external tables are imported. This bug is fixed.
    7.  Bug fix: pg\_dumpall fails to dump rds\_superuser. This bug is fixed.
-   December 26, 2019
    1.  The bug that causes unavailability of MADlib on some server types is fixed.
    2.  The bug that causes occasional failures in creating an extension is fixed.
    3.  The bug that causes failures in creating a temporary table or index is fixed.
    4.  The tablefunc extension is added.
-   December 21, 2019
    1.  The postgres\_fdw extension is added.
    2.  The default maximum number of concurrent queries allowed in a resource queue is increased from 20 to 500.
-   November 25, 2019
    1.  The restart of an instance is no longer required when you enable or disable the global deadlock detection mechanism of that instance.
    2.  The memory computing logic used in the out of memory \(OOM\) protection mechanism is optimized to prevent OOM errors in the event of highly concurrent queries.
    3.  The superuser account is authorized to grant permissions to and revoke permissions from standard accounts.
    4.  The RoaringBitmap extension is added.
-   October 17, 2019
    1.  The bug that causes occasional failures in creating an instance is fixed.
-   October 15, 2019
    1.  Online transaction processing \(OLTP\) is optimized. Point select, insert, update, and delete operations based on the partition key can be committed by using phase 1 to reduce network broadcast overheads.
    2.  The ORCA version is upgraded to 3.77.0.
    3.  The global deadlock detection mechanism is enabled by default.
-   September 12, 2019
    1.  The ORCA version is upgraded to 3.67.0.
    2.  By default, the query optimizer provided with PostgreSQL is used for simple queries from individual tables.
    3.  The performance of instance scaling and configuration upgrade is increased.
    4.  The bug that causes PANIC errors in the checkpoint process on the secondary server after you truncate an AO table multiple times in a transaction and then abort the transaction is fixed.

## AnalyticDB for PostgreSQL V4.3

-   December 24, 2019
    1.  The bug that causes failures in executing statements such as `SET` during instance scaling is fixed.
    2.  The bug that causes failures in distributed queries when compute nodes run different versions is fixed.
    3.  AUTO VACUUM, AUTO VACUUM FREEZE, and AUTO ANALYZE statements are supported. If the number of rows you want to insert or delete exceeds the preset threshold, the system executes a VACUUM or ANALYZE statement to avoid a performance hit.
-   December 18, 2019
    1.  The bug that causes failures in explicitly enabling read-only transactions during instance scaling is fixed.
    2.  The bug that causes an instance to break down at a certain probability due to deadlocks produced from a full resource queue is fixed.
-   November 13, 2019
    1.  The metadata of MetaScan is optimized to include the block offset. The block offset enables AnalyticDB for PostgreSQL to directly read blocks that meet the specified filter criteria. This reduces the I/O of disks. For more information, see
    2.  The rds\_superuser account is authorized to grant permissions to and revoke permissions from other non-superuser accounts.
    3.  The adbpg.sql script is introduced to define functions used to batch grant or revoke permissions. \(**Note** that you must submit a ticket to request the execution of the script in the background.\)
-   October 16, 2019
    1.  The bug that causes the global deadlock detection mechanism to break down when the number of XIDs on segments reaches the value of the xid\_warn\_limit parameter is fixed.
-   October 15, 2019
    1.  The transmission of tables that have only one index each during instance scaling or configuration upgrade is optimized.
    2.  The global deadlock detection mechanism is optimized.
    3.  The global deadlock detection mechanism is enabled by default.
    4.  The bugs that causes the HyperLogLog extension to occupy more memory resources than it has applied for is fixed.
-   September 10, 2019
    1.  The global deadlock detection mechanism is introduced. When you execute an UPDATE or DELETE statement on a table, the table is no longer locked.
    2.  The default value of the gp\_max\_slices parameter is increased from 50 to 500.
    3.  ORDER BY clauses are supported by the uuid-ossp extension.
    4.  The speed of instance scaling and configuration upgrade is increased.
-   September 2, 2019
    1.  The upgrade from V4.3 to V6.0 is supported during instance migration.
    2.  The bug that causes failures in querying data from a column-oriented table after an index is created for the LOB column in that table is fixed.
    3.  The bug that causes the gpexpand extension to abnormally exit during instance scaling is fixed.
    4.  The uuid-ossp extension is supported.
    5.  The gp\_max\_slices parameter is introduced to limit the maximum number of slices allowed in a query. This parameter helps prevent kernel instability that may arise due to a large number of running threads.

