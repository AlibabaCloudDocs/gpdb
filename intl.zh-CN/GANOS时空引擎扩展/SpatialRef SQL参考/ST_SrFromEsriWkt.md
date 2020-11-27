# ST\_SrFromEsriWkt

将一个Esri Wkt规范的字符串转为OGC规范的字符串。

## 语法

```
cstring ST_SrFromEsriWkt(cstring sr);
```

## 参数

|参数名称|描述|
|----|--|
|sr1|基于Esri Wkt规范的空间参考字符串。|

## 示例

```
SELECT ST_srFromEsriWkt('GEOGCS["China Geodetic Coordinate System 2000",DATUM["D_China_2000",SPHEROID["CGCS2000",6378137,298.257222101]],PRIMEM["Greenwich",0],UNIT["Degree",0.017453292519943295]]');

st_srfromesriwkt                                                                             
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 GEOGCS["China Geodetic Coordinate System 2000",DATUM["China_2000",SPHEROID["CGCS2000",6378137,298.257222101]],PRIMEM["Greenwich",0],UNIT["Degree",0.017453292519943295]]
```

