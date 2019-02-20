# Create an instance {#concept_r32_mgr_52b .concept}

You can create or purchase a HybridDB for PostgreSQL instance by using one of the following methods:

-   Create an instance in the HybridDB for PostgreSQL console.
-   Purchase an instance on the HybridDB for PostgreSQL Purchase Page.

This document describes the detailed steps for creating a HybridDB for PostgreSQL instance in the console.

## Billing method {#section_k3g_knl_gfb .section}

HybridDB for PostgreSQL only supports the **Pay-As-You-Go** method.

## Prerequisites {#section_jy4_lnl_gfb .section}

You have registered an account and signed up.

## Procedure { .section}

1.  Log on the [HybridDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
2.  Click **Create Instance**.

3.  Select the instance configuration. The options include:

    -   Region and zone: for guidance on how to select, see [Regions and zones](https://www.alibabacloud.com/help/doc-detail/40654.htm).

    -   Engine: the database type. Only supports **Storage Included**.

    -   Instance Class: the instance type. It is the unit of computing resources. Different classes have different storage spaces and computing capabilities. For details, see [Instance types](../../../../../reseller.en-US/Product Introduction/Instance types.md#).

    -   Instance Groups: the number of purchased instances. The minimum is two. More groups provide higher linear performance.

4.  Confirm your order information, and then click **Buy Now**.

5.  Click **Activate** to activate the instance.

6.  Go to the **Instance List** page of [HybridDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb) to view the newly created instance.


**Note:** The instance initialization takes some time. You can perform subsequent operations only after the instance status becomes Running.

## Related API {#section_my2_bm5_vgb .section}

|API|Description|
|---|-----------|
|[EN-US\_TP\_16918.md\#](reseller.en-US/API Reference/Instance management/CreateDBInstance.md#)|Creates a database instance.|

