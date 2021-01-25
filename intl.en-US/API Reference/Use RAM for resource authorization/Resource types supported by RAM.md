# Resource types supported by RAM

Currently, you can only use RAM to authorize DBInstance resources.

The following table lists the Alibaba Cloud Resource Names \(ARNs\) of such resources.

|Resource type|ARN in the authorization policy|
|-------------|-------------------------------|
|DBInstance|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid

 acs:gpdb:$regionid:$accountid:dbinstance/\*

 acs:gpdb:\* : \*:dbinstance/\* |

**Note:**

-   $regionid: Set this parameter to the ID of a region or to an asterisk \(\*\).
-   $dbinstanceid: Set this parameter to the ID of a database instance or to an asterisk \(\*\).
-   $accoutid: Set this parameter to the ID of the account that owns the resource or to an asterisk \(\*\).

