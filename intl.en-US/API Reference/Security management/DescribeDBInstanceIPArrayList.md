# DescribeDBInstanceIPArrayList

## Description

Queries the IP addresses that are allowed to access an instance.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to DescribeDBInstanceIPArrayList.|
|DBInstanceId|String|Yes|The ID of the instance.|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>| |For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|
|Items|List<DBInstanceIPArray\>|An array consisting of IP address whitelists.|

|Parameter|Type|Description|
|---------|----|-----------|
|DBInstanceIPArrayName|String|The name of the IP address whitelist.|
|DBInstanceIPArrayAttribute|String|The attribute of the IP address whitelist. Default value: null. A whitelist with the `hidden` attribute does not appear in the console.|
|SecurityIPList|String|The IP addresses in the whitelist. A whitelist can contain up to 1,000 IP addresses that are separated with commas \(,\). The formats are as follows:-   0.0.0.0/0.
-   10.23.12.24 \(IP address\).
-   Classless Inter-Domain Routing \(CIDR\) blocks, such as 10.23.12.24/24, where /24 indicates that the prefix of the CIDR block is 24-bit. You can replace 24 with a value ranging from 1 to 32. |

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstanceIPArrayList
&DBInstanceId=gp-xxxxxxx
&<Common request parameters>
```

## Sample responses

**XML format**

```
<DescribeDBInstanceIPArrayListResponse>
  <RequestId>CB7AA0BF-BE41-480E-A3DC-C97BF85A391B</RequestId>
  <Items>
    <DBInstanceIPArray>
        <groupName>default</groupName>
		 <groupTag></groupTag>
		 <securityIPList>127.0.0.1</securityIPList>
    </DBInstanceIPArray>
</DescribeDBInstanceIPArrayListResponse>
```

**JSON format**

```
{
    "Items":{
        "DBInstanceIPArray":[
            {
              "groupName":"default",
              "groupTag":"",
              "securityIPList":"127.0.0.1"
            }
        ]
    },
    "RequestId":"CB7AA0BF-BE41-480E-A3DC-C97BF85A391B"
}
```

