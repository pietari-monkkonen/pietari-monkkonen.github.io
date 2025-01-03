---
title: QSM direct method
layout: default
nav_order: 11
---

# QSM direct method


## Function call
Foliage is generated on a QSM by calling the function `generate_foliage_qsm_direct`.  The format of the basic function call is
```matlab
Leaves = generate_foliage_qsm_direct(QSM,TargetDistributions,...
    LeafProperties,totalLeafArea);
```
where `QSM` is a struct containing a quantitative structure model, `TargetDistributions` is a struct containing the target distribution types and parameters for the leaf distributions, `LeafProperties` is a struct containing leaf base geometry and twig length information, and `totalLeafArea` is a value for the total leaf area to be added on the tree model.

## Inputs

### QSM

LeafGen uses the QSM format produced by the [TreeQSM] method. Optionally, a QSM can be intialized by defining a struct containing the basic information of the cylinders, in which case the required fields for the `qsm` struct are:
|Field name|Description|Type|
|:-|:-|:-|
|`cylinder.start`|Cylinder start points|single|
|`cylinder.axis`|Cylinder axis|single|
|`cylinder.length`|Cylinder lengths|single|
|`cylinder.radius`|Cylinder radii|single|
|`cylinder.parent`|Cylinder parent cylinders|integer|
|`cylinder.branch`|Cylinder branch index|integer|


### Target Distributions

### Leaf Properties

### Total leaf area
The required fields for the `TargetDistributions` struct are:

|Field name|Description|Type|
|:-|:-|:-|
|`dTypeLADDh`|LADD relative height distribution type|string|
|`dTypeLADDd`|LADD relative distance along subbranch distribution type|string|
|`dTypeLADDc`|LADD compass direction distribution type|string|
|`hParams`|LADD heightwise distribution parameters|double|
|`dParams`|LADD distancewise distribution parameters|double|
|`cParams`|LADD directionwise distribution parameters|double|
|`dTypeLODinc`|LOD inclination angle distribution type|string|
|`dTypeLODaz`|LOD azimuth angle distribution type|string|
|`fun_inc_params`|Function for LOD inclination angle distribution parameters|function handle|
|`fun_az_params`|Function for LOD azimuth angle distribution parameters|function handle|
|`dTypeLSD`|LSD distribution type|string|
|`fun_size_params`|Function for LSD distribution parameters|function handle|

The required fields for the `LeafProperties` struct are:

|Field name|Description|Type|
|:-|:-|:-|
|`petioleLengthLimits`|Leaf petiole length limits|double|
|`vertices`|Vertice coordinates for leaf base geometry triangles|double|
|`triangles`|Indicators for the vertices of each triangle|double|

[TreeQSM]: https://github.com/InverseTampere/TreeQSM