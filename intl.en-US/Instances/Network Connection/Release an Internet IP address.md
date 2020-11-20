# Release an Internet IP address

If the network environment changed after the Internet address is allocated, you can release the Internet address on AnalyticDB for PostgreSQL console if you don't need it any more. After releasing the Internet address, make sure to change the application configurations which related to this address.

Before performing this operation, please read the following scenarios.

## Scenarios

Internet IP addresses and intranet IP addresses are used in the following scenarios:

-   Use an intranet IP addresses only:
    -   Your application is deployed on an ECS instance in the same region as your AnalyticDB for PostgreSQL instance and shares the same [network type](/intl.en-US/FAQ/Set the network type.md) with the ECS instance.
-   Use an Internet IP addresses only:
    -   The ECS instance where your application is deployed and your AnalyticDB for PostgreSQL instance are in different regions.
    -   Your application is deployed in a third-party system other than Alibaba Cloud.
-   Use both Internet and intranet IP addresses:
    -   Some application modules are deployed on an ECS instance in the same region with the same [network type](/intl.en-US/FAQ/Set the network type.md), while other modules are deployed on an ECS instance in a different region.
    -   Some modules of the application are deployed on an ECS instance in the same region with the same [network type](/intl.en-US/FAQ/Set the network type.md), while other modules are deployed in systems other than Alibaba Cloud.

## Procedure

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).
2.  Log on to the [AnalyticDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
3.  Select the **Region** of the instance.
4.  Click the ID of the instance to go to its **Basic Information** page.
5.  Click **Database Connection** on the left-side navigation.
6.  On the Database Connection page, Click **Release Internet Address**.

    If you haven't applied for an Internet address since you created an instance, there is only **Apply for Internet address** on the Database Connection page.

7.  Click **OK** on the dialog box to release the Internet address.

## Related API

|API|Description|
|---|-----------|
|[ReleaseInstancePublicConnection](/intl.en-US/API Reference/Network management/ReleaseInstancePublicConnection.md)|Release the Internet address of an instance.|

