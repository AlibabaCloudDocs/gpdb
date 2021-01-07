# ST\_SrReg

This topic describes the ST\_SrReg function, which registers a spatial reference system.

## Syntax

```
integer ST_SrReg(cstring sr);
integer ST_SrReg(cstring auth_name, integer auth_id, cstring sr);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|sr|The string representing the spatial reference system. It must be an OGC WKT or PROJ.4 string.|
|auth\_name|The author who defines the spatial reference system. Example: EPSG.|
|auth\_id|The spatial reference system identifier \(SRID\) of the spatial reference system.|

## Description

If the spatial reference system already exists, this function returns the SRID of the existing spatial reference system. If the spatial reference system does not exist, this function inserts a record into the spatial\_ref\_sys table and returns the SRID of the new spatial reference system.

## Example:

```
-- Register a spatial reference system that already exists.
select 4490, ST_srReg('GEOGCS["China Geodetic Coordinate System 2000",DATUM["China_2000",SPHEROID["CGCS2000",6378137,298.257222101,AUTHORITY["EPSG","1024"]],AUTHORITY["EPSG","1043"]],PRIMEM["Greenwich",0,AUTHORITY["EPSG","8901"]],UNIT["degree",0.0174532925199433,AUTHORITY["EPSG","9122"]],AUTHORITY["EPSG","4490"]]');
 st_srreg 
----------
     4490

-- Register a new spatial reference system.
select ST_srReg('user_defined',100, 'GEOGCS["User Geodetic Coordinate System ",DATUM["China_2000",SPHEROID["CGCS2000",6378137,298.257222101,AUTHORITY["EPSG","903"]],AUTHORITY["EPSG","1043"]],PRIMEM["Greenwich",0,AUTHORITY["EPSG","8901"]],UNIT["degree",0.0174532925199433,AUTHORITY["EPSG","9122"]],AUTHORITY["EPSG","4491"]]');
 st_srreg 
----------
    10001

select ST_srReg('+proj=tmerc +lat_0=1 +lon_0=112 +k=1 +x_0=19500001 +y_0=0 +ellps=krass +towgs84=24.47,-130.89,-81.56,0,0,0.13,-0.22 +units=m +no_defs');
 st_srreg 
----------
   10002
```

