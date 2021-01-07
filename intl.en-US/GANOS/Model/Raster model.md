# Raster model

## Overview

Raster data consists of a matrix of cells or pixels that are organized into rows and columns, or a grid. Each cell contains a value that represents information, such as temperature.

Raster data can be digital aerial photographs, satellite images, digital images, or scanned maps.

Ganos implements the raster model in AnalyticDB for PostgreSQL. This allows Ganos to store and analyze raster data in a cost-effective manner by using database technologies and methods.

## Quick start

-   Create extensions.

    ```
    Create extension ganos_spatialref;
    Create extension ganos_geometry;
    Create Extension Ganos_Raster;
    ```

-   Create a raster table.

    ```
    -- Specify the raster column as the distribution column of the table.
    Create Table raster_table(id integer, raster_obj raster) 
    DISTRIBUTED BY (raster_obj);
    ```

-   Import raster data from Object Storage Service \(OSS\).

    ```
    Insert into raster_table 
    Values(1, ST_ImportFrom('chunk_table','OSS://<id>:<key>@oss-cn.aliyuncs.com/mybucket/data/my_image.tif')),
    (2, ST_CreateRast('OSS://<id>:<key>@oss-cn.aliyuncs.com/mybucket/data/my_image.tif'));
    ```

-   Query raster object information.

    ```
    Select ST_Height(raster_obj),ST_Width(raster_obj) From raster_table Where id = 1;
     st_height 
    -----------
          1241
    (1 rows)
    ```

-   Create a pyramid.

    ```
    DO $$
    declare
        rast raster;
    begin
        select raster_obj into rast from raster_table where id = 1;
        rast = st_buildpyramid(rast);
        update raster_table set raster_obj = rast where id = 1;
    end;    
    $$ LANGUAGE 'plpgsql';
    
    DO $$
    declare
        rast raster;
    begin
        select raster_obj into rast from raster_table where id = 2;
        rast = st_buildpyramid(rast, -1, 'Near', 'chunk_table');
        update raster_table set raster_obj = rast where id = 2;
    end;    
    $$ LANGUAGE 'plpgsql';
    ```

-   Calculate the optimal pyramid level based on the world space, width, and height of a viewport.

    ```
    Select ST_BestPyramidLevel(rast, '((128.0, 30.0),(128.5, 30.5))', 800, 600) from raster_table where id = 1;
    
    ---------------------
    3
    ```

-   Retrieve a pixel matrix from the specified space of a raster object.

    ```
    DO $$
    declare
        rast raster;
    begin
        select raster_obj into rast from raster_table where id = 1;
        rast = st_buildpyramid(rast);
        Select ST_Clip(rast, 0, '((128.980,30.0),(129.0,30.2))', 'World');
    end;    
    $$ LANGUAGE 'plpgsql';
    ```

-   Calculate the raster space of a clipped area.

    ```
    Select ST_ClipDimension(raster_obj, 2, '((128.0, 30.0),(128.5, 30.5))') 
    from raster_table where id = 1;
    
    ------------------------------------
    '((600, 720),(200, 300))'
    ```

-   Delete the extensions.

    ```
    DROP Extension Ganos_Raster;
    DROP extension ganos_geometry;
    DROP extension ganos_spatialref;
    ```


## SQL reference

For more information, see [Concepts](/intl.en-US/Spatio-temporal Database/Raster SQL reference/Concepts.md).

