# Authentication rules for APIs

When you use APIs to access resources as a RAM user, AnalyticDB for PostgreSQL checks whether the RAM user is granted the required permissions by querying Resource Access Management \(RAM\).

When you access cross-account resources by using APIs, AnalyticDB for PostgreSQL checks whether you are granted the required permissions to access these resources by querying RAM. The permissions to be checked are determined by the resources used by an API and the API operation. For more information about authentication rules for each API, see [Table 1](#table_zcx_cft_jbb).

|Action|Authentication rule|
|------|-------------------|
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

