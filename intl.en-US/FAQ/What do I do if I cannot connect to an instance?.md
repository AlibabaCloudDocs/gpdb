# What do I do if I cannot connect to an instance?

## Problem description

An error occurs when you connect to an AnalyticDB for PostgreSQL instance from a client, as shown in the following figure.

![2021030101](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2663654161/p244023.png)

## Causes

The AnalyticDB for PostgreSQL instance cannot communicate with the IP address in the error message. The following lists describe the possible causes:

-   The IP address in the error message is a local area network \(LAN\) IP address.
-   The IP address in the error message is not added to an IP address whitelist of the AnalyticDB for PostgreSQL instance.

## Solution

-   **Obtain the IP address of your on-premises client and add the IP address to an IP address whitelist of the AnalyticDB for PostgreSQL instance.**

    In complex network environments, you may be unable to find the IP address of your on-premises client or add the IP address to the an IP address whitelist. This section describes how to obtain the IP address of your on-premises client.

    **Procedure**

    1.  Perform the following operations to add `0.0.0.0/0` to an IP address whitelist of the AnalyticDB for PostgreSQL instance:
        1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).
        2.  In the upper-left corner, select the region where the instance resides.
        3.  Find the instance and click its ID. The **Basic Information** page appears.
        4.  In the left-side navigation pane, click **Security Controls**. The Security Controls page appears.

        5.  On the Whitelist Settings tab, click **Modify** to the right of the default IP address whitelist. The Modify Whitelist panel appears.
        6.  In the **IP Addresses** field, delete the IP address 127.0.0.1 and enter `0.0.0.0/0`.

            **Note:** `0.0.0.0/0` indicates that all IP addresses are allowed to access the instance. This may raise security risks. We recommend that you delete 0.0.0.0/0 after you no longer need it.

        7.  Click **OK**.
    2.  Run the following command by using the psql tool to connect to the AnalyticDB for PostgreSQL instance. For more information about how to download the psql tool, see [Connect to an AnalyticDB for PostgreSQL instance](/intl.en-US/Quick Start/Connect to an AnalyticDB for PostgreSQL instance.md).

        ```
        psql -h yourgpdbaddress.gpdb.rds.aliyuncs.com -p 3432 -d postgres -U gpdbaccount
        ```

        Parameter description:

        -   -h: the host address.
        -   -p: the port used to connect to the database.
        -   -d: the name of the database. The default value is postgres.
        -   -U: the account used to connect to the database.
        -   You can run the `psql --help` command to view more options. You can also run the `\?` command to view the commands supported in psql.
    3.  After you connect to the database, run the following command in the SQL command line window to query the IP address of your on-premises client:

        ```
        select * from pg_stat_activity;
        ```

        The value of the CLIENT\_ADDR field in the query results is the IP address of your on-premises client.

        ![2021030102](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7931129951/p38978.png)

    4.  In the AnalyticDB for PostgreSQL console, delete `0.0.0.0/0` from the default IP address whitelist, and enter the IP address of your on-premises client to access the AnalyticDB for PostgreSQL instance.

