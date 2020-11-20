# Manage public endpoints

If your application is deployed on an ECS instance within the same region and has the same [network type](/intl.en-US/FAQ/Set the network type.md) as your AnalyticDB for PostgreSQL instance, you do not need to apply for a public endpoint. If your application is deployed on a third-party system or on an ECS instance in a different region or has a different network type from your AnalyticDB for PostgreSQL instance, you must apply for a public endpoint.

**Note:** If your application is deployed on an ECS instance within the same region \(but maybe in different zones\) and has the same network type as your AnalyticDB for PostgreSQL instance, the application can connect to the AnalyticDB for PostgreSQL instance over their internal network.

## Scenarios

Internal and public endpoints apply to the following scenarios:

-   Only use an internal endpoint.
    -   Your application is deployed on an ECS instance within the same region and has the same [network type](/intl.en-US/FAQ/Set the network type.md) as your AnalyticDB for PostgreSQL instance.
-   Only use a public endpoint.
    -   Your application is deployed on an ECS instance in a region different from your AnalyticDB for PostgreSQL instance.
    -   Your application is deployed on a third-party system.
-   Use both an internal endpoint and a public endpoint.
    -   Some modules of your application are deployed on an ECS instance within the same region and have the same [network type](/intl.en-US/FAQ/Set the network type.md) as your AnalyticDB for PostgreSQL instance, but other modules are deployed on an ECS instance in a region different from your AnalyticDB for PostgreSQL instance.
    -   Some modules of your application are deployed on an ECS instance within the same region and have the same [network type](/intl.en-US/FAQ/Set the network type.md) as your AnalyticDB for PostgreSQL instance, but other modules are deployed on a third-party system.

## Precautions

-   Before you connect your application to your AnalyticDB for PostgreSQL instance, you must add the IP address or the corresponding CIDR block of the application to a whitelist of the instance. For more information, see [Configure an IP address whitelist for an AnalyticDB for PostgreSQL instance](/intl.en-US/Quick Start/Configure an IP address whitelist for an AnalyticDB for PostgreSQL instance.md).
-   For security purposes, exercise caution when you use the public endpoint to connect to your AnalyticDB for PostgreSQL instance. To increase security and expedite transmission, we recommend that you migrate your application to an ECS instance in the same region as your AnalyticDB for PostgreSQL instance.

## Apply for a public endpoint

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).
2.  Log on to the [AnalyticDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
3.  In the upper-left corner of the page, select the **region** where the target instance resides from the drop-down list.
4.  Find the target instance and click its ID.
5.  On the Basic Information page that appears, click **Apply for Public Endpoint**. Alternatively, in the left-side navigation pane, click **Database Connection**.
6.  On the Database Connection page, click **Apply for Public Endpoint**.

On the Database Connection page, you can click **Release Public Endpoint** to release the public endpoint.

## Release a public endpoint

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).
2.  Log on to the [AnalyticDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
3.  In the upper-left corner of the page, select the **region** where the target instance resides from the drop-down list.
4.  Find the target instance and click its ID.
5.  In the left-side navigation pane, click Database Connection.
6.  On the Database Connection page, click **Release Public Endpoint**.

    If you have not applied for a public endpoint, only the **Apply for Public Endpoint** button is displayed on the Database Connection page.

7.  In the message that appears, click **OK** to release the public endpoint.

## Related API operations

|API|Description|
|---|-----------|
|[AllocateInstancePublicConnection](/intl.en-US/API Reference/Network management/AllocateInstancePublicConnection.md)|Specifies the public endpoint of the instance.|
|[ReleaseInstancePublicConnection](/intl.en-US/API Reference/Network management/ReleaseInstancePublicConnection.md)|Releases the public endpoint of the instance.|
|[ModifyDBInstanceConnectionString]()|Modifies the internal or public endpoint and the port of the instance.|

