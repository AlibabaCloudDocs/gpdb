# Responses

After you call an API operation, a response is returned in a unified format. A returned HTTP status code of `2xx` indicates that the call succeeded. A returned HTTP status code of `4xx` or `5xx` indicates that the call failed.

Success responses are returned in the XML or JSON format. You can enter a parameter to specify the response format when you call an API operation from an external system. The default format is XML. Sample responses in this topic are formatted to be reader-friendly. Actual responses are not formatted with line feeds, indentation, or other layouts.

## Sample success responses

**XML format**

A response in the XML format includes a message that indicates whether the request succeeded and the specific service data. Example:

```
<? xml version="1.0" encoding="utf-8"? >  
<!--Result Root Node--> 
<API Operation Name+Response> 
    <!--Return Request Tag--> 
 <RequestId>4C467B38-3910-447D-87BC-AC049166F216</RequestId> 
    <!--Return Result Data--> 
</API Operation Name+Response>
            
```

**JSON format**

```
{ 
    "RequestId": "4C467B38-3910-447D-87BC-AC049166F216", 
    /* Return result */ 
} 
            
```

## Sample error responses

If the call to an API operation failed, no results are returned. You can locate the errors by following the instructions provided in [Client error codes](/intl.en-US/API Reference/Appendix/Error codes/Client error codes.md).

If the call to an API operation failed, an HTTP status code of `4xx` or `5xx` is returned. The body of the returned message contains the specific error code and error message. The message body also contains the globally unique ID of the request \(RequestId\) and the ID of the requested site \(HostId\). If you cannot locate an error, contact Alibaba Cloud customer service and provide the HostId and RequestId so the error can be located as soon as possible.

**XML format**

```
<? xml version="1.0" encoding="UTF-8"? > 
<Error> 
   <RequestId>8906582E-6722-409A-A6C4-0E7863B733A5</RequestId> 
   <HostId>gpdb.aliyuncs.com</HostId> 
   <Code>InvalidAction</Code> 
   <Message>The specified action is not supported. </Message> 
</Error> 
            
```

**JSON format**

```
{ 
    "RequestId": "7463B73D-35CC-4D19-A010-6B8D65D242EF", 
    "HostId": "gpdb.aliyuncs.com", 
    "Code": "InvalidAction", 
    "Message": "The specified action is not valid." 
} 
                
```

