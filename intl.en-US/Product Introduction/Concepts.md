# Concepts {#concept_emc_dfr_52b .concept}

The following table lists the basic concepts involved in AnalyticDB for PostgreSQL.

|Term|Description|
|----|-----------|
|Group|The operation unit in AnalyticDB for PostgreSQL. -   A AnalyticDB for PostgreSQL instance is composed of multiple groups.
-   Increasing the number of groups can improve the linear performance.

 |
|Group type|The package of computing resources available. -   Each group type includes CPU, I/O, memory, and disk resources. Different group types have different performance.
-   Resources in one group are allocated to the same physical host.

 |
|Number of groups|The number of purchased groups. -   The minimum unit is two.
-   Each group type has different group number limit.

 |
|MPP|Massively Parallel Processing, a distributed Shared Nothing computing architecture. MPP improves the performance by parallel computing of multiple shared-nothing nodes \(known as groups in AnalyticDB for PostgreSQL\).|

