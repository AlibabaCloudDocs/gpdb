# Change connection addresses {#concept_bcx_2gf_hfb .concept}

In HybridDB for PostgreSQL, you can change the connection address of an instance. For example, if you switch your service to a different HybridDB for PostgreSQL instance, you do not need to modify the application. You only need to configure the new instance to use the connection address of the old instance.

## Prerequisites { .section}

-   The instance is running.
-   The instance has set up a whitelist.

## Procedure { .section}

1.  Log on to the [HybridDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
2.  Select the **Region** of the instance.
3.  Locate the target instance. In the Actions column, click **Manage**.
4.  In the left-side navigation pane, click **Database Connection**.
5.  On the Database Connection page, click **Modify Connection Address**.
6.  In the dialog box displayed, click **Connection Type** and select a network type.

    You can select **Intranet Address** or **Internet Address**. The **Internet Address** option is only available after you have applied for a public IP address.

7.  Enter the relevant information in **Connection Address** and **Port**, and click **OK**.

    After the page is refreshed, the new connection address is displayed.


## Related API {#section_cz5_t45_vgb .section}

|API|Description|
|---|-----------|
|[EN-US\_TP\_16946.md\#](reseller.en-US/API Reference/Network management/ModifyDBInstanceConnectionString.md#)|Modifies the connection address and the port for an instance.|

