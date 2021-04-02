# Features of AnalyticDB for PostgreSQL V6.0

AnalyticDB for PostgreSQL V6.0 is a data warehousing service that is developed based on the open source Greenplum 6.0 kernel and PostgreSQL 9.4 kernel. This service offers an improved capability for processing concurrent transactions in real-time data warehousing scenarios. Transaction locks are optimized to support hybrid transaction/analytical processing \(HTAP\) services. The new features of AnalyticDB for PostgreSQL V6.0 are listed in the following sections.

**Note:** You cannot upgrade the major version of AnalyticDB for PostgreSQL instances from V4.3 to V6.0. If you want to upgrade an instance from V4.3 to V6.0, submit a ticket.

## Kernel updates

The Greenplum kernel is updated from 4.3 to 6.0, and the PostgreSQL kernel is updated from 8.2 to 9.4. The following list describes the new kernel features:

-   The JSONB data type is supported to offer more JSON processing functions and higher processing efficiency.
-   The UUID data type is supported.
-   GINs and SP-GiST indexes are supported to allow high-performance fuzzy match and retrieval.
-   Schema- and column-level permissions can be granted and managed.
-   VACUUM statements are supported. When they are executed to reclaim storage space, pages that have locks are temporarily skipped. A round-robin algorithm is used to check pages that have locks. If the locks are released, VACUUM statements are executed to reclaim the storage space. This reduces blocking.
-   Cross-database query and access are supported over database links.
-   Recursive common table expressions \(CTEs\) are supported to allow recursive queries of SQL statements. The CTEs are used for hierarchical or tree-structure data.
-   PL/SQL is enhanced to support RETURN QUERY EXECUTE statements. This way, SQL statements can be dynamically executed, and you can define anonymous blocks.
-   Coordinator nodes can be scaled out to eliminate the limits of the original architecture that has a single coordinator node. If no bottlenecks occur on compute nodes, the number of connections and the I/O capabilities can linearly increase with the number of coordinator nodes. This way, the overall performance of the system is enhanced.

## HTAP improvement

The global deadlock detection mechanism is introduced to check and eliminate global deadlocks by dynamically collecting and analyzing lock information. In this scenario, the update and modification operations of heap tables can be completed only by using fine-grained row locks. In addition, the modification, deletion, and query operations can be simultaneously performed for a large number of tables. This improves the concurrency and throughput of the system. Transaction locks are optimized to reduce competition for resources when transactions start and end. This way, high-throughput transaction processing is supported in addition to high-performance OLAP.

Instances that have a single coordinator node support the following features:

-   In typical OLTP scenarios, 100,000 transactions per second \(TPS\) are allowed in TPC-C.
-   150,000 TPS are supported for SELECT operations in TPC-B.
-   50,000 TPS are supported for INSERT operations in TPC-B.
-   20,000 TPS are supported for UPDATE operations in TPC-B.

For instances that have multiple coordinator nodes, the number of connections and the I/O capabilities can linearly increase with the number of coordinator nodes if no bottlenecks occur on compute nodes. For more information, see [TPC benchmarks for instances that have multiple coordinator nodes](/intl.en-US/Performance index/TPC benchmarks for instances that have multiple coordinator nodes.md).

## OLAP

-   Replicated tables are supported. For dimension tables in data warehouses, replicated tables can be created by using the DISTRIBUTED REPLICATED clause. This reduces data transmission and improves query efficiency.
-   The Zstandard compression algorithm is supported. This algorithm offers compression and decompression performance that is three times better than the zlib algorithm.

