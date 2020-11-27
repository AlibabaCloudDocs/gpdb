# ST\_SrReg

注册一个新的空间参考。

## 语法

```
integer ST_SrReg(cstring sr);
integer ST_SrReg(cstring auth_name, integer auth_id, cstring sr);
```

## 参数

|参数名称|描述|
|----|--|
|sr|空间参考字符串，必须是OGC WKT或者Proj4形式的字符串。|
|auth\_name|空间参考系统定义的作者，例如EPSG。|
|auth\_id|空间参考系统定义的空间参考ID。|

## 描述

如果空间参考已经存在，则返回已经存在的空间参考srid；如果空间参考不存在，则会向spatial\_ref\_sys表中插入一条记录并返回新空间参考的srid。

## 示例

```
--空间参考已存在
select 4490, ST_srReg('GEOGCS["China Geodetic Coordinate System 2000",DATUM["China_2000",SPHEROID["CGCS2000",6378137,298.257222101,AUTHORITY["EPSG","1024"]],AUTHORITY["EPSG","1043"]],PRIMEM["Greenwich",0,AUTHORITY["EPSG","8901"]],UNIT["degree",0.0174532925199433,AUTHORITY["EPSG","9122"]],AUTHORITY["EPSG","4490"]]');
 st_srreg 
----------
     4490

--新空间参考
select ST_srReg('user_defined',100, 'GEOGCS["User Geodetic Coordinate System ",DATUM["China_2000",SPHEROID["CGCS2000",6378137,298.257222101,AUTHORITY["EPSG","903"]],AUTHORITY["EPSG","1043"]],PRIMEM["Greenwich",0,AUTHORITY["EPSG","8901"]],UNIT["degree",0.0174532925199433,AUTHORITY["EPSG","9122"]],AUTHORITY["EPSG","4491"]]');
 st_srreg 
----------
    10001

select ST_srReg('+proj=tmerc +lat_0=1 +lon_0=112 +k=1 +x_0=19500001 +y_0=0 +ellps=krass +towgs84=24.47,-130.89,-81.56,0,0,0.13,-0.22 +units=m +no_defs');
 st_srreg 
----------
   10002
```

