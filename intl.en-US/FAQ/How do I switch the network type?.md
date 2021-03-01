# How do I switch the network type?

By default, the network type of AnalyticDB for PostgreSQL is Virtual Private Cloud \(VPC\). If you want to switch the network type of an instance from the classic network to VPC, the instance and the VPC must be in the same region.

## Background information

VPC: You can use a VPC to build an isolated network environment in Alibaba Cloud. You can customize route tables, CIDR blocks, and gateways in a VPC. If you want to migrate your applications to the cloud, you can build a virtual data center by connecting your self-managed data center to the resources in a VPC over a leased line or a virtual private network \(VPN\).

## Procedure

1.  Create a VPC in the same region with the AnalyticDB for PostgreSQL instance.
2.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).
3.  In the upper-left corner, select the region where the instance resides.
4.  Find the instance and click **Manage** in the Actions column. The Basic Information page appears.
5.  In the left-side navigation pane, click **Database Connection**. The Connection Information page appears.
6.  Click **Switch to VPC**. The Switch to VPC panel appears.
7.  Select a VPC and a vSwitch. Click **OK**.

    **Note:** After you switch the network type of the instance from classic network to VPC, the network type of the internal endpoint is switched from classic network to VPC. ECS instances in the classic network cannot access the VPC-type AnalyticDB for PostgreSQL instances. The public endpoint remains unchanged.


## API references

|API|Description|
|---|-----------|
|[t16947.md\#](/intl.en-US/API Reference/Network management/ModifyDBInstanceNetworkType.md)|Switches the network type of an instance between VPC and classic network.|

