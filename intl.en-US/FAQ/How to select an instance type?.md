# How to select an instance type? {#concept_unc_jhg_v2b .concept}

When creating or upgrading an instance in AnalyticDB for PostgreSQL, you must specify the [group type and number of groups](../../../../intl.en-US/Product Introduction/Concepts.md#).

AnalyticDB for PostgreSQL supports multiple group types. For more information, see [Instance types](../../../../intl.en-US/Product Introduction/Instance types.md#). You can also specify multiple groups with the same type. We recommend that you consider the following factors when selecting your group type and quantity:

-   The storage space required
-   The computing capability required

## Storage space {#section_z2r_tnz_gfb .section}

The storage space of a group type and group quantity combo = the storage space of the group type \* the number of groups.

Note the following when determining the expected storage space:

-   Make the storage space slightly larger than the actual space evaluated. Because data processing can generate some logs and temporary files.
-   Select distribution keys appropriately and avoid data skews. Allocate the distribution keys evenly as much as possible, otherwise data skews may occur. Data skews can exhaust the storage space in a computing group and leave the storage space of other groups idle, resulting in low usage of storage resource.

## Computing capability {#section_bfr_tnz_gfb .section}

A group type and group quantity combo matches a specific computing capability. The computing capability is determined by the computing group type, number of CPUs, memory size, and the number of groups.

AnalyticDB for PostgreSQL has two group types available:

-   High-performance group: the group type name starts with gpdb.group.**segsdx**. This type features a better I/O capability that secures higher performance.

-   High-capacity group: the group type name starts with gpdb.group.**seghdx**. This type features a larger and more affordable space to meet higher storage demands.


Three other factors also contribute to linear increase of performance. Take a high-performance type for example. The SQL execution durations in the following use cases are close to each other:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16871/155781953212980_en-US.png)

In addition, you also need to take the price into account.

