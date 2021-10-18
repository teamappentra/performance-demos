# Parallelware Analyzer performance demos

This repository serves to showcase the performance gains when parallelizing code with the help of Parallelware Analyzer.

Different code examples have been parallelized using CPU multi-threading and GPU offloading with Parellelware Analyzer. You will need either `gcc` or `clang` to build OpenMP multi-threading versions and `nvc` for OpenACC GPU offloading versions. Additionally, common tools such as `unzip` or `sed` are needed to run and benchmark the examples.

- Run the `benchmark-omp-multi.sh` script to benchmark the OpenMP multi-threaded versions.
- Run the `benchmark-acc-gpu.sh` script to benchmark the OpenACC GPU offloading versions.
- Invoke `make parallelize` to automatically generate the parallel versions using Parallelware Analyzer. (Note that this is not required as they are already in the repository, this is provided so you can reproduce how they were created.)

## Structure

Each folder corresponds to a different example code:
- ATMUX: sparse matrix-vector multiplication.
- CANNY: image edge detection.
- COULOMB: computation of the electric potential created by a set of charges in an n x n 2D plane.
- HACCmk: short force evaluation kernel of the HACC application.
- MATMUL: matrix multiplication.
- NPB CG: Conjugate Gradient, irregular memory access and communication-
- PI: PI number approximation.

Inside each one of them, the `serial` subfolder corresponds to the original sequential (ie. non-parallelized) code. Additionally, one or more subfolders corresponding to parallel versions are provided. `pwa-omp-multi` corresponds to an OpenMP multi-threaded version generated by Parallelware Analyzer while `pwa-acc-offload` corresponds to an OpenACC GPU offloading version.

You can use `make` to build, run and even invoke Parallelware Analyzer to create the parallel versions of the code:
- `make` and `make run`: will build and run all the codes.
- `make build`: will just build the codes without running them.
- `make parallelize`: will invoke Parallelware Analyzer to create the same parallel versions provided. These will be generated in the top folder of each example having the same suffix as the corresponding subfolder containing the already parallelized provided version.

You can also use those same commands inside any of the subfolders to run only a specific example or version.

## Example output

The following output corresponds to an execution on a laptop running Ubuntu 21.04 and equipped with an AMD Ryzen 4800H CPU and 16 GBs of RAM:

```
$ ./benchmark-omp-multi.sh
...
Code            Serial  Multi   Speedup Time reduced
=============== ======= ======= ======= ============
ATMUX           0.30    0.12    2.48x   59.68%
CANNY           11.83   6.55    1.81x   44.63%
COULOMB         8.45    1.11    7.63x   86.90%
HACCmk          38.17   12.42   3.07x   67.45%
MATMUL          5.49    1.04    5.28x   81.07%
NPB_CG          34.99   10.21   3.43x   70.82%
PI              3.33    0.46    7.30x   86.30%
```
