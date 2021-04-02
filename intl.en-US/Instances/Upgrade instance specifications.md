# Upgrade instance specifications

When you use AnalyticDB for PostgreSQL, the data size and computing workload may dynamically surge, and data processing speed may hit a bottleneck due to the computing resource specifications such as CPU, memory, disk space, and the number of data processing nodes. You can upgrade specifications of AnalyticDB for PostgreSQL instances in a dynamic way. AnalyticDB for PostgreSQL allows you to upgrade instance specifications online. This way, you can avoid interruptions to your service due to increased computing workloads. This topic describes how to upgrade instance specifications.

AnalyticDB for PostgreSQL allows you to upgrade instance specifications such as the **number of coordinator nodes**, the **specifications of a compute node**, and the **storage capacity of a compute node**. For more information about the specifications of compute nodes, see [Instance specifications](/intl.en-US/Specifications and Pricing/Instance specifications.md).

## Upgrade the specifications of compute nodes

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/gpdb/cn-hangzhou/list).
2.  In the top navigation bar, select the region where an instance resides. For example, select the China \(Hangzhou\) region.
3.  Find the instance and click its ID to go to the Basic Information page.

    In the Configuration Information section, the instance specifications are displayed, including the **number of coordinator nodes**, the **specifications of a compute node**, and the **storage capacity of a compute node**.

    AnalyticDB for PostgreSQL supports the following instance specifications:

    -   High-performance: The name of this specification type starts with SSD. High-performance instance specifications provide better I/O capabilities and higher analysis performance based on SSD storage.
    -   High-capacity: The name of this specification type starts with HDD. High-capacity instance specifications provide larger storage capacity at a lower cost based on HDD storage.
    **Note:** If the original architecture in which an instance has a single coordinator node is used, the specifications, number, and storage details of compute groups are displayed.

4.  Click **Add Segment Node**. On the page that appears, specify the number of compute nodes, read and select the AnalyticDB for PostgreSQL Terms of Service, and then click **Buy Now** to add compute nodes.
5.  Click **Segment Disk Expansion**. On the page that appears, specify the storage capacity for each compute node, read and select the AnalyticDB for PostgreSQL Terms of Service, and then click **Buy Now** to increase the storage capacity of each compute node.

    Different specifications and numbers of compute nodes can be combined. For more information, see [Pricing](https://www.aliyun.com/price/product#/gpdb/detail).AnalyticDB for PostgreSQL The new combination of the specifications and the number of compute nodes must meet the following requirements:

    -   The storage type remains unchanged. The new specifications of compute nodes must be higher or the same as the previous specifications.
    -   If the new specifications of compute nodes is the same as the previous specifications, the new number of compute nodes must be greater than the previous number.
    If the preceding requirements are met, you must also evaluate the actual data size and computing workload to choose an appropriate combination. For more information, see [How do I choose an AnalyticDB for PostgreSQL instance type?](/intl.en-US/FAQ/How do I choose an AnalyticDB for PostgreSQL instance type?.md)

    **Note:** When you upgrade your instance specifications, the duration of the process varies from 30 minutes to dozens of hours based on data size. The data size is determined by factors such as the numbers of tables, partitioned tables, and indexes, compressed data size, total data size, and instance specifications. During this process, the instance remains read-only to ensure data consistency and experiences two network interruption errors. We recommend that you take precautionary actions in advance. After the instance specifications are upgraded, the instance returns to the Running state. You can access the instance, and the engine version of the instance is automatically upgraded to the latest version.


After you complete the preceding steps, check the status of the instance. If the upgrade is complete, the instance enters the **Running** state. Otherwise, the instance remains in the **Upgrading** state.

## Add or remove coordinator nodes

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/gpdb/cn-hangzhou/list).
2.  In the top navigation bar, select the region where an instance resides. For example, select the China \(Hangzhou\) region.
3.  Find the instance and click **Add Master Node** or **Reduce Master Node** in the Action column.
4.  Specify a required number of coordinator nodes, read and select the AnalyticDB for PostgreSQL Terms of Service, and then click **Buy Now**.

After you complete the preceding steps, check the status of the instance. If the upgrade or downgrade is complete, the instance enters the **Running** state. Otherwise, the instance remains in the **Upgrading** or Downgrading state.

