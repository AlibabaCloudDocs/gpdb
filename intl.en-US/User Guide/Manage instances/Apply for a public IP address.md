# Apply for a public IP address {#concept_cj2_cff_hfb .concept}

If your application is deployed on an ECS instance in the same region as your HybridDB for PostgreSQL instance and has the same [network type](../../../../reseller.en-US/Quick Start/Set up an instance/Set the network type.md#), you do not need to apply for a public IP address. If your application is deployed on an ECS that is located in a different region or has a network type different from your HybridDB for PostgreSQL instance, or is deployed on a platform other than Alibaba Cloud, an public IP address is necessary for access to the HybridDB for PostgreSQL instance.

**Note:** Instances in the same region \(can be in different zones\) with the same network type can access each other through the internal network.

## Scenarios { .section}

Public IP addresses and internal IP addresses are used in the following scenarios:

-   Use internal IP addresses only:
    -   Your application is deployed on an ECS instance in the same region as your HybridDB for PostgreSQL instance and shares the same [network type](../../../../reseller.en-US/Quick Start/Set up an instance/Set the network type.md#) with the ECS instance.
-   Use the public IP addresses only:
    -   The ECS instance where your application is deployed and your HybridDB for PostgreSQL instance are in different regions.
    -   Your application is deployed in a third-party system other than Alibaba Cloud.
-   Use both public and internal IP addresses:
    -   Some application modules are deployed on an ECS instance in the same region with the same [network type](../../../../reseller.en-US/Quick Start/Set up an instance/Set the network type.md#), while other modules are deployed on an ECS instance in a different region.
    -   Some modules of the application are deployed on an ECS instance in the same region with the same [network type](../../../../reseller.en-US/Quick Start/Set up an instance/Set the network type.md#), while other modules are deployed in systems other than Alibaba Cloud.

## Notes { .section}

-   Before connecting to the database, you must add the IP addresses or IP ranges to a whitelist. For more information, see [Set up a whitelist](../../../../reseller.en-US/Quick Start/Set up an instance/Set up a whitelist.md#).
-   Exercise caution when you select a public IP address, because the instance may be exposed to security risks. To reach a higher transmission rate and higher security level, we recommend you transfer your applications to the ECS instance located in the same region as the HybridDB for PostgreSQL instance.

## Procedure { .section}

1.  Log on to the [HybridDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
2.  Select the **Region** of the instance.
3.  Locate the target instance. In the Actions column, click **Manage**.
4.  On the Basic Information page, click **Apply for internet address** to go to the Database Connection page. You can also directly click **Database Connection** on the left-side pane.
5.  On the Database Connection page, click **Apply for internet address**.
6.  On the dialog box that appears, click **OK** to generate a public IP address.

After allocating the public IP address, you can click **Release Internet Address** on the Database Connection page to release the public IP address.

