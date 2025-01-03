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

