# ST\_SrEqual

判断两个空间参考是否相同。

## 语法

```
boolean ST_SrEqual(cstring sr1, cstring sr2, boolean strict default true);
```

## 参数

|参数名称|描述|
|----|--|
|sr1|空间参考1的字符串。必须是OGC WKT或者Proj4形式的字符串。|
|sr2|空间参考2的字符串。必须是OGC WKT或者Proj4形式的字符串。|
|strict|是否采用严格比较方式。如为true，则会对参考椭球体的名称进行比较。默认为true。|

## 描述

本函数通过解析语义的方式对空间参考进行比较，会对投影方式、参考椭球、长短半轴等参数信息进行比较。如果相同，返回t；如果不同，返回f。

## 示例

```
--比较两个基于文本的空间参考。
select ST_srEqual('GEOGCS["WGS 84",DATUM["WGS_1984",SPHEROID["WGS 84",6378137,298.257223563,AUTHORITY["EPSG","7030"]],AUTHORITY["EPSG","6326"]],PRIMEM["Greenwich",0,AUTHORITY["EPSG","8901"]],UNIT["degree",0.0174532925199433,AUTHORITY["EPSG","9122"]],AUTHORITY["EPSG","4326"]]', '+proj=longlat +datum=WGS84 +no_defs');

st_srequal 
------------
 t 

 --寻找spatial_ref_sys表中空间参考的srid。
 select srid from spatial_ref_sys where st_srequal(srtext::cstring, '+proj=longlat +ellps=GRS80 +no_defs ') limit 1;

 srid 
------
 3824
```

