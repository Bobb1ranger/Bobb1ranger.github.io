---
layout: post
title: Mujoco
date: 2025-11-14 20:14:00-0500
description: Some basic features of Mujoco.
tags: robotics, simulation, programming.
related_posts: true
---

# MuJoCo Overview

MuJoCo is a full-featured simulator designed from the ground up for the purpose of **model-based** optimization, and in particular optimization through contacts. Because of its efficiency and high accuracy, MuJoCo makes it possible to evaluate many computationally intensive techniques—such as optimal control, physically consistent state estimation, and system identification—on complex dynamical systems with **contact-rich** behaviors. It also supports more traditional applications, including validating control schemes before deploying them on physical robots, interactive scientific visualization, virtual environments.

Key features:
* Simulation in generalized coordinates, avoiding joint violations
* Inverse dynamics that are well-defined **even in the presence of contacts**.
* Kinematic constraints treatment.
* Many types of available actuators.
* Vast options of ODE solvers and integrators.
* XML model format (called MJCF) and built-in model compiler. Cross-platform GUI in OpenGL. 


# Cmake and its use in MujoCo
## What is CMake
CMake is a cross-platform build system generator.
* reads configuration files (CMakeLists.txt)
* Detects the system, compilers, dependencies and paths.
* Generates platform -specific build systems. For example, Makefiles on Linux.

CMake = the tool that prepares the actual build environment, so you can build the same project across many platforms without rewriting build scripts.

## CMake's functrelated_posts: false
  - "/blog/2025/Mujoco_notes"ion in Mujoco
Mujoco uses CMake to configure, build and install its source code. Specifically, CMake handles several things.

* Building and configuring the Mujoco library. (If we're using precompiled Mujoco binaries, we don't need CMake.)
* Managing dependencies. (GL/GLEW)
* Cross-platform support.
* Configuring compile options.
* Installation and packaging. (Let CMake install headers and libraries to a clean, standard layout.)









Convert Solidworks drawings to MJCF
related_posts: false
  - "/blog/2025/Mujoco_notes"

## File Type and Simulation Building Process

### Model Format:
.xml type file (Reference: Mujoco official site)
Note .xml is super easy to program.

### Compiling
### Visualization Node

---related_posts: false
  - "/blog/2025/Mujoco_notes"
### Root Node:
Compiler node: used for simulation computations.
* range of physical variables

Option node: simulation configuration. 
* Time steps (Default 0.002)
* Gravity (Exogenous forces)
* Air drag/ wind environment. Air density and viscousity. (Mujoco is mainly used for multi-body kinetic simulation)
* Integrator. Euler is quick, RK4(Runge-Kutta) is accurate.
* solver 

The difference between integrator and solver:
* The integrator is responsible for numerical time integration of the system's equations of motion.It uses the current state and computed accelerations/velocities to determine the state at the next time step.
* The solver is a lower-level component used by the integrator (primarily the semi-implicit Euler variants) to resolve forces and impulses arising from constraints.

Visual node. Lighting conditions and Rendering options.
Visual map affects the Mouse interactions. Don't need to change in most scenarios.

## Resources Allocation:

asset: construct by vertices/ .obj/ .stl.
* texture. can load .png as texture.
* Only "skybox" texture can be directly loaded. All other textures should be instead called in materials.
* <hfield>: Terrain features. Usually, it's loaded from files.



Difference between URDF and MJCF:
