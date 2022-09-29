---
layout: single
title: Running OSIRIS
permalink: /documentation/run

toc: true
toc_label: Running OSIRIS

sidebar:
  nav: "docs"
---

## Launching a simulation

OSIRIS simulations are set up using an input file (also referred to as input deck throughout the documentation) with a specific format described [here](missing). Assuming you have an OSIRIS binary named `osiris` and an input file named `input_file` in the currend directory, you can launch a (serial) OSIRIS simulation by doing:

```bash
$ ./osiris input_file
```

## OSIRIS Command Line Options

The OSIRIS binaries will accept the following command line options:

```bash
$ osiris [-t] [-r] [-w work_dir] [input_file]
```

### Arguments

* `input_file` (optional) - Input file to use. If not specified defaults to `os-stdin`. _Note_: the input file does not need to be in the same directory where the code is being executed. Also, only the root node needs to access the input file, so it can be placed on a non-shared file system.

### Options

* `-t` Test only. If set OSIRIS will only test the validity of the specified input file and all other options are ignored.
* `-r` Restart. If set it will force OSIRIS to restart from checkpoint data.
* `-w work_dir` Work directory. If set OSIRIS will change to this directory before starting the simulation.

## Environment variables

Alternatively you can use the following environment variables to control
Osiris behavior:

* `OSIRIS_TEST` - Setting this to any value is the same as setting the `-t` option above.
* `OSIRIS_RESTART` - Setting this to any value is the same as setting the `-r` option above.
* `OSIRIS_WDIR` - Set this to work_dir as in the `-w` option above.

The environment variables override the command line options.

## Parallel (MPI/OpenMP) simulations

Launching a parallel (MPI) OSIRIS simulation depends on the specific MPI library and/or queing system used. For example, if your system supports `mpiexec` then OSIRIS can be launched as any other MPI job, e.g.:

```bash
$ mpiexec -np nnodes ./osiris input_file
```

Where `nnodes` is the number of MPI nodes to be used, which must match the values specified in the `node_conf` section of the input file. If the binary was compiled with OpenMP support and its use was configured in the same `node_conf` section of the input file, then it will be used automatically, and no other command line options need to be specified.

For larger HPC systems that use queueing/batch systems you should check their documentation on how to launch MPI or MPI/OpenMP hybrid parallel jobs. Again, OSIRIS can be launched like any other MPI or MPI/OpenMP job.