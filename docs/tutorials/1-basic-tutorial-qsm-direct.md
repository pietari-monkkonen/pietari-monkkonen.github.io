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
LeafProperties.triangles = [1, 2, 3;
                            1, 3, 4];
```

These matrices define a leaf geometry based on two triangles which looks like this:

<img src="/assets/images/base-geometry-visualization.png" width="50" height="50" />

![leaf base geometry](/assets/images/base-geometry-visualization.png){: height="100" }


[TreeQSM]: https://github.com/InverseTampere/TreeQSM