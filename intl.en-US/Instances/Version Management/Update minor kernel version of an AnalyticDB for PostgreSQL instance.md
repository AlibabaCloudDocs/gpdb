# Update minor kernel version of an AnalyticDB for PostgreSQL instance

To better meet your requirements, AnalyticDB for PostgreSQL updates the minor kernel version on a regular basis. When you create an AnalyticDB for PostgreSQL instance, the latest minor kernel version is used by default. When an update is released, you can update the minor kernel version to use the extensions provided with the latest kernel. This topic describes how to update the minor kernel version of an instance.

**Note:** You can update only the minor kernel version of an instance in the console. The update of the major version from V4.3 to V6.0 is not supported. For more information about how to update the major version from V4.3 to V6.0, see [Compatibility comparisons between AnalyticDB for PostgreSQL V4.3 and V6.0](/intl.en-US/Instances/Version Management/Compatibility comparisons between AnalyticDB for PostgreSQL V4.3 and V6.0.md) and [Check incompatibility between AnalyticDB for PostgreSQL V4.3 and V6.0](/intl.en-US/Instances/Version Management/Check incompatibility between AnalyticDB for PostgreSQL V4.3 and V6.0.md).

**Note:** When you update the minor kernel version of an instance, the instance restarts and is unavailable. We recommend that you update the minor kernel version during off-peak hours.

To update the minor kernel version of an instance, perform the following steps:

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).

2.  In the top navigation bar of the page, select the region where the instance resides. For example, select the China \(Hangzhou\) region.

3.  Find the instance and click its ID.

4.  In the upper-right corner of the page that appears, click **Upgrade Minor Version**. In the message that appears, click **OK**. If your account is bound to your mobile phone number, you must provide a verification code.

    **Note:** The update of a minor version takes 3 to 30 minutes. When an instance is being updated, the instance is unavailable. We recommend that you make appropriate preparations before you update the instance. After the instance is updated, the instance enters the Running state, and you can access the databases of the instance.

    You can view the instance state in the console. If the update is complete, the instance enters the Running state. Otherwise, the instance remains in the Upgrading version state. Before the update, the system checks the minor kernel version of your instance. If the latest kernel is used, the system skips the update and restarts of the instance.


## View the minor kernel version of an instance

Connect to the instance and run `show rds_release_date;`.

```
postgres=# show rds_release_date;
 rds_release_date
------------------
 20190603
(1 row)
```

If you want to check whether you are using the latest kernel or view the features provided by the current kernel, see [.](/intl.en-US/Release Notes/Release notes.md)

## API references

|API|Description|
|---|-----------|
|[UpgradeDBInstance](/intl.en-US/API Reference/Instance management/UpgradeDBInstance.md)|Updates the minor kernel version of an instance.|

