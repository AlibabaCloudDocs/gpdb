# How do I view the distribution key and partitions of a table?

## View the distribution key of a table

-   Run the following command in psql to view the distribution key of a table:

    ```
    \d <table_name>
    ```

-   Execute the following SQL statement to view the distribution key of a table:

    ```
    # AnalyticDB for PostgreSQL V4.3
    
    SELECT attname FROM pg_attribute WHERE attrelid='<schema_name>.<table_name>'::regclass and attnum in (SELECT unnest(attrnums) FROM pg_catalog.gp_distribution_policy t WHERE localoid='<schema_name>.<table_name>'::regclass);
    
    # AnalyticDB for PostgreSQL V6.0
    
    SELECT attname FROM pg_attribute WHERE attrelid='<schema_name>.<table_name>'::regclass and attnum in (SELECT unnest(distkey) FROM pg_catalog.gp_distribution_policy t WHERE localoid='<schema_name>.<table_name>'::regclass);
    ```


**Note:**

-   `<schema_name>`: schema name of the table.
-   `<table_name>`: The name of the table.

## View the partitions of a table

-   Run the following command in psql to view the partitions of a table:

    ```
    \d+ <table_name>
    ```

-   Execute the following SQL statement to view the partitions of a table:

    ```
    SELECT pg_get_partition_def('<schema_name>.<table_name>'::regclass,true);
    ```


**Note:**

-   `<schema_name>`: schema name of the table.
-   `<table_name>`: The name of the table.

