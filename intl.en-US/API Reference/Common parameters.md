# Common parameters

## Common request parameters

Common request parameters refer to the request parameters that each API requires.

|Name|Type|Required|Description|
|----|----|--------|-----------|
|Format|String|No|The format of the returned value. Values: JSON and XML. The default format is XML.|
|Version|String|Yes|The version of the API, in the format of YYYY-MM-DD. The current version is 2016-05-03.|
|AccessKeyId|String|Yes|The Key ID provided by Alibaba Cloud for you to access services.|
|Signature|String|Yes|The result string of the signature. For more information about how to calculate a signature, see [Sign signatures](/intl.en-US/API Reference/Sign signatures.md).|
|SignatureMethod|String|Yes|The method of the signature. HMAC-SHA1 is supported currently.|
|Timestamp|String|Yes|The timestamp of the request. The format of the date follows the [ISO8601](http://zh.wikipedia.org/wiki/ISO_8601) standard and uses UTC time. The format is as follows:

```
YYYY-MM-DDThh:mm:ssZ
```

For example, 2013-08-15T12:00:00Z indicates 20:00:00, Jan 10, 2013, Beijing time. |
|SignatureVersion|String|Yes|The algorithm version of the signature. The current version is 1.0.|
|SignatureNonce|String|Yes|The unique random number that is used to prevent network replay attacks. You must use different random numbers for the requests.|

**Examples**

```
https://gpdb.aliyuncs.com/ 
? Format=xml 
&Version=2013-08-15 
&Signature=Pc5WB8gokVn0xfeu%2FZV%2BiNM1dgI%3D  
&SignatureMethod=HMAC-SHA1 
&SignatureNonce=15215528852396 
&SignatureVersion=1.0 
&AccessKeyId=key-test 
&Timestamp=2013-06-01T12:00:00Z
            
```

## Common response parameters

Each time you make a call to an API, the system returns a unique identification code \(RequestId\), regardless whether the request is successful.

**Example:**

```
<? xml version="1.0" encoding="utf-8"? >  
<!-Result root node--> 
<API name+Response> 
 <!-Return request tag--> 
 <RequestId>4C467B38-3910-447D-87BC-AC049166F216</RequestId> 
 <!-Return result data--> 
</API name+Response> 
            
```

