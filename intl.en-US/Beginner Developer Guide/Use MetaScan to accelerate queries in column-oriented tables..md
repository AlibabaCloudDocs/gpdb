# Use MetaScan to accelerate queries in column-oriented tables.

**Note:**

-   Only AnalyticDB for PostgreSQL V4.3 supports the MetaScan feature.
-   If you use AnalyticDB for PostgreSQL V6.0 or other versions, ignore the MetaScan feature.

AnalyticDB for PostgreSQL supports column-oriented storage and features a high data compression ratio and improved query performance. However, AnalyticDB for PostgreSQL still must read data from the entire column or create a B-tree index when it processes a query that filters most of the data. B-tree indexes also have two potential issues. One is that serious data bloat may occur because indexes are not compressed. The other is that indexes may fail and the cost is higher than sequential scans if the result sets are large. To address the issues, AnalyticDB for PostgreSQL provides the MetaScan feature, which offers excellent filter performance and occupies minimal storage space.

## Enable MetaScan

The MetaScan feature is controlled by the following parameters:

-   RDS\_ENABLE\_CS\_ENHANCEMENT

    Specifies whether to collect metadata. If it is set to ON, metadata collection is enabled. If it is set to OFF, metadata collection is disabled. By default, this parameter is set to ON. In this situation, if the data in column-oriented tables changes, the system automatically collects the metadata of the tables. This parameter is an instance-level parameter. If you want to change its setting, contact technical support by submitting a ticket.

-   RDS\_ENABLE\_COLUMN\_META\_SCAN

    Specifies whether to enable the MetaScan feature for a query. If it is set to ON, the feature is enabled for a query. If it is set to OFF, the feature is disabled for a query. By default, this parameter is set to OFF. This parameter is a session-level parameter. You can view or change its setting by executing the following SQL statements:

    ```
    --- Check whether MetaScan is enabled.
    Show RDS_ENABLE_COLUMN_META_SCAN;
    --- Disable MetaScan.
    Set RDS_ENABLE_COLUMN_META_SCAN = OFF;
    --- Enable MetaScan.
    Set RDS_ENABLE_COLUMN_META_SCAN = ON;
    ```


**Note:** After you set RDS\_ENABLE\_CS\_ENHANCEMENT to OFF, metadata collection is stopped for all column-oriented tables. If you want to use the MetaScan feature after setting RDS\_ENABLE\_CS\_ENHANCEMENT to ON, execute the ALTER TABLE table\_name SET WITH \(REORGANIZE=TRUE\) statement to collect the metadata of the tables again.

## Check whether MetaScan is used for a query

You can use the EXPLAIN statement to check whether a SELECT statement uses MetaScan.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1166177951/p65633.png)

The "Append-only Columnar Meta Scan" node displayed is the MetaScan node. This indicates that the MetaScan feature is used in the query.

## Data types and operators supported by MetaScan

MetaScan supports the following data types:

-   INT2/INT4/INT8
-   FLOAT4/FLOAT8
-   TIME/TIMETZ/TIMESTAMP/TIMESTAMPTZ
-   VARCHAR/TEXT/BPCHAR
-   CASH

MetaScan supports the following operators:

-   Equal to \(=\), less than \(<\), less than or equal to \(≤\), greater than \(\>\), and greater than or equal to \(≥\)
-   Logical AND operator

## Update existing instances to use MetaScan

If you want to use MetaScan for existing instances, perform the following operations:

-   Update the kernel of your AnalyticDB for PostgreSQL instance

    In the AnalyticDB for PostgreSQL console, find the instance whose kernel you want to update and click its ID. In the upper-right corner of the page that appears, click Upgrade Minor Version.

    ![14207702](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1107249161/p268434.png)

-   Update the metadata of tables

    Update the metadata of tables to its latest version after you update the kernel. If RDS\_ENABLE\_CS\_ENHANCEMENT is set to OFF, contact technical support by submitting a ticket to update the metadata. Otherwise, perform the following steps:

    1.  Use the administrator account to create an update function.

        ```
        CREATE OR REPLACE FUNCTION UPGRADE_AOCS_TABLE_META(tname TEXT) RETURNS BOOL AS $$
        DECLARE
            tcount INT := 0;
        BEGIN
            -- CHECK TABLE NAME
            EXECUTE 'SELECT COUNT(1) FROM PG_AOCSMETA WHERE RELID = ''' || tname || '''::REGCLASS' INTO tcount;
            IF tcount IS NOT NULL THEN
                IF tcount > 1 THEN
                    RAISE EXCEPTION 'found more than one table of name %', tname;
                ELSEIF tcount = 0 THEN
                    RAISE EXCEPTION 'not found target table in pg_aocsmeta, table name:%', tname;
                END IF;
            END IF;
        
            EXECUTE 'ALTER TABLE ' || tname || ' SET WITH(REORGANIZE=TRUE)';
            RETURN TRUE;
        END;
        $$  LANGUAGE PLPGSQL;
        ```

    2.  Execute the following SQL statement to update the target table as the administrator or table owner:

        ```
        SELECT UPGRADE_AOCS_TABLE_META(table_name);
        ```

    3.  Execute the following SQL statement to check the metadata version:

        ```
        SELECT version = 4 AS is_latest_version  FROM pg_appendonly WHERE relid = 'test'::REGCLASS
        ```


## Enhance MetaScan performance by using sort keys

The sort key feature of AnalyticDB for PostgreSQL sorts data in tables by a specific column. Using MetaScan together with the sort key feature improves the performance of MetaScan. Column-oriented tables store data in blocks. MetaScan uses metadata to check whether blocks meet query criteria and skips blocks that do not meet query criteria. This reduces I/Os and improves scanning performance. If data in filtered columns is distributed in all blocks, all blocks need to be scanned even though most required data is filtered. If you create a sort key for each filtered column, the same data in a column is combined into several consecutive blocks. This way, MetaScan can filter out blocks that do not meet criteria to improve scanning performance.

For more information about how to create a sort key, see [Use sort keys in column-oriented tables](/intl.en-US/Beginner Developer Guide/Use sort keys in column-oriented tables.md).

## Limits

MetaScan is incompatible with the ORCA optimizer and is unavailable if the ORCA optimizer is used. You can execute the following statement to check whether the ORCA optimizer is used:

```
SHOW OPTIMIZER;
```

If ON is returned, the ORCA optimizer is used. For more information about the ORCA optimizer, see [Choose a query optimizer](/intl.en-US/Best Practices/Optimize query performance.md).

