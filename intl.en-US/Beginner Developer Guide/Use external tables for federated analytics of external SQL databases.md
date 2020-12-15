# Use external tables for federated analytics of external SQL databases

AnalyticDB for PostgreSQL allows you to use Java Database Connectivity \(JDBC\) to query data from external data sources including Oracle, PostgreSQL, and MySQL databases.

**Note:**

-   This feature is available only for AnalyticDB for PostgreSQL instances in elastic storage mode. Moreover, the AnalyticDB for PostgreSQL instances must be in the same VPC as the external data sources.
-   This feature is unavailable for the existing AnalyticDB for PostgreSQL instances in elastic storage mode that were created before September 6, 2020. This is because the existing instances cannot be connected to external databases over different network architectures. If you want to use this feature for the existing instances, we recommend that you contact Alibaba Cloud technical support to apply for new instances and migrate data.

![Federated analytics of external databases](../images/p166373.png)

## Configure a JDBC server

JDBC server configurations vary based on user requirements. To use JDBC to query data from external data sources for federated analytics, you must [submit a ticket](https://workorder.console.aliyun.com/console.htm#/ticket/add?productCode=gpdb) to request technical support to configure the specified JDBC server. The following table describes the information that is required to query data from external data sources.

|External data source|Required information|
|--------------------|--------------------|
|SQL Database\(PostgreSQL, Mysql, Oracle\)|The URL that the JDBC driver uses to connect to the database, the database username, and the database password.|

## Create an external table for federated analytics of an external SQL database

-   **Create an extension**

    ```
    CREATE extension pxf; 
    ```


-   **Create an external table**

    ```
    CREATE EXTERNAL TABLE <table_name>
            ( <column_name> <data_type> [, ...] | LIKE <other_table> )
    LOCATION('pxf://<path-to-data>? PROFILE[&<custom-option>=<value>[...]] &[SERVER=value]')
    FORMAT '[TEXT|CSV|CUSTOM]' (<formatting-properties>);
    ```

    For more information about the syntax that is used to create an external table, see [CREATE EXTERNAL TABLE](/intl.en-US/Beginner Developer Guide/SQL reference.md).

    |Field|Description|
    |-----|-----------|
    |path-to-data|The path to the data to be queried. Example: public.test\_a.|
    |PROFILE \[&<custom-option\>=<value\>\[...\]\]|The method to query external data.To use JDBC to query data from external databases, set PROFILE to Jdbc. |
    |FORMAT '\[TEXT\|CSV\|CUSTOM\]'|The format of the file to be queried.|
    |formatting-properties|The formatting option of the specified data format. Set this field to formatter or delimiter.    -   Valid values when you set FORMAT to CUSTOM:
        -   formatter='pxfwritable\_import'
        -   formatter='pxfwritable\_export'
    -   Valid values when you set FORMAT to TEXT or CSV:

        -   delimiter=E'\\t'
        -   delimiter ':'
**Note:** E is used to escape possible special characters. |
    |SERVER|The location of the configuration file of the server, which is provided by the technical support of AnalyticDB for PostgreSQL.|


## Example: Query data from an external PostgreSQL database

After the JDBC server is configured, you can execute the following SQL statements in AnalyticDB for PostgreSQL to create an external table and query data from the specified external PostgreSQL database:

```
postgres=# CREATE EXTERNAL TABLE pxf_ext_pg(a int, b int)
  LOCATION ('pxf://public.t? PROFILE=Jdbc&SERVER=postgresql')
FORMAT 'CUSTOM' (FORMATTER='pxfwritable_import')
ENCODING 'UTF8';

postgres=# select * from pxf_ext_pg;
   a   |   b
-------+-------
     1 |     2
     2 |     4
     3 |     6
     4 |     8
     5 |    10
     6 |    12
     7 |    14
--more--      
```

The following list describes the fields in the LOCATION parameter:

-   pxf://: the PXF protocol, which cannot be modified.
-   public.t: the t table in the public database to be queried.
-   PROFILE=Jdbc: the method that is used to query external data. In this example, JDBC is used to query the external data source.
-   SERVER=postgresql: the location of the configuration file of the server, which is provided by the technical support of AnalyticDB for PostgreSQL after you submit the ticket. In this example, SERVER=postgresql indicates that the file located in the PXF\_SERVER/postgresql/ directory is used to configure the server.
-   FORMAT 'CUSTOM' \(FORMATTER='pxfwritable\_import'\): the formatting option of the external data source. In this example, FORMAT is set to CUSTOM and FORMATTER is set to pxfwritable\_import.

```
CREATE EXTERNAL TABLE pxf_ext_test_a( id int,name varchar)
  LOCATION ('pxf://public.test_a? PROFILE=Jdbc&server=postgresql')
FORMAT 'CUSTOM' (FORMATTER='pxfwritable_import')
ENCODING 'UTF8';
```

## Supported data types

-   INTEGER, BIGINT, SMALLINT
-   REAL, FLOAT8
-   NUMERIC
-   BOOLEAN
-   VARCHAR, BPCHAR, TEXT
-   DATE
-   TIMESTAMP
-   BYTEA

