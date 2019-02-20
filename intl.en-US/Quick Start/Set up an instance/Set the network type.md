# Set the network type {#concept_jw3_fmr_52b .concept}

Alibaba Cloud ApsaraDB supports two network types: classic network and Virtual Private Cloud \(VPC\). By default, HybridDB for PostgreSQL uses the classic network. If you want to use VPC, ensure that the HybridDB for PostgreSQL instance and the VPC are in the same region.

This document mainly describes the differences between the two network types and how to configure the settings.

## Background { .section}

The classic network and VPC have the following differences:

-   Classic network: The cloud service in a classic network is not isolated, and unauthorized access can only be blocked by the whitelist policy of the cloud service.

-   Virtual Private Cloud \(VPC\): VPC helps you build an isolated network environment on Alibaba Cloud. You can customize the routing table, IP address range and gateway in the VPC. You can also combine your IDC and cloud resources on the Alibaba Cloud VPC into a virtual IDC by using a leased line or VPN to seamlessly migrate applications to the cloud.


## Procedure { .section}

1.  Create a VPC in the same region with the target HybridDB for PostgreSQL instance. For detailed steps, see [Create a VPC](https://www.alibabacloud.com/help/doc-detail/27710.htm).

2.  Log on the [HybridDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
3.  Select the region where the target instance is located.

4.  Click the ID of the instance to go to the **Basic Information** page of the instance.

5.  Click **Database Connection**.

6.  Click **Switch to VPC**.

7.  Select a VPC and virtual switch, and then click **OK**.

    **Note:** 

    After the network is switched to VPC, the original intranet address changes from a classic network address to a VPC address. ECS on the classic network can no longer access the HybridDB for PostgreSQL instance. The original Internet address remains unchanged.


## Related API {#section_kgx_2wt_vgb .section}

|API|Description|
|---|-----------|
|[EN-US\_TP\_16947.md\#](reseller.en-US/API Reference/Network management/ModifyDBInstanceNetworkType.md#)|Switches the network connection type for an instance.|

