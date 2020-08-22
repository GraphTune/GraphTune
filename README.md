# GraphTune
------------
GraphTune is an effective and lightweight runtime system, which can be integrated into existing distributed graph processing systems. The user only needs to make a minor extension to existing system via simply inserting a few lightweight APIs provided by GraphTune, and then the system can obtain much higher throughput brought by GraphTune. No modification is needed for graph applications.


# Integrated with existing graph processing systems
------------

To efficiently load the shared graph data, Access() replaces the original data load operation of existing system. Sync() replaces its original grab operation for synchronous processing of the chunks. Regularize() replaces its communication operation to achieve the optimized communication. Note that, to obtain the active chunks before each iteration, GetActiveChunks() is also provided, because this operation is used by some systems to skip the processing of inactive chunks. In the following part, we take D-Galois as an example to show how to execute the CGP jobs on the D-Galois integrated with GraphTune.

# Dependencies
------------

D-Galois-T (i.e., the version of D-Galois integrated with GraphTune) depends on the following software:

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

# Compiling
------------

To build D-Galois-T, certain CMake flags must be specified:

`cmake ${GALOIS_ROOT} -DGALOIS_ENABLE_DIST=1`

Once CMake is successfully completed, you can build the `concurrent_jobs` and `Preprocessing` with the following command:

`make -j`



# Preprocessing
------------

We first store graphs in a binary format called *D-Galois-T graph file*  (`.gr` file extension). Other formats such as edge-list can be
converted to `.gr` format with the tool `graph-convert` provided in D-Galois-T. 
You can build graph-convert as follows:

```Shell
cd $BUILD_DIR
make graph-convert
./tools/graph-convert/graph-convert --help
```
Then, the graph is preprocessed for D-Galois-T as follows:

`GALOIS_DO_NOT_BIND_THREADS=1 mpirun -n=<# of processes> -hosts=<machines to run on> ./Preprocessing <input graph> -partition=<partitioning policy>`


# Running Applications
------------
We can submmit the CGP jobs to D-Galois-T according to the real trace as follows:

`GALOIS_DO_NOT_BIND_THREADS=1 mpirun -n=<# of processes> -hosts=<machines to run on> ./concurrent_jobs <input graph> <number of submissions> <trace>`
