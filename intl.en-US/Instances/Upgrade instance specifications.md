# Upgrade instance specifications

When you use AnalyticDB for PostgreSQL, the data size and computing workload dynamically surge, and data processing speed may hit a bottleneck due to some computing resource specifications such as CPU, memory, and disk space as well as the number of data processing nodes. You can scale out AnalyticDB for PostgreSQL instances in a dynamic way. AnalyticDB for PostgreSQL allows you upgrade instance specifications online. You cannot downgrade instance specifications. This topic describes how to upgrade instance specifications.

## View the specifications of a specified instance

The instance specifications include **compute node specifications** and **the number of compute nodes**. For more information about compute node specifications, see [Instance specifications](/intl.en-US/Specifications and Pricing/Instance specifications.md).

To view the specifications of a specified instance, perform the following steps:

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).
2.  In the top navigation bar, select the region where the instance resides. For example, select the China \(Hangzhou\) region.

3.  Click the name of the instance to go to the Basic Information page.

    On the Basic Information page, you can view the instance specifications, including **compute node specifications** and **the number of compute nodes**. If the previous instance type definition is used, the specifications, number, and storage details of the compute groups are displayed.


AnalyticDB for PostgreSQL supports the following instance specifications:

-   High-performance: The name of this type of instance specifications starts with SSD. High-performance instance specifications provide better I/O capabilities and higher analysis performance based on SSD storage.
-   High-capacity: The name of this type of instance specifications starts with HDD. High-capacity instance specifications provide larger storage capacity at a lower cost based on HDD storage.

## Upgrade instance specifications

To upgrade instance specifications, perform the following steps:

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).
2.  In the top navigation bar, select the region where the instance resides. For example, select the China \(Hangzhou\) region.

3.  Click **Upgrade** in the Actions column corresponding to the instance. The Upgrade/Downgrade page is displayed.

4.  Select the specifications and the number of compute nodes, and click Buy Now.

    Different specifications and numbers of compute nodes can be combined. For more information, see [Pricing](https://www.aliyun.com/price/product#/gpdb/detail). The new combination of the specifications and the number of compute nodes must meet the following requirements:

    -   The storage type remains unchanged. The new specifications of compute nodes must be higher or the same as the previous specifications.
    -   If the new specifications of compute nodes is the same as the previous specifications, the new number of compute nodes must be greater than the previous number.
    If the preceding requirements are met, evaluate the actual data size and computing workload to choose an appropriate combination. For more information, see [How do I choose an AnalyticDB for PostgreSQL instance type?](/intl.en-US/FAQ/How do I choose an AnalyticDB for PostgreSQL instance type?.md)

    **Note:** The duration of upgrading instance specifications varies from 30 minutes to dozens of hours based on data size. The data size is determined by factors such as the numbers of tables, partitioned tables, indexes, compressed data size, total data size, and instance specifications. During this process, the instance remains read-only to ensure data consistency and the transient connection error occurs twice. Be well prepared in advance. After the instance specifications are upgraded, the instance returns to the Running state. You can access the database, and the engine version of the instance is automatically upgraded to the latest.


After you complete the preceding steps, check the status of the instance. If the upgrade is complete, the instance enters the **Running** state. Otherwise, the instance remains in the **Upgrading** state.

