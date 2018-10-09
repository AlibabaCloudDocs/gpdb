# Upgrade the instance configuration {#concept_fks_vkz_52b .concept}

During the usage of HybridDB for PostgreSQL, some computing resources \(such as CPU, disk space, memory, and the number of data processing nodes\) may become the bottleneck hindering further growth of data processing speed as the data size and computing workload surge dynamically.

HybridDB for PostgreSQL provides online upgrading of the instance configuration to support dynamic expansion of instances, but downgrading instance configuration is not supported. This document describes how to upgrade the instance configuration.

## View the instance configuration { .section}

HybridDB for PostgreSQL instance configuration includes [group types](../../../../reseller.en-US/Product Introduction/Concepts.md#) and [number of groups](../../../../reseller.en-US/Product Introduction/Concepts.md#). For more information, see [Instance types](../../../../reseller.en-US/Product Introduction/Instance types.md#).

Follow these steps to view the current instance configuration.

1.  Log on the [HybridDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
2.  Select the region of the instance. For example, China East 1.

3.  Click **Manage** on the right side of the target instance.


On the Basic Information page, the Configuration Information section displays the instance class, instance details, instance groups, and the total computing resources.

HybridDB for PostgreSQL currently has two instance classes available:

-   High-performance group: the group type name starts with gpdb.group.**segsdx**. This type features a better I/O capability that secures higher performance.
-   High-capacity group: the group type name starts with gpdb.group.**seghdx**. This type features a larger and more affordable space to meet higher storage demands.

## Upgrade the instance configuration { .section}

Follow these steps to upgrade the instance configuration.

1.  Log on the [HybridDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
2.  Select the region of the instance. For example, China East 1.

3.  Click the **Upgrade** on the right side of the target instance.

4.  On the Configuration upgrade page, select the expected group type and group quantity, and then click Activate.

    HybridDB for PostgreSQL supports a diversity of group type and group quantity combos. For more information, see [Configuration combo lookup table](https://www.aliyun.com/price/product#/gpdb/detail). A new group type and group quantity combo must meet the following constraints:

    -   The new and old computing groups must be of the same instance class, and the new group configuration must be equal to or higher than the old one.
    -   If the new group configuration is equal to the old group type, the new group quantity must be larger than the old one.
    Apart from the preceding constraints, you must also evaluate the data size and computing workload of your service to select a proper group type and quantity combo. For more information, see [How to select instance type](../../../../reseller.en-US/FAQ/How to select instance type?.md#).

    **Note:** 

    The instance upgrading process may take 30 minutes to three hours depending on your data size. Your instance remains read-only in this process to ensure data consistency, and experiences two transient disconnections. Be prepared in advance. When the upgrading process is completed, the instance resumes the running state. You can visit the database normally and the instance’s database kernel is automatically upgraded to the latest version.


After the preceding operations are done, you can return to the console to check the running state of the target instance. When the upgrading process is completed, the instance state becomes **Running**. Otherwise, it is in **Upgrading**.

