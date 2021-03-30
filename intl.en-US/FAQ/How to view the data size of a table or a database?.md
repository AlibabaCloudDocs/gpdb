# How to view the data size of a table or a database?

Suppose that the table schema is `<schemaname>` and the table name is `<tablename>`.

-   Run the following command to query the **total size of a table** \(unit: MB, including the table index and data\):

    ```
    select pg_size_pretty(pg_total_relation_size('<schemaname>.<tablename>'));
    ```

-   Run the following command to query the **data size of a table** \(unit: MB, excluding the index\):

    ```
    select pg_size_pretty(pg_relation_size('<schemaname>.<tablename>'));
    ```

-   Run the following command to query the **total size of all the partitions** in a partition table \(unit: MB, including the table index and data\):

    ```
    select schemaname,tablename,round(sum(pg_total_relation_size(schemaname || '.' || partitiontablename))/1024/1024) "MB" from pg_partitions where schemaname='<schemaname>' and tablename='<tablename>' group by 1,2;
    ```

-   Run the following command to query the **total size of all the tables under a schema** \(unit: MB, including the index and data\):

    ```
    select schemaname ,round(sum(pg_total_relation_size(schemaname||'.'||tablename))/1024/1024) "Size_MB" from pg_tables where schemaname='<schemaname>' group by 1;
    ```

-   Run the following command to query **the size of each database** \(unit: MB\):

    ```
    select datname,pg_size_pretty(pg_database_size(datname)) from pg_database;
    ```


