---
layout: single
title: Input File Format
permalink: /documentation/input_format
usemathjax: true
toc: true
toc_label: Input File Format

sidebar:
  nav: "docs"
---

OSIRIS simulations are initialized using plain text input files using the syntax and structure defined below. For instructions on how to launch a simulation using a particular input file please check the [Running OSIRIS](/documentation/run) section of the documentation.

## General syntax

OSIRIS input files are a sequence of sections specifying the several parameters for the simulation to be performed. The format for these sections is the following:

```text
section-name {
 data
} 
```

The section data is specified using standard Fortran namelist conventions i.e. "var = value," (don't forget the trailing comma). Here are two examples of sections on OSIRIS input decks:

```text
! grid parameters 
grid {
    nx_p(1:2) = 4096, 512,      ! use a 4096 x 512 cell grid
    coordinates = "cartesian",  ! with cartesian coordinates
}
   
! empty section, use default parameters
current 
{}
```

When any variable of the data section is not specified it will default to a pre-defined value. The opening and closing brackets can be placed in the same line as the section name.

### Comments

Any text following an exclamation mark "!" is ignored until the end of the line is reached. Blank lines are also ignored.

## Input file structure

An OSIRIS input file is made out of all of the following sections, placed in the order defined below. The order of the sections __CAN NOT__ be changed and doing so will result in a run-time error.

### General Simulation Parameters

* `node_conf{}` - node configuration, and periodic boundary settings
* `grid{}` - grid and coordinate system
* `time_step{}` - time step and dump frequency
* `space{}` - spacial dimensions and moving window
* `time{}` - time limits

### Electro-Magnetic Fields

* `emf_bound{}` - electro-magnetic field boundaries

### Particles

* `particles{}` - number of species in the simulation.

The following items are repeated for every species

* `species{}` - information on the particle species
* `spe_bound{}` - boundary conditions for the species

### Optional sections

Additionally, users can add optional sections describing diagnostics, particle initialization options, laser pulses, and other simulation parameters. Please check the reference guide for a complete list of all OSIRIS input file sections.

## Minimal input file

The following is an example of a minimal OSIRIS input file for a 2D simulation using periodic boundaries. It will run for 1 timestep with no particles or diagnostic output:

```text
node_conf {
    ! defaults to serial run
    
    ! set periodic boundaries
    if_periodic(1:2) = .true., .true.,
}

grid { 
    ! grid size must be specified
    nx_p(1:2) = 256, 256,   
}

time_step {
    ! time-step must be specified
    dt = 0.07,
}

space {
    ! Spatial dimensions must be specified
    xmin(1:2) = 0, 0,
    xmax(1:2) = 25.6, 25.6,
}

time {
    ! defaults to tmax = 0.0
}

emf_bound {
    ! when not using periodic boundaries, user must specify
    ! EM field boundaries in this section
}

particles {
    ! defaults to no particle species
}
```

## 2D simulation example

The following is an annotated example of an OSIRIS input file setting up a simulation of an electron plasma cloud with a positron plasma cloud. This is a 2D simulation with the plasma clouds initially moving in the direction perpendicular to the simulation plane. This scenario is susceptible to Weibel instability and will lead to the development of a magnetic field structure. The simulation has a minimal set of diagnostics focusing only on the magnetic field and the electric current in the direction perpendicular to the simulation plane. The simulation is small enough to be run on a modern laptop; it is configured to use a total of 4 parallel nodes with 2 threads per node which should be adequate for a 4 core system (with hyperthreading).

```text
node_conf {
  node_number(1:2) =  2, 2,           ! Use 2 x 2 parallel nodes
  if_periodic(1:2) = .true., .true.,  ! Periodic boundaries in both x and y
  n_threads = 2,                      ! Use 2 threads per node
}

grid { 
  nx_p(1:2) = 256, 256,               ! Use a 256 x 256 grid
}

time_step {
  dt     =   0.07,                    ! \Delta t = 0.07
  ndump  =  5,                        ! Global diagnostic frequency set to 5
}

space {
  xmin(1:2) =   -12.8 , -12.8,        ! Simulation box will range from -12.8 to 12.8
  xmax(1:2) =    12.8 ,  12.8,        ! in both dimensions
}

time {
  tmax  = 30.0,                       ! Run simulation up to t = 30
}

emf_bound {
  ! No need to set because we are using periodic boundaries
}

diag_emf {
  ndump_fac = 10,                     ! diagnostics at every 10 * 5 = 50 timesteps
  reports = "b1", "b2", "b3",         ! save all magnetic field components
}

particles {
  num_species = 2,                    ! Use 2 particle species
}


species {
  name = "electrons" ,                ! Name used for diagnostics
  num_par_x(1:2) = 8, 8,              ! 8 x 8 particles per cell
  rqm=-1.0,                           ! m/q = -1 (electrons)
}

udist {
  uth(1:3) =  0.1 , 0.1 , 0.1 ,       ! Initial thermal velocity
  ufl(1:3) =  0.0 , 0.0 , 0.6 ,       ! Initial fluid velocity (0.6 c along z)
}

spe_bound {
  ! No need to set because we are using periodic boundaries
}

species
{
  name = "positrons" ,                ! Name used for diagnostics
  num_par_x(1:2) = 8, 8,              ! 8 x 8 particles per cell
  rqm=+1.0,                           ! m/q = +1 (positrons)
}

udist
{
  uth(1:3) =  0.1 , 0.1 , 0.1 ,       ! Initial thermal velocity
  ufl(1:3) =  0.0 , 0.0 , -0.6 ,      ! Initial fluid velocity (0.6 c along -z)
}

spe_bound {
  ! No need to set because we are using periodic boundaries
}

current{}

diag_current {
  ndump_fac = 10,                       ! diagnostics at every 10 * 5 = 50 timesteps
  reports = "j3",                       ! save z component of current density
}
```
