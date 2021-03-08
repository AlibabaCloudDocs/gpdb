# Enable SQL audit

AnalyticDB for PostgreSQL allows you to use the SQL audit feature to view SQL statement details and audit SQL statements on a regular basis. The SQL audit feature does not affect instance performance.

## Background information

SQL audit records all data manipulation language \(DML\) and data description language \(DDL\) operations. The system captures such operations by analyzing data transmitted over network protocols. A few records may be lost when a large number of SQL queries are sent to database instances.

## Precautions

-   SQL audit logs are retained for 30 days.
-   By default, the SQL audit feature is disabled. After this feature is enabled, you are charged additional fees. For more information, see [AnalyticDB for PostgreSQL Pricing](https://www.aliyun.com/price/product?#/gpdb/detail).

## Procedure

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).
2.  In the upper-left corner of the page, select the **region** where the desired instance resides from the drop-down list.
3.  Find the instance and click its ID in the **Instance Name** column.
4.  In the left-side navigation pane of the instance details page, click **Security Controls**.
5.  Click the SQL Audit tab, and click **Enable SQL Audit**.
6.  In the message that appears, click **OK** to enable the SQL audit feature.
    -   After you enable the SQL audit feature, you can query SQL information based on conditions that you specify in the **Select Time Range**, **DB**, **User**, and **Keyword** fields.
    -   To disable the SQL audit feature, click **Disable SQL Audit** on the SQL Audit tab.

## Related API operations

|API|Description|
|---|-----------|
|[t16943.md\#](/intl.en-US/API Reference/Log management/ModifySQLCollectorPolicy.md)|Enables or disables the SQL collection feature for an AnalyticDB for PostgreSQL instance.|

