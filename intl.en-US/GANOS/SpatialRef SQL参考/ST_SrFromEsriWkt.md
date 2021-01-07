# ST\_SrFromEsriWkt

This function converts the specified string from the Esri well-known text \(WKT\) format to the Open Geospatial Consortium \(OGC\) WKT format.

## Syntax

```
cstring ST_SrFromEsriWkt(cstring sr);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|sr1|The spatial reference string that uses the Esri WKT format.|

## Examples

```
SELECT ST_srFromEsriWkt('GEOGCS["China Geodetic Coordinate System 2000",DATUM["D_China_2000",SPHEROID["CGCS2000",6378137,298.257222101]],PRIMEM["Greenwich",0],UNIT["Degree",0.017453292519943295]]');

st_srfromesriwkt                                                                             
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 GEOGCS["China Geodetic Coordinate System 2000",DATUM["China_2000",SPHEROID["CGCS2000",6378137,298.257222101]],PRIMEM["Greenwich",0],UNIT["Degree",0.017453292519943295]]
```

