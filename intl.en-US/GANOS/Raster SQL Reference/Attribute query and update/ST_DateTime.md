# ST\_DateTime

This topic describes the ST\_DateTime function, which obtains the time information of a raster or band.

## Syntax

```
text ST_DateTime(raster raster_obj);
timetamp ST_DateTime(raster raster_obj,integer band);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster whose time information you want to obtain.|
|band|The sequence number of the band whose time information you want to obtain. Valid values start from 0.|

## Description

This function obtains the time information of a raster or band. If you specify the band parameter, this function returns the time information of the specified band. Otherwise, this function returns the time information of all bands of the raster by using a JSON-formatted string.

## Examples

```
SELECT ST_DateTime(raster_obj)
FROM raster_table;

                                                                                                         st_datetime                                                                                                         
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 {"0":"Mon Dec 31 00:00:00 2018","1":"Mon Dec 31 01:00:00 2018","2":"Mon Dec 31 02:00:00 2018","3":"Mon Dec 31 03:00:00 2018","4":"Mon Dec 31 04:00:00 2018","5":"Mon Dec 31 05:00:00 2018","6":"Mon Dec 31 06:00:00 2018",".
.7":"Mon Dec 31 07:00:00 2018","8":"Mon Dec 31 08:00:00 2018","9":"Mon Dec 31 09:00:00 2018","10":"Mon Dec 31 10:00:00 2018","11":"Mon Dec 31 11:00:00 2018","12":"Mon Dec 31 12:00:00 2018","13":"Mon Dec 31 13:00:00 2018".
.,"14":"Mon Dec 31 14:00:00 2018","15":"Mon Dec 31 15:00:00 2018","16":"Mon Dec 31 16:00:00 2018","17":"Mon Dec 31 17:00:00 2018","18":"Mon Dec 31 18:00:00 2018","19":"Mon Dec 31 19:00:00 2018","20":"Mon Dec 31 20:00:00 .
.2018","21":"Mon Dec 31 21:00:00 2018","22":"Mon Dec 31 22:00:00 2018","23":"Mon Dec 31 23:00:00 2018"}
 
 
SELECT ST_DateTime(raster_obj, 0)
FROM raster_table;

         datetime          
---------------------------
"Mon Dec 31 00:00:00 2018"
```

