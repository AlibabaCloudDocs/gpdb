# Update the minor kernel version of an AnalyticDB for PostgreSQL instance

To better meet your requirements, AnalyticDB for PostgreSQL updates the minor kernel version on a regular basis. By default, the latest minor kernel version is used when you create an AnalyticDB for PostgreSQL instance. When a new version is released, you can update the minor kernel version to use the extensions that are provided with the latest kernel version. This topic describes how to update the minor kernel version of an instance.

**Note:** The console supports only updates to the minor kernel version. The upgrade of the major version from V4.3 to V6.0 is not supported in the console. For more information about how to upgrade the major version from V4.3 to V6.0, see [Compatibility comparisons between AnalyticDB for PostgreSQL V4.3 and V6.0](/intl.en-US/Instances/Version Management/Compatibility comparisons between AnalyticDB for PostgreSQL V4.3 and V6.0.md) and [Check incompatibility between AnalyticDB for PostgreSQL V4.3 and V6.0](/intl.en-US/Instances/Version Management/Check incompatibility between AnalyticDB for PostgreSQL V4.3 and V6.0.md).

**Note:** When you update the minor kernel version of an instance, the instance restarts and is unavailable. We recommend that you update the minor kernel version during off-peak hours.

To update the minor kernel version of an instance, perform the following steps:

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/gpdb/cn-hangzhou/list).

2.  In the top navigation bar, select the region where the instance resides. For example, select the China \(Hangzhou\) region.

3.  Find the instance and click its ID. In the upper-right corner of the Basic Information page, click **Upgrade Minor Version**. In the message that appears, click **OK**. If your account is bound to your mobile phone number, you must provide a verification code.

    **Note:** The update of a minor kernel version takes 3 to 30 minutes. When an instance is being updated, it is unavailable. We recommend that you make appropriate preparations before you update the instance. After the instance is updated, it enters the Running state. Then, you can access the databases of the instance.

    After you complete the preceding steps, check the status of the instance. If the update is complete, the instance enters the Running state. Otherwise, the instance remains in the Upgrading version state. Before the update, the system checks the minor kernel version of your instance. If the latest kernel is used, the system skips the process that updates and restarts the instance.


## View the minor kernel version of an instance

To view the latest minor kernel version of an instance, connect to the instance and execute the `show rds_release_date;` statement.

```
postgres=# show rds_release_date;
 rds_release_date
------------------
 20190603
(1 row)
```

If you want to check whether you are using the latest kernel version or view the features provided by the current kernel version, see [Release notes]().

## Related API operations

|Operation|Description|
|---------|-----------|
|[UpgradeDBInstance](/intl.en-US/API Reference/Instance management/UpgradeDBInstance.md)|Updates the minor kernel version of an instance.|

