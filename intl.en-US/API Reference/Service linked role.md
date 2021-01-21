# Service linked role

This topic describes the application scenarios of the service linked role AliyunServiceRoleForADBPG of AnalyticDB for PostgreSQL and provides guidance on how to delete the role.

## Application scenarios

To implement some features, AnalyticDB for PostgreSQL uses the service linked role named AliyunServiceRoleForADBPG to access other Alibaba Cloud services. The service linked role is a type of Resource Access Management \(RAM\) role. For more information, see [Service-linked roles](/intl.en-US/RAM Role Management/Service-linked roles.md).

## Role description

Role name: AliyunServiceRoleForADBPG

Permission policy attached to the role: AliyunServiceRolePolicyForADBPG

Permission description:

```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
        "ecs:CreateNetworkInterface",
        "ecs:DeleteNetworkInterface",
        "ecs:AttachNetworkInterface",
        "ecs:DetachNetworkInterface",
        "ecs:DescribeNetworkInterfaces",
        "ecs:CreateNetworkInterfacePermission",
        "ecs:DescribeNetworkInterfacePermissions",
        "ecs:CreateSecurityGroup",
        "ecs:DeleteSecurityGroup",
        "ecs:DescribeSecurityGroupAttribute",
        "ecs:DescribeSecurityGroups",
        "ecs:ModifySecurityGroupAttribute",
        "ecs:AuthorizeSecurityGroup",
        "ecs:AuthorizeSecurityGroupEgress",
        "ecs:RevokeSecurityGroup",
        "ecs:RevokeSecurityGroupEgress"
      ],
      "Resource": "*",
      "Effect": "Allow"
    },
    {
      "Action": "ram:DeleteServiceLinkedRole",
      "Resource": "*",
      "Effect": "Allow",
      "Condition": {
        "StringEquals": {
          "ram:ServiceName": "adbpg.aliyuncs.com"
        }
      }
    }
  ]
}
```

## Delete the service linked role

Before you delete the AliyunServiceRoleForADBPG role, release all the instances that the role is bound to.

-   For information about how to release an AnalyticDB for PostgreSQL instance, see [Release an instance](/intl.en-US/Instances/Release an instance.md).
-   For information about how to delete a service linked role, see [Delete the service-linked role AliyunServiceRoleForDAS](/intl.en-US/RAM Role Management/Service-linked roles.md).

