# ModifyDBInstanceConnectionMode

## 描述

该接口用于变更实例连接模式，目前提供两种访问模式：

-   标准访问模式
-   高安全访问模式

## 请求参数

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](/cn.zh-CN/API参考/公共参数.md)。|
|Action|String|是|系统规定参数，取值为ModifyDBInstanceConnectionMode。|
|DBInstanceId|String|是|实例名。|
|ConnectionMode|String|是|-   Standard：标准访问模式
-   Safe：高安全访问模式 |

## 返回参数

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>|-|详见[公共返回参数](/cn.zh-CN/API参考/公共参数.mdsection_apd_1rv_3bb)。|

## 请求示例

```
https://gpdb.aliyuncs.com/?Action=ModifyDBInstanceConnectionMode
&DBInstanceId=gp-xxxxxxx
&ConnectionMode=Safe
&<公共请求参数>

```

## 返回示例

**XML格式**

```
<BaseResponse> 
	<RequestId>8361E350-4CCC-4D28-B020-55B4DCB75D3B</RequestId>
</BaseResponse>
```

**JSON格式**

```
{
    "RequestId":"8361E350-4CCC-4D28-B020-55B4DCB75D3B",
}
```

