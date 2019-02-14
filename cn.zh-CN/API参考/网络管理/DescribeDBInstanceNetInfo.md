# DescribeDBInstanceNetInfo {#concept_qvg_jhm_q2b .concept}

## 描述 { .section}

描述实例的连接信息。

## 输入参数 { .section}

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](intl.zh-CN/API参考/公共参数.md#)。|
|Action|String|是|系统规定参数，取值：DescribeDBInstanceNetInfo。|
|DBInstanceId|String|是|实例名。|

## 返回参数 { .section}

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>| |详见[公共返回参数](intl.zh-CN/API参考/公共参数.md#section_apd_1rv_3bb)。|
|DBInstanceNetInfos|List<DBInstanceNetInfo\>|实例的连接信息。|
|InstanceNetworkType|String| -   Classic：经典网络。
-   VPC：VPC网络。

 |

|名称|类型|描述|
|--|--|--|
|ConnectionString|String|DNS连接串。|
|IPAddress|String|IP地址。|
|IPType|String| -   经典网络类型的实例IPType为：Inner、Public。
-   VPC类型的实例IPType为：Private、Public。

 |
|Port|String|端口信息。|
|VPCId|String|VPC ID。|
|VSwitchId|String|VSwitch ID，多个值用英文逗号“,”隔开。|

## 请求示例 { .section}

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstanceNetInfo
&DBInstanceId=gp-xxxxxxx
&<公共请求参数>
```

## 返回示例 { .section}

**XML格式**

```
<DescribeDBInstanceNetInfoResponse>
    <instanceNetworkType>Classic</instanceNetworkType>
	<DBInstanceNetInfos>
		<DBInstanceNetInfo>
			<DBInstanceNetType>1</DBInstanceNetType>
			<connectionString>gp-xxxxxxx.gpdb.rds.aliyuncs.com</connectionString>
			<ipAddress>127.0.0.1</ipAddress>
			<port>3432</port>
			<userVisible>1</userVisible>
		</DBInstanceNetInfo>
	</DBInstanceNetInfos>
	<RequestId>7565770E-7C45-462D-BA4A-8A5396F2CAD1</RequestId>
</DescribeDBInstanceNetInfoResponse>
```

**JSON格式**

```
{
    "instanceNetworkType":"Classic",
    "DBInstanceNetInfos": {
      "DBInstanceNetInfo": [
        {
           "DBInstanceNetType":"1",
            "connectionString":"gp-xxxxxxx.gpdb.rds.aliyuncs.com",
            "ipAddress":"127.0.0.1",
            "port":"3432",
            "userVisible":1
        }
      ]
    }, 
    "RequestId": "7565770E-7C45-462D-BA4A-8A5396F2CAD1"
  }
```

