# Migrate data using different solutions {#concept_h3n_mjt_vfb .concept}

HybridDB for PostgreSQL provides various migration solutions, which meet different business needs such as migrating data from user-created PostgreSQL databases to HybridDB for PostgreSQL databases and migrating data between cloud services. It enables you to smoothly migrate data between HybridDB for PostgreSQL databases and other databases without affecting your business. Implement data migration from databases such as HybridDB for PostgreSQL, Greenplum Database, PostgreSQL, PPAS, and Amazon Redshift to HybridDB for PostgreSQL.

Data migration scenarios of HybridDB for PostgreSQL and related operations are as follows:

|Operation|Migration Type|Scenario|
|---------|--------------|--------|
|[Parallel import from OSS or export to OSS](reseller.en-US/Quick Start/Import data/Parallel import from OSS or export to OSS.md#)|Migrate data to HybridDB for PostgreSQL/Migrate data between Alibaba Cloud products/Migrate data to user-created database|Use OSS external tables to import and export data between HybridDB for PostgreSQL and OSS.|
|[Use Data Integration to synchronize data](reseller.en-US/Quick Start/Import data/Use Data Integration to synchronize data.md#)|Migrate data between Alibaba Cloud products|Use Data Integration to import data into or export data from HybridDB for PostgreSQL.|
|[Migrate data from MySQL to HybridDB for PostgreSQL](reseller.en-US/Quick Start/Import data/Migrate data from MySQL to HybridDB for PostgreSQL.md#)|Migrate data to HybridDB for PostgreSQL|Use the mysql2pgsql tool to import tables from local MySQL into HybridDB for PostgreSQL.|
|[Migrate data from PostgreSQL to HybridDB for PostgreSQL](reseller.en-US/Quick Start/Import data/Migrate data from PostgreSQL to HybridDB for PostgreSQL.md#)|Migrate data to HybridDB for PostgreSQL/Migrate data between Alibaba Cloud products|Use the mysql2pgsql tool to import tables from HybridDB for PostgreSQL, Greenplum, PostgreSQL, or PPAS into HybridDB for PostgreSQL.|
|[Migrate data to HybridDB for PostgreSQL by using the Copy command](reseller.en-US/Quick Start/Import data/Migrate data to HybridDB for PostgreSQL by using the Copy command.md#)|Migrate data to HybridDB for PostgreSQL|Use the `\COPY` command to import data from local text files into HybridDB for PostgreSQL.|

