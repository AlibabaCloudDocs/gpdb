# DescribeDBInstanceIPArrayList

You can call this operation to query whitelists of IP addresses that are allowed to access an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceIPArrayList&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|DBInstanceId|String|Yes|gp-xxxxxxxxxxx|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|CB7AA0BF-BE41-480E-A3DC-C97BF85A391B|The ID of the request. |
|Items|Array| |Details about the IP address whitelists. |
|DBInstanceIPArrayName|String|default|The name of the IP address whitelist. |
|DBInstanceIPArrayAttribute|String|hidden|This parameter is empty by default. You can use this parameter to distinguish IP address whitelists that have different attributes. The groups that have the `hidden` attribute is not displayed in the console. |
|SecurityIPList|String|127.0.0.1|The IP addresses listed in the whitelist. You can add up to 1,000 IP addresses to the whitelist. Separate multiple IP addresses with commas \(,\). The IP addresses must use one of the following formats:

 -   0.0.0.0/0
-   10.23.12.24. This is a standard IP address.
-   10.23.12.24/24. This is a Classless Inter-Domain Routing \(CIDR\) block. The value `/24` indicates that the prefix of the CIDR block is 24-bit long. You can replace 24 with a value within the range of `[1,32]`. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeDBInstanceIPArrayList
&DBInstanceId=gp-xxxxxxxxxxx
&<common request parameters>
```

Sample success responses

`XML` format

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

`JSON` format

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

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

