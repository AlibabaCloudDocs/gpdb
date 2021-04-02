# TPC benchmarks for instances that have multiple coordinator nodes

## TPC-C

-   Test specifications: AnalyticDB for PostgreSQL 6.0, elastic storage, 4 cores and 32 GB memory per compute node, 32 compute nodes per instance, enhanced SSD \(ESSD\).
-   Test method: Use the TPC-C benchmark to test instances that are configured with the preceding specifications and a varying number of coordinator nodes in the range of 1 to 4. For more information, see [TPC-C](/intl.en-US/Performance index/TPC-C.md).
-   The following table describes the test results for instances that have multiple coordinator nodes.

    |Number of coordinator nodes|TpmC|Performance ratio|
    |---------------------------|----|-----------------|
    |1 \(64 concurrent transactions\)|19975.27|1 \(100%\)|
    |2 \(128 concurrent transactions\)|39349.72|1.97 \(98.5%\)|
    |3 \(192 concurrent transactions\)|58606.68|2.93 \(97.8%\)|
    |4 \(256 concurrent transactions\)|79826.89|3.996 \(99.9%\)|

    The following table describes the test results for instances that have a single coordinator node.

    |Number of concurrent transactions|TpmC|
    |---------------------------------|----|
    |1|696.253234|
    |8|9421.58|
    |16|16045.81|
    |32|19315.04|
    |64|19808.72|
    |128|19142.56|

    The preceding test results indicate that TPC-C performance linearly increases with the number of coordinator nodes and the number of concurrent transactions based on the condition that the number of compute nodes remains unchanged.


## TPC-B

-   Test specifications: AnalyticDB for PostgreSQL 6.0, elastic storage, 4 cores and 32 GB memory per compute node, 32 compute nodes per instance, enhanced SSD \(ESSD\).
-   Test method: Use the TPC-B benchmark to test instances that have the preceding specifications and a varying number of coordinator nodes in the range of 1 to 4. For more information, see [TPC-B](/intl.en-US/Performance index/TPC-B.md).

-   **TPC-B**

    The following table describes the test results for instances that have multiple coordinator nodes.

    |Number of coordinator nodes|Tps|Performance ratio|
    |---------------------------|---|-----------------|
    |1 \(64 concurrent transactions\)|3726.414536|1 \(100%\)|
    |2 \(128 concurrent transactions\)|7420.330380|1.99 \(99.6%\)|
    |3 \(196 concurrent transactions\)|11074.404963|2.97 \(99.1%\)|
    |4 \(256 concurrent transactions\)|14569.240161|3.91 \(97.7%\)|

    The following table describes the test results for instances that have a single coordinator node.

    |Number of concurrent transactions|Tps|
    |---------------------------------|---|
    |1|144.814643|
    |8|1143.098047|
    |16|2146.930340|
    |32|3246.044723|
    |64|3668.369726|
    |128|3658.159183|

-   **TPC-B \(Select-Only\)**

    The following table describes the test results for instances that have multiple coordinator nodes.

    |Number of coordinator nodes|Tps|Performance ratio|
    |---------------------------|---|-----------------|
    |1 \(64 concurrent transactions\)|22815.6 \(EC mode\)|1 \(100%\)|
    |2 \(128 concurrent transactions\)|44275 tps|1.94 \(97%\)|
    |3 \(192 concurrent transactions\)|64639.7 tps|2.83 \(94%\)|
    |4 \(256 concurrent transactions\)|85310.2 tps|3.74 \(93.5%\)|

    The following table describes the test results for instances that have a single coordinator node.

    |Number of concurrent transactions|Tps|
    |---------------------------------|---|
    |1|1125.090283|
    |8|8552.457660|
    |16|14874.467761|
    |32|20708.188841|
    |64|22496.637455|
    |128|22043.953276|

-   **TPC-B \(Insert-Only\)**

    The following table describes the test results for instances that have multiple coordinator nodes.

    |Number of coordinator nodes|Tps|Performance ratio|
    |---------------------------|---|-----------------|
    |1 \(64 concurrent transactions\)|23626.0 \(EC mode\)|1 \(100%\)|
    |2 \(128 concurrent transactions\)|45868.2 tps|1.94 \(97%\)|
    |3 \(192 concurrent transactions\)|64639.7 tps|2.74 \(91.3%\)|
    |4 \(256 concurrent transactions\)|84871.9 tps|3.60 \(90%\)|

    **Note:** In this mode, linearity decreases when the number of coordinator nodes is 4. This is because the underlying compute nodes and ESSDs reach bottlenecks of storage bandwidth. In the future, AnalyticDB for PostgreSQL will support more specifications for compute nodes to improve storage performance.

    The following table describes the test results for instances that have a single coordinator node.

    |Number of concurrent transactions|Tps|
    |---------------------------------|---|
    |1|696.253234|
    |8|5770.611358|
    |16|11277.564613|
    |32|19579.718539|
    |64|23534.760806|
    |128|23394.270989|

    The preceding test results indicate that TPC-B performance linearly increases with the number of coordinator nodes and the number of concurrent transactions based on the condition that the number of compute nodes remains unchanged.


