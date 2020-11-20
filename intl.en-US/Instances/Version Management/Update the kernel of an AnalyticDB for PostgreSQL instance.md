# Update the kernel of an AnalyticDB for PostgreSQL instance

To better meet your requirements, AnalyticDB for PostgreSQL regularly updates its kernels. When you create an AnalyticDB for PostgreSQL instance, the latest kernel is used by default. When a kernel update is released, you need to update the kernel of your instance to use the extensions provided with the latest kernel. This topic describes how to update the kernel of an instance.

**Note:** When you update the kernel of an instance, the instance restarts and is unavailable during the restart. We recommend that you update a kernel during off-peak hours.

To update the kernel of an instance, follow these steps:

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).

2.  In the top navigation bar of the page, select the region where the target instance resides, such as China \(Hangzhou\).

3.  Find the target instance and click its ID.

4.  In the upper-right corner of the page that appears, click **Upgrade Minor Version**. In the message that appears, click **OK**. If your account is bound to your mobile phone number, you must provide a verification code.

    **Note:** The update of a kernel can take 3 to 30 minutes, and the target instance is unavailable during the update. Therefore, we recommend that you make appropriate preparations before the update. After the update is complete, the instance enters the Running state, and you can access databases in it.

    You can view the instance status in the console. If the update is complete, the instance enters the Running state. Otherwise, the instance remains in the Upgrading state. Before the update, the system checks the kernel version of your instance. If you are already using the latest kernel, the system skips the update and instance restart.


## View the kernel version of an instance

Connect to the target instance and run `show rds_release_date;`.

```
postgres=# show rds_release_date;
 rds_release_date
------------------
 20190603
(1 row)
```

If you want to check whether you are using the latest kernel or want to view the features provided by the current kernel, see [Release notes](/intl.en-US/Release Notes/Release notes.md).

## Related operations

|Operation|Description|
|---------|-----------|
|[UpgradeDBInstance](/intl.en-US/API Reference/Instance management/UpgradeDBInstance.md)|Updates the kernel of an AnalyticDB for PostgreSQL instance.|

