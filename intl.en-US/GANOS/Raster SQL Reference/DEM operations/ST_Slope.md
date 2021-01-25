# ST\_Slope

This function calculates the slope from each cell of a raster surface and returns an array of slopes in units of degrees.

## Syntax

```
float8[] ST_Slope(raster rast, integer pyramid_level, integer band, Box extent, BoxType type, float8 zfactor);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast|The raster object.|
|pyramid\_level|The pyramid level.|
|Band|The sequence number of the band.|
|box|The area to be analyzed, in the format of `'((minX,minY),(maxX,maxY))'`.|
|type|The coordinate type of the area to be analyzed. You can specify only one value. Valid values: -   Raster: pixel coordinates
-   World: world coordinates |
|zfactor|The conversion factor that adjusts the units of measure for the vertical \(or elevation\) units when the units are different from the horizontal coordinate \(x,y\) units of the input surface. Default value: 1.|

## Description

The [slope](http://desktop.arcgis.com/zh-cn/arcmap/10.3/tools/spatial-analyst-toolbox/slope.htm) function calculates the maximum rate of change in value from each cell to its neighbors. Basically, the maximum change in elevation over the distance between the cell and its eight neighbors identifies the steepest downhill descent from the cell.

## Examples

```
select st_slope(rast, 0, 0, '(0,0), (5,5)', 'Raster', 2.0) from t_surface where id=1;
                                                        st_slope                                                        
------------------------------------------------------------------------------------------------------------------------
 {0.210279822382945,0.369954478614613,0.220241089748741,0.415504885724335,0.523380429142649,0.19079849696355,0.32493941.
.6771785,0.592806538904308,0.559435506670953,0.487598684366856,0.17107200555711,0.0840217922621739,0.381860307736563,0..
.580949493078414,0.638382145824945,0.43822706997925,0.317039337499457,0.284095352866283,0.284298114561498,0.49448931974.
.7884,0.817870125632039,0.645927342349833,0.241209216517688,0.211549813235801,0.339040463954188,0.636582806346833,0.934.
.672430381381,0.7814534477193,0.0832677675316725,0.0544326656785603,0.529537557031012,0.836305912538514,0.9446346707062.
.92,0.747858756227042,0.0284106287186713,0.0616861154423774}
(1 row)
```

