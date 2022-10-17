---
layout: single
title: Prerequisites
permalink: /documentation/prerequisites
toc: true
toc_label: Prerequisites

sidebar:
  nav: "docs"
---

To get OSIRIS running on any system you will need a standard development toolchain including `gcc, `make`, and a recent Fortran 2003 compiler (`gfortran` works great). You will also need to have an MPI library installed (including Fortran bindings). See below for a detailed list of prerequisites for building OSIRIS on your system.

## Compilers/Tools

* A recent Fortran 2003 compiler. OpenMP support is optional (but recommended). The following compilers are known to work:
  * GNU `gfortran` (>= 6.0)
  * Intel `ifort` (>= 2016)
  * IBM XL Fortran `xlf2003` (>= 14.1)
  * LLVM `flang` (>=14.0)
* `gcc` - This is used as a preprocessor for the Fortran code
* A C compiler. Support for SIMD intrinsics is optional (but recommended). `gcc` works well, but a system-specific compiler may yield better performance.
* A GNU-compatible `make` (AIX `make` is not compatible, but `gmake` is usually available)

## Libraries

### Required

* __MPI__ - Any recent implementation should do: [MPICH](https://www.mpich.org), [Open MPI](https://www.open-mpi.org), and [MVAPICH](http://mvapich.cse.ohio-state.edu) are known to work.

_Note_: If the available MPI implementation doesn't support `MPI_IN_PLACE` operations these can be disabled in source/os-config.h. MPE can also be used for logging and profiling if available.

### Optional

* [HDF5](https://www.hdfgroup.org/solutions/hdf5/) - Versions >= 1.10 are recommended, although older versions are known to work. Support for parallel I/O is optional.
* [PAPI](https://icl.utk.edu/papi/) - PAPI can be used for detailed code profiling. Only the C interface is required.
* [SIONlib](https://apps.fz-juelich.de/jsc/sionlib/docu/index.html) - SIONlib can be used to reduce the number of files used for writing restart information.
