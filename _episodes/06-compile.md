---
title: "Compile code on the cluster"
teaching: 15
exercises: 15
questions:
- "How to compile code on the cluster"
objectives:
- "Compile code in several languages and using OpenMP and MPI"
keypoints:
- "Compile code in several languages and using OpenMP and MPI"
---

Compiling code is one of the tasks that HPC users have to do as part of their research. This small tutorial shows how to compile relatively simple pieces of code in C and Fortran

## MPI

MPI is a library for distrubuted parallel computing. Today is the library of choice for solving large numerical problems on HPC clusters.

This is a very minimal example of a code written using MPI

~~~
#include <mpi.h>
#include <stdio.h>

int main(int argc, char** argv) {
    // Initialize the MPI environment
    MPI_Init(NULL, NULL);

    // Get the number of processes
    int world_size;
    MPI_Comm_size(MPI_COMM_WORLD, &world_size);

    // Get the rank of the process
    int world_rank;
    MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);

    // Get the name of the processor
    char processor_name[MPI_MAX_PROCESSOR_NAME];
    int name_len;
    MPI_Get_processor_name(processor_name, &name_len);

    // Print off a hello world message
    printf("Hello world from processor %s, rank %d out of %d processors\n",
           processor_name, world_rank, world_size);

    // Finalize the MPI environment.
    MPI_Finalize();
}
~~~
{: .source}

To compile this code you need to load the libraries from a MPI implementation.
For example load the module for OpenMPI

~~~
$ module load lang/gcc/8.2.0 parallel/openmpi/3.1.4_gcc82
~~~
{: .language-bash}

To compile a code with MPI you need to use the wrappers to the compilers provided by the implementation. In this case the wrapper for C is called `mpicc`

~~~
$ mpicc example_mpi.c -o example_mpi
~~~
{: .language-bash}

To execute the code use:

~~~
$ mpirun -np 4 example_mpi
~~~
{: .language-bash}

In this case you are running using 4 cores.

## HDF5

HDF5 is a data model, library, and file format for storing and managing data. It supports an unlimited variety of datatypes, and is designed for flexible and efficient I/O and for high volume and complex data. HDF5 is portable and is extensible, allowing applications to evolve in their use of HDF5. The HDF5 Technology suite includes tools and applications for managing, manipulating, viewing, and analyzing data in the HDF5 format.

Learning how to code using HDF5 is beyond the scope, but we will use some examples to show how you can compile code that uses the library.

Lets start with the basic example that shows how to create a dataset.
The

~~~
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * Copyright by The HDF Group.                                               *
 * Copyright by the Board of Trustees of the University of Illinois.         *
 * All rights reserved.                                                      *
 *                                                                           *
 * This file is part of HDF5.  The full HDF5 copyright notice, including     *
 * terms governing use, modification, and redistribution, is contained in    *
 * the COPYING file, which can be found at the root of the source code       *
 * distribution tree, or in https://support.hdfgroup.org/ftp/HDF5/releases.  *
 * If you do not have access to either file, you may request a copy from     *
 * help@hdfgroup.org.                                                        *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

/*
 *  This example illustrates how to create a dataset that is a 4 x 6
 *  array.  It is used in the HDF5 Tutorial.
 */

#include "hdf5.h"
#define FILE "dset.h5"

int main() {

   hid_t       file_id, dataset_id, dataspace_id;  /* identifiers */
   hsize_t     dims[2];
   herr_t      status;

   /* Create a new file using default properties. */
   file_id = H5Fcreate(FILE, H5F_ACC_TRUNC, H5P_DEFAULT, H5P_DEFAULT);

   /* Create the data space for the dataset. */
   dims[0] = 4;
   dims[1] = 6;
   dataspace_id = H5Screate_simple(2, dims, NULL);

   /* Create the dataset. */
   dataset_id = H5Dcreate2(file_id, "/dset", H5T_STD_I32BE, dataspace_id,
                          H5P_DEFAULT, H5P_DEFAULT, H5P_DEFAULT);

   /* End access to the dataset and release resources used by it. */
   status = H5Dclose(dataset_id);

   /* Terminate access to the data space. */
   status = H5Sclose(dataspace_id);

   /* Close the file. */
   status = H5Fclose(file_id);
}
~~~
{: .language-c}

There are basically two ways to compile a code like this. For both we need to load the HDF5 library. Execute:

~~~
$ module load lang/gcc/8.2.0 libs/hdf5/1.10.5_gcc82
~~~
{: .language-bash}


Now, if you try to compile with `gcc` you will get:

~~~
$ gcc HDF5_create.c
~~~
{: .language-bash}
~~~
/gpfs/shared/software/lang/gcc/8.2.0/bin/../lib/gcc/x86_64-pc-linux-gnu/8.2.0/../../../../x86_64-pc-linux-gnu/bin/ld: /tmp/cccMM5bk.o: in function `main':
HDF5_create.c:(.text+0x18): undefined reference to `H5check_version'
/gpfs/shared/software/lang/gcc/8.2.0/bin/../lib/gcc/x86_64-pc-linux-gnu/8.2.0/../../../../x86_64-pc-linux-gnu/bin/ld: HDF5_create.c:(.text+0x1d): undefined reference to `H5open'
/gpfs/shared/software/lang/gcc/8.2.0/bin/../lib/gcc/x86_64-pc-linux-gnu/8.2.0/../../../../x86_64-pc-linux-gnu/bin/ld: HDF5_create.c:(.text+0x36): undefined reference to `H5Fcreate'
/gpfs/shared/software/lang/gcc/8.2.0/bin/../lib/gcc/x86_64-pc-linux-gnu/8.2.0/../../../../x86_64-pc-linux-gnu/bin/ld: HDF5_create.c:(.text+0x60): undefined reference to `H5Screate_simple'
/gpfs/shared/software/lang/gcc/8.2.0/bin/../lib/gcc/x86_64-pc-linux-gnu/8.2.0/../../../../x86_64-pc-linux-gnu/bin/ld: HDF5_create.c:(.text+0x69): undefined reference to `H5open'
/gpfs/shared/software/lang/gcc/8.2.0/bin/../lib/gcc/x86_64-pc-linux-gnu/8.2.0/../../../../x86_64-pc-linux-gnu/bin/ld: HDF5_create.c:(.text+0x70): undefined reference to `H5T_STD_I32BE_g'
/gpfs/shared/software/lang/gcc/8.2.0/bin/../lib/gcc/x86_64-pc-linux-gnu/8.2.0/../../../../x86_64-pc-linux-gnu/bin/ld: HDF5_create.c:(.text+0x97): undefined reference to `H5Dcreate2'
/gpfs/shared/software/lang/gcc/8.2.0/bin/../lib/gcc/x86_64-pc-linux-gnu/8.2.0/../../../../x86_64-pc-linux-gnu/bin/ld: HDF5_create.c:(.text+0xab): undefined reference to `H5Dclose'
/gpfs/shared/software/lang/gcc/8.2.0/bin/../lib/gcc/x86_64-pc-linux-gnu/8.2.0/../../../../x86_64-pc-linux-gnu/bin/ld: HDF5_create.c:(.text+0xba): undefined reference to `H5Sclose'
/gpfs/shared/software/lang/gcc/8.2.0/bin/../lib/gcc/x86_64-pc-linux-gnu/8.2.0/../../../../x86_64-pc-linux-gnu/bin/ld: HDF5_create.c:(.text+0xc9): undefined reference to `H5Fclose'
collect2: error: ld returned 1 exit status
~~~
{: .output}

All those error are due to the lacking library for HDF5, for this simple clase, all that you have to do is add `-lhdf5` to include those libraries.

~~~
$ gcc HDF5_create.c -o HDF5_create -lhdf5
~~~
{: .language-bash}

HDF5 is a relatively complex library, so for complex cases it is better to use the wrapper that compiles code for HDF5

~~~
$ h5cc HDF5_create.c -o HDF5_create
~~~
{: .language-bash}

This command is adding all the needed libraries and paths to the compilation line. At the end you get the binary `a.out`

## FFTW3

FFTW is a C subroutine library for computing the discrete Fourier transform (DFT) in one or more dimensions, of arbitrary input size, and of both real and complex data (as well as of even/odd data, i.e. the discrete cosine/sine transforms or DCT/DST). We believe that FFTW, which is free software, should become the FFT library of choice for most applications.

The example below shows how a few scattered set of points are transform

~~~
//2Dfftw.cpp
//2 dimensional complex-to-real inverse FFT test.
//Produces a 256 x 256 real-valued matrix
#include <fstream>
#include <iostream>
#include <complex>
#include <fftw3.h>

int main(int argc, char** argv)
{
    int FFTSIZE = 256;

    std::complex<double>* input_array;
    std::complex<double>* output_array;

   // input_array = new std::complex<double>[FFTSIZE * FFTSIZE];
    //output_array = new double[FFTSIZE * FFTSIZE];
    output_array = (std::complex<double>*)fftw_alloc_complex(FFTSIZE*FFTSIZE);
    input_array = (std::complex<double>*)fftw_alloc_complex(FFTSIZE * FFTSIZE);

    for(int i = 0; i < FFTSIZE * FFTSIZE; i++) input_array[i] = 0;

    input_array[50000]   = std::complex<double>(100, 0);
    input_array[10000]  = std::complex<double>(100, 0);
    input_array[16000]  = std::complex<double>(100, 0);
    input_array[48000]  = std::complex<double>(100, 0);
    input_array[32000] = std::complex<double>(100, 0);

    fftw_plan p = fftw_plan_dft_2d(FFTSIZE, FFTSIZE, (fftw_complex*)input_array, (fftw_complex*)output_array, FFTW_BACKWARD, FFTW_DESTROY_INPUT | FFTW_ESTIMATE);
    fftw_execute(p);

    std::ofstream InputDump("input.txt");
    for(int j = 0; j < FFTSIZE; j++)
    {
        for(int i = 0; i < FFTSIZE; i++)
        {
            InputDump << " " << input_array[j * FFTSIZE + i].real();
        }
        InputDump << std::endl;
    }
    InputDump.close();

    std::ofstream OutputDump("output.txt");
    for(int j = 0; j < FFTSIZE; j++)
    {
        for(int i = 0; i < FFTSIZE; i++)
        {
            OutputDump << " " << output_array[j * FFTSIZE + i].real();
        }
        OutputDump << std::endl;
    }
    OutputDump.close();

}
~~~
{: .source}

To compile this code we need the module for FFTW and add the right library to the compilation command

We load the module with:

~~~
$ module load lang/gcc/8.2.0 libs/fftw/3.3.8_gcc82 lang/python/3.7.2_gcc82
~~~
{: .language-bash}

The libraries for fftw are now available both for compilation and execution.
We added a Python module that will help us produce the plots for the input and and output arrays.

~~~
g++ 2Dfftw.c -o 2Dfftw -lfftw3
~~~
{: .source}

Execute the binary `./2Dfftw` and create the plots with:

~~~
./plot_fft.py
~~~
{: .source}

That will generate 2 figures that you can transfer to your computer to see.

>## Exercise: Compiling codes.
>
> On the repository for `2019-Data-HandsOn` there is a folder with the examples presented on this episode `1.Intro-HPC/compile`.
>
> There you will find a couple of source codes: `example_gsl.c` and `matmult.c` that have not being compiled yet.
>
> The challenge is to compile those codes.
>{: .source}
{: .challenge}

{% include links.md %}
