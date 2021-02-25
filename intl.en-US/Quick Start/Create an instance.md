# Create an instance

This topic describes how to create an instance in the AnalyticDB for PostgreSQL console.

## Prerequisites

-   An Alibaba Cloud account is created. To create an Alibaba Cloud account, go to the [Alibaba Cloud official website](http://www.aliyun.com/).

## Procedure

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/gpdb/cn-hangzhou/list).
2.  In the upper-right corner, click **Create Instance**. The AnalyticDB for PostgreSQL buy page appears.
3.  Select Subscription or Pay-As-You-Go as the billing method.

    -   **Pay-as-you-go**: A pay-as-you-go instance is charged on an hourly basis based on your actual resource usage. We recommend that you select this billing method for short-term use. You can release the instance that is no longer used. This helps you reduce costs.
    -   **Subscription**: You must pay an upfront subscription fee when you create an instance. We recommend that you select this billing method for long-term use because this method is more cost-effective than the pay-as-you-go billing method. Larger discounts are provided for longer subscription periods.
    **Note:** You can change the billing method of an instance from pay-as-you-go to subscription. However, you cannot change the billing method from subscription to pay-as-you-go.

4.  Configure the following parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Region**|Select a region to deploy the instance. You cannot change the region after the instance is created.

    -   We recommend that you select a region near the location of your desired users to improve access speed.
    -   Make sure that the instance is deployed in the same region as that of the Elastic Compute Service \(ECS\) instance to which you want to connect. Otherwise, the AnalyticDB for PostgreSQL instance and the ECS instance can only communicate over the Internet instead of an internal network. This may compromise performance. |
    |**Zone**|Select a zone to deploy the instance. Each zone is considered a physical location. Zones within a region are independent of each other. No differences exist among these zones.

You can deploy your AnalyticDB for PostgreSQL instance and ECS instance in the same zone or in different zones. |
    |**Network Type**|Set the network type of the instance. The default value is VPC. A virtual private cloud \(VPC\) is an isolated virtual network that provides higher security and better performance than the classic network. Before you specify this parameter, make sure that you have created a VPC and a VSwitch that are deployed in the same region as that of the instance. For more information, see .|
    |**VPC**|Select a VPC.|
    |**VSwitch**|Select a VSwitch in the specified VPC.|
    |**Instance resource type**|**Instance Resource Type**: You cannot extend independent disks on the fly in the console. |
    |**Engine Version**|Only **6.0** is supported. |
    |**Node specification（segment）**|Select the specifications of the computing resources. Different node types have different storage capacities and computing capabilities. For more information about specifications, see [Instance specifications](/intl.en-US/Specifications and Pricing/Instance specifications.md) for AnalyticDB for PostgreSQL.|
    |**Node Number（segment）**|Set the number of nodes in the instance. An instance must contain a minimum of two nodes. The performance of an instance improves if the number of nodes increases.|
    |**Storage Disk Type**|Select **Enhanced SSD \(ESSD\)** or **Ultra Disk**. An **Enhanced SSD \(ESSD\)** outperforms **Ultra Disk** in the read/write performance whereas the latter is more cost-effective.|
    |**Single Node Storage Capacity**|Set the storage capacity of each node in the instance.|

5.  After you configure the parameters, click **Buy Now**.
6.  On the Confirm Order page, select the Terms of Service check box, and click **Pay**.
7.  Verify that the instance is created on the Instances page.

    **Note:** Wait until the instance enters the **Running** state. Then, you can view and manage the instance.


## Related operations

|API|Description|
|---|-----------|
|[t16918.md\#](/intl.en-US/API Reference/Instance management/CreateDBInstance.md)|Creates an instance.|

