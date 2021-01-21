# Authorized resource type through RAM

Currently, RAM only supports authorizing DBInstance.

The description of resources to be authorized through RAM is as follows.

|Resource type|Description in authorization policy|
|-------------|-----------------------------------|
|DBInstance|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid

 acs:gpdb:$regionid:$accountid:dbinstance/\*

 acs:gpdb:\* : \*:dbinstance/\* |

**Note:**

-   The value of $regionid should be the id of a region or a "\*".
-   The value of $dbinstanceid should be the id of a DBInstance or an asterisk \(\*\).
-   The value of $accoutid should be the userid of the resource owner or a "\*".

