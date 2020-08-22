# GraphTune
------------

It is an efficient dependency-aware substrate for high throughput of the distributed concurrent graph processing jobs, and a lightweight runtime system which runs in existing graph processing systems.

# Integrated with existing graph processing systems
------------

To efficiently load the shared graph data, Access() replaces the original data load operation of existing system. Sync() replaces its original grab operation for synchronous processing of the chunks. Regularize() replaces its communication operation to achieve the optimized communication. Note that, to obtain the active chunks before each iteration, GetActiveChunks() is also provided, because this operation is used by some systems to skip the processing of inactive chunks.

# Dependencies
------------

D-Galois-T builds, runs, and has been tested on GNU/Linux. Even though D-Galois-T may build on systems similar to Linux, we have not tested correctness or performance, so please
beware. 
D-Galois-T depends on the following software:

- A modern C++ compiler compliant with the C++-17 standard (gcc >= 7, Intel >= 19.0.1, clang >= 7.0)
- CMake (>= 3.13)
- Boost library (>= 1.58.0, we recommend building/installing the full library)
- libllvm (>= 7.0 with RTTI support)
- libfmt (>= 4.0)
- Doxygen (>= 1.8.5) for compiling documentation as webpages or latex files 
- PAPI (>= 5.2.0.0 ) for profiling sections of code
- Vtune (>= 2017 ) for profiling sections of code
- MPICH2 (>= 3.2) if you are interested in building and running distributed system
  applications in Galois
- Eigen (3.3.1 works for us) for some matrix-completion app variants

#Compiling
------------

To build D-Galois-T, certain CMake flags must be specified:

`cmake ${GALOIS_ROOT} -DGALOIS_ENABLE_DIST=1`

Once CMake is successfully completed, you can build the `concurrent_jobs` with the following command:

`make -j`


Graph Format
------------

We store graphs in a binary format called *D-Galois-T graph file*  (`.gr` file extension). Other formats such as edge-list or Matrix-Market can be
converted to `.gr` format with `graph-convert` tool provided in D-Galois-T. 
You can build graph-convert as follows:

```Shell
cd $BUILD_DIR
make graph-convert
./tools/graph-convert/graph-convert --help
```

# Running Applications
------------
We concurrently submmit muntiple CGP jobs to D-Galois-T follwoing the real trace through the concurrent_jobs application. To concurrently run this application, just need to give the follwing parameters, and the command can be specified with the following:

`GALOIS_DO_NOT_BIND_THREADS=1 mpirun -n=<# of processes> -hosts=<machines to run on> ./concurrent_jobs <input graph> <number of submissions> <trace>`

There are a few common command line flags that are worth noting. More details can be found by running the applications with the -help flag.

`-partition=<partitioning policy>`

Specifies the partitioning that you would like to use when splitting the graph among multiple hosts.
