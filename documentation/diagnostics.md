---
layout: single
title: Simulation diagnostics
permalink: /documentation/diagnostics
usemathjax: true
toc: true
toc_label: Simulation diagnostics

sidebar:
  nav: "docs"
---

The OSIRIS code includes a comprehensive set of simulation diagnostics, covering both EM fields and simulation particles, as well as other simulation quantities such as the electric current density. These go well beyond simple data dumps, and can greatly simplify extracting meaningful physics from simulation results, while reducing storage space and processing time.

Output files are saved either using the HDF5 format or the ZDF format. In both cases, each file includes metadata describing the quantity, units, and time information for the particular file, allowing for easy post-processing of simulation data. All diagnostic files are saved in an `MS` directory below the OSIRIS run directory.

## Global diagnostic frequency

Most OSIRIS diagnostics are done at regular iteration intervals. These intervals are defined using two parameters: a global `ndump` parameter, set in the `time_step` section, and specific `ndump_fac` (or similar) parameters in each diagnostics section. The actual interval between each diagnostic output will be the product of these two values.

Setting the global `ndump` parameter to 1 means that each `ndump_fac` parameter refers to actual iteration numbers; setting it to 0 disables diagnostic output. The following example sets the global diagnostic frequency to 50:

```text
time_step {
  dt     =  0.014,
  ndump  =  50,
}
```

## EM field diagnostics

Diagnostics for EM fields are defined in an optional `diag_emf` section of the input file. The main diagnostic options available are controlled using the `reports` and `ndump_fac` parameters.

The following example will save the $E_x$ and $E_z$ electric field components at every global `ndump` iterations.

```text
diag_emf {
  ndump_fac = 1,
  reports  = "e1", "e3", 
}
```

There are many other options available, including spatial and temporal averaging, lineouts, and energy diagnostics. Please check the `diag_emf` section of the reference guide for details.

## Particle diagnostics

Most particle diagnostics are set individually for each particle species using an optional `diag_species` section. The main types of diagnostics available are charge density (`reports` and `ndump_fac` parameters), individual particle data (`ndump_fac_raw` parameter), and phase-space density (`phasespaces` and `ndump_fac_pha` parameters).

The following example, from a 1D simulation, saves the charge density, all particles with forward momentum above 1.0 c, and a $p_1 - x_1$ phase-space with $ 256 \times 256 $ cells, and $p_1$ values in the $-2,+2$ range. The default range for $x_1$ matches the simulation domain.

```text
diag_species 
{
  ndump_fac = 1,
  reports = "charge",

  ndump_fac_raw = 1,
  raw_math_expr = "p1 > 1.0",
  
  ndump_fac_pha = 1,
  ps_pmin(1) = -2,
  ps_pmax(1) = +2,
  ps_np(1) = 256,
  ps_nx(1) = 256,
  phasespaces = "p1x1",
}
```

There are many other options available, including particle tracking and energy diagnostics. Please check the `diag_species` section of the reference guide for details.

## Current density diagnostics

Diagnostics for current density are defined in an optional `diag_current` section of the input file. The diagnostic options available are controlled using the `reports` and `ndump_fac` parameters.

The following example will save all current density components, $j_x$, $j_y$ and $j_z$, at every global `ndump` iterations.

```text
diag_current {
  ndump_fac = 1,
  reports  = "j1", "j2", "j3",
  
}
```
