# DescribeDBClusterPerformance

查看某个用户实例在某个时间段内指定性能参数的性能监控数据。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeDBClusterPerformance&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeDBClusterPerformance|系统规定参数。取值：DescribeDBClusterPerformance。 |
|DBInstanceId|String|是|gp-xxxxxxxxx|实例ID。 |
|EndTime|String|是|2020-11-03T15:10Z|查询结束时间，必须晚于查询开始时间，格式：`YYYY-MM-DDTHH:mmZ`。 |
|Key|String|是|adbpg\_group\_cpu\_used\_percent|性能参数名称，多个指标用英文半角`,`分隔，详见[性能参数表](~~86943~~)。 |
|StartTime|String|是|2020-11-03T15:00Z|查询开始时间，格式：`YYYY-MM-DDTHH:mmZ`。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|DBClusterId|String|gp-xxxxxxxxx|实例ID。 |
|EndTime|String|2020-11-03T15:10Z|查询结束时间，格式：YYYY-MM-DDTHH:mmZ。 |
|PerformanceKeys|Array of PerformanceKey| |实例性能参数值列表。 |
|Name|String|adbpg\_group\_cpu\_used\_percent|性能参数名称。 |
|Series|Array of SeriesItem| |各节点的性能参数集合。 |
|Name|String|standby-xxxxxxxx-cpu|计算节点或计算组名称。 |
|Role|String|standby|节点的角色，master/standby/segment。 |
|Values|Array of ValueItem| |性能参数值，每个值对应一个节点。 |
|Point|List|\[ "2020-09-27T07:00:00+08:00", "5.84" \],\[ "2020-09-27T07:01:00+08:00", "5.09" \]|数组格式。 |
|Unit|String|%|数据单位。 |
|RequestId|String|BBE00C04-A3E8-4114-881D-0480A72CB92E|请求ID。 |
|StartTime|String|2020-11-03T15:00Z|查询开始时间，格式：YYYY-MM-DDTHH:mmZ。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeDBClusterPerformance
&DBInstanceId=gp-xxxxxxxxx
&EndTime=2020-09-27T07:10Z
&Key=adbpg_group_cpu_used_percent
&StartTime=2020-09-27T07:00Z
&<公共请求参数>
```

正常返回示例

`XML` 格式

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

`JSON` 格式

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

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

