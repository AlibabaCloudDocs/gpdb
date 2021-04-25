# DescribeParameters

You can call this operation to query parameter configurations of an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeParameters&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeParameters|The operation that you want to perform. Set the value to DescribeParameters. |
|DBInstanceId|String|Yes|gp-xxxxxx|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Parameters|Array of Parameters| |Details about the parameter configurations. |
|CurrentValue|String|1000|The current value of the parameter. |
|ForceRestartInstance|String|false|Indicates whether a restart is required for parameter modifications to take effect. |
|IsChangeableConfig|String|true|Indicates whether the parameter is modifiable. |
|OptionalRange|String|\[1-10000\]|The valid values of the parameter. |
|ParameterDescription|String|param description|The description of the parameter. |
|ParameterName|String|testkey|The name of the parameter. |
|ParameterValue|String|800|The default value of the parameter. |
|RequestId|String|7565770E-7C45-462D-BA4A-8A5396F2CAD1|The ID of the request. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeParameters
&<Common request parameters>
&DBInstanceId=gp-xxxxxx
```

Sample success responses

`XML` format

```
<Parameters>
    <OptionalRange>[0-2147483647]</OptionalRange>    <ParameterValue>10800000</ParameterValue>    <IsChangeableConfig>true</IsChangeableConfig>    <CurrentValue>11800000</CurrentValue>    <ParameterName>statement_timeout</ParameterName>    <ParameterDescription>Sets the maximum allowed duration of any statement, A value of 0 turns off the timeout.</ParameterDescription>    <ForceRestartInstance>false</ForceRestartInstance></Parameters><Parameters>    <OptionalRange>[multi_write_ec|multi_write_sc|single|multi_read]</OptionalRange>    <ParameterValue>single</ParameterValue>    <IsChangeableConfig>true</IsChangeableConfig>    <CurrentValue>single</CurrentValue>    <ParameterName>rds_master_mode</ParameterName>    <ParameterDescription>Enable global strong consistency when rds_master_mode is set to multi_write_sc, and global eventual consistency when rds_master_mode is set to multi_write_ec</ParameterDescription>    <ForceRestartInstance>true</ForceRestartInstance></Parameters><RequestId>8CA0F95F-0F2D-4E85-ADD1-32C7FCE5E17D</RequestId>
    <ParameterValue>10800000</ParameterValue>
    <IsChangeableConfig>true</IsChangeableConfig>
    <CurrentValue>11800000</CurrentValue>
    <ParameterName>statement_timeout</ParameterName>
    <ParameterDescription>Sets the maximum allowed duration of any statement，A value of 0 turns off the timeout.</ParameterDescription>
    <ForceRestartInstance>false</ForceRestartInstance>
</Parameters>
<Parameters>
    <OptionalRange>[multi_write_ec|multi_write_sc|single|multi_read]</OptionalRange>
    <ParameterValue>single</ParameterValue>
    <IsChangeableConfig>true</IsChangeableConfig>
    <CurrentValue>single</CurrentValue>
    <ParameterName>rds_master_mode</ParameterName>
    <ParameterDescription>Enable global strong consistency when rds_master_mode is set to multi_write_sc, and global eventual consistency when rds_master_mode is set to multi_write_ec</ParameterDescription>
    <ForceRestartInstance>true</ForceRestartInstance>
</Parameters>
<RequestId>8CA0F95F-0F2D-4E85-ADD1-32C7FCE5E17D</RequestId>
```

`JSON`格式

```

    },
    {
      "OptionalRange": "[multi_write_ec|multi_write_sc|single|multi_read]",
      "ParameterValue": "single",
      "IsChangeableConfig": "true",
      "CurrentValue": "single",
      "ParameterName": "rds_master_mode",
      "ParameterDescription": "Enable global strong consistency when rds_master_mode is set to multi_write_sc, and global eventual consistency when rds_master_mode is set to multi_write_ec",
      "ForceRestartInstance": "true"
    }
  ],
  "RequestId": "8CA0F95F-0F2D-4E85-ADD1-32C7FCE5E17D"
} "[multi_write_ec|multi_write_sc|single|multi_read]",
      "ParameterValue": "single",
      "IsChangeableConfig": "true",
      "CurrentValue": "single",
      "ParameterName": "rds_master_mode",
      "ParameterDescription": "Enable global strong consistency when rds_master_mode is set to multi_write_sc, and global eventual consistency when rds_master_mode is set to multi_write_ec",
      "ForceRestartInstance": "true"
    }
  ],
  "RequestId": "8CA0F95F-0F2D-4E85-ADD1-32C7FCE5E17D"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

