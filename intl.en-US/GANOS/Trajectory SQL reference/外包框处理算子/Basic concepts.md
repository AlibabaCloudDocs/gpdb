# Basic concepts

A bounding box-processing operator is used to check whether the bounding boxes of the two objects that are specified by the left and right operands meet a specified spatio-temporal relationship.

## Operator marks

The following operator marks are supported:

-   The mark for INTERSECT operators: `&& or &`
-   The mark for INCLUDE operators: `@>`
-   The mark for INCLUDED operators: `<@`

**Note:**

-   If a dimension mark resides between a pair of operator marks, the operator compares two objects in this dimension in addition to the x and y dimensions.
-   If a dimension mark resides on the left and right sides of an operator mark, the operator compares two objects only in this dimension. In this case, the operator does not compare the two objects in the other dimensions, such as the x and y dimensions.

## Dimension marks

The following dimension marks are supported:

-   The mark for the z dimension: /
-   The mark for the t dimension: \#

**Note:**

-   If an operator does not contain a dimension mark, the operator specifies a two-dimensional relationship.
-   If an operator contains both dimension marks, the mark for the z dimension precedes the mark for the t dimension.

