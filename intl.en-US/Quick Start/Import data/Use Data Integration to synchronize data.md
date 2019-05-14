# Use Data Integration to synchronize data {#concept_sm2_kmr_52b .concept}

[Data Integration](https://www.aliyun.com/product/cdp/) is a data synchronization platform provided by Alibaba Cloud big data service. The platform offers offline \(full/incremental\) data access channels for more than 20 data sources of different network environments and supports data storage across heterogeneous systems and elastic expansion, featuring high reliability, high security, and low costs. Check out the [Supported data source types](https://www.alibabacloud.com/help/doc-detail/53008.htm) to learn about data sources available.

This document describes how to use Data Integration for **Data Import** and **Data Export** with AnalyticDB for PostgreSQL. It provides both procedures in the **Wizard Mode** \(guided by a visualized interface\) and sample code in the **Script Mode**\(template-based parameter configuration\).

## Use cases { .section}

Using the synchronization jobs in Data Integration, you can:

-   Synchronize data in AnalyticDB for PostgreSQL to other data sources and perform expected processing on the data.

-   Synchronize processed data from other data sources to AnalyticDB for PostgreSQL.


## Prerequisites { .section}

Complete the following operations on the Data Integration and AnalyticDB for PostgreSQL ends respectively.

**Data Integration**

Follow these steps to create a project in Data Integration.

1.  Open a real-name-authenticated account on the official Alibaba Cloud website and create an AccessKey for accessing the account.

2.  Activate MaxCompute and the system automatically generates a default ODPS data source. Log on to Data IDE by using the primary account.

3.  Create a project. Users can collaborate in projects to complete a workflow and jointly maintain data and jobs. For this reason, you must create a project first before using Data IDE.

4.  If you want to create data integration jobs by using a subaccount, you must grant related permissions to the subaccount.


## AnalyticDB for PostgreSQL { .section}

Before importing data, you must create the target database and table in AnalyticDB for PostgreSQL you want to migrate data to on the PostgreSQL client.

If the source database to export data from is AnalyticDB for PostgreSQL, we recommend that you [set the IP whitelist](https://www.alibabacloud.com/help/doc-detail/50207.htm) in the AnalyticDB for PostgreSQL console. You can follow these steps to set the IP whitelist.

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com/).

2.  Select the expected instance, and click **Add Whitelist Group** on the Whitelist Settings page under the Data Securitypage.

3.  Add the following IP addresses: `10.152.69.0/24,10.153.136.0/24,10.143.32.0/24,120.27.160.26,10.46.67.156,120.27.160.81,10.46.64.81,121.43.110.160,10.117.39.238,121.43.112.137,10.117.28.203,118.178.84.74,10.27.63.41,118.178.56.228,10.27.63.60,118.178.59.233,10.27.63.38,118.178.142.154,10.27.63.15,100.64.0.0/8`.


**Note:** If you use a custom resource group to schedule a AnalyticDB for PostgreSQL data synchronization job, you must add the IP address of the computer hosting the custom resource group to the AnalyticDB for PostgreSQL whitelist.

## Add data source {#addsource .section}

A new AnalyticDB for PostgreSQL data source must added to Data Integration before you can use Data Integration for data synchronization to AnalyticDB for PostgreSQL. Follow these steps to add a data source.

1.  Log on to the [DataWorks console](https://workbench.data.aliyun.com/console) as an administrator and click **Enter Workspace** in the actions column of the relevant project in the Project List.
2.  Click **Data Integration** in the top navigation bar to go to the **Data Source** page.
3.  Click **New Source** source to pop up the supported data source.
4.  In the New Data Source window, select PostgreSQL as the **Data Source Type**.
5.  Select to configure the PostgreSQL data source in the form of a **JDBC** instance. The parameters include:

    -   Type: data source without a public IP address.
    -   Name: It is a combination of letters, numbers, and underlines It must begin with a letter or underline and cannot exceed 60 characters.
    -   Description: It is a brief description of the data source with no more than 80 characters.
    -   Resource Group: It is used to run synchronization tasks, and generally multiple machines can be bound when you add a resource group. For details, see [Add scheduling resources](https://www.alibabacloud.com/help/doc-detail/72979.htm#concept_wfz_j45_q2b).
    -   JDBC URL: Format: jdbc:mysql://ServerIP:Port/database.
    -   Username/Password: The user name and password used to connect to the database.
6.  When you complete the settings, click **Test Connectivity**.

7.  When the connectivity test is passed, click **Complete**.


## Import data by using Data Integration { .section}

You can use one of the following methods to configure the synchronization job.

-   If you use the visualized wizard, see [Configure synchronization jobs in the wizard mode](#importwizard). The wizard mode can be switched to the script mode.

-   If you use template-based parameter configuration, see [Configure synchronization jobs in the script mode](#importscript). The script mode cannot be switched to the wizard mode.


Before going ahead, make sure you have added the AnalyticDB for PostgreSQL data source to Data Integration by following the [Add data source](#addsource) procedure.

**Configure synchronization jobs in the wizard mode**

Follow these steps to configure the synchronization job.

1.  Select the **Wizard Mode** to create a synchronization job.

2.  Select a data source. The parameters include:

    -   **Data Source**: select **odps\_first\(odps\)**, that is, MaxCompute.
    -   **Table**: select **hpg**.
    -   **Data Preview**: the window is collapsed by default. You can click it to expand it.
    After entering the preceding information, click **Next**.

3.  Select a target. The parameters include:

    -   Data Source: select **I\_PostGreSql\(postgresql\)**.
    -   Table: select **public.person**.
    -   Prepared Statement Before Import: enter the SQL statement to run before the data synchronization job starts.

        Currently, you can run only one SQL statement in the wizard mode. But you can run multiple SQL statements in the script mode. For example, to clear old data.

    -   Prepared Statement after Import: enter the SQL statement to run after the data synchronization job starts.

        Currently, you can run only one SQL statement in the wizard mode. But you can run multiple SQL statements in the script mode. For example, to add a time stamp.

    -   Primary Key Conflict: select **Insert Into**. If the primary key conflicts with the unique index, Data Integration processes the data as dirty data.
    After entering the preceding information, click **Next**.

4.  Map fields. You must configure the field mapping relationships. The Source Table Fields on the left correspond one to one with the Target Table Fields on the right.

    **Description**:

    -   You can enter constants. The value must be enclosed in single-byte single quotation marks. For example, 'abc' or '123'.
    -   Scheduling parameters can be used together. For example, `${bdp.system.bizdate}` and others.
    -   You can enter the partition columns to synchronize. For example, partition columns with PT.
    -   If the value you entered cannot be parsed, the type is displayed as ‘Unrecognized’.
    -   You cannot configure ODPS functions.
    After that, click **Next**.

5.  Control channels. You can configure the maximum job rate and dirty data checking rules. The parameters include:

    -   **Maximum Job Rate:** determines the highest rate possible for data synchronization jobs. The actual rate of the job may vary with the network environment, database configuration, and other factors.

    -   Number of Concurrent Jobs: the maximum job rate = Number of concurrent jobs \* Transmission rate of a single concurrent job. When the maximum job rate is specified, use the following method to select the number of concurrent jobs:

        -   If your data source is an online business database, we recommend that you not set a large value for the concurrent job count to avoid interfering with the online database.
        -   If you require a high data synchronization rate, we recommend that you select the highest job rate and a large concurrent job count.
6.  Preview and save settings. After the preceding configuration, you can scroll up or down to view the job configuration. After that, click **Save**.

7.  Get results. After saving a synchronization job,

    -   Click **Run Job** to run the job immediately.
    -   Click **Submit** on the right to submit the synchronization job to the scheduling system.
    The scheduling system automatically and periodically runs the job from the next day according to the configuration attributes. For related scheduling configuration, see [Scheduling configuration](https://www.alibabacloud.com/help/doc-detail/50130.htm).


**Configure synchronization jobs in the script mode**

The sample code is as follows:

```
{
  "configuration": {
    "reader": {
      "plugin": "odps",
      "parameter": {
        "partition": "pt=${bdp.system.bizdate}",//Partition information
        "datasource": "odps_first",//Data source name. We recommend that you add the data source before configuring synchronization jobs. The value of this configuration item must be the same as the name of the data source you added.
        "column": [
          "id",
          "name",
          "year",
          "birthdate",
          "ismarried",
          "interest",
          "salary"
        ],
        "table": "hpg"//Source table name
      }
    },
    "writer": {
      "plugin": "postgresql",
      "parameter": {
        "postSql": [],//Prepare the statement after the import
        "datasource": "l_PostGreSql",//Data source name. We recommend that you add the data source before configuring synchronization jobs. The value of this configuration item must be the same as the name of the data source you added.
        "column": [
          "id",
          "name",
          "year",
          "birthdate",
          "ismarried",
          "interest",
          "salary"
        ],
        "table": "public.person",//Target table name
        "preSql": []//Prepare the statement before the import
      }
    },
    "setting": {
      "speed": {
        "concurrent": 7,//Number of concurrent jobs
        "mbps": 7//The maximum job rate
      }
    }
  },
  "type": "job",
  "version": "1.0"
}
```

## Export data by using Data Integration { .section}

You can use one of the following methods to configure the synchronization job.

-   If you use the visualized wizard, see [Configure synchronization jobs in the wizard mode](#exportwizard).
-   If you use template-based parameter configuration, see [Configure synchronization jobs in the script mode](#exportscript).

Before going ahead, make sure you have added the AnalyticDB for PostgreSQL data source to Data Integration by following the [Add data source](#addsource) procedure.

**Configure synchronization jobs in the wizard mode**

Follow these steps to configure the synchronization job.

1.  Select the **Wizard Mode** to create a synchronization job.

2.  Select a source. The parameters include:

    -   Data Source: select **I\_PostGreSql\(postgresql\)**.
    -   Table: select **public.person**.
    -   Data Preview: the window is collapsed by default. You can click it to expand it.
    -   Data Filtering: set the filtering condition for data synchronization. PostgreSQLReader concatenates an SQL statement based on the specified column, table, and WHERE conditions, and extracts data according to the SQL statement.

        For example, you can specify the actual use case in the where condition during a test. Usually the data on the day is selected for synchronization. In this case, you can set the where condition to id \> 2 and sex = 1. The where condition can effectively help with incremental business data synchronization. If the where condition is not configured or is left null, full table data synchronization applies.

    -   Split key: if you specify the splitPk when using PostgreSQLReader to extract data, it means that you want to use the fields represented by the splitPk for data sharding. In this case, the Data Integration initiates concurrent jobs to synchronize data, which greatly improves the efficiency of data synchronization.

        We recommend that you use primary keys of tables, because primary keys are generally evenly distributed with less risks of data hot spots. The splitPk only supports splitting integers, and does not support strings, floating points, dates, and other types. If the non-supported data type is specified as the splitPk, the splitPk feature is ignored and data is synchronized in a single channel. If the splitPk value is not provided, including a null value is provided, data in the table is synchronized in a single channel.

3.  Select a target. The parameters include:

    -   Data Source: select **odps\_first\(odps\)**, that is, MaxCompute.
    -   Table: select **hpg**.
    After entering the preceding information, click **Next**.

4.  Map fields. You must configure the field mapping relationships. The Source Table Fields on the left correspond one to one with the Target Table Fields on the right. After that, click **Next**.

5.  Control channels. You can configure the maximum job rate and dirty data checking rules. After that, click **Next**.

6.  Preview and save settings. After the preceding configuration, you can scroll up or down to view the job configurations. After that, click **Save**.


So far, you have created a data synchronization job in the wizard mode to export data from AnalyticDB for PostgreSQL.

**Configure synchronization jobs in the script mode**

The sample code is as follows:

```
{
  "configuration": {
    "reader": {
      "plugin": "postgresql",
      "parameter": {
        "datasource": "l_PostGreSql",//Data source name. We recommend that you add the data source before configuring synchronization jobs. The value of this configuration item must be the same as the name of the data source you added.
        "table": "public.person",//Source table name
        "where": "",//Filtering condition
        "column": [
          "id",
          "name",
          "year",
          "birthdate",
          "ismarried",
          "interest",
          "salary"
        ],
        "splitPk": ""//Split key
      }
    },
    "writer": {
      "plugin": "odps",
      "parameter": {
        "datasource": "odps_first",//Data source name. We recommend that you add the data source before configuring synchronization jobs. The value of this configuration item must be the same as the name of the data source you added.
        "column": [
          "id",
          "name",
          "year",
          "birthdate",
          "ismarried",
          "interest",
          "salary"
        ],
        "table": "hpg",//Target table name
        "truncate": true,
        "partition": "pt=${bdp.system.bizdate}"//Partition information
      }
    },
    "setting": {
      "speed": {
        "mbps": 5,//The maximum job rate
        "concurrent": 5//Number of concurrent jobs
      }
    }
  },
  "type": "job",
  "version": "1.0"
}
```

