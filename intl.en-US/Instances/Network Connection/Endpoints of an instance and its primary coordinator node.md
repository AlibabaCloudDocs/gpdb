# Endpoints of an instance and its primary coordinator node

Starting from February 8, 2021, AnalyticDB for PostgreSQL allows you to configure multiple coordinator nodes.

You can add multiple coordinator nodes to an instance to push beyond the limits of the original architecture in which an instance has a single coordinator node. If compute nodes permit, the number of connections and the I/O capabilities can linearly increase with the number of coordinator nodes. This way, the overall performance of the system is enhanced. However, if you use multiple coordinator nodes in an instance, the endpoints of the instance have limits. For your convenience, AnalyticDB for PostgreSQL provides endpoints for both the primary coordinator node and the instance. You can choose an endpoint to use based on the considerations of compatibility and performance. If you have questions, join the DingTalk group for technical support or [submit a ticket](https://workorder.console.aliyun.com/console.htm#/ticket/add?productCode=gpdb).

If you set more than one coordinator node when you create an AnalyticDB for PostgreSQL instance, separate endpoints are created for the primary coordinator node and the instance.

**Endpoints of the primary coordinator node**: Requests initiated from this type of endpoints are forwarded to the primary coordinator node. Secondary coordinator nodes do not handle requests. If you choose to use this type of endpoints, the capabilities of the system are fully compatible with an AnalyticDB for PostgreSQL instance that has a single coordinator node.

**Endpoints of the instance**: Requests initiated from this type of endpoints are forwarded to the primary coordinator node and secondary coordinator nodes because SLB is automatically connected.

