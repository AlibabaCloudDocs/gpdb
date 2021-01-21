# API的鉴权规则

当子用户通过API 进行资源访问时，后台向RAM进行权限检查，以确保调用者拥有相应权限。

当用户通过 OpenAPI 进行跨账户的 AnalyticDB for PostgreSQL资源访问时， AnalyticDB for PostgreSQL后台向RAM进行权限检查，以确保资源拥有者已经将相关资源的相关权限授予调用者。每个不同的OpenAPI会根据涉及到的资源以及API的语义来确定需要检查哪些资源的权限。具体每个API的鉴权规则参见[表 1](#table_zcx_cft_jbb)。

|Action|鉴权规则|
|------|----|
|CreateDBInstance|acs:gpdb:$regionid: dbinstance /$\*|
|DeleteDBInstance|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeDBInstanceAttribute|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeDBInstances|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeRegions|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeResourceUsage|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|ModifyDBInstanceConnectionMode|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|ModifyDBInstanceDescription|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|ModifyDBInstanceMaintainTime|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|RestartDBInstance|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DeleteDatabase|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeAccounts|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|ModifyAccountDescription|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|CreateAccount|acs:gpdb:$regionid: dbinstance /$\*|
|ResetAccountPassword|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeDBInstanceIPArrayList|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeDBClusterIPArrayList|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|ModifySecurityIps|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeSlowLogRecords|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeSQLCollectorPolicy|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeSQLLogFiles|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeSQLLogRecords|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|ModifySQLCollectorPolicy|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeDBInstancePerformance|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|AllocateInstancePublicConnection|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|DescribeDBInstanceNetInfo|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|ReleaseClusterPublicConnection|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|ModifyDBInstanceConnectionString|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|ModifyDBInstanceNetworkType|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|
|ReleaseInstancePublicConnection|acs:gpdb:$regionid: $accountid:dbinstance/$dbinstanceid|

