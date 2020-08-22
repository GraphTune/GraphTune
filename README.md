# GraphTune
------------

It is an efficient dependency-aware substrate for high throughput of  the distributed concurrent graph processing jobs, and a lightweight runtime system which runs in existing graph processing sytems

# Integrated with existing graph processing systems
------------

To efficiently load the shared graph data, Access() replaces the original data load operation of existing system. Sync() replaces its original grab operation for synchronous processing of the chunks. Regularize() replaces its communication operation to achieve the optimized communication. Note that, to obtain the active chunks before each iteration, GetActiveChunks() is also provided, because this operation is used by some systems to skip the processing of inactive chunks.

# Dependencies
------------

Galois builds, runs, and has been tested on GNU/Linux. Even though
Galois may build on systems similar to Linux, we have not tested correctness or performance, so please
beware. 

At the minimum, Galois depends on the following software:

- A modern C++ compiler compliant with the C++-17 standard (gcc >= 7, Intel >= 19.0.1, clang >= 7.0)
- CMake (>= 3.13)
- Boost library (>= 1.58.0, we recommend building/installing the full library)
- libllvm (>= 7.0 with RTTI support)
- libfmt (>= 4.0)

Here are the dependencies for the optional features: 

- Linux HUGE_PAGES support (please see [www.kernel.org/doc/Documentation/vm/hugetlbpage.txt](https://www.kernel.org/doc/Documentation/vm/hugetlbpage.txt)). Performance will most likely degrade without HUGE_PAGES
  enabled. Galois uses 2MB huge page size and relies on the kernel configuration to set aside a large amount of 2MB pages. For example, our performance testing machine (4x14 cores, 192GB RAM) is configured to support up to 65536 2MB pages:
  ```Shell
  cat /proc/meminfo | fgrep Huge
  AnonHugePages:    104448 kB
  HugePages_Total:   65536
  HugePages_Free:    65536
  HugePages_Rsvd:        0
  HugePages_Surp:        0
  Hugepagesize:       2048 kB
  
  - libnuma support. Performance may degrade without it. Please install
  libnuma-dev on Debian like systems, and numactl-dev on Red Hat like systems. 
- Doxygen (>= 1.8.5) for compiling documentation as webpages or latex files 
- PAPI (>= 5.2.0.0 ) for profiling sections of code
- Vtune (>= 2017 ) for profiling sections of code
- MPICH2 (>= 3.2) if you are interested in building and running distributed system
  applications in Galois
- CUDA (>= 8.0) if you want to build GPU or distributed heterogeneous applications
- Eigen (3.3.1 works for us) for some matrix-completion app variants

