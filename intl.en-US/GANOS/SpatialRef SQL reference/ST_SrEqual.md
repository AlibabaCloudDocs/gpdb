# ST\_SrEqual

This topic describes the ST\_SrEqual function, which determines whether two spatial reference systems are the same.

## Syntax

```
boolean ST_SrEqual(cstring sr1, cstring sr2, boolean strict default true);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|sr1|The string representing spatial reference system 1. It must be an OGC WKT or PROJ.4 string.|
|sr2|The string representing spatial reference system 2. It must be an OGC WKT or PROJ.4 string.|
|strict|Specifies whether to use the strict comparison method. The value true specifies to compare the names of the reference ellipsoids if the spatial reference systems are georeferenced. Default value: true.|

## Description

This function parses the semantics of two spatial reference systems to compare the parameter information of these systems. The compared parameter information includes the projection methods, the reference ellipsoids, and the long and short axes of the reference ellipsoids. If the spatial reference systems are the same, this function returns t. Otherwise, this function returns f.

## Example:

```
-- Compare two text-based spatial reference systems.
select ST_srEqual('GEOGCS["WGS 84",DATUM["WGS_1984",SPHEROID["WGS 84",6378137,298.257223563,AUTHORITY["EPSG","7030"]],AUTHORITY["EPSG","6326"]],PRIMEM["Greenwich",0,AUTHORITY["EPSG","8901"]],UNIT["degree",0.0174532925199433,AUTHORITY["EPSG","9122"]],AUTHORITY["EPSG","4326"]]', '+proj=longlat +datum=WGS84 +no_defs');

st_srequal 
------------
 t 

 -- Search for the spatial reference system identifier (SRID) of a spatial reference system from the spatial_ref_sys table.
 select srid from spatial_ref_sys where st_srequal(srtext::cstring, '+proj=longlat +ellps=GRS80 +no_defs ') limit 1;

 srid 
------
 3824
```

