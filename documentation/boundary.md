---
layout: single
title: Boundary conditions
permalink: /documentation/boundary
usemathjax: true
toc: true
toc_label: Boundary conditions

sidebar:
  nav: "docs"
---

When preparing an OSIRIS simulation, users must set the boundary conditions to be used for both EM fields and simulation particles. No default boundary conditions are assumed, and users must set the boundary condition types for all simulation boundaries.

Boundary types are usually defined independently for each simulation object (EM fields, particle species) and it is up to the user to ensure consistency between the multiple choices. Periodic boundaries are a special case: these are defined globally and override any other boundary type choices.

Please note that when using a moving simulation window, all boundary conditions for EM fields and particles along x are automatically set accordingly.

## Periodic boundaries

Choosing global periodic boundaries will override any other boundary condition choices specified in the code, and force all simulation objects to use this type of boundary. To choose global periodic boundaries you must set the `if_periodic` parameter in the `node_conf` section of the input file.

For example, to set periodic boundaries only along z in a 3D simulation you would choose:

```text
node_conf {
  node_number(1:3) =  16, 16, 8,
  if_periodic(1:3) = .false., .false., .true.,
}
```

Please check the `node_conf` section of the reference guide for more details.

## EM field boundaries

EM field boundary options are set using the `type` parameter of the `emf_bound` section of the input file. Users must define the type for all simulation boundaries.

Users may define different boundary types for the lower/upper boundary of each simulation dimension. The following 2D example sets periodic boundaries along y, perfect electric conductor boundary for the lower (left) x boundary, and open boundary for the upper (right) x boundary:

```text
node_conf {
  node_number(1:2) =  16, 16,
  if_periodic(1:2) = .false., .true.,
}

(...)

emf_bound {
    type(1,1) = "pec",
    type(2,1) = "open",
}
```

Please check the `emf_bound` section of the reference guide for more details.

## Particle boundaries

Boundary conditions for simulation particles are set per species using a `spe_bound` section. Just like for the EM fields, users may define different boundary types for the lower/upper boundary of each simulation dimension. The following 2D example sets periodic boundaries along y, reflecting boundary for the lower (left) x boundary, and open boundary for the upper (right) x boundary:

```text
node_conf {
  node_number(1:2) =  16, 16,
  if_periodic(1:2) = .false., .true.,
}

(...)

species {
  (...)
}

(...)

spe_bound {
    type(1,1) = "reflecting",
    type(2,1) = "open",
}
```

Please check the `spe_bound` section of the reference guide for more details.