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
The absolute dimensions of the leaf base geometry are not important, since the base geometry is scaled to the correct size in the LSD sampling phase. Only the shape of the base geometry is preserved.

## Setting petiole length limits

The leaf petioles are sampled unifromly from the interval defined to the `LeafProperties` struct:

```matlab
LeafProperties.petioleLengthLimits = [0.08 0.10];
```

The units of the length limits are meters, so in this case the petioles are sampled between 8 and 10 centimeters.

## Defining target leaf distributions

The target distribution types and parameters for LADD, LOD, and LSD are defined in the `TargetDistributions` struct.

The LADD marginal distribution types and parameters with respect to the structural variables are defined as:

```matlab
% LADD relative height
TargetDistributions.dTypeLADDh = 'beta';
TargetDistributions.pLADDh = [22 3];

% LADD relative branch distance
TargetDistributions.dTypeLADDd = 'weibull';
TargetDistributions.pLADDd = [3.3 2.8];

% LADD compass direction
TargetDistributions.dTypeLADDc = 'vonmises';
TargetDistributions.pLADDc = [5/4*pi 0.1];
```

Fields `dTypeLADDh` and `pLADDh` define the distribution type and parameters for the LADD marginal distribution on the relative height, in this case a beta distribution with parameters $\alpha$ = 22 and $\beta$ = 3. Similarly, `dTypeLADDd` and `pLADDd` define the relative branch distance marginal distribution as a truncated Weibull distribution with the scale parameter of $\lambda$ = 3.3 and the shape parameter of $k$ = 2.8, and `dTypeLADDc` and `pLADDc` define the compass direction marginal distribution as a von Mises distribution with the mean direction of $\mu = \frac{5\pi}{4}$ and the concentration parameter of $\kappa$ = 0.1.

The LOD marginal distribution types and parameters with respect to the inclination and azimuth angles of leaf normals are defined as:

```matlab
% LOD inclination angle
TargetDistributions.dTypeLODinc = 'dewit';
TargetDistributions.fun_pLODinc = @(h,d,c) [1 2];

% LOD azimuth angle
TargetDistributions.dTypeLODaz = 'uniform';
TargetDistributions.fun_pLODaz = @(h,d,c) [];
```

The `dTypeLODinc` and `fun_pLODinc` fields define the distribution type for leaf inclination angle and the function for the distribution parameters, in this case a generalized de Wit's distribution with parameters a = 1 and b = 2. The definition of distribution parameters, compared to LADD, is now done with a function, since the shape of LOD is allowed to change with respect to the structural variables. In this example, the parameters for inclination angle are set to be identical for the whole tree, so the field `fun_pLODinc` is set to be a handle for a constant function over all variable values. The `dTypeLODaz` and `fun_pLODaz`  fields define the distribution type and parameters for the leaf azimuth angle, in this case a uniform distribution and thus an empty parameter function as no parameters are needed for uniform distribuion.

Similarly to LOD, the LSD type and parameters are defined as:

```matlab
% LSD
TargetDistributions.dTypeLSD = 'normal';
TargetDistributions.fun_pLSD = @(h,d,c) [0.004 0.00025^2];
```

The `dTypeLSD` and `fun_pLSD` fields define the distribution type and parameter function for the LSD, in this case a normal distribution with expected value of 40 square centimeters and a variance of 2.5$^2$ square centimeters.

{: .note } 
The parameters of LOD inclination and azimuth angle distributions as well as LSD have to be given as function handles, even in the case they are constant or empty.


[TreeQSM]: https://github.com/InverseTampere/TreeQSM