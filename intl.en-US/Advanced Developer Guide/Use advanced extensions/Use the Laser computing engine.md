# Use the Laser computing engine

The Laser computing engine is developed by Alibaba Cloud for AnalyticDB for PostgreSQL. Laser is transparent to customers and can improve the performance of sophisticated computing. Compared with the native engine, Laser provides more than twice performance when the 22 SQL queries of the TPC-H benchmark are executed on 1 GB, 100 GB, 1 TB, or 10 TB of data.

## Enable and disable Laser

You can set the global user configuration \(GUC\) parameter laser.enable to enable or disable Laser. To enable Laser, set the parameter to on. If the parameter is set to on, SELECT statements are processed by Laser. To disable Laser, set the parameter to off. If the parameter is set to off, SELECT statements are processed by the native engine. The default value is off. You can enable Laser for sessions, databases, and clusters. If you enable Laser for sessions, Laser is automatically disabled when the sessions are ended. If you enable Laser for databases, Laser immediately takes effect. If you enable Laser for clusters, Laser takes effect after you restart the clusters. You can execute the following SQL statements to check the Laser status and modify the status for sessions, databases, and clusters:

```
--- Check whether Laser is enabled.
show laser.enable;
--- Disable Laser for a session.
set laser.enable = off;
--- Enable Laser for a session.
set laser.enable = on;
--- Disable Laser for a database.
alter database ${DBNAME} set laser.enable = off;
--- Enable Laser for a database.
alter database ${DBNAME} set laser.enable = on;
```

**Note:** We recommend that you enable Laser for databases and sessions. To enable Laser for your clusters, contact the Alibaba Cloud administrator.

## Supported data types and operators

Laser supports the following data types:

-   INT2, INT4, and INT8
-   FLOAT4, FLOAT8, and NUMERIC
-   DATE, TIME, TIMETZ, TIMESTAMP, and TIMESTAMPTZ
-   VARCHAR, TEXT, and BPCHAR

Laser supports the following operators:

-   =, <, <=, \>, \>=, <\>, !=, BETWEEN, IS NOT NULL, IS NULL, and LIKE
-   Logical operators: AND, OR, and NOT

## Notes

-   We recommend that you use the ORCA query optimizer.
-   Laser is supported only in AnalyticDB for PostgreSQL V6.0 and later.

