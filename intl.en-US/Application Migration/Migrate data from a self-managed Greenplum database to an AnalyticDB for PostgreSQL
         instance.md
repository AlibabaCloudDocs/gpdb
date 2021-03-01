# Migrate data from a self-managed Greenplum database to an AnalyticDB for PostgreSQL instance

AnalyticDB for PostgreSQL is fully compatible with the open source Greenplum database and supports smooth migration of applications. This topic describes how to migrate data from a self-managed Greenplum database to an AnalyticDB for PostgreSQL instance.

## Migration solution

AnalyticDB for PostgreSQL V6.0 is optimized by Alibaba Cloud based on the open source Greenplum 6.0 architecture. AnalyticDB for PostgreSQL V6.0 supports vector computing and transaction processing within a multi-master architecture for coordinator nodes and uses the same APIs as open source Greenplum. A complete migration migrates both applications and data. You can smoothly migrate your application and select from a variety of data migration solutions to migrate your database.

1.  Create an AnalyticDB for PostgreSQL instance. For more information about instance specifications, see [Instance specifications](/intl.en-US/Specifications and Pricing/Instance specifications.md).
2.  Migrate Data Definition Language \(DDL\) statements from self-managed Greenplum tables to the AnalyticDB for PostgreSQL instance based on pg\_dumpall to create table schemas.

    ```
    pg_dumpall --gp-syntax --schema-only > db_dump.sql
    ```

3.  Migrate your data by using one of the following solutions:

    -   Migrate full data table by table on the cloud by using the data integration feature of DataWorks.
    -   Export data from the self-managed Greenplum database, upload data to Elastic Compute Service \(ECS\), and use the COPY command of AnalyticDB for PostgreSQL to import data to the AnalyticDB for PostgreSQL instance. For more information, see [Use the \\COPY command](/intl.en-US/Data Access/Use the \COPY command.md).
    -   Export data from the self-managed Greenplum database, upload data to Object Storage Service \(OSS\), and use the OSS external table feature to concurrently import data to the AnalyticDB for PostgreSQL instance. For more information, see [Import or export OSS data by using OSS external tables](/intl.en-US/Data Access/Import or export OSS data by using OSS external tables.md).
    You can use DataWorks to easily synchronize data and use the COPY command or the OSS external table feature to quickly synchronize data. The OSS external table feature takes less time than the COPY command.


