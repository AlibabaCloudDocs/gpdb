# Configure parameters

AnalyticDB for PostgreSQL allows you to configure some key kernel parameters to meet customized requirements in various scenarios. This topic describes how to configure these parameters in the AnalyticDB for PostgreSQL console.

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/gpdb/cn-hangzhou/list).

2.  In the top navigation bar, select the region where the instance for which you want to configure parameters resides.

3.  Find the instance and click its ID to go to the Basic Information page.

4.  In the left-side navigation pane, click **Parameter Configuration**.

5.  Click the Edit icon next to the parameter that you want to modify, modify the value of the parameter, and then click **OK**.

    |Parameter|Description|
    |---------|-----------|
    |statement\_timeout|The maximum timeout period. The default value is 10800000. Unit: milliseconds. If the timeout period exceeds the value of the parameter, the statement is terminated.|
    |rds\_master\_mode|The consistency model of the AnalyticDB for PostgreSQL instance. This parameter is valid only when the instance has multiple coordinator nodes. Default value: Single. Valid values:     -   Single: The instance has only a single coordinator node.
    -   multi\_write\_ec: the session consistency model. Monotonic reads, monotonic writes, reads after writes, and writes after reads are consistent in a session.
    -   multi\_write\_sc: the global consistency model. This mode provides compatibility of atomicity, consistency, isolation, durability \(ACID\) and linearizability. |


