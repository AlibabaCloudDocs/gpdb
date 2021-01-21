# Instance types

Currently, AnalyticDB for PostgreSQL provides two instance type families.

-   High-performance: The type names start with gpdb.group.**segsdx**. This instance type family offers better I/O performance.

-   High-capacity: The type names start with gpdb.group.**seghdx**. This instance type family offers larger and more cost-effective storage space.


We recommend that you choose the appropriate instance type based on the storage space and compute capacity as required.

AnalyticDB for PostgreSQL supports OSS-based external tables and data compression in external storage through gzip. You can store the data that are not involved in real-time computing to external storage to save costs.

## Type specification

The specification for high-performance instances is shown in the table as follows.

|High-performance type family|CPU|Memory|Storage space|
|----------------------------|---|------|-------------|
|gpdb.group.segsdx1|1 Core|8 GB|80 GB SSD|
|gpdb.group.segsdx2|2 cores|16 GB|160 GB SSD|
|gpdb.group.segsdx16|16 Cores|128 GB|1.28 TB SSD|

The specification for high-capacity instances is shown in the following table.

|High-capacity type family|CPU|Memory|Storage space|
|-------------------------|---|------|-------------|
|gpdb.group.seghdx4|4 Cores|32 GB|2 TB HDD|
|gpdb.group.seghdx36|36 Cores|288 GB|18 TB HDD|

