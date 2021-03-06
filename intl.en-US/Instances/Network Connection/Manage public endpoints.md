# Manage public endpoints

If your application is deployed on an ECS instance that is located within the same region and has the same [network type](/intl.en-US/FAQ/How do I switch the network type?.md) as your AnalyticDB for PostgreSQL instance, you do not need to apply for a public endpoint. If your application is deployed on a third-party system or on an ECS instance that is located within a different region or has a different network type from your AnalyticDB for PostgreSQL instance, you must apply for a public endpoint.

**Note:** If your application is deployed on an ECS instance that is located within the same region \(but maybe in different zones\) and has the same network type as your AnalyticDB for PostgreSQL instance, the application can connect to the AnalyticDB for PostgreSQL instance over the internal network.

## Scenarios

Internal and public endpoints apply to the following scenarios:

-   Scenarios where only an internal endpoint is required:
    -   Your application is deployed on an ECS instance that is located within the same region and has the same [network type](/intl.en-US/FAQ/How do I switch the network type?.md) as your AnalyticDB for PostgreSQL instance.
-   Scenarios where only a public endpoint is required:
    -   Your application is deployed on an ECS instance that is located within a region different from your AnalyticDB for PostgreSQL instance.
    -   Your application is deployed on a third-party system.
-   Scenarios where both an internal endpoint and a public endpoint are required:
    -   Some modules of your application are deployed on an ECS instance that is located within the same region and have the same [network type](/intl.en-US/FAQ/How do I switch the network type?.md) as your AnalyticDB for PostgreSQL instance, but other modules are deployed on an ECS instance that is located within a region different from your AnalyticDB for PostgreSQL instance.
    -   Some modules of your application are deployed on an ECS instance that is located within the same region and have the same [network type](/intl.en-US/FAQ/How do I switch the network type?.md) as your AnalyticDB for PostgreSQL instance, but other modules are deployed on a third-party system.

## Precautions

-   Before you connect your application to your AnalyticDB for PostgreSQL instance, you must add the IP address or the corresponding CIDR block of the application to a whitelist of the instance. For more information, see [Configure an IP address whitelist for an AnalyticDB for PostgreSQL instance](/intl.en-US/Quick Start/Configure an IP address whitelist for an AnalyticDB for PostgreSQL instance.md).
-   If you connect to the instance by using the public endpoint, security is compromised. Proceed with caution. To increase security and accelerate data transfer, we recommend that you migrate your application to an ECS instance that is located within the same region as your AnalyticDB for PostgreSQL instance.

## Apply for a public endpoint

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).
2.  In the upper-left corner of the page, select the **region** where the desired instance resides from the drop-down list.
3.  Find the instance for which you want to apply for a public endpoint, and click its ID.
4.  On the Basic Information page, click **Apply for Public Endpoint**. Alternatively, in the left-side navigation pane, click **Database Connection**.
5.  On the Database Connection page, click **Apply for Public Endpoint**.
6.  A public endpoint is generated.****

On the Database Connection page, you can click **Release Public Endpoint** to release the public endpoint.

## Release a public endpoint

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).
2.  In the upper-left corner of the page, select the **region** where the desired instance resides from the drop-down list.
3.  Find the instance for which you want to release the public endpoint and click the instance ID.
4.  In the left-side navigation pane, click Database Connection.
5.  On the Database Connection page, click **Release Public Endpoint**.

    If you have not applied for a public endpoint, only the **Apply for Public Endpoint** option is displayed on the Database Connection page.

6.  In the message that appears, click **OK** to release the public endpoint.

## Related API operations

|API|Description|
|---|-----------|
|[AllocateInstancePublicConnection](/intl.en-US/API Reference/Network management/AllocateInstancePublicConnection.md)|Applies for a public endpoint for an AnalyticDB for PostgreSQL instance.|
|[ReleaseInstancePublicConnection](/intl.en-US/API Reference/Network management/ReleaseInstancePublicConnection.md)|Releases the public endpoint of an AnalyticDB for PostgreSQL instance.|

