# Configure an alert rule

This topic describes how to configure an alert rule for an AnalyticDB for PostgreSQL instance to monitor resource usage. If the conditions specified in an alert rule are met, the system notifies all contacts in the specified contact groups.

## Background information

The monitoring and alerting feature of AnalyticDB for PostgreSQL is implemented by using [CloudMonitor](https://www.aliyun.com/product/jiankong). CloudMonitor allows you to set metrics and specify contact groups. If an alert is triggered, CloudMonitor notifies all contacts in the specified contact groups. You can maintain contact groups for metrics to ensure that the contacts receive alerts at the earliest opportunity.

CloudMonitor supports threshold-triggered and event-triggered alert rules. The following table lists supported alert rule types for various instance resource types.

|Instance resource type|Threshold-triggered alert|Event-triggered alert|
|----------------------|-------------------------|---------------------|
|Flexible storage mode|Supported|Unsupported|
|Reserved storage mode|Supported|Supported|

**Note:** To enable the monitoring and alerting feature, you must configure alert rules.

## View monitoring data

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/gpdb/cn-hangzhou/list).
2.  Select a region in which the instance is deployed.
3.  Find the instance and click its ID to go to the Basic Information page.
4.  In the upper-right corner of the page, click **CloudMonitor** to go to the [CloudMonitor](https://cloudmonitor.console.aliyun.com/#/alarmservice/product=&searchValue=&searchType=&searchProduct=) console.

    CloudMonitor provides different metrics for instances in flexible storage mode and instances in reserved storage mode. However, metrics cannot be filtered by resource type. All metrics are displayed on the same page.

5.  Move the pointer over a chart and click the enlarge icon to zoom in the chart.

    ![2021040608](../images/p260641.png)

    **Note:** Metric names of compute nodes are displayed in different ways in the **CloudMonitor** and AnalyticDB for PostgreSQL consoles. In the AnalyticDB for PostgreSQL console, the names of compute nodes are displayed. In the CloudMonitor console, the names of the hosts for compute nodes are displayed.


## Configure a threshold-triggered alert

1.  In the [CloudMonitor](https://cloudmonitor.console.aliyun.com/#/alarmservice/product=&searchValue=&searchType=&searchProduct=) console, move the pointer over a chart and click the alert icon to configure a threshold-triggered alert for the corresponding metric.

    ![2021040609](../images/p260659.png)

2.  On the page that appears, configure an alert rule. To configure multiple alert rules, click **Add Alert Rule**. You can create an individual alert rule for each metric. If the value of a metric exceeds the related threshold, the system sends you an alert notification.

    ![2021040606](../images/p260664.png)

    -   Metric: You can select metrics whose names start with flexible for instances in flexible storage mode, and select metrics whose names start with reserved for instances in reserved storage mode.
    -   1 Minute cycle: The monitoring data within 1 minute is aggregated into one monitoring data point for comparison with the specified threshold. CloudMonitor provides one data point within 1 minute. If you select 1 Minute cycle, one data point is generated and no aggregation is required. If you select 5 Minute cycle, five data points are generated and the system aggregates the five data points into one data point.
    -   Duration: If you select Continue for 3 periods for 1 Minute cycle, the alert is triggered after the metric value exceeds the threshold for 3 minutes.
    -   Average value, maximum value, and minimum value: If you select 5 Minute cycle, five data points need to be aggregated. Suppose that the five data points are 10, 20, 30, 40, and 50. The average value is 30, the maximum value is 50, and the minimum value is 10. You can specify a threshold for the average value, the maximum value, or the minimum value.
    -   instance\_component: You can select a specific server or component for which you want to create the alert. You can also select All.
    -   Mute for: This parameter specifies the interval at which an alert notification is sent if the issue is not rectified after the first notification.
3.  Select an existing contact group or create a contact group.
4.  \(Optional\) Enter the email content.
5.  Click **Confirm** to go to the Alert Rules page. The created threshold-triggered alert is displayed.

    On the Threshold Value Alert tab, you can view the alert status or alert logs. You can also disable the alert.


## Configure an event-triggered alert

1.  In the [CloudMonitor](https://cloudmonitor.console.aliyun.com/#/alarmservice/product=&searchValue=&searchType=&searchProduct=) console, choose **Alerts** \> **Alert Rules**.
2.  On the **Event Alert** tab, click **Create Event Alert**.
    -   Product Type: Select AnalyticDB for PostgreSQL.
    -   Event Type: Select All types or a specific type.
    -   Event Level: Select All Levels or a specific level.
    -   Event Name: Select All Events or a specific event.

        |Event name|Description|
        |----------|-----------|
        |The instance is unavailable.|The instance cannot be connected or cannot process requests. No alerts are triggered when the instance is restarted, created, upgraded, deleted, or locked.|
        |The number of long-running transactions is greater than or equal to 5.|If a transaction is active for more than 2 hours but is not in the idle state and holds a lock, it is defined as a long-running transaction.|

3.  Set Contact Group and Notification Method, and then click **OK**.

    On the Event Alert tab, you can view the alert status or alert logs. You can also disable the alert.


