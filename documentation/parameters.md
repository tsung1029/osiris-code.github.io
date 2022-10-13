---
layout: single
title: Simulation parameters
permalink: /documentation/parameters
usemathjax: true
toc: true
toc_label: Simulation parameters

sidebar:
  nav: "docs"
---

In a particle-in-cell simulation (PIC) plasmas are modeled using a set of macro-particles, with each macro-particle representing multiple plasma particles, that interact through EM fields discretized on a grid. In OSIRIS PIC simulations users must define the initial conditions for these quantities using a simple text input file.

A minimal OSIRIS input file must specify the:

* __Grid parameters__ - Number of points and spatial dimensions of the simulation domain
* __Time parameters__ - Time-step and total simulation time
* __Particle parameters__ - Number of particle species (and individual species parameters)
* __Parallelization parameters__ - Parallel decomposition to use and/or number of threads per process

For details on the overall syntax and structure of an OSIRIS input file, and in particular, the order in which these parameters should be set, please check the `input file` section of the reference guide. All values are specified using normalized units, please check the [units and normalization](/documentation/units) section for details.

## Grid

To define the simulation grid you must set both the number of points on your grid and the spatial dimensions (in simulation units) of your simulation domain. These are set using the `grid` and `space` sections of the input file.

For example, to specify a 2D simulation grid with 256 cells in each direction and spatial coordinates ranging from -12.8 to +12.8 in both directions you would choose in your input file:

```text
grid {  
    nx_p(1:2) = 256, 256,
}

space {
  xmin(1:2) =   -12.8 , -12.8,  ! in normalized units
  xmax(1:2) =    12.8 ,  12.8,
}
```

### Moving simulation window

OSIRIS also allows for a moving simulation window that moves along $x$ at the speed of light (Note that the simulation is performed in the laboratory frame). This can be set in the `space` section of the input file using the `if_move` parameter:

```text
space 
{
  xmin(1:2)  =  0.0, -25.6,
  xmax(1:2)  = 20.0,  25.6,
  if_move(1) = .true., ! use a moving window
}
```

For more details please check the `grid` and `space` sections of the reference guide.

## Time

Users must also define both the time step to be used for the simulation and the total simulation time that should be used. These are set in the `time_step` and `time` sections of the input file.

For example, to set a time-step $\Delta_t = 0.07$ and to run the simulation up to $t_{max} = 70.0$ (both in simulation units) you would choose in your input file:

```text
time_step {
  dt     =   0.07,  ! in normalized units
}

time {
  tmax  = 70.0,     ! in normalized units
}
```

For more details please check the `time_step` and `time` sections of the reference guide.

## Parallelization

PIC codes are computationally expensive, and even medium-sized simulations require parallel computer systems to be performed. OSIRIS was implemented from scratch to be used in parallel computers, using both MPI (for distributed memory systems) and OpenMP (for shared memory systems).

All parallelization is defined in the `node_conf` section of the input file.

### Distributed memory parallelism (MPI)

The main parallelization used in OSIRIS is done through MPI for use in distributed memory systems. OSIRIS uses a spatial domain decomposition, meaning that each computing node will be assigned a (spatial) section of the global simulation volume. It is up to the user to define how this parallelization is to be done, namely by specifying how to divide the computational domain. Note that this works on both distributed and shared memory systems; in the latter case, you will be running multiple OSIRIS processes in the same shared memory node.

For example, to parallelize a 2D simulation using 16 k processes (128 parallel nodes in each direction), you would choose in your input file:

```text
node_conf {
    node_number(1:2) =  128, 128,
}
```

For more details please check the `node_conf` section of the reference guide.

### Shared memory parallelism (OpenMP)

You can also parallelize your simulation inside shared memory nodes using OpenMP (this requires compiling the code with OpenMP support). For this parallelization, OSIRIS will divide the particles inside each node evenly across available processors. This ensures a perfect load balance between those processors, at the expense of a slightly more complex algorithm.

For example, if you want to use 8 threads per MPI process in a 3D simulation using 4 k ($16^3$ parallel nodes in each direction), you would choose in your input file:

```text
node_conf {
    node_number(1:3) =  16, 16, 16,
    n_threads = 8,
}
```

For more details please check the `node_conf` section of the reference guide.
