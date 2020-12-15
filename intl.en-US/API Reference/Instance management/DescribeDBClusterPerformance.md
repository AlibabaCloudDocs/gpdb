# DescribeDBClusterPerformance

You can call this operation to query specified performance metric values for an AnalyticDB for PostgreSQL instance over a specific time range.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeDBClusterPerformance&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeDBClusterPerformance|The operation that you want to perform. Set the value to DescribeDBClusterPerformance. |
|DBInstanceId|String|Yes|gp-xxxxxxxxx|The ID of the instance. |
|EndTime|String|Yes|2020-11-03T15:10Z|The end of the time range to query. Specify the time in the ISO 8601 standard in the `YYYY-MM-DDTHH:mmZ` format. The end time must be later than the start time. |
|Key|String|Yes|adbpg\_group\_cpu\_used\_percent|One or more performance metrics that you want to query. Separate multiple metrics with `commas (,)`. For more information, see [Performance parameters](~~86943~~). |
|StartTime|String|Yes|2020-11-03T15:00Z|The beginning of the time range to query. Specify the time in the ISO 8601 standard in the `YYYY-MM-DDTHH:mmZ` format. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|DBClusterId|String|gp-xxxxxxxxx|The ID of the instance. |
|EndTime|String|2020-11-03T15:10Z|The end time of the query. The time follows the ISO 8601 standard in the YYYY-MM-DDTHH:mmZ format. |
|PerformanceKeys|Array of PerformanceKey|N/A|One or more performance metric values of the instance. |
|Name|String|adbpg\_group\_cpu\_used\_percent|The name of the performance metric. |
|Series|Array of SeriesItem|N/A|One or more performance metrics of compute nodes for the instance. |
|Name|String|standby-xxxxxxxx-cpu|The name of the compute node or the compute group. |
|Role|String|standby|The role of the compute node. The role can be master, standby, or segment. |
|Values|Array of ValueItem|N/A|One or more performance metric values. Each value matches a compute node. |
|Point|List|\[ "2020-09-27T07:00:00+08:00", "5.84" \],\[ "2020-09-27T07:01:00+08:00", "5.09" \]|An array of performance data. |
|Unit|String|%|The unit of the performance data. |
|RequestId|String|BBE00C04-A3E8-4114-881D-0480A72CB92E|The ID of the request. |
|StartTime|String|2020-11-03T15:00Z|The start time of the query. The time follows the ISO 8601 standard in the YYYY-MM-DDTHH:mmZ format. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=DescribeDBClusterPerformance
&DBInstanceId=gp-xxxxxxxxx
&EndTime=2020-09-27T07:10Z
&Key=adbpg_group_cpu_used_percent
&StartTime=2020-09-27T07:00Z
&<Common request parameters>
```

Sample success responses

`XML` format

```
<PerformanceKeys>
    <Series>
        <Role>master</Role>
        <Values>
            <Point>2020-11-03T15:00:00+08:00</Point>
            <Point>5.84</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:01:00+08:00</Point>
            <Point>5.31</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:02:00+08:00</Point>
            <Point>5.28</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:03:00+08:00</Point>
            <Point>5.27</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:04:00+08:00</Point>
            <Point>5.63</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:05:00+08:00</Point>
            <Point>5.44</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:06:00+08:00</Point>
            <Point>5.27</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:07:00+08:00</Point>
            <Point>5.27</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:08:00+08:00</Point>
            <Point>5.96</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:09:00+08:00</Point>
            <Point>6.51</Point>
        </Values>
        <Name>master-xxxxxxxx-cpu</Name>
    </Series>
    <Series>
        <Role>standby</Role>
        <Values>
            <Point>2020-11-03T15:00:00+08:00</Point>
            <Point>0.01</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:01:00+08:00</Point>
            <Point>0</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:02:00+08:00</Point>
            <Point>0.01</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:03:00+08:00</Point>
            <Point>0</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:04:00+08:00</Point>
            <Point>0</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:05:00+08:00</Point>
            <Point>0.01</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:06:00+08:00</Point>
            <Point>0.01</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:07:00+08:00</Point>
            <Point>0.01</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:08:00+08:00</Point>
            <Point>0</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:09:00+08:00</Point>
            <Point>0.01</Point>
        </Values>
        <Name>standby-xxxxxxxx-cpu</Name>
    </Series>
    <Series>
        <Role>segment</Role>
        <Values>
            <Point>2020-11-03T15:00:00+08:00</Point>
            <Point>0.13</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:01:00+08:00</Point>
            <Point>0.12</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:02:00+08:00</Point>
            <Point>0.14</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:03:00+08:00</Point>
            <Point>0.13</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:04:00+08:00</Point>
            <Point>0.12</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:05:00+08:00</Point>
            <Point>0.14</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:06:00+08:00</Point>
            <Point>0.16</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:07:00+08:00</Point>
            <Point>0.16</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:08:00+08:00</Point>
            <Point>0.16</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:09:00+08:00</Point>
            <Point>0.12</Point>
        </Values>
        <Name>compute-node-xxxxxxxx-cpu</Name>
    </Series>
    <Series>
        <Role>segment</Role>
        <Values>
            <Point>2020-11-03T15:00:00+08:00</Point>
            <Point>0.15</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:01:00+08:00</Point>
            <Point>0.13</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:02:00+08:00</Point>
            <Point>0.12</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:03:00+08:00</Point>
            <Point>0.12</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:04:00+08:00</Point>
            <Point>0.12</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:05:00+08:00</Point>
            <Point>0.13</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:06:00+08:00</Point>
            <Point>0.15</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:07:00+08:00</Point>
            <Point>0.17</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:08:00+08:00</Point>
            <Point>0.15</Point>
        </Values>
        <Values>
            <Point>2020-11-03T15:09:00+08:00</Point>
            <Point>0.12</Point>
        </Values>
        <Name>compute-node-xxxxxxxx-cpu</Name>
    </Series>
    <Unit>%</Unit>
    <Name>adbpg_group_cpu_used_percent</Name>
</PerformanceKeys>
<RequestId>8E8990F0-C81E-4C94-8F51-5F825D359E79</RequestId>
<EndTime>2020-11-03T07:10Z</EndTime>
<DBClusterId>gp-xxxxxxxx</DBClusterId>
<StartTime>2020-11-03T07:00Z</StartTime>
```

`JSON` format

```
{
    "PerformanceKeys": [
        {
            "Series": [
                {
                    "Role": "master", 
                    "Values": [
                        {
                            "Point": [
                                "2020-11-03T15:00:00+08:00", 
                                "5.84"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:01:00+08:00", 
                                "5.31"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:02:00+08:00", 
                                "5.28"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:03:00+08:00", 
                                "5.27"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:04:00+08:00", 
                                "5.63"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:05:00+08:00", 
                                "5.44"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:06:00+08:00", 
                                "5.27"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:07:00+08:00", 
                                "5.27"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:08:00+08:00", 
                                "5.96"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:09:00+08:00", 
                                "6.51"
                            ]
                        }
                    ], 
                    "Name": "master-xxxxxxxx-cpu"
                }, 
                {
                    "Role": "standby", 
                    "Values": [
                        {
                            "Point": [
                                "2020-11-03T15:00:00+08:00", 
                                "0.01"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:01:00+08:00", 
                                "0"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:02:00+08:00", 
                                "0.01"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:03:00+08:00", 
                                "0"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:04:00+08:00", 
                                "0"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:05:00+08:00", 
                                "0.01"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:06:00+08:00", 
                                "0.01"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:07:00+08:00", 
                                "0.01"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:08:00+08:00", 
                                "0"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:09:00+08:00", 
                                "0.01"
                            ]
                        }
                    ], 
                    "Name": "standby-xxxxxxxx-cpu"
                }, 
                {
                    "Role": "segment", 
                    "Values": [
                        {
                            "Point": [
                                "2020-11-03T15:00:00+08:00", 
                                "0.13"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:01:00+08:00", 
                                "0.12"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:02:00+08:00", 
                                "0.14"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:03:00+08:00", 
                                "0.13"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:04:00+08:00", 
                                "0.12"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:05:00+08:00", 
                                "0.14"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:06:00+08:00", 
                                "0.16"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:07:00+08:00", 
                                "0.16"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:08:00+08:00", 
                                "0.16"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:09:00+08:00", 
                                "0.12"
                            ]
                        }
                    ], 
                    "Name": "compute-node-xxxxxxxx-cpu"
                }, 
                {
                    "Role": "segment", 
                    "Values": [
                        {
                            "Point": [
                                "2020-11-03T15:00:00+08:00", 
                                "0.15"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:01:00+08:00", 
                                "0.13"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:02:00+08:00", 
                                "0.12"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:03:00+08:00", 
                                "0.12"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:04:00+08:00", 
                                "0.12"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:05:00+08:00", 
                                "0.13"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:06:00+08:00", 
                                "0.15"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:07:00+08:00", 
                                "0.17"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:08:00+08:00", 
                                "0.15"
                            ]
                        }, 
                        {
                            "Point": [
                                "2020-11-03T15:09:00+08:00", 
                                "0.12"
                            ]
                        }
                    ], 
                    "Name": "compute-node-xxxxxxxx-cpu"
                }
            ], 
            "Unit": "%", 
            "Name": "adbpg_group_cpu_used_percent"
        }
    ], 
    "RequestId": "8E8990F0-C81E-4C94-8F51-5F825D359E79", 
    "EndTime": "2020-11-03T07:10Z", 
    "DBClusterId": "gp-xxxxxxxx", 
    "StartTime": "2020-11-03T07:00Z"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

