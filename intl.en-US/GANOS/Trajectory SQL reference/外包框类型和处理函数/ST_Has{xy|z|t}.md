# ST\_Has\{xy\|z\|t\}

The ST\_Has\{xy\|z\|t\} function is used to check whether a bounding box has one or more specified dimensions.

## Syntax

```
bool ST_HasXY(boxndf box);
bool ST_HasZ(boxndf box);
bool ST_HasT(boxndf box);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|box|The bounding box that you want to query.|

## Description

This function allows you to check whether a bounding box has one or more specified dimensions. The x and y dimensions are bound to each other. Therefore, the bounding box has both or neither of the x and y dimensions.

## Example

```
postgres=# select ST_hasz(ST_MakeBox2dt(0,0,'2000-01-01'::timestamp, 20,20, '2020-01-01'::timestamp));
 st_hasz 
---------
 f

postgres=# select ST_hast(ST_MakeBox2dt(0,0,'2000-01-01'::timestamp, 20,20, '2020-01-01'::timestamp));
 st_hast 
---------
 t
```

