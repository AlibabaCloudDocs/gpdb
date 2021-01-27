# DescribeDBInstanceIPArrayList

调用DescribeDBInstanceIPArrayList查询允许访问实例的IP名单。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceIPArrayList&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|DBInstanceId|String|是|gp-xxxxxxxxxxx|实例ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|CB7AA0BF-BE41-480E-A3DC-C97BF85A391B|请求ID。 |
|Items|Array| |实例的IP白名单分组列表。 |
|DBInstanceIPArrayName|String|default|IP白名单分组的名字。 |
|DBInstanceIPArrayAttribute|String|hidden|默认为空。用以区分不同的属性值，控制台不显示带有`hidden`属性的分组。 |
|SecurityIPList|String|127.0.0.1|IP白名单分组下的IP列表，最多1000个以逗号隔开，有以下三种格式：

 -   0.0.0.0/0
-   10.23.12.24（IP）
-   10.23.12.24/24（CIDR模式，无类域间路由，`/24`表示了地址中前缀的长度，范围为`[1,32]`） |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstanceIPArrayList
&DBInstanceId=gp-xxxxxxxxxxx
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<DescribeDBInstanceIPArrayListResponse>
  <Items>
        <DBInstanceIPArray>
              <DBInstanceIPArrayName>default</DBInstanceIPArrayName>
              <DBInstanceIPArrayAttribute>hidden</DBInstanceIPArrayAttribute>
              <securityIPList>127.0.0.1</securityIPList>
        </DBInstanceIPArray>
  </Items>
  <RequestId>CB7AA0BF-BE41-480E-A3DC-C97BF85A391B</RequestId>
</DescribeDBInstanceIPArrayListResponse>
```

`JSON` 格式

```
{
    "Items":{
        "DBInstanceIPArray":[
            {
              "DBInstanceIPArrayName":"default",
              "DBInstanceIPArrayAttribute":"hidden",
              "securityIPList":"127.0.0.1"
            }
        ]
    },
    "RequestId":"CB7AA0BF-BE41-480E-A3DC-C97BF85A391B"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/gpdb)查看更多错误码。

