# ST\_Value

This function returns the value of a cell based on the specified band, row number, and column number.

## Syntax

```
float8 ST_Value(raster rast, integer band, integer colsn, integer rowsn, boolean exclude_nodata_value);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast|The raster object.|
|band|The sequence number of the band.|
|colsn|The column number of the cell.|
|rowsn|The row number of the cell.|
|exclude\_nodata\_value|Specifies whether to exclude the NoData value. Default value: true.|

## Description

This function returns the value of a cell based on the specified band, row number, and column number. The band sequence number starts from 0. The default band sequence number, row number, and column number are all 0.

## Examples

```
select st_value(rast, 1, 3, 4) from t_pixel where id=2;
 st_value 
----------
       88
(1 row)
```

