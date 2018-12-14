# ModifyDBInstanceNetworkType {#concept_ofq_mhm_q2b .concept}

## 描述 { .section}

该接口用于将实例的网络类型从VPC切换成经典网络，或从经典网络切换成VPC。

## 请求参数 { .section}

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](intl.zh-CN/API参考/公共参数.md#)。|
|Action|String|是|系统规定参数，取值为ModifyDBInstanceNetworkType。|
|DBInstanceId|String|是|实例名。|
|InstanceNetworkType|String|是|指定网络类型：-   VPC：专有网络类型
-   Classic：经典网络类型

|
|VPCId|String|否|VPC ID。|
|VSwitchId|String|否|VSwitch ID；若传入VPCId的值，则该参数必传。|
|PrivateIpAddress|String|否|您可以指定VSwitchId下的VPC IP。如果不输入，系统通过VPCId和VSwitchId自动分配。|

## 返回参数 { .section}

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>|-|详见[公共返回参数](intl.zh-CN/API参考/公共参数.md#section_apd_1rv_3bb)。|

## 请求示例 { .section}

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceNetworkType
&DBInstanceId=gp-xxxxxxx
&InstanceNetworkType=VPC
&<公共请求参数>
```

## 返回示例 { .section}

**XML格式**

```
<ModifyDBInstanceNetworkTypeResponse>
         <RequestId>2d0c35a9-f5da-44ba-852d-741e27b7eb0b</RequestId>
</ModifyDBInstanceNetworkTypeResponse>
```

**JSON格式**

```
{
  "RequestId":"2d0c35a9-f5da-44ba-852d-741e27b7eb0b"
}
```

