# Check for data skew

You must check for data skew on a regular basis during the maintenance period of an instance to prevent the instance from being locked due to excessive data on some compute nodes.

You can use the following methods to check for and identify data skew. In the examples, SQL statements are used.

-   For a single table or database, check the space usage on each compute node to determine whether data is skewed.
    -   Execute the following statement to determine whether the data in a database is skewed:

        ```
        select pg_size_pretty(pg_database_size('dbname')) from gp_dist_random('gp_id');
        ```

        The execution result of the preceding statement shows the space occupied by the dbname database on each compute node. If the space of the database on one or more compute nodes is significantly greater than that on other compute nodes, the data in this database is skewed.

    -   Execute the following statement to determine whether the data in a table is skewed:

        ```
        select pg_size_pretty(pg_relation_size('tblname')) from gp_dist_random('gp_id');
        ```

        The execution result of the preceding statement shows the space occupied by the tblname table on each compute node. If the space of the table on one or more compute nodes is significantly greater than that on other compute nodes, the data in this table is skewed. You must modify the distribution key to redistribute the data.

-   Use system views to determine whether data is skewed.
    -   Execute the following statement to check whether the storage space is skewed, which is similar to the preceding method that is used to check for space usage:

        ```
        select * from gp_toolkit.gp_skew_coefficients;
        ```

        This view allows you to check the data volume of rows in a table. The larger the table, the more time it takes for the check to complete.

    -   Use the gp\_toolkit.gp\_skew\_idle\_fractions view to calculate the percentage of idle system resources during a table scan to determine whether the data is skewed:

        ```
        select * from gp_toolkit.gp_skew_idle_fractions;
        ```


For more information, see [Distribution and Skew](https://gpdb.docs.pivotal.io/43180/ref_guide/gp_toolkit.html#topic49).

