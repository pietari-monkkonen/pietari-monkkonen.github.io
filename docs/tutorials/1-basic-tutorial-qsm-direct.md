---
title: Basic tutorial - QSM direct method
layout: default
parent: Tutorials
nav_order: 1
---

# Basic tutorial - QSM direct method

This tutorial goes over the example code `main_qsm_direct.m` provided along the GitHub repository, and demonstrates the very basic usage of the QSM direct method.

## Initialization

The code starts by clearing the workspace (were the variables are stored), closing all open figures, and adding the paths to subdirectories required in this demo.

```matlab
clear, close all
addpath("qsm-direct-method")
addpath("classes")
addpath("common-functions")
addpath("visualization")
```

## Defining the QSM

The quantitative structure model used as a basis for leaf generation is defined as the example QSM provided with the code repository of LeafGen. This is found in the example-data directory.

```matlab
filename = "example-data/ExampleQSM.mat";
QSM = importdata(filename);
```

The example QSM is of the format given by [TreeQSM], but the input QSM could also be a struct containing information of the QSM cylinders (see [QSM direct method]({% link docs/basic-functionality/1-qsm-direct.md %}))

## Defining leaf base geometry

The leaf base geometry is defined by setting the vertice points of the geometry and indicating the triangles with respect to the vertices. These values are assigned to the `LeafProperties` struct.

```matlab
% Vertices of the leaf base geometry
LeafProperties.vertices = [0.0    0.0    0.0;
                           -0.04  0.02   0.0;
                           0.0    0.10   0.0;
                           0.04   0.02   0.0];

% Triangles of the leaf base geometry
LeafProperties.triangles = [1 2 3;
                            1 3 4];
```

In the `LeafProperties.vertices` matrix each row corresponds to a singe vertice point and in the `LeafProperties.triangles` each row contains the row indices of vertices used for the triangles. These matrices define a leaf geometry based on two triangles which looks like this:

<img src="/assets/images/base-geometry-visualization.png" width="250" height="250" />

{: .note } 
The absolute dimensions of the leaf base geometry are not important, since the base geometry is scaled to the correct size in the LSD sampling phase. Only the shape of the base geometry is reserved.

## Setting petiole length limits

The leaf petioles are sampled unifromly from the interval defined to the `LeafProperties`struct:

```matlab
LeafProperties.petioleLengthLimits = [0.08 0.10];
```

The units of the length limits are meters.

## Defining target leaf distributions

The target distribution types and parameters for LADD, LOD, and LSD are defined in the `TargetDistributions` struct.

The LADD marginal distribution types and parameters with respect to the structural variables are defined as:

```matlab
% LADD relative height
TargetDistributions.dTypeLADDh = 'beta';
TargetDistributions.hParams = [22 3];

% LADD relative branch distance
TargetDistributions.dTypeLADDd = 'weibull';
TargetDistributions.dParams = [3.3 2.8];

% LADD compass direction
TargetDistributions.dTypeLADDc = 'vonmises';
TargetDistributions.cParams = [5/4*pi 0.1];
```

Fields `dTypeLADDh` and `hParams` define the distribution type and parameters for the LADD marginal distribution on the relative height, in this case a beta distribution with parameters $\alpha = 22$ and $\beta = 3$.


[TreeQSM]: https://github.com/InverseTampere/TreeQSM