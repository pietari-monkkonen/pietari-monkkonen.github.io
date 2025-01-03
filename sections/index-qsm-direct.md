---
title: QSM direct method
layout: default
nav_order: 11
---

# QSM direct method

## Basics

### Function call
Foliage is generated on a QSM by calling the function `generate_foliage_qsm_direct`.  The format of the basic function call is
```matlab
Leaves = generate_foliage_qsm_direct(QSM,TargetDistributions,...
    LeafProperties,totalLeafArea);
```
where `QSM` is a struct containing a quantitative structure model, `TargetDistributions` is a struct containing the target distribution types and parameters for the leaf distributions, `LeafProperties` is a struct containing leaf base geometry and twig length information, and `totalLeafArea` is a value for the total leaf area to be added on the tree model.

### Inputs
Generation of foliage on the QSM requires the structs `TargetDistributions` and `LeafProperties` as inputs. The required fields for `TargetDistributions` struct are:

|Field name|Description|Type|
|:-|:-|:-|
|`dTypeLADDh`|LADD relative height distribution type|string|
|`dTypeLADDd`|LADD relative distance along subbranch distribution type|string|
|`dTypeLADDc`|LADD compass direction distribution type|string|
|`hParams`|LADD heightwise distribution parameters|double (vector)|
|`dParams`|LADD distancewise distribution parameters|double (vector)|
|`cParams`|LADD directionwise distribution parameters|double (vector)|
|`dTypeLODinc`|LOD inclination angle distribution type|string|
|`dTypeLODaz`|LOD azimuth angle distribution type|string|
|`fun_inc_params`|Function for LOD inclination angle distribution parameters|function handle|
|`fun_az_params`|Function for LOD azimuth angle distribution parameters|function handle|
|`dTypeLSD`|LSD distribution type|string|
|`fun_size_params`|Function for LSD distribution parameters|function handle|

The required fields for `LeafProperties` struct are:

|`petioleLengthLimits`|Leaf petiole length limits||
|`vertices`|Vertice coordinates for leaf base geometry triangles||
|`triangles`|Indicators for the vertices of each triangle||
