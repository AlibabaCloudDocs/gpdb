# Migrate data using different solutions {#concept_h3n_mjt_vfb .concept}

AnalyticDB for PostgreSQL provides various migration solutions, which meet different business needs such as migrating data from on-premises PostgreSQL databases to AnalyticDB for PostgreSQL databases and migrating data between cloud services. It enables you to smoothly migrate data between AnalyticDB for PostgreSQL databases and other databases without affecting your business. Implement data migration from databases such as AnalyticDB for PostgreSQL, Greenplum Database, PostgreSQL, PPAS, and Amazon Redshift to AnalyticDB for PostgreSQL.

Data migration scenarios of AnalyticDB for PostgreSQL and related operations are as follows:

|Operation|Migration Type|Scenario|
|---------|--------------|--------|
|[Parallel import from OSS or export to OSS](intl.en-US/Quick Start/Import data/Parallel import from OSS or export to OSS.md#)|Migrate data to AnalyticDB for PostgreSQL/Migrate data between Alibaba Cloud products/Migrate data to on-premises database|Use OSS external tables to import and export data between AnalyticDB for PostgreSQL and OSS.|
|[Use Data Integration to synchronize data](intl.en-US/Quick Start/Import data/Use Data Integration to synchronize data.md#)|Migrate data between Alibaba Cloud products|Use Data Integration to import data into or export data from AnalyticDB for PostgreSQL.|
|[Migrate data from MySQL to HybridDB for PostgreSQL](intl.en-US/Quick Start/Import data/Migrate data from MySQL to AnalyticDB for PostgreSQL.md#)|Migrate data to AnalyticDB for PostgreSQL|Use the mysql2pgsql tool to import tables from local MySQL into AnalyticDB for PostgreSQL.|
|[Migrate data from PostgreSQL to HybridDB for PostgreSQL](intl.en-US/Quick Start/Import data/Migrate data from PostgreSQL to AnalyticDB for PostgreSQL.md#)|Migrate data to AnalyticDB for PostgreSQL/Migrate data between Alibaba Cloud products|Use the mysql2pgsql tool to import tables from AnalyticDB for PostgreSQL, Greenplum, PostgreSQL, or PPAS into AnalyticDB for PostgreSQL.|
|[Migrate data to HybridDB for PostgreSQL by using the Copy command](intl.en-US/Quick Start/Import data/Migrate data to AnalyticDB for PostgreSQL by using the Copy command.md#)|Migrate data to AnalyticDB for PostgreSQL|Use the `\COPY` command to import data from local text files into AnalyticDB for PostgreSQL.|

