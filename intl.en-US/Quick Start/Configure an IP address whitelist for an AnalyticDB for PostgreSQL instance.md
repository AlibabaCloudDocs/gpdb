# Configure an IP address whitelist for an AnalyticDB for PostgreSQL instance

This topic describes how to configure an IP address whitelist for an AnalyticDB for PostgreSQL instance before you start that instance. An IP address whitelist contains the IP addresses or Classless Inter-Domain Routing \(CIDR\) blocks that are allowed to access your instance. Whitelists make your instance more stable and secure.

## Background information

The scenarios in which you access an AnalyticDB for PostgreSQL instance from an ECS instance are as follows:

-   Access the AnalyticDB for PostgreSQL instance over the Internet.

-   Access the AnalyticDB for PostgreSQL instance over an internal network. In this scenario, make sure that the AnalyticDB for PostgreSQL and ECS instances have the same network type.

-   Access the AnalyticDB for PostgreSQL instance over both the Internet and an internal network. In this scenario, make sure that the AnalyticDB for PostgreSQL and ECS instances have the same network type.


**Note:** For information about how to configure network types, see [Set the network type](/intl.en-US/FAQ/Set the network type.md).

## Procedure

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/gpdb/cn-hangzhou/list).
2.  In the top navigation bar, select the region where the target AnalyticDB for PostgreSQL instance resides.
3.  Find the target AnalyticDB for PostgreSQL instance and click its ID. The **Basic Information** page appears.
4.  In the left-side navigation pane, click **Security Controls**. The Security Controls page appears.

5.  On the Whitelist Settings tab, find the IP address whitelist labeled default and click **Modify** to its right. The Modify Group dialog box appears.

    **Note:**

    You can click **Clear** next to the default IP address whitelist to delete that whitelist and then click **Add Group** to create an IP address whitelist.

6.  Delete 127.0.0.1 from the default IP address whitelist and add IP addresses or CIDR blocks to that whitelist as needed. The parameters are described as follows:
    -   **Group Name**: The group name must be 2 to 32 characters in length. It can contain lowercase letters, digits, and underscores \(\_\). It must start with a lowercase letter and end with a lowercase letter or digit. The default IP address whitelist cannot be deleted and its name cannot be changed.

    -   **White List**: Enter IP addresses or CIDR blocks and separate them with commas \(,\).

        -   An IP address whitelist can contain IP addresses such as 10.10.10.1 and CIDR blocks such as 10.10.10.0/24. The 10.10.10.0/24 CIDR block indicates that all IP addresses in the 10.10.10.X format are granted access to the AnalyticDB for PostgreSQL instance.

        -   The wildcard \(%\) or 0.0.0.0/0 indicates that all IP addresses are granted access to the AnalyticDB for PostgreSQL instance.

            **Note:**

            This setting is risky, so we recommend that you do not apply this setting unless necessary.

        -   The loopback IP address 127.0.0.1 is configured in the default IP address whitelist upon instance creation. This loopback IP address indicates that no external IP addresses are allowed to access the AnalyticDB for PostgreSQL instance.

7.  Click **OK**.

## What to do next

Properly configured whitelists make your AnalyticDB for PostgreSQL instance more secure. We recommend that you maintain the whitelists on a regular basis.

You can click **Modify** to the right of a whitelist to modify it. You can also click **Delete** next to a whitelist to delete it.

## Related operations

|Operation|Description|
|---------|-----------|
|[DescribeDBInstanceIPArrayList](/intl.en-US/API Reference/Security management/DescribeDBInstanceIPArrayList.md)|Queries an IP address whitelist of an AnalyticDB for PostgreSQL instance.|
|[ModifySecurityIps](/intl.en-US/API Reference/Security management/ModifySecurityIps.md)|Modifies an IP address whitelist of an AnalyticDB for PostgreSQL instance.|

