# ST\_MetaItems

This topic describes the ST\_MetaItems function, which returns the customized metadata items of a raster.

## Syntax

```
text[] ST_metaItems(raster raster_obj);
text[] ST_metaItems(raster raster_obj,
                    integer band);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster whose customized metadata items you want to obtain.|
|band|The sequence number of the band whose customized metadata items you want to obtain. A raster has one or more bands. Valid values start from 0.|

## Examples

```
-- meta data items of a raster
SELECT ST_MetaItems(raster_obj) 
FROM raster_table;
                                                                       st_metaitems                                                                                                                                    
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 {latitude#long_name,latitude#units,longitude#long_name,longitude#units,NC_GLOBAL#Conventions,NC_GLOBAL#history,NETCDF_DIM_EXTRA,NETCDF_DIM_time_DEF,NETCDF_DIM_time_VALUES}
  
-- meta data items of a band
SELECT ST_MetaItems(raster_obj, 0) 
FROM raster_table;

                                       st_metaitems                                        
-------------------------------------------------------------------------------------------
{add_offset,long_name,missing_value,NETCDF_DIM_time,NETCDF_VARNAME,scale_factor,units,_FillValue}                                                                                  
```

