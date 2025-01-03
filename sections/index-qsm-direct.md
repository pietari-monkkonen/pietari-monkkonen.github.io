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
Generation of foliage on the QSM requires the structs `TargetDistributions` and `LeafProperties` as inputs. The contents for these structs are the following:

|`dTypeLADDh`|LADD relative height distribution type|
|`dTypeLADDd`|LADD relative distance along subbranch distribution type|
|`dTypeLADDc`|LADD compass direction distribution type|
|`hParams`|LADD heightwise distribution parameters|
|`dParams`|LADD distancewise distribution parameters|
|`cParams`|LADD directionwise distribution parameters|
|`dTypeLODinc`|LOD inclination angle distribution type|
|`dTypeLODaz`|LOD azimuth angle distribution type|
|`fun_inc_params`|Function for LOD inclination angle distribution parameters|
|`fun_az_params`|Function for LOD azimuth angle distribution parameters|
|`dTypeLSD`|LSD distribution type|
|`fun_size_params`|Function for LSD distribution parameters|

