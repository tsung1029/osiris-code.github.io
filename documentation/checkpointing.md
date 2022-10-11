---
layout: single
title: Checkpointing
permalink: /documentation/checkpointing
usemathjax: true
toc: true
toc_label: Checkpointing

sidebar:
  nav: "docs"
---

OSIRIS simulations may write checkpointing information that allows for the simulation to be interrupted and resumed at a later time. To use this feature the user must:

1. Instruct the code to write checkpoint data
2. When relaunching the simulation, instruct the code to initialize the simulation from the checkpoint data

Please note that writing checkpoint data may be a time-consuming operation and that it uses a lot of disk space.

## Writing checkpoint files

The writing of checkpoint files is controlled using the `restart` section of the input file. The user may choose to write checkpoint data:

* At fixed iteration intervals
* At fixed wall clock time intervals
* At a given iteration value

Data will be saved in the `RE` directory below the OSIRIS run directory, with 1 file per parallel node. When writing new checkpoint data, older files may optionally be deleted. After checkpoint data has been saved, OSIRIS will create soft links to the latest files.

For example, if you want to write checkpoint data at every 2500 iterations you would write:

```text
restart {
  ndump_fac = 2500,
}
```

For more details on writing checkpoint files please check the `restart` section of the reference guide.

## Resuming a simulation from checkpoint files

To resume an OSIRIS simulation from checkpoint data you will need both the original input file and the checkpoint files in an `RE` directory below the OSIRIS run directory. Please note that while you can change some input parameters when restarting a simulation (e.g. diagnostic options), you cannot change parallelization parameters.

To instruct OSIRIS to resume from checkpoint data you can simply add the `-r` option to the command line parameters, e.g.:

```text
$ mpiexec -np 16 ../bin/osiris-2D.e -r lwfa.2d
...
**********************************************
* Restarting simulation from checkpoint data *
**********************************************
...
```

Alternatively in can use an environment variable or the `if_restart` parameter in the `restart` section to resume the simulation from checkpoint data. Please check the [Running OSIRIS](/documentation/run) and `restart` sections of the documentation/reference guide for more details.
