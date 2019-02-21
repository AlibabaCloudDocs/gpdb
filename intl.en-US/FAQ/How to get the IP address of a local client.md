# How to get the IP address of a local client {#concept_gq5_qpk_2gb .concept}

## Symptom {#section_qx4_55w_pfb .section}

Due to the complexity and diversity of the network, the user may not be able to correctly locate the IP address of the local client which should be add in the instance whitelist. This article describes how to view the IP of a local client.

Here is an example to denote the detailed steps of the operation.

## Procedure {#section_pyc_tzz_qfb .section}

1.  Add `0.0.0.0/0` to the whitelist of the HybridDB for PostgreSQL instance.
    1.  Log on the [HybridDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
    2.  Select the region where the target instance is located.

    3.  Click the ID of the instance to go to the **Basic Information** page of the instance.

    4.  In the left-side navigation pane, click **Security Controls**.

    5.  In the **Whitelist Settings** page, click **Modify** under the default whitelist group to go to the **Modify Group** page.

    6.  Delete the default address 127.0.0.1 from the whitelist and then add `0.0.0.0/0` to the whitelist.

        **Note:** The `0.0.0.0/0` address which allows any IP to access the database involves a high security risk, and this configuration should be deleted as soon as possible.

    7.  Click **OK** to finish the operation.

2.  Connect to the HybridDB for PostgreSQL instance using a local client. See [Connect to a HybridDB for PostgreSQL database](../../../../../reseller.en-US/Quick Start/Connect to a HybridDB for PostgreSQL database.md#) to download and install the psql client. Issue the following statement to connect to the database:

    ```
    psql -h yourgpdbaddress.gpdb.rds.aliyuncs.com -p 3432 -d postgres -U gpdbaccount
    ```

    Parameter descriptions are as follows:

    -   -h: specifies the host address.
    -   -p: specifies the port number.
    -   -d: specifies the database \(the default database is postgres\).
    -   -U: specifies the connected user.

    You can view more parameters by performing `psql-- help`. And in the psql prompt, you can view more supported psql commands by performing `\?`.

3.  Run the following statement in the SQL command line window in the database to query the IP address of the local client.

    ```
    select * from pg_stat_activity;
    ```

    The value in the CLIENT\_ADDR of the query result is the IP address of the local client.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80825/155073358738978_en-US.png)

4.  Remove `0.0.0.0/0` from the whitelist, and add the IP address in the previous step to access the database with a client.

