# Migrate data from PostgreSQL to AnalyticDB for PostgreSQL {#concept_hnr_qmr_52b .concept}

The pgsql2pgsql tool supports migrating tables in AnalyticDB for PostgreSQL, Greenplum Database, PostgreSQL, or PPAS to AnalyticDB for PostgreSQL, Greenplum Database, PostgreSQL, or PPAS without storing the data separately.

## Features {#section_o2y_55r_gfb .section}

pgsql2pgsql supports the following features:

-   Full-database migration from PostgreSQL, PPAS, Greenplum Database, or AnalyticDB for PostgreSQL to PostgreSQL, PPAS, Greenplum Database, or AnalyticDB for PostgreSQL.

-   Full-database migration and incremental data migration from PostgreSQL or PPAS \(9.4 or later versions\) to PostgreSQL, or PPAS.


## Parameters configuration {#section_p2y_55r_gfb .section}

Modify the my.cfg configuration file, and configure the source and target database connection information.

-   The connection information of the source PostgreSQL database is shown as follows:

    **Note:** The user is preferably the corresponding database owner in the source PostgreSQL database connection information.

    ```
    [src.pgsql]
    connect_string = "host=192.168.1.1 dbname=test port=3432 user=test password=pgsql"
    ```

-   The connection information of the local temporary PostgreSQL database is shown as follows:

    ```
    [local.pgsql]
    connect_string = "host=192.168.1.2 dbname=test port=3432 user=test2 password=pgsql"
    ```

-   The connection information of the target PostgreSQL database is shown as follows:

    **Note:** You need to have the write permission on the target table of the target PostgreSQL database.

    ```
    [desc.pgsql]
    connect_string = "host=192.168.1.3 dbname=test port=3432 user=test3 password=pgsql"
    ```


**Note:** 

-   If you want to perform incremental data synchronization, the connected source database must have the permission to create replication slots.
-   PostgreSQL 9.4 and later versions support logic stream replication, so it supports the incremental migration if PostgreSQL serves as the data source. The kernel only supports logic stream replication after you enable the following kernel parameters.

    -   wal\_level = logical
    -   max\_wal\_senders = 6
    -   max\_replication\_slots = 6

## Use pgsql2pgsql { .section}

**Full-database migration**

Run the following command to perform a full-database migration:

```
./pgsql2pgsql
```

By default, the migration program migrates the table data of all the users in the corresponding PostgreSQL database to PostgreSQL.

## Status information query { .section}

Connect to the local temporary database, and you can view the status information in a single migration process. The information is stored in the db\_sync\_status table, including the start and end time of the full-database migration, the start time of the incremental data migration, and the data situation of incremental synchronization.

## Download and instructions { .section}

-   [Download the binary installer of rds\_dbsync](https://github.com/aliyun/rds_dbsync/releases)

-   [View the rds\_dbsync source code compilation instructions](https://github.com/aliyun/rds_dbsync/blob/master/doc/design.md)


