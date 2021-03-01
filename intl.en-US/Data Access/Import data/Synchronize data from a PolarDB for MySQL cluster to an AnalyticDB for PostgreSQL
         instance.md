# Synchronize data from a PolarDB for MySQL cluster to an AnalyticDB for PostgreSQL instance

This topic describes how to use Data Transmission Service \(DTS\) to synchronize data from a PolarDB for MySQL cluster to an AnalyticDB for PostgreSQL instance. DTS facilitates data transmission and the centralized analysis of enterprise data.

-   The binary logging feature is enabled for the PolarDB for MySQL cluster. For more information, see [Enable binary logging](https://www.alibabacloud.com/help/zh/doc-detail/113546.htm).
-   The tables to be synchronized from the PolarDB for MySQL cluster contain primary keys.
-   An AnalyticDB for PostgreSQL instance is created. For more information, see [Create an instance](https://www.alibabacloud.com/help/zh/doc-detail/50200.htm).

## Precautions

-   DTS uses read and write resources of the source and destination databases during initial full data synchronization. This may increase the loads of the database servers. If the database performance is unfavorable, the specification is low, or the data volume is large, database services may become unavailable. For example, DTS occupies a large amount of read and write resources in the following cases: a large number of slow SQL queries are performed on the source database, the tables have no primary keys, or a deadlock occurs in the destination database. Before you synchronize data, evaluate the impact of data synchronization on the performance of the source and destination databases. We recommend that you synchronize data during off-peak hours. For example, you can synchronize data when the CPU utilization of the source and destination databases is less than 30%.
-   During initial full data synchronization, concurrent INSERT operations cause fragmentation in the tables of the destination instance. After initial full data synchronization is complete, the tablespace of the destination instance is larger than that of the source cluster.

## Limits

-   You can select only tables as the objects to be synchronized.
-   DTS does not synchronize the following types of data: BIT, VARBIT, GEOMETRY, ARRAY, UUID, TSQUERY, TSVECTOR, and TXID\_SNAPSHOT.
-   Prefix indexes cannot be synchronized. If the source database contains prefix indexes, data may fail to be synchronized.
-   We recommend that you do not use gh-ost or pt-online-schema-change to perform data definition language \(DDL\) operations on objects during data synchronization. Otherwise, data synchronization may fail.

## SQL operations that can be synchronized

-   Data manipulation language \(DML\) operations: INSERT, UPDATE, and DELETE
-   DDL operation: ADD COLUMN

    **Note:** The CREATE TABLE operation is not supported. To synchronize data from a new table, you must add the table to the selected objects. For more information, see [Add an object to a data synchronization task](/intl.en-US/Data Synchronization/Synchronization task management/Add an object to a data synchronization task.md).


## Supported synchronization topologies

-   One-way one-to-one synchronization
-   One-way one-to-many synchronization
-   One-way many-to-one synchronization

## Term mappings

|PolarDB for MySQL|AnalyticDB for PostgreSQL|
|:----------------|:------------------------|
|Database|Schema|
|Table|Table|

## Procedure

1.  Purchase a data synchronization instance. For more information, see [Purchase a DTS instance]().

    **Note:** On the buy page, set Source Instance to **PolarDB**, set Target Instance to **AnalyticDB for PostgreSQL**, and set Synchronization Topology to **One-Way Synchronization**.

2.  Log on to the [DTS console](https://dts-intl.console.aliyun.com/).

3.  In the left-side navigation pane, click **Data Synchronization**.

4.  At the top of the Synchronization Tasks page, select the region where the destination instance resides.

    ![Select a region](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4130359951/p50604.png)

5.  Find the data synchronization instance and click **Configure Synchronization Channel** in the Actions column.

6.  Configure the source and destination instances.

    ![Configure the source and destination instances](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4970034061/p103968.png)

    |Section|Parameter|Description|
    |:------|:--------|:----------|
    |N/A|Synchronization Task Name|DTS automatically generates a task name. We recommend that you specify an informative name for easy identification. You do not need to use a unique task name.|
    |Source Instance Details|Instance Type|The value of this parameter is set to **PolarDB Instance** and cannot be changed.|
    |Instance Region|The region of the PolarDB for MySQL cluster. The region is the same as the source region that you selected on the buy page. You cannot change the value of this parameter.|
    |POLARDB Instance ID|Select the ID of the PolarDB for MySQL cluster.|
    |Database Account|Enter the database account of the PolarDB for MySQL cluster. **Note:** The database account must have the read permission on the objects that you want to synchronize. |
    |Database Password|Enter the password of the source database account.|
    |Destination Instance Details|Instance Type|The value of this parameter is set to **AnalyticDB for PostgreSQL** and cannot be changed.|
    |Instance Region|The destination region that you selected on the buy page. You cannot change the value of this parameter.|
    |Instance ID|Select the ID of the AnalyticDB for PostgreSQL instance.|
    |Database Name|Enter the name of the destination database in the AnalyticDB for PostgreSQL instance.|
    |Database Account|Enter the **initial account** of the AnalyticDB for PostgreSQL instance. For more information, see [t16843.md\#](/intl.en-US/Quick Start/Configure an account.md). **Note:** You can also enter an account that has the RDS\_SUPERUSER permission. For more information, see [Manage users and permissions](/intl.en-US/Beginner Developer Guide/Manage users and permissions.md). |
    |Database Password|Enter the password of the destination database account.|

7.  In the lower-right corner of the page, click **Set Whitelist and Next**.

    **Note:** DTS adds the CIDR blocks of DTS servers to the whitelists of the PolarDB for MySQL cluster and the AnalyticDB for PostgreSQL instance. This ensures that DTS servers can connect to the source cluster and destination instance.

8.  Select the synchronization policy and the objects to be synchronized.

    ![Synchronize data from MySQL to AnalyticDB for PostgreSQL](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4703019951/p96654.png)

    |Setting|Parameter|Description|
    |:------|:--------|:----------|
    |Select the synchronization policy|Initial Synchronization|You must select both **Initial Schema Synchronization** and **Initial Full Data Synchronization** in most cases. After the precheck, DTS synchronizes the schemas and data of the required objects from the source instance to the destination instance. The schemas and data are the basis for subsequent incremental synchronization.|
    |Processing Mode In Existed Target Table|    -   **Clear Target Table**

Skips the **Schema Name Conflict** item during the precheck. Clears the data in the destination table before initial full data synchronization. If you want to synchronize your business data after testing the data synchronization task, you can select this mode.

    -   **Ignore**

Skips the **Schema Name Conflict** item during the precheck. Adds data to the existing data during initial full data synchronization. If you want to synchronize data from multiple tables to one table, you can select this mode. |
    |Synchronization Type|Select the types of operations that you want to synchronize based on your business requirements.

    -   **Insert**
    -   **Update**
    -   **Delete**
    -   **AlterTable** |
    |Select the objects to be synchronized|N/A|Select one or more tables from the Available section and click the ![Right arrow](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3457359951/p40698.png) icon to move the tables to the Selected section.

**Note:**

    -   You can select only tables as the objects to be synchronized.
    -   You can use the object name mapping feature to change the names of the columns that are synchronized to the destination database. For more information, see [Specify the name of an object in the destination instance](/intl.en-US/Data Synchronization/Synchronization task management/Specify the name of an object in the destination instance.md). |

9.  Specify the primary key column and distribution column of the table that you want to synchronize to the AnalyticDB for PostgreSQL instance.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4703019951/p65402.png)

    **Note:** The page in this step appears only if you select **Initial Schema Synchronization**. For more information about primary key columns and distribution columns, see [Define constraints](https://www.alibabacloud.com/help/zh/doc-detail/118150.htm#title-8zk-q8o-q3l) and [Define table distribution](https://www.alibabacloud.com/help/zh/doc-detail/120143.htm).

10. In the lower-right corner of the page, click **Precheck**.

    **Note:**

    -   Before you can start the data synchronization task, DTS performs a precheck. You can start the data synchronization task only after the task passes the precheck.
    -   If the task fails to pass the precheck, you can click the ![Info icon](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3457359951/p47468.png) icon next to each failed item to view details. You can troubleshoot the issues based on the causes and run a precheck again.
11. Close the Precheck dialog box after the following message is displayed: **The precheck is passed.** Then, the data synchronization task starts.

12. Wait until the initial synchronization is complete and the data synchronization task is in the **Synchronizing** state.

    You can view the status of the data synchronization task on the Synchronization Tasks page.

    ![View the status of a data synchronization task](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5130359951/p41059.png)


