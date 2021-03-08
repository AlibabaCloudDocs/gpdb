# DescribeRegions

You can call this operation to query available regions and zones. You must call the DescribeRegions operation to query the available regions before you call the CreateDBInstance operation.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeRegions&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeRegions|The operation that you want to perform. Set the value to DescribeRegions. |
|Region|String|No|cn-hangzhou|The region ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|FF8EB261-5447-4B1B-9F14-294CEA008A9F|The ID of the request. |
|Regions|Array| |Details about the regions. |
|RegionId|String|cn-hangzhou|The region ID of the instance. |
|Zones|Array| |Details about the zones. |
|ZoneId|String|cn-hangzhou-c|The zone ID of the instance. |
|VpcEnabled|Boolean|true|Indicates whether the VPC in the zone is available. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=DescribeRegions
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>FF8EB261-5447-4B1B-9F14-294CEA008A9F</RequestId>
<Regions>
    <Region>
        <RegionId>cn-beijing</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>cn-beijing-c</ZoneId>
            </Zone>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>cn-beijing-g</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>cn-zhangjiakou</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>cn-zhangjiakou-b</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>cn-hangzhou</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>cn-hangzhou-b</ZoneId>
            </Zone>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>cn-hangzhou-e</ZoneId>
            </Zone>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>cn-hangzhou-f</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>cn-shanghai</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>cn-shanghai-b</ZoneId>
            </Zone>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>cn-shanghai-d</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>cn-shenzhen</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>cn-shenzhen-a</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>ap-southeast-1</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>ap-southeast-1b</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>ap-southeast-2</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>ap-southeast-2a</ZoneId>
            </Zone>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>ap-southeast-2b</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>us-east-1</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>us-east-1b</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>us-west-1</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>us-west-1a</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>cn-chengdu</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>cn-chengdu-a</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>ap-southeast-3</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>ap-southeast-3a</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>cn-huhehaote</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>cn-huhehaote-a</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>ap-south-1</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>ap-south-1a</ZoneId>
            </Zone>
        </Zones>
    </Region>
    <Region>
        <RegionId>ap-southeast-5</RegionId>
        <Zones>
            <Zone>
                <VpcEnabled>true</VpcEnabled>
                <ZoneId>ap-southeast-5a</ZoneId>
            </Zone>
        </Zones>
    </Region>
</Regions>
```

`JSON` format

```
{
    "RequestId":"FF8EB261-5447-4B1B-9F14-294CEA008A9F",
    "Regions":{
        "Region":[
            {
                "RegionId":"cn-beijing",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"cn-beijing-c"
                        },
                        {
                            "VpcEnabled":true,
                            "ZoneId":"cn-beijing-g"
                        }
                    ]
                }
            },
            {
                "RegionId":"cn-zhangjiakou",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"cn-zhangjiakou-b"
                        }
                    ]
                }
            },
            {
                "RegionId":"cn-hangzhou",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"cn-hangzhou-b"
                        },
                        {
                            "VpcEnabled":true,
                            "ZoneId":"cn-hangzhou-e"
                        },
                        {
                            "VpcEnabled":true,
                            "ZoneId":"cn-hangzhou-f"
                        }
                    ]
                }
            },
            {
                "RegionId":"cn-shanghai",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"cn-shanghai-b"
                        },
                        {
                            "VpcEnabled":true,
                            "ZoneId":"cn-shanghai-d"
                        }
                    ]
                }
            },
            {
                "RegionId":"cn-shenzhen",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"cn-shenzhen-a"
                        }
                    ]
                }
            },
            {
                "RegionId":"ap-southeast-1",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"ap-southeast-1b"
                        }
                    ]
                }
            },
            {
                "RegionId":"ap-southeast-2",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"ap-southeast-2a"
                        },
                        {
                            "VpcEnabled":true,
                            "ZoneId":"ap-southeast-2b"
                        }
                    ]
                }
            },
            {
                "RegionId":"us-east-1",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"us-east-1b"
                        }
                    ]
                }
            },
            {
                "RegionId":"us-west-1",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"us-west-1a"
                        }
                    ]
                }
            },
            {
                "RegionId":"cn-chengdu",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"cn-chengdu-a"
                        }
                    ]
                }
            },
            {
                "RegionId":"ap-southeast-3",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"ap-southeast-3a"
                        }
                    ]
                }
            },
            {
                "RegionId":"cn-huhehaote",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"cn-huhehaote-a"
                        }
                    ]
                }
            },
            {
                "RegionId":"ap-south-1",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"ap-south-1a"
                        }
                    ]
                }
            },
            {
                "RegionId":"ap-southeast-5",
                "Zones":{
                    "Zone":[
                        {
                            "VpcEnabled":true,
                            "ZoneId":"ap-southeast-5a"
                        }
                    ]
                }
            }
        ]
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

