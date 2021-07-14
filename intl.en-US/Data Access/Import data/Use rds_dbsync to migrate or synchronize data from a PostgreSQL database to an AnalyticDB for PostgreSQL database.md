# Use rds\_dbsync to migrate or synchronize data from a PostgreSQL database to an AnalyticDB for PostgreSQL database

rds\_dbsync is an open source tool. You can use the pgsql2pgsql function of this tool to migrate data of tables from an AnalyticDB for PostgreSQL database, Greenplum Database, PostgreSQL database, or PPAS database to another AnalyticDB for PostgreSQL database, Greenplum Database, PostgreSQL database, or PPAS database.

## Features of pgsql2pgsql

pgsql2pgsql provides the following features:

-   Full data migration from a PostgreSQL database, PPAS database, Greenplum Database, or AnalyticDB for PostgreSQL database to another PostgreSQL database, PPAS database, Greenplum Database, or AnalyticDB for PostgreSQL database

-   Full and incremental data migration from a PostgreSQL or PPAS database \(9.4 or later\) to another PostgreSQL or PPAS database


## Parameter configuration

Modify the postgresql.conf file and configure the connection information of both the source and destination databases.

-   The connection information of the source database is as follows:

    **Note:** In the connection information of the source database, we recommend that you set the user to the owner of the source database.

    ```
    [src.pgsql]
    connect_string = "host=192.168.1.1 dbname=test port=3432 user=test password=pgsql"
    ```

-   The connection information of the local temporary database is as follows:

    ```
    [local.pgsql]
    connect_string = "host=192.168.1.2 dbname=test port=3432 user=test2 password=pgsql"
    ```

-   The connection information of the destination database is as follows:

    **Note:** You must have write permission on the destination table in the destination database.

    ```
    [desc.pgsql]
    connect_string = "host=192.168.1.3 dbname=test port=3432 user=test3 password=pgsql"
    ```


**Note:**

-   If you want to synchronize incremental data, you must have permission to create replication slots in the source database.
-   PostgreSQL 9.4 and later support logic flow replication. Therefore, source databases of these versions support incremental data migration. The kernel supports logic flow replication only after you configure the following parameters:

    ```
    wal_level = logical
    max_wal_senders = 6
    max_replication_slots = 6
    ```


## pgsql2pgsql usage

**Full database migration**

Run the following command to perform a full database migration:

```
./pgsql2pgsql
```

By default, the migration program migrates all user table data from the source database to the destination database.

**Status query**

You can connect to the local temporary database and view the status of a migration task. The status information is stored in the db\_sync\_status table and includes the start and end time of full data migration, the start time of incremental data migration, and the status of incremental data synchronization.

## Download and instructions

-   To download the binary installation package of [rds\_dbsync](https://github.com/aliyun/rds_dbsync/releases).
-   To view the instructions on source code compilation of [rds\_dbsync](https://github.com/aliyun/rds_dbsync/blob/master/doc/design.md).

