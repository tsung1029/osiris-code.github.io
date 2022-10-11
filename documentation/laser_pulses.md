---
layout: single
title: Laser pulses
permalink: /documentation/laser_pulses
usemathjax: true
toc: true
toc_label: Laser pulses

sidebar:
  nav: "docs"
---

The OSIRIS code offers many possibilities for injecting EM radiation into the simulation. The main options available are:

* Initializing a laser pulse inside the simulation box
* Continuously injecting an EM wave from one of the simulation boundaries

Please check the `zpulse` and `antenna` sections of the reference guide for all the available options.

## Gaussian beam

The simplest way to add a laser pulse to your simulation setup is to initialize it completely at the beginning of the simulation. This can be done by adding a `zpulse` section at the end of the input file specifying the laser pulse spatial and temporal parameters. Please note that there is no limit on the number of laser pulses that can be added.

### Initializing the complete laser pulse at once

The following is an example of a laser pulse propagating along x in a 2D simulation. It uses a Gaussian profile with a normalized intensity of $a_0 = 3$, a waist of $w_0 = 2.82$ (in simulation units), and a duration $\Delta t = 2.82$ using a $\sin^2$ temporal profile. The laser frequency is $\omega_0 = 10$, normalized to the simulation frequency, and the laser is polarized out of the plane. The laser front edge is at position $x = 19.5$ (in simulation units) and the position of the focal plane is at $x = 0$.

```text
zpulse {
  a0             = 3.,
  omega0         = 10.0, 
  propagation    = "forward",

  pol            = 90,

  per_type       = "gaussian",
  per_w0         = 2.82,
  per_focus      = 0.0,
  
  lon_type       = "sin2", 
  lon_rise       = 2.82,
  lon_fall       = 2.82,
  lon_flat       = 0.0,

  lon_start      = 19.5,
}
```

### Injecting from a wall

If instead, you want to gradually inject the laser pulse from a simulation wall you can use a `zpulse_wall` section to accomplish this. The laser pulse parameters are the same as those defined in the previous section, but the laser pulse duration is now set using the `tenv_*` parameters, and the code will be starting to inject the laser pulse at $t = 0$ (`launch_time` parameter.)

```text
zpulse_wall {
  a0             = 3.,
  omega0         = 10.0,
  propagation    = "forward",

  pol            = 90,

  per_type       = "gaussian",
  per_w0         = 2.82,
  per_focus      = 0.0,

  tenv_type       = "sin2",
  tenv_rise       = 2.82,
  tenv_fall       = 2.82,
  tenv_flat       = 0.0,

  launch_time     = 0.0,
}
```

For more details please check the `zpulse` sections of the reference guide.