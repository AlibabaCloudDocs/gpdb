# Geometry model

## Overview

Ganos Geometry is a spatial geometry extension of AnalyticDB for PostgreSQL. Ganos Geometry complies with OpenGIS specifications and enables AnalyticDB for PostgreSQL to store and manage 2D \(X, Y\), 3D \(X, Y, Z\), and 4D \(X, Y, Z, M\) spatial geometry data. Ganos Geometry also provides a wide range of features such as spatial geometry objects, indexes, functions, and operators. The geometry model is fully compatible with PostGIS functions and allows you to smoothly migrate existing applications.

## Quick start

-   Create extensions.

    ```
    -- Create geometry extensions.
    Create extension ganos_spatialref;
    Create extension ganos_geometry;
    ```

-   Create a geometry table.

    ```
    -- Method 1: Create a table that contains a geometry field.
    CREATE TABLE ROADS ( ID int4, ROAD_NAME varchar(25), geom geometry(LINESTRING,3857)) DISTRIBUTED BY (ID);
    ```

    ```
    -- Method 2: Create a common table and add a geometry field to the table.
    CREATE TABLE ROADS ( ID int4, ROAD_NAME varchar(25) )  DISTRIBUTED BY (ID);
    SELECT AddGeometryColumn( 'roads', 'geom', 3857, 'LINESTRING', 2);
    ```

-   Add geometry constraints.

    ```
    ALTER TABLE ROADS ADD CONSTRAINT geometry_valid_check CHECK (ST_IsValid(geom));
    ```

-   Import geometry data.

    ```
    INSERT INTO roads (id, geom, road_name)
      VALUES (1,ST_GeomFromText('LINESTRING(191232 243118,191108 243242)',3857),'North Fifth-Ring Road');
    INSERT INTO roads (id, geom, road_name)
      VALUES (2,ST_GeomFromText('LINESTRING(189141 244158,189265 244817)',3857),'East Fifth-Ring Road');
    INSERT INTO roads (id, geom, road_name)
      VALUES (3,ST_GeomFromText('LINESTRING(192783 228138,192612 229814)',3857),'South Fifth-Ring Road');
    INSERT INTO roads (id, geom, road_name)
      VALUES (4,ST_GeomFromText('LINESTRING(189412 252431,189631 259122)',3857),'West Fifth-Ring Road');
    INSERT INTO roads (id, geom, road_name)
      VALUES (5,ST_GeomFromText('LINESTRING(190131 224148,190871 228134)',3857),'East Chang'an Avenue');
    INSERT INTO roads (id, geom, road_name)
      VALUES (6,ST_GeomFromText('LINESTRING(198231 263418,198213 268322)',3857),'West Chang'an Avenue');
    ```

-   Query geometry object information.

    ```
    SELECT id, ST_AsText(geom) AS geom, road_name FROM roads;
    
    --------------------------------
         id | geom                                    | road_name
    --------+-----------------------------------------+-----------
        1 | LINESTRING(191232 243118,191108 243242) | North Fifth-Ring Road
        2 | LINESTRING(189141 244158,189265 244817) | East Fifth-Ring Road
        3 | LINESTRING(192783 228138,192612 229814) | South Fifth-Ring Road
        4 | LINESTRING(189412 252431,189631 259122) | West Fifth-Ring Road
        5 | LINESTRING(190131 224148,190871 228134) | East Chang'an Avenue
        6 | LINESTRING(198231 263418,198213 268322) | West Chang'an Avenue
    (6 rows)
    ```

-   Create indexes.

    ```
    -- Create a GiST index.
    CREATE INDEX [indexname] ON [tablename] USING GIST ( [geometryfield] ); 
    CREATE INDEX [indexname] ON [tablename] USING GIST ([geometryfield] gist_geometry_ops_nd);
    VACUUM ANALYZE [table_name] [(column_name)];
    
    -- Example
    Create INDEX sp_geom_index ON ROADS USING GIST(geom);
    VACUUM ANALYZE ROADS (geom);
    ```

-   Measure and analyze spatial data.

    ```
    --Create Table bc_roads: 
    Column      | Type              | Description
    ------------+-------------------+-------------------
    gid         | integer           | Unique ID
    name        | character varying | Road Name
    the_geom    | geometry          | Location Geometry (Linestring)
    
    --Create table bc_municipality:
    Column     | Type              | Description
    -----------+-------------------+-------------------
    gid        | integer           | Unique ID
    code       | integer           | Unique ID
    name       | character varying | City / Town Name
    the_geom   | geometry          | Location Geometry (Polygon)
    
    -- Calculate the length.
    SELECT sum(ST_Length(the_geom))/1000 AS km_roads FROM bc_roads;
    
    km_roads
    ------------------
    70842.1243039643
    (1 row)
    
    -- Calculate the area.
    SELECT ST_Area(the_geom)/10000 AS hectares FROM bc_municipality WHERE name = 'PRINCE GEORGE';
    
    hectares
    ------------------
    32657.9103824927
    (1 row)
    ```

-   Identify spatial relationships.

    ```
    --ST_Contains
    SELECT  m.name, sum(ST_Length(r.the_geom))/1000 as roads_km
    FROM
      bc_roads AS r, bc_municipality AS m
    WHERE
      ST_Contains(m.the_geom,r.the_geom)
    GROUP BY m.name
    ORDER BY roads_km;
    
    name                        | roads_km
    ----------------------------+------------------
    SURREY                      | 1539.47553551242
    VANCOUVER                   | 1450.33093486576
    LANGLEY DISTRICT            | 833.793392535662
    BURNABY                     | 773.769091404338
    PRINCE GEORGE               | 694.37554369147
    ...
    
    --ST_Covers,a circle covering a circle
    SELECT ST_Covers(smallc,smallc) As smallinsmall,
      ST_Covers(smallc, bigc) As smallcoversbig,
      ST_Covers(bigc, ST_ExteriorRing(bigc)) As bigcoversexterior,
      ST_Contains(bigc, ST_ExteriorRing(bigc)) As bigcontainsexterior
    FROM (SELECT ST_Buffer(ST_GeomFromText('POINT(1 2)'), 10) As smallc,
      ST_Buffer(ST_GeomFromText('POINT(1 2)'), 20) As bigc) As foo;
      --Result
     smallinsmall | smallcoversbig | bigcoversexterior | bigcontainsexterior
    --------------+----------------+-------------------+---------------------
     t            | f              | t                 | f
    (1 row) 
    
    --ST_Disjoint
    SELECT ST_Disjoint('POINT(0 0)'::geometry, 'LINESTRING ( 2 0, 0 2 )'::geometry);
     st_disjoint
    ---------------
     t
    (1 row)
    SELECT ST_Disjoint('POINT(0 0)'::geometry, 'LINESTRING ( 0 0, 0 2 )'::geometry);
     st_disjoint
    ---------------
     f
    (1 row)
    
    --ST_Overlaps
    SELECT ST_Overlaps(a,b) As a_overlap_b,
          ST_Crosses(a,b) As a_crosses_b,
        ST_Intersects(a, b) As a_intersects_b, ST_Contains(b,a) As b_contains_a
    FROM (SELECT ST_GeomFromText('POINT(1 0.5)') As a, ST_GeomFromText('LINESTRING(1 0, 1 1, 3 5)')  As b)
      As foo
    
    a_overlap_b | a_crosses_b | a_intersects_b | b_contains_a
    ------------+-------------+----------------+--------------
    f           | f           | t              | t
    
    --ST_Relate
    SELECT ST_Relate(ST_GeometryFromText('POINT(1 2)'), ST_Buffer(ST_GeometryFromText('POINT(1 2)'),2), '0FFFFF212');
    st_relate
    -----------
    t
    
    --ST_Touches
    SELECT ST_Touches('LINESTRING(0 0, 1 1, 0 2)'::geometry, 'POINT(1 1)'::geometry);
     st_touches
    ------------
    f
    (1 row)
    
    SELECT ST_Touches('LINESTRING(0 0, 1 1, 0 2)'::geometry, 'POINT(0 2)'::geometry);
     st_touches
    ------------
     t
    (1 row)
    
    --ST_Within
    SELECT ST_Within(smallc,smallc) As smallinsmall,
      ST_Within(smallc, bigc) As smallinbig,
      ST_Within(bigc,smallc) As biginsmall,
      ST_Within(ST_Union(smallc, bigc), bigc) as unioninbig,
      ST_Within(bigc, ST_Union(smallc, bigc)) as biginunion,
      ST_Equals(bigc, ST_Union(smallc, bigc)) as bigisunion
    FROM
    (
    SELECT ST_Buffer(ST_GeomFromText('POINT(50 50)'), 20) As smallc,
      ST_Buffer(ST_GeomFromText('POINT(50 50)'), 40) As bigc) As foo;
    --Result
     smallinsmall | smallinbig | biginsmall | unioninbig | biginunion | bigisunion
    --------------+------------+------------+------------+------------+------------
     t            | t          | f          | t          | t          | t
    (1 row)
    ```

-   Store geometry objects.

    ```
    SELECT ST_IsSimple(ST_GeomFromText('POLYGON((1 2, 3 4, 5 6, 1 2))'));
     st_issimple
    -------------
     t
    (1 row)
    
     SELECT ST_IsSimple(ST_GeomFromText('LINESTRING(1 1,2 2,2 3.5,1 3,1 2,2 1)'));
     st_issimple
    -------------
     f
    (1 row)
    
    
    -- Query the largest city that has traffic circles in the terrain.
    SELECT gid, name, ST_Area(the_geom) AS area
    FROM bc_municipality
    WHERE ST_NRings(the_geom) > 1
    ORDER BY area DESC LIMIT 1;
    
    gid  | name         | area
    -----+--------------+------------------
    12   | Anning        | 257374619.430216
    (1 row)
    ```

-   Delete the extensions.

    ```
    Drop extension ganos_geometry;
    DROP extension ganos_spatialref;
    ```


## SQL reference

For more information, visit [PostGIS Reference](http://postgis.net/docs/reference.html).

