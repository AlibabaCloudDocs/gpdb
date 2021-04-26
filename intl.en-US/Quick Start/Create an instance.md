# Create an instance

This topic describes how to create an instance in the AnalyticDB for PostgreSQL console.

## Prerequisites

-   An Alibaba Cloud account is created. To create an Alibaba Cloud account, go to the [Alibaba Cloud official website](http://www.aliyun.com/).
-   If this is the first time that you create an AnalyticDB for PostgreSQL instance, make sure that a service-linked role is created. For more information, see [Service linked role](/intl.en-US/API Reference/Service linked role.md).
    1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/gpdb/cn-hangzhou/list).
    2.  In the upper-right corner, click **Create Instance**.

        ![5020001](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6588149161/p268310.png)

    3.  In the **Create Service Linked Role** message, click **OK**.

        ![Create Service-linked Role](../images/p161621.png)


## Procedure

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/gpdb/cn-hangzhou/list).
2.  In the upper-right corner, click **Create Instance** to go to the AnalyticDB for PostgreSQL buy page.
3.  Set the billing method to Subscription or Pay-as-you-go.

    -   **Pay-as-you-go**: A pay-as-you-go instance is charged on an hourly basis based on your actual resource usage. We recommend that you select this billing method for short-term use. You can release the instances that are no longer used. This helps you reduce costs.
    -   **Subscription**: You must pay an upfront subscription fee when you create an instance. We recommend that you select this billing method for long-term use because this method is more cost-effective than the pay-as-you-go billing method. Larger discounts are provided for longer subscription periods.
    **Note:** You can change the billing method of an instance from pay-as-you-go to subscription. However, you cannot change the billing method from subscription to pay-as-you-go.

4.  Configure the following parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Region**|Select a region to deploy the instance. You cannot change the region after you create the instance.

    -   We recommend that you select a region that is close to your business to improve access speed.
    -   Make sure that the instance is deployed in the same region as that of the Elastic Compute Service \(ECS\) instance to which you want to connect. Otherwise, the AnalyticDB for PostgreSQL and ECS instances can communicate only over the Internet instead of an internal network, which may compromise performance. |
    |**Zone**|Select a zone to deploy the instance. A zone is an independent geographical location within a region. No substantial differences exist between different zones.

You can create the AnalyticDB for PostgreSQL instance in the same zone as the ECS instance to which you want to connect or in a different zone. |
    |**Network Type**|Set the network type of the instance. The default value is VPC. A virtual private cloud \(VPC\) is an isolated virtual network that provides higher security and better performance than the classic network. Before you specify this parameter, make sure that you have created a VPC and a vSwitch in the region where you want to create the instance. For more information, see [Work with VPCs](/intl.en-US/VPCs and vSwitchs/Work with VPCs.md).|
    |**VPC**|Select a VPC.|
    |**VSwitch**|Select a vSwitch in the specified VPC.|
    |**Instance Resource Type**|    -   Flexible Storage Mode: You can scale up each disk and smoothly scale out nodes online.
    -   Reserved Storage Mode: You cannot scale up each disk or smoothly scale out nodes online. |
    |**Engine Version**|Only 6.0 Standard Edition is supported. |
    |**Number of Coordinator Nodes**|Set the number of coordinator nodes in the instance. The minimum value is 1.|
    |**Compute Node Specifications**|Select specifications for each compute node. The storage space and compute capability of a compute node vary based on the specified specifications. For more information about specifications, see [Instance specifications](/intl.en-US/Specifications and Pricing/Instance specifications.md) for AnalyticDB for PostgreSQL.|
    |**Number of Compute Nodes**|Set the number of compute nodes in the instance. The minimum value is 2. The performance of an instance linearly increases with the number of coordinator nodes.|
    |**Storage Disk Type**|Select Enhanced SSD \(ESSD\) or Ultra Disk. ESSDs outperform ultra disks in the read/write performance, whereas ultra disks are more cost-effective.|
    |**Single Node Storage Capacity**|Set the storage capacity of each node in the instance.|

5.  After you configure the parameters, click **Buy Now**.
6.  On the Confirm Order page, read and select AnalyticDB for PostgreSQL Agreement of Service, and click **Activate Now**.
7.  Verify that the instance is created on the Instances page.

    **Note:** After the instance enters the **Running** state, you can view and manage the instance.


## Related operations

|Operation|Description|
|---------|-----------|
|[CreateDBInstance](/intl.en-US/API Reference/Instance management/CreateDBInstance.md)|Creates an instance.|

