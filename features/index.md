---
layout: single
title: Code Features
permalink: /features/
toc: true
toc_label: Features
sidebar:
  nav: "short"
---

## State-of-the-art PIC code

* Fully Relativistic Electro-Magnetic Particle-In-Cell code
* High-order particle interpolation (up to 4th order)
* Advanced filtering schemes for currents and EM fields
* Massively parallel using hybrid MPI / OpenMP parallelization
* Demonstrated parallel scalability to over 2 million processes
* Explicit CPU vectorization using SSE, AVX2, AVX512 (KNL), QPX, Neon
* Explicit GPU support using CUDA

## Extended PIC algorithm

* Field / impact ionization
* Classical radiation damping
* Particle merging
* QED package (pair creation, radiation reaction, non-linear Compton scattering)
* Customizable EM field solver
* Alternative particle pushers

## Multiple geometries and reduced models

* 1D, 2D, 3D cartesian and 2D cylindrical
* Quasi-3D with azimuthal mode expansion
* Ponderomotive guiding center (PGC) in 2D (x-y), 2D (r-z) and 3D
* Modified Spherical coordinates
* 2D shearing co-rotating framework

## Advanced initialization routines

* Laser Pulses with OAM / sliding focus / speckle
* Particle beams with Twiss parameters and self-consistent initial fields

## Advanced diagnostics

* Particle tracking
* Short-wavelength radiation
