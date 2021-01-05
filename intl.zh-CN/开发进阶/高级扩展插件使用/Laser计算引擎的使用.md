# Laser计算引擎的使用

Laser计算引擎是阿里巴巴自研的ADBPG计算引擎，对客户透明，可以提升复杂计算的性能，经实测，1GB、100GB、1TB、10TB数据规模下，TPCH 22条SQL的测试性能是原生引擎的2倍以上。

## 打开/关闭Laser

Laser 计算引擎通过GUC参数laser.enable打开或者关闭，on 表示使用Laser计算引擎，select查询会通过Laser返回结果，off表示计算通过原生引擎返回结果。默认状态为关闭状态。该参数可以设置为session级别、库级别和集群级别，session结束恢复到默认状态，库级别设置以后立即生效，集群级别重启后生效。可以通过下面SQL查看、修改：

```
--- 查看Laser是否开启
show laser.enable;
--- session级别关闭Laser
set laser.enable = off;
--- session级别开启Laser
set laser.enable = on;
--- 库级别关闭Laser
alter database ${DBNAME} set laser.enable = off;
--- 库级别开启Laser
alter database ${DBNAME} set laser.enable = on;
```

**说明：** : 集群级别的设置请联系阿里云管理员，建议使用库级别或者session级别的设置。

## 支持的数据类型和操作

Laser支持如下数据类型：

-   INT2/INT4/INT8
-   FLOAT4/FLOAT8/NUMERIC
-   DATE/TIME/TIMETZ/TIMESTAMP/TIMESTAMPTZ
-   VARCHAR/TEXT/BPCHAR

Laser支持如下操作符：

-   =、<、<=、\>、\>=、<\> or !=、BETWEEN、 IS NOT NULL、 IS NULL、LIKE
-   逻辑运算符：and、or、not

## Laser的限制

-   推荐使用ORCA优化器
-   只支持ADBPG 6.0 及以上版本

