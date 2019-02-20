# Restart an instance {#concept_amy_bkz_52b .concept}

HybridDB for PostgreSQL keeps updating the database kernel version to better meet your requirements. The latest database kernel version is used by default when you create an instance. After a new version is released, you can restart the instance to update its database kernel to enjoy new features in the new version. This document describes how to restart an instance.

## Procedure { .section}

Follow these steps to restart an instance.

1.  Log on the [HybridDB for PostgreSQL console](https://partners-intl.console.aliyun.com/#/gpdb).
2.  Select the region of the instance. For example, China East 1.

3.  Click **Manage** on the right side of the target instance to go to the Basic Information page.

4.  Click **Restart Instance** on the upper right corner of the page and click **OK** in the dialog box. If you have your mobile phone associated to the instance, you must verify the operation by using the verification code sent to your mobile phone.

    **Note:** The restart process usually takes 3 to 30 minutes. During this period, the instance is unavailable for external services. Be prepared in advance. After the instance is restarted, the instance resumes the running state and you can visit the database normally.


After the preceding operations are done, you can return to the console to check the running state of the target instance. When the instance restart is completed, the instance state becomes **Running**. Otherwise, it is in **Restarting**.

## Related API {#section_cz5_t45_vgb .section}

|API|Description|
|---|-----------|
|[EN-US\_TP\_16928.md\#](reseller.en-US/API Reference/Instance management/RestartDBInstance.md#)|Restart an instance.|

