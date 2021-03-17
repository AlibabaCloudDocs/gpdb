# Instance specifications

This topic describes AnalyticDB for PostgreSQL instance specifications and provides recommendations.

## 存储资源模式

云原生数据仓库AnalyticDB PostgreSQL版，新增了存储资源模式的选择。

-   存储预留模式

    这是传统的模式，即用户在选择规格后，即分配固定配额的存储空间。当用户空间不足时，只能通过扩展节点的方式间接扩大存储空间。

-   存储弹性模式

    存储弹性模式这是一种新的模式，即用户在购买时，除选择规格外，还需指定存储空间。当用户空间不足时，不仅可通过扩展节点方式扩容，也可通过增大存储空间的方式扩容。


**存储类型**

根据存储资源模式的不同，可分为不同的选择。

-   **存储预留模式**
    -   **高性能SSD存储**：可以提供更好的I/O能力，带来更高的分析性能。
    -   **大容量HDD存储**：可以提供更大、更实惠的空间，满足更高的存储需求。
-   **存储弹性模式**
    -   **高性能ESSD存储**：可以提供更好的I/O能力，带来更高的分析性能。
    -   **大容量高效云盘存储**：可以提供更大、更实惠的空间，满足更高的存储需求。

|资源模式|节点（分区）存储类型|节点核数|内存|有效存储空间|说明|
|----|----------|----|--|------|--|
|预留|高性能SSD|1|8GB|80GB|只适合低并发场景（<5并发），且节点数少于32个的场景；支持节点范围2-128个。|
|高性能SSD|4|32GB|320GB|高性能SSD存储的主要推荐规格；支持节点范围8-4096个。|
|大容量HDD|2|16GB|1TB|只适合低并发场景（<5并发），且节点数少于8个的场景；支持节点范围4-32个。|
|大容量HDD|4|32GB|2TB|大存储HDD主要推荐规格；支持节点范围8-4096个。|
|弹性|高性能ESSD|2|16GB|50GB~1TB|只适合低并发场景，支持节点范围4-128个。|
|高性能ESSD|4|32GB|50GB~2TB|只适合高并发场景，支持节点范围4-128个|
|大容量高效云盘|2|16GB|50GB~3TB|只适合低并发场景，支持节点范围4-128个|
|大容量高效云盘|4|32GB|50GB~4TB|只适合中并发场景，支持节点范围4-128个|

**实例节点数**

一个实例由多个节点组成，每个节点即MPP架构下一个数据分区，单实例最多支持4096个节点。每个节点存储并处理一部分数据。MPP 架构下，存储空间随节点数线性增加，而查询响应时间 RT 不变。

**Note:** 关于产品价格，请参见[Pricing](/intl.en-US/Specifications and Pricing/Pricing.md)。

## Storage types

-   **High-performance SSD**: provides better I/O capabilities and higher analysis performance.
-   **High-capacity HDD**: provides larger storage capacity at a lower cost.

|Storage type|Number of cores per node|Memory|Storage space|Description|
|------------|------------------------|------|-------------|-----------|
|High-performance SSD|1|8 GB|80 GB|These specifications are recommended for low-concurrency scenarios that require less than 5 concurrent queries and less than 32 nodes. These specifications are available for 2 to 128 nodes.|
|High-performance SSD|4|32 GB|320 GB|These specifications are recommended for high-performance SSD storage and available for 8 to 4,096 nodes.|
|High-capacity HDD|2|16 GB|1 TB|These specifications are recommended for low-concurrency scenarios that require less than 5 concurrent queries and less than 8 nodes. These specifications are available for 4 to 32 nodes.|
|High-capacity HDD|4|32 GB|2 TB|These specifications are recommended for high-capacity HDD storage and available for 8 to 4,096 nodes.|

**Number of nodes**

A single instance can have up to 4,096 nodes. In the massively parallel processing \(MPP\) architecture, each node is a partition that is used to store and process a portion of data on the instance. You can add nodes to increase the storage capacity and maintain a stable query response time.

## Recommendations on how to select instance specifications

When you create or upgrade the specifications of an AnalyticDB for PostgreSQL instance, you must configure **Storage Type**, **Node Cores**, and **Node Num**. AnalyticDB for PostgreSQL supports data storage to Object Storage Service \(OSS\) external tables. You can use the gzip utility to compress data that is not needed for real-time computing and then upload the data to OSS buckets to reduce storage costs.

-   **存储类型选择**
    -   对于性能优先类场景，建议以SSD或ESSD构建分析数据仓库实例；
    -   对于以数据存储类优先的场景，可以考虑HDD或高效云盘类型。
-   **Storage type**
    -   If high performance is your primary concern, we recommend that you choose the SSD storage type.
    -   If large storage capacity is your primary concern, we recommend that you choose the HDD storage type.
-   **单节点核数选择**

    每个节点会存储并处理每个用户表其中一个分片的数据，即MPP架构下一个数据分区。推荐核数配置为单节点4核；SSD存储单节点1核或ESSD存储单节点2核，只适合32节点内实例的低并发执行场景；HDD存储单节点2核或高效云盘存储单节点2核，只适合8节点内实例的低并发执行场景

-   **Number of cores per node**

    Each node stores and processes data from a partition of each user table. We recommend that you configure four cores for each node. The SSD configuration that supports one core per node is suitable only for an instance that has 32 nodes or less and processes a small amount concurrent queries. The HDD configuration that supports two cores per node is suitable only for an instance that has eight nodes or less and processes few concurrent queries.

-   **Number of nodes**

    AnalyticDB for PostgreSQL uses the MPP architecture. This architecture enables the data processing capability of an instance to linearly increase in proportion with the number of nodes. However, the query response time remains constant when the data volume increases. You can determine the number of nodes the instance needs based on your business scenario and the volume of raw data.


**Row store and column store**

AnalyticDB for PostgreSQL supports two storage models: row store and column store. You can specify a storage model when you create a table.

-   If you want to write data in real time or frequently update data by executing INSERT, UPDATE, and DELETE statements, we recommend that you choose row store.

    If you choose row store, 1 TB of raw data requires about 1 TB of storage space. However, the indexes, logs, and temporary files generated during computing also occupy storage space. Therefore, we recommend that you reserve 2 TB of storage space for every 1 TB of raw data. To improve query performance, you can add nodes to increase available CPU and memory resources.

-   In batch extract, transform, and load \(ETL\) scenarios, we recommend that you choose column store. Data is rarely updated by executing UPDATE and DELETE statements and most queries require aggregations and joins of table data based on only a small amount of columns

    Column store provides a compression ratio in the range of 1:5 to 1:2. For example, if 1 TB of raw data is reduced to 0.5 TB or less after compression, you need to reserve only 1 TB of storage space for user data.


## Examples

If you want to process 5 TB of raw data with high performance to respond to more than 100 concurrent queries, we recommend that you choose the SSD storage type to support 4 cores per node and 32 nodes per instance. In this scenario, a total of 10 TB of storage space is available for user data.

**Note:** From August 23, 2019, the basic building block of an AnalyticDB for PostgreSQL instance is compute node. The previous building block was compute group. A compute group contains multiple partitions, whereas a compute node is equivalent to an MPP partition. This simplifies the definition of instance specifications and complies with cluster database naming conventions. For information about the mappings between compute groups and compute nodes, see [Mappings between compute node types and compute group types](/intl.en-US/FAQ/Mappings between compute node types and compute group types.md).

