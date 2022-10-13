---
layout: single
title: Particles
permalink: /documentation/particles
usemathjax: true
toc: true
toc_label: Particles

sidebar:
  nav: "docs"
---

Simulation particles are organized in "species", meaning sets of charged particles that share the same charge over mass ratio. You can control the number of species in the simulation using the `particles` section, and the parameters for each species using `species` sections. Users may choose to use as many species as they like (including none).

For example, to set up a simulation using three particles species you would choose in your input file:

```text
particles {
  num_species = 3,
}
```

## Species parameters

For each of the species selected in the `particles` section, the user must add a `species` section defining the main parameters for the particle species, namely:

* The number of particles per cell used when creating the species (`num_par_x` parameter). This parameter should be set for all simulation directions.
* The mass over charge ratio for the particle species (`rqm` parameter) in simulation units (for electrons this would be -1)

The user may also define a name for the species that will be used for diagnostic output.

Following each of these `species` sections, the user may optionally define the initial thermal/fluid distribution (`udist` section), and initial density profile (`profile` section). Users must also include a `spe_bound` section defining the boundary conditions for the species.

For example, to set up a simulation using a single reduced mass ratio ion species ($m/q = 100$), with initial fluid velocity 0.2 c along the y direction and temperature $10^-4$ in all directions, filling only the box for positions $x > 0$, you would choose in your input file:

```text
particles {
  num_species = 1,
}

species
{
  name = "ions-up" ,
  num_par_x(1:2) = 2, 2,
  rqm=100.0,
}

udist
{
  ufl(1:3) =  0.0 , 0.2 , 0.0 ,
  uth(1:3) =  0.0001 , 0.0001 , 0.0001 ,
}

profile {
  profile_type = "math func",
  math_func_expr = 'x1 > 0',
}
```

For more details please check the `particles` and `species` sections of the reference guide.
