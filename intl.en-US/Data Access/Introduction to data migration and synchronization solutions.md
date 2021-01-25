# Introduction to data migration and synchronization solutions

This topic introduces the data migration and synchronization solutions supported by AnalyticDB for PostgreSQL. You can migrate or synchronize data between AnalyticDB for PostgreSQL databases and other types of data sources without interruptions to your business. Compatible data sources include ApsaraDB RDS for MySQL, Apsara PolarDB for MySQL, ApsaraDB RDS for PostgreSQL, ApsaraDB RDS for PPAS, MaxCompute, Greenplum Database, user-created MySQL databases, user-created PostgreSQL databases, and Amazon Redshift. AnalyticDB for PostgreSQL supports both the following Alibaba Cloud solutions and data synchronization products from third parties such as DSG.

The following table lists data migration and synchronization scenarios supported by AnalyticDB for PostgreSQL and related operations.

|Operation|Type|Scenario|
|---------|----|--------|
|[Import or export OSS data by using OSS external tables](/intl.en-US/Data Access/Import or export OSS data by using OSS external tables.md)|Data migration|Use OSS external tables to migrate data between an AnalyticDB for PostgreSQL instance and OSS.|
|[Use Data Integration to migrate and batch synchronize data](/intl.en-US/Data Access/Use Data Integration to migrate and batch synchronize data.md)|Data synchronization or migration|Use Data Integration to migrate or synchronize data between an AnalyticDB for PostgreSQL instance and a heterogeneous data source within a few minutes.|
|[Use the \\COPY command](/intl.en-US/Data Access/Use the \COPY command.md)|Data migration|Use the `\COPY` command to migrate data in local text files to an AnalyticDB for PostgreSQL instance.|
|[Use rds\_dbsync to migrate or synchronize data from a MySQL database to an AnalyticDB for PostgreSQL database](/intl.en-US/Data Access/Import data/Use rds_dbsync to migrate or synchronize data from a MySQL database to an AnalyticDB for PostgreSQL database.md)|Data synchronization or migration|Use the mysql2pgsql function of the rds\_dbsync tool to synchronize table data from an on-premises MySQL database to an AnalyticDB for PostgreSQL database.|
|[Use rds\_dbsync to migrate or synchronize data from a PostgreSQL database to an AnalyticDB for PostgreSQL database](/intl.en-US/Data Access/Import data/Use rds_dbsync to migrate or synchronize data from a PostgreSQL database to an AnalyticDB for PostgreSQL database.md)|Data synchronization or migration|Use the pgsql2pgsql function of the rds\_dbsync tool to synchronize table data from an AnalyticDB for PostgreSQL database, Greenplum Database, PostgreSQL database, or PPAS database to another AnalyticDB for PostgreSQL database.|

1.  Data migration: Data in a database instance or local data is migrated to an AnalyticDB for PostgreSQL instance.
2.  Data synchronization: Data in a database is synchronized to an AnalyticDB for PostgreSQL instance in real time.

