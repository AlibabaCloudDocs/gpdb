# Set up a whitelist {#concept_v4f_dmr_52b .concept}

You must set up the whitelist before starting an instance. Add IP addresses or IP segments that are allowed to access a database to ensure security and stability.

## Background { .section}

There are three scenarios for accessing AnalyticDB for PostgreSQL databases:

-   Access from the Internet.
-   Access from the intranet. The network types of AnalyticDB for PostgreSQL and ECS instances must be identical.
-   Access from the intranet and Internet at the same time. The network types of AnalyticDB for PostgreSQL and ECS instances must be identical.

**Note:** To set the network type, see [Set the Network Type](intl.en-US/Quick Start/Set up an instance/Set the network type.md#).

**Procedure** 

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com/).
2.  Select the region where the target instance is located.

3.  Click the ID of the instance to go to the **Basic Information** page of the instance.

4.  In the left-side navigation pane, click **Security Controls**.

5.  In the **Whitelist Settings** page, click **Modify** under the default whitelist group to go to the **Modify Group** page.

**Note:** You can also click **Clear** under the default whitelist group to clear the IP addresses included, and then click **Add Whitelist Group** to create a custom group.

6.  Delete the default address 127.0.0.1 from the whitelist and then enter a custom whitelist. Parameters are described as follows:

    -   **Group Name**: The group name contains 2 to 32 characters, and consists of lowercase letters, numbers, or underscores \(\_\). The group name must start with a lowercase letter and end with a letter or number. The default group name cannot be modified or deleted.

    -   **Whitelist**: Enter the IP addresses or IP segments that are allowed to access the database. IP addresses or IP segments are separated by commas \(,\).

        -   The whitelist can contain IP addresses \(for example, 10.10.10.1\) or IP segments \(for example, 10.10.10.0/24, which indicates that any IP address in the format of 10.10.10.X can access the database\).

        -   % or 0.0.0.0/0 indicates that any IP address is allowed to access the database.

            **Note:** We recommend that you not use this configuration unless necessary, because it can greatly reduce database security.

        -   After an instance is created, the local loopback IP address 127.0.0.1 is added to the default whitelist, which prevents all external IP addresses from accessing the instance.

    -   **Choose an existing ECS IP Address**: Click it to display all the ECS instances belonging to the same account. You can select ECS IP addresses to add the ECS instances to the whitelist.

7.  Click **OK** to add the whitelist.


## Next { .section}

The whitelist provides an advanced access protection for AnalyticDB for PostgreSQL. So, we recommend that you maintain the whitelist on a regular basis.

During the subsequent operations, you can click **Modify** under the group name to modify an existing group, or click **Delete**to delete an existing custom group.

## Related API {#section_wvn_lvt_vgb .section}

|API|Description|
|---|-----------|
|[DescribeDBInstanceIPArrayList](../../../../intl.en-US/API Reference/Security management/DescribeDBInstanceIPArrayList.md#)|Returns a list of IP addresses that are allowed to access the database instance.|
|[ModifySecurityIps](../../../../intl.en-US/API Reference/Security management/ModifySecurityIps.md#)|Modifies the whitelist of IP addresses.|

