# Instance types {#concept_mp2_hbr_52b .concept}

HybridDB for PostgreSQL instances are categorized into the following type families:

-   High-performance: The name of this type family starts with `gpdb.group.**segsdx**`. This type family features a better I/O capability that secures higher performance.

-   High-capacity: The name of this type family starts with `gpdb.group.**seghdx**`. This type family features a larger and more affordable space to meet higher storage demands.


When selecting a type family, you only need to take the storage space and computing capability into consideration.

Besides, HybridDB for PostgreSQL supports OSS-based external table extensions and data compression in external storage through gzip. You can store data that is not required for real-time computation in an external storage to further reduce storage costs.

## Instance types {#section_jmw_hdl_gfb .section}

The **high-performance** instances offer the following types:

|Instance type|CPU|Memory|Disk|
|-------------|---|------|----|
|gpdb.group.segsdx1|1 Core|8 GB|80 GB SSD|
|gpdb.group.segsdx2|2 Cores|16 GB|160 GB SSD|
|gpdb.group.segsdx16|16 Cores|128 GB|1.28 TB SSD|

The **high-capacity** instances offer the following types:

|Instance type|CPU|Memory|Disk|
|-------------|---|------|----|
|gpdb.group.seghdx4|4 Cores|32 GB|2 TB HDD|
|gpdb.group.seghdx36|36 Cores|288 GB|18 TB HDD|

## Related information {#section_yhb_bff_xgb .section}

[How to select an instance type?](../../../../../reseller.en-US/FAQ/How to select an instance type?.md#)

