# Use rds\_dbsync to migrate or synchronize data from a MySQL database to an AnalyticDB for PostgreSQL database

## rds\_dbsync

rds\_dbsync is an open source tool used to migrate or synchronize data. You can use the mysql2pgsql function of this tool to migrate data from a MySQL database to an AnalyticDB for PostgreSQL database, Greenplum Database, PostgreSQL database, or PPAS database without having to store data. This tool connects to both the source MySQL database and the destination database, retrieves the data you want to export from the source MySQL database, and then uses a COPY statement to import the data to the destination database. This tool supports simultaneous data import over multiple threads. Each worker thread imports data of some database tables.

## Parameter configuration

Modify the my.cfg file and configure the connection information of both the source and destination databases.

-   The connection information of the source MySQL database is as follows:

    **Note:** You must have read permission on all user tables in the source MySQL database.

    ```
    [src.mysql]
    host = "192.168.1.1"
    port = "3306"
    user = "test"
    password = "test"
    db = "test"
    encodingdir = "share"
    encoding = "utf8"
    ```

-   The connection information of the destination database \(such as PostgreSQL, PPAS, or AnalyticDB for PostgreSQL\) is as follows:

    **Note:** You must have write permission on the destination table in the destination database.

    ```
    [desc.pgsql]
    connect_string = "host=192.168.1.2 dbname=test port=3432  user=test password=pgsql"
    ```


## mysql2pgsql usage

The usage of mysql2pgsql is described as follows:

```
./mysql2pgsql -l <tables_list_file> -d -n -j <number of threads> -s <schema of target table>
```

Parameters

-   -l: optional. This parameter specifies a text file that contains tables whose data needs to be synchronized. If you do not specify this parameter, the data of all tables within the source database is synchronized. `<tables_list_file>` specifies the name of a file that contains a collection of tables to be synchronized and conditions for table queries. The content format is as follows:

    ```
    table1 : select * from table_big where column1 < '2016-08-05'
    table2 : 
    table3
    table4: select column1, column2 from tableX where column1 ! = 10
    table5: select * from table_big where column1 >= '2016-08-05'
    ```

-   -d: optional. This parameter indicates that only the DDL statement used to create the destination table is generated and data is not synchronized.

-   -n: optional. This parameter indicates that the definitions of table partitions are not included in the DDL statement. It must be used with the -d parameter.

-   -j: optional. This parameter indicates the number of threads concurrently used to synchronize data. If you do not specify this parameter, five concurrent threads are used.

-   -s: optional. This parameter indicates the schema of the destination table. Set it to public.


## Typical usage

**Full database migration**

Follow these steps:

1.  Run the following command to obtain the DDL statement used to create a table in the destination database:

    ```
    ./mysql2pgsql -d
    ```

2.  Use the DDL statement to create a table in the destination database, and then specify a distribution key for the table.
3.  Run the following command to synchronize data:

    ```
    ./mysql2pgsql
    ```

    This command migrates the data of all tables from the source MySQL database to the destination database. By default, five concurrent threads are used to concurrently read and import data.


**Partial table migration**

1.  Create a file named tab\_list.txt and add the following content to it:

    ```
    t1
    t2 : select * from t2 where c1 > 138888
    ```

2.  Run the following command to synchronize data of the t1 and t2 tables \(note that for the t2 table, only data that meets the "c1 \> 138888" criterion is migrated\):

    ```
    ./mysql2pgsql -l tab_list.txt
    ```


## Download and instructions

-   To download the binary installation package of [mysql2pgsql](https://github.com/aliyun/rds_dbsync/releases).

-   To view the instructions on source code compilation of [mysql2pgsql](https://github.com/aliyun/rds_dbsync/blob/master/README.md).


