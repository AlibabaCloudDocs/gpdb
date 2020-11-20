# Upgrade the instance configuration

During the usage of AnalyticDB for PostgreSQL, some computing resources \(such as CPU, disk space, memory, and the number of data processing nodes\) may become the bottleneck hindering further growth of data processing speed as the data size and computing workload surge dynamically.

AnalyticDB for PostgreSQL provides online upgrading of the instance configuration to support dynamic expansion of instances, but downgrading instance configuration is not supported. This document describes how to upgrade the instance configuration.

## View the instance configuration

AnalyticDB for PostgreSQL instance configuration includes [group types](/intl.en-US/Product Introduction/Terms.md) and [number of groups](/intl.en-US/Product Introduction/Terms.md). For more information, see [Instance types](/intl.en-US/Specifications and Pricing/Instance specifications.md).

Follow these steps to view the current instance configuration.

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com/).
2.  Log on the [AnalyticDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
3.  Select the region of the instance. For example, China East 1.

4.  Click on the instance name in the list to enter the basic information page.


On the Basic Information page, displays the instance class, instance details, instance groups, and the total computing resources.

AnalyticDB for PostgreSQL currently has two instance classes available:

-   High-performance group: the group type name starts with gpdb.group.**segsdx**. This type features a better I/O capability that secures higher performance.
-   High-capacity group: the group type name starts with gpdb.group.**seghdx**. This type features a larger and more affordable space to meet higher storage demands.

## Upgrade the instance configuration

Follow these steps to upgrade the instance configuration.

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com/).
2.  Log on the [AnalyticDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
3.  Select the region of the instance. For example, China East 1.

4.  Click the **Upgrade** on the right side of the target instance.

5.  On the Configuration upgrade page, select the expected group type and group quantity, and then click Activate.

    AnalyticDB for PostgreSQL supports a diversity of group type and group quantity combos. For more information, see [Configuration combo lookup table](https://www.alibabacloud.com/product/hybriddb-postgresql/pricing?spm=a3c0i.7938564.8215766810.44.e442441eBIP1L7). A new group type and group quantity combo must meet the following constraints:

    -   The new and old computing groups must be of the same instance class, and the new group configuration must be equal to or higher than the old one.
    -   If the new group configuration is equal to the old group type, the new group quantity must be larger than the old one.
    Apart from the preceding constraints, you must also evaluate the data size and computing workload of your service to select a proper group type and quantity combo. For more information, see [How to select instance type](/intl.en-US/FAQ/How do I choose an AnalyticDB for PostgreSQL instance type?.md).

    **Note:**

    The instance upgrading process may take 30 minutes to three hours depending on your data size. Your instance remains read-only in this process to ensure data consistency, and experiences two transient disconnections. Be prepared in advance. When the upgrading process is completed, the instance resumes the running state. You can visit the database normally and the instance’s database kernel is automatically upgraded to the latest version.


After the preceding operations are done, you can return to the console to check the running state of the target instance. When the upgrading process is completed, the instance state becomes **Running**. Otherwise, it is in **Upgrading**.

