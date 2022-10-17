---
layout: single
title: Quickstart
permalink: /quickstart/
toc: true
toc_label: Quickstart

sidebar:
  nav: "short"
---

## 0. Prerequisites

To get OSIRIS running on your computer you will need a standard development toolchain including `gcc`, `make`, and a recent Fortran 2003 compiler (`gfortran` works great). You will also need to have an MPI library installed (e.g. [Open MPI](https://www.open-mpi.org)) and the [HDF5](https://www.hdfgroup.org/solutions/hdf5/) library.

For more details on the prerequisites for compiling and running OSIRIS check the [Prerequisites](/documentation/prerequisites/) section of the documentation.

## 1. Download the source code

You can download the latest version of OSIRIS _here_(coming soon). Alternatively, you can clone the OSIRIS repository using:

```bash
$ git clone https://github.com/osiris-code/osiris.git
```

For more details on how to download the code check the [Download OSIRIS](/download/) section of the documentation.

## 2. Configure and compile the code

In the `config` folder of the OSIRIS source tree, look for a file with a configuration similar to the system you are using (e.g. `osiris_sys.linux.gnu` for a Linux system using gnu compilers). Edit that file so that it matches your particular configuration (e.g. path to the MPI installation). You can also set more advanced compilation options (e.g. use single or double precision).

Once you have created the configuration file you can now configure compilation using the `configure` command at the top of the OSIRIS source tree. For example, if you are using the `osiris_sys.linux.gnu` configuration file and you want the 2D version of the code you would do:

```bash
$ ./configure -d 2 -s linux.gnu
OSIRIS Configuration:
--------------------
           System : 'linux.gnu'
 Compilation Type : 'production'
       Dimensions : '2'
```

For more details on the OSIRIS configuration files, check the [Compiling OSIRIS](/documentation/compile) section of the documentation. Once you have finished configuring the compilation you can go ahead and create an OSIRIS binary simply by calling `make` in the top-level folder of the OSIRIS source tree:

```bash
$ make
...
[F03] os-main.f03
[LINK] osiris.e
Copying binary to ../bin/osiris-4.4.4-225-ga23a242-2D.e
Creating symbolic link
Done!
```

This will create an OSIRIS binary in the `bin` directory.

## 3. Run a test problem

Once you have compiled the OSIRIS binary navigate to the `test` folder in the OSIRIS source tree. Assuming you compiled the 2D version of OSIRIS and that your MPI distribution supports the `mpiexec` command, you can launch the "weibel.2d" input file by doing:

```bash
$ mpiexec -np 4 ../bin/osiris-2D.e weibel.2d
Osiris running on host ...
...
n =          0, t = .000
n =          1, t = .070
...
n =        214, t = 14.980
n =        215, t = 15.050

  Simulation completed.


 Osiris run completed normally
```

For more details on how to call the OSIRIS binaries please check the [Running OSIRIS](/documentation/run) section of the input file.

And that's it, you have successfully completed an OSIRIS simulation! By default, the output files will be saved in a directory called `MS` inside the directory where OSIRIS was called. Please check the documentation for more details.
