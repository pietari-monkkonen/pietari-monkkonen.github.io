---
title: Basic tutorial - QSM leaf cylinder library method
layout: default
parent: Tutorials
nav_order: 2
---

# Basic tutorial - QSM leaf cylinder library method

This tutorial goes over the example code `main_qsm_leaf_cylinder_library.m` provided along the GitHub repository, and demonstrates the very basic usage of the QSM leaf cylinder library method.

## Initialization

The code starts by clearing the workspace (were the variables are stored), closing all open figures, and adding the paths to subdirectories required in this demo.

```matlab
clear, close all
addpath("qsm-leaf-cylinder-library-method")
addpath("classes")
addpath("common-functions")
addpath("visualization")
```

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

## Defining library LOD and LSD distribution types

For generatin a leaf cylinder library, we have to choose distribution type for the LOD inclination and azimuth distributions as well as for the LSD. This information is det into the `LibraryVariables` struct:

```matlab
% Leaf orientation distribution types
LibraryDistributions.dTypeLODinc = 'dewit';
LibraryDistributions.dTypeLODaz  = 'vonmises';

% Leaf size distribution type
LibraryDistributions.dTypeLSD    = 'uniform';
```

In this example, the inclination angle marginal distribution is a generalized de Wit's distribution, the azimuth angle marinal distribution is a von Mises distribution, and the LSD is a uniform distribution.

## Defining the library nodes

The library consists of leaf cylinders with different attributes. These attributes are discretized and the discretization points are called nodes. So each node corresponds to some selection of cylinder and LOD & LSD attributes, and can contain one or multiple leaf cylinders with these attributes. When a leaf cylinder library is used to generate foliage on a QSM, for each cylinder of the QSM LeafGen finds the library node that corresponds closest to the attributes of the cylinder and picks one of the node's leaf cylinders. The discretizations are defined to a struct named `Nodes`.

The leaf distribution attributes to discretize are:

- LOD inclination angle distribution parameters (two parameters)
- LOD azimuth angle distribution parameters (two parameters)
- LSD parameters (two parameters)

Since a library conists of single leaf cylinders, the LADD does not need to be defined at this point. It is only needed after the library generation when calculating leaf area budgets for a QSM.

The leaf distribution parameter discretizations are defined to the `Nodes` struct:

```matlab
% Inclination angle distribution nodes
Nodes.pLODinc1 = [-1 0 1];
Nodes.pLODinc2 = [2 3 4];

% Azimuth angle distribution nodes
Nodes.pLODaz1   = [0 pi/2 pi 3*pi/2];
Nodes.pLODaz2   = [0.01 0.5];

% Leaf size distribution nodes
Nodes.pLSD1 = [0.002 0.0025];
Nodes.pLSD2 = [0.003 0.0035];
```

The cylinder attributes to discretize are:

- length
- radius
- inclination angle
- azimuth angle
- leaf area

These are also defined to `Nodes` struct:

```matlab
Nodes.cylinderLength = [0.10 0.20];
Nodes.cylinderRadius = [0.01 0.05 0.1];
Nodes.cylinderInclinationAngle = [pi/4 3*pi/4];
Nodes.cylinderAzimuthAngle = [pi/4 3*pi/4 5*pi/4 7*pi/4];
Nodes.cylinderLeafArea = [0.2 0.5];
```

{: .note }
The number of discretization points in `Nodes` also defines the size of the leaf cylinder library. As the size of the library can grow exponentially when adding discratization points, the user should consider carefully for which attributes there should be more discretization points and which can do with less.

## Leaf cylinder library generation

When we have the leaf base geometry, petiole lengths, LOD and LSD types, and discretization nodes, we can generate a leaf cylinder library with the function `generate_leaf_cylinder_library'. 

```matlab
LeafCylinderLibrary = generate_leaf_cylinder_library( ...
                          LibraryDistributions,Nodes,LeafProperties);
```

The function opens a parallel pool of computing cores to utilize parallel computation of the library cylinders. The progress of library generation is shown in a separate progress bar window.

# Saving the leaf cylinder library

As generating a library requires a long computation, it is worth to save it for later use as a .mat file. This can be done with the command

```matlab
save("NewLeafCyliderLibrary.mat",'-struct','LeafCylinderLibrary')
```
This creates a file called NewLeafCyliderLibrary.mat to the current working directory.

## Defining the QSM

After generating a leaf cylinder library, we can use it to generate foliage on QSMs. The QSM used as a basis for leaf generation is defined as the example QSM provided with the code repository of LeafGen. This is found in the example-data directory.

```matlab
filename = "example-data/ExampleQSM.mat";
QSM = importdata(filename);
```

