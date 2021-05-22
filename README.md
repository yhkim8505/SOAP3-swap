# SOAP3-swap

SOAP3-swap is a GPU-based software considering swap of adjacent bases based on SOAP3-dp for aligning short reads to a reference sequence.

# <a name="Section1"></a>1. Hardware and Platform

To run SOAP3-swap, you need a linux workstation equipped with

  * a multi-core CPU with at least 16GB main memory (default quad-core);
  * a CUDA-enabled GPU with compute capability 9.0 and at least 3GB memory.

SOAP3-swap has been tested with the following GPU: NVIDIA Geforce 2080 Ti (11GB memory).

SOAP3-swap was developed under the 64-bit linux platform and the CUDA Driver version 9.0. (users are advised to remove the Virtual Memory limit using the command "ulimit -v unlimited").
