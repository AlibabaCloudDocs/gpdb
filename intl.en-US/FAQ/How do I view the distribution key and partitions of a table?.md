# How do I view the distribution key and partitions of a table?

## View the distribution key of a table

-   Run the following command in psql to view the distribution key of a table:

    ```
    \d tblname
    ```

-   Execute the following SQL statement to view the distribution key of a table:

    ```
    AnalyticDB for PostgreSQL V4.3
    
    SELECT attname FROM pg_attribute WHERE attrelid='schemaname.tblname'::regclass and attnum in (SELECT unnest(attrnums) FROM pg_catalog.gp_distribution_policy t WHERE localoid='schemaname.tblname'::regclass);
    
    AnalyticDB for PostgreSQL V6.0
    
    SELECT attname FROM pg_attribute WHERE attrelid='schemaname.tblname'::regclass and attnum in (SELECT unnest(distkey) FROM pg_catalog.gp_distribution_policy t WHERE localoid='schemaname.tblname'::regclass);
    ```

    **Note:**

    -   The schemaname parameter specifies the schema name of the table.
    -   The tblname parameter specifies the name of the table.

## View the partitions of a table

-   Run the following command in psql to view the partitions of a table:

    ```
    \d+ tblname
    ```

-   Execute the following SQL statement to view the partitions of a table:

    ```
    SELECT pg_get_partition_def('schemaname.tblname'::regclass,true);
    ```

    -   The schemaname parameter specifies the schema name of the table.
    -   The tblname parameter specifies the name of the table.

