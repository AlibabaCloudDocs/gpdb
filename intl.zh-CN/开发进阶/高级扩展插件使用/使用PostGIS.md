# 使用PostGIS

PostGIS是数据库PostgreSQL的一个扩展，PostGIS提供如下空间信息服务功能：空间对象、空间索引、空间操作函数和空间操作符。

**说明：**

-   PostGIS遵循Open Geospatial Consortium（OGC）规范。
-   AnalyticDB for PostgreSQL支持PostGIS扩展（4.3版本对应PostGIS v2.0.3，6.0版本对应PostGIS v2.5.3）。

## 通用操作

**1）客户端连接实例**

可参考[客户端连接](/intl.zh-CN/快速入门/客户端连接.md)

**2）初次装载PostGIS扩展模块**

创建扩展：

```
create extension postgis;
```

查看版本：

```
select postgis_version();
select postgis_full_version();
```

**3）空间数据写入数据库表**

首先创建带Geometry字段的表，SQL参考：

```
create table testg ( id int, geom geometry ) 
distributed by (id);
```

该SQL表示插入的空间数据不区分几何类型，几何类型包括Point / MultiPoint / Linestring / MultiLinestring / Polygon / MultiPolygon等。如果在创建表时已知Geometry类型和SRID，SQL参考：

```
create table test ( id int, geom geometry(point, 4326) ) 
distributed by (id);
```

数据插入，SQL参考：

```
-- without srid
insert into testg values (1, ST_GeomFromText('point(116 39)'));

-- with srid
insert into test values (1, ST_GeomFromText('point(116 39)', 4326));
```

JDBC Java程序参考：

```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;

public class PGJDBC {
    public static void main(String args[]) {
        Connection conn = null;
        Statement stmt = null;
        try{
            Class.forName("org.postgresql.Driver");
            //conn = DriverManager.getConnection("jdbc:postgresql://<host>:3432/<database>","<user>", "<password>");
            conn.setAutoCommit(false);
            stmt = conn.createStatement();

            String sql = "INSERT INTO test VALUES (1001, ST_GeomFromText('point(116 39)', 4326) )";
            stmt.executeUpdate(sql);

            stmt.close();
            conn.commit();
            conn.close();
        } catch (Exception e) {
            System.err.println(e.getClass().getName() + " : " + e.getMessage());
            System.exit(0);
        }
        System.out.println("insert successfully");
    }
}
```

**4）空间索引管理**

-   创建空间索引

```
create index idx_test_geom on test using gist(geom);
```

idx\_test\_geom为自定义索引名，test为表名，geom为Geometry列名。

-   查看表有哪些索引

```
select * from pg_stat_user_indexes 
where relname='test';
```

-   查看索引大小

```
select pg_indexes_size('idx_test_geom');
```

-   索引重构

```
reindex index idx_test_geom;
```

-   删除索引

```
drop index idx_test_geom;
```

**5）典型空间查询SQL**

-   矩形范围查询

```
-- without srid
select st_astext(geom) from testg
where ST_Contains(ST_MakeBox2D(ST_Point(116, 39),ST_Point(117, 40)), geom);

-- with srid
select st_astext(geom) from test 
where ST_Contains(ST_SetSRID(ST_MakeBox2D(ST_Point(116, 39),ST_Point(117, 40)), 4326), geom);
```

ST\_MakeBox2D算子生成一个Envelope。

-   几何缓冲范围查询

```
-- without srid
select st_astext(geom) from testg
where ST_DWithin(ST_GeomFromText('POINT(116 39)'), geom, 0.01);

-- with srid
select st_astext(geom) from test 
where ST_DWithin(ST_GeomFromText('POINT(116 39)', 4326), geom, 0.01);
```

ST\_DWithin用法参考：[ST\_DWithin](http://postgis.net/docs/ST_DWithin.html)

-   多边形相交判定（在内部或在边界上）

```
-- without srid
select st_astext(geom) from testg
where ST_Intersects(ST_GeomFromText('POLYGON((116 39, 116.1 39, 116.1 39.1, 116 39.1, 116 39))'), geom);

-- with srid
select st_astext(geom) from test 
where ST_Intersects(ST_GeomFromText('POLYGON((116 39, 116.1 39, 116.1 39.1, 116 39.1, 116 39))', 4326), geom);
```

ST\_\*算子对大小写不敏感，更多用法可参考 [PostGIS官方资料](https://postgis.net/workshops/postgis-intro/index.html)

**说明：**

AnalyticDB PG 6.0不完全兼容PostGIS功能集，例如不支持 **create extension postgis\_topology**，不推荐用Geography类型创建表（非要用，SRID默认为0或4326）。

## 典型案例

**1）电子围栏场景**

某客运监控服务运营商，通过安装在客车上的GPS定位终端收集定位数据，常见的业务有偏航报警、常去的服务区频次、驶入特定区域提醒（例如易发事故地段、积水结冰地段）等，这类业务是比较典型的电子围栏应用场景。以驶入特定区域提醒业务为例，特定区域不会频繁变更且数据量偏少，可以一次采集定期更新，考虑区域表采用复制表，SQL参考：

```
CREATE TABLE ky_region (
  rid     serial,
  name    varchar(256),
  geom    geometry)
DISTRIBUTED REPLICATED;
```

插入Polygon / MultiPolygon类型的特定区域数据后，完成统计信息收集（Analyze 表名）并构建GIST索引。判定驶入区域，可以分为两种情况：一种完全在区域内，一种是到达边界就要提醒。两种情况用到的空间算子有所区别，SQL参考：

```
-- 完全在区划内
select rid, name from ky_region
where ST_Contains(geom, ST_GeomFromText('POINT(116 39)'));

-- 考虑边界情况
select rid, name from ky_region
where ST_Intersects(geom, ST_GeomFromText('POINT(116 39)'));
```

SQL解释：输入变化的经纬度，查询区域表geom字段包含或相交与输入点的记录，如果为0条记录表示未驶入任何区域，如果为1条记录表示驶入某个区域，如果大于1条记录表示驶入多个区域（说明区域表有空间重叠的区域，需要从业务上验证空间重叠的合理性）。

**2）智慧交通场景**

某智慧交通场景，数据库包含线型轨迹表和其他业务表，一业务功能为查找历史轨迹表中曾经驶入过某一区域的轨迹ID，相关轨迹表结构：

```
create table vhc_trace_d (
 stat_date        text, 
 trace_id         text, 
 vhc_id           text, 
 rid_wkt          geometry) 
Distributed by (vhc_id) partition by LIST(stat_date)
(
 PARTITION p20191008 VALUES('20191008'),
 PARTITION p20191009 VALUES('20191009'),
 ......
);
```

轨迹按照天创建分区表，每天导入数据后做统计信息收集，并对分区表创建GIST空间索引。SQL参考：

```
SELECT trace_id FROM vhc_trace_d
WHERE ST_Intersects(
  ST_GeomFromText('Polygon((118.732461  29.207363,118.732366  29.207198,118.732511  29.205951,118.732296  29.205644,
                  118.73226  29.205469,118.732350  29.20470,118.731708  29.203399,118.731701  29.202401, 118.754689 29.213488,
                  118.750827 29.21316,118.750272 29.213337,118.749677 29.213257,118.748699 29.213388,118.747715 29.213206,
                  118.746580 29.213831,118.74639 29.213872,118.744989 29.213858,118.743442 29.213795,118.74174 29.213002,
                  118.735633 29.208167,118.734422 29.207699,118.733045 29.207450,118.732803 29.207342,118.732461  29.207363))'), rid_wkt);
```

亿级轨迹表做空间查询的响应时间在80ms内。

**3）商业客流分析**

某互联网生活服务运营商，基于AnalyticDB for PostgreSQL做店铺客流量分析，数据库有两张业务表：User签到表和Shop店铺区域表，表结构参考：

```
-- user
create table user_label (
  ghash7          int, 
  uid             int, 
  workday_geo     geometry, 
  weekend_geo     geometry) 
distributed by (ghash7);

-- shop
create table user_shop (
  ghash7          int, 
  sid             int, 
  shop_poly       geometry) 
distributed by (ghash7);
```

业务表比较巧的设计是用Geohash或ZOrder编码等方式将地理空间几何降维作为分布键，而不用构建空间索引。客流统计的SQL参考：

```
SELECT COUNT(1)
FROM (
  SELECT DISTINCT T0.uid FROM user_label T0 JOIN user_shop T1 
  ON T1.ghash7 = T0.ghash7
  WHERE T1.sid IN (1,2,3) AND (ST_Intersects(T0.workday_geo, T1.shop_poly) 
                               OR ST_Intersects(T0.weekend_geo, T1.shop_poly))
) c;
```

