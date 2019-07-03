---
title: "Package and Environemnt Management (Conda)"
teaching: 60
exercises: 30
questions:
- "How to create my own Environemnt and install packages with Conda"
objectives:
- "Learn about the different components in Conda"
keypoints:
- "With conda you are able to install a number of packages not available among the centrally managed packages"
---

Conda is an open source package management system and environment management system. Conda quickly installs, runs and updates packages and their dependencies. Conda easily creates, saves, loads and switches between environments on your local computer. Conda as a package manager helps you find and install packages. Conda knows the recipes of compilation for a number of packages. Originally created for Python it is able to manage packages, dependencies for any language like Python, R, Ruby, Lua, Scala, Java, JavaScript, C/ C++ and FORTRAN.

There are two major versions on Conda: **Anaconda** and **Miniconda**. The difference among **Anaconda** and **Miniconda** is that Miniconda only comes the package management system. So when you install it, there is just the management system and not coming with a bundle of pre-installed packages like Anaconda does. Once Conda is installed, you can then install whatever package you need from scratch

So for your Desktop environment Anaconda is probably a better option. For and HPC cluster, inside a container, or for a Continuos Integration (CI) system, Miniconda is a better solution, lighter and easier to customize.

## Packages, Channels and Environments

Those 3 concepts are critical to understand how conda works from the user perspective.
A **package** is a compressed tarball file (.tar.bz2) that contains the binaries, libraries and metadata to allow Conda to manage the installation and follow the dependencies.
Conda packages are downloaded from **channels**, which are URLs to directories containing conda **packages**. The conda command searches a default set of channels, and packages are automatically downloaded from the corresponding **channel**.
Bioconda for example, is a channel specialized in software packages for bioinformatics.
Finally, a conda **environment** is a directory that contains a collection of conda packages that you have installed.
You can activate or deactivate **environments**, and switch packages or versions of them.
Environments are created with recipes that you can also share with others. We can have environments maintained centrally and you can create your own.

With these concepts we can know learn how to create new environments, adding channels and install packages using them.

## Activating Conda

The command to activate conda on Spruce is:

~~~
source /shared/software/miniconda3/etc/profile.d/conda.sh
~~~
{: .language-bash}

If you forget this path in the future, all that you have to do is execute:

~~~
module load conda
~~~
{: .language-bash}

There is no change to environment variables with this load, it will just show you the command that you have to source.

## Creating Environments

To create a new environment use:

~~~
$ conda create --name myenv
~~~
{: .language-bash}
~~~
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /users/username/.conda/envs/myenv



Proceed ([y]/n)? y

Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate myenv
#
# To deactivate an active environment, use
#
#     $ conda deactivate
~~~
{: .output}

What we just create was a new environment with no packages in it. You will add packages a bit later. For the moment notice that you can activate and deactivate with the conda itself (This is a new feature, from version 4.6 and beyond).

~~~
$ conda activate myenv
(myenv) $
~~~
{: .language-bash}

Notice that the prompt is modified, adding the name of the environment to as a prefix. This is useful to know which environment is activated at a given moment in the shell.

To know the environments accesible by conda execute:

~~~
$ conda info --envs
~~~
{: .language-bash}
~~~
# conda environments:
#
base                     /shared/software/miniconda3
qiime2-2018.8            /shared/software/miniconda3/envs/qiime2-2018.8
tpd0001                  /shared/software/miniconda3/envs/tpd0001
myenv                 *  /users/username/.conda/envs/myenv
~~~
{: .output}

The list includes not only the environment you just create but also environments that are centrally managed and created for general usage.

To leave the current environment execute:

~~~
(myenv) $ conda deactivate
$
~~~
{: .language-bash}

When the environment is deactivated, its name is no longer shown in your prompt.

The command `conda activate` send you to the lowest environment called `base`.

## Installing Packages

Inside your own environment, you can install and uninstall packages for any of your environments. But first, lets search for a package on the active channels on the cluster:

~~~
(myenv) $ conda hdf5
~~~
{: .language-bash}
~~~
Loading channels: done
# Name                       Version           Build  Channel             
hdf5                          1.8.16               3  conda-forge         
hdf5                          1.8.16               4  conda-forge         
hdf5                          1.8.17               0  conda-forge         
hdf5                          1.8.17              10  conda-forge         
hdf5                          1.8.17              11  conda-forge         
hdf5                          1.8.17               2  conda-forge         
hdf5                          1.8.17               3  conda-forge         
hdf5                          1.8.17               4  conda-forge         
hdf5                          1.8.17               5  conda-forge         
hdf5                          1.8.17               6  conda-forge         
hdf5                          1.8.17               7  conda-forge         
hdf5                          1.8.17               8  conda-forge         
hdf5                          1.8.17               9  conda-forge         
hdf5                          1.8.18               0  conda-forge         
hdf5                          1.8.18               1  conda-forge         
hdf5                          1.8.18               2  conda-forge         
hdf5                          1.8.18               3  conda-forge         
hdf5                          1.8.18      h525d4c3_0  pkgs/main           
hdf5                          1.8.18      h6792536_1  pkgs/main           
hdf5                          1.8.19               0  conda-forge         
hdf5                          1.8.19               1  conda-forge         
hdf5                          1.8.19               2  conda-forge         
hdf5                          1.8.20               0  conda-forge         
hdf5                          1.8.20               1  conda-forge         
hdf5                          1.8.20      hba1933b_1  pkgs/main           
hdf5                          1.10.1               0  conda-forge         
hdf5                          1.10.1               1  conda-forge         
hdf5                          1.10.1               2  conda-forge         
hdf5                          1.10.1      h9caa474_1  pkgs/main           
hdf5                          1.10.1      hb0523eb_0  pkgs/main           
hdf5                          1.10.2               0  conda-forge         
hdf5                          1.10.2      hba1933b_1  pkgs/main           
hdf5                          1.10.2      hc401514_1  conda-forge         
hdf5                          1.10.2      hc401514_2  conda-forge         
hdf5                          1.10.2      hc401514_3  conda-forge         
hdf5                          1.10.3   hba1933b_1001  conda-forge         
hdf5                          1.10.3      hc401514_0  conda-forge         
hdf5                          1.10.3      hc401514_1  conda-forge         
hdf5                          1.10.3      hc401514_2  conda-forge         
hdf5                          1.10.4      hb1b8bf9_0  pkgs/main           
hdf5                          1.10.4 mpi_mpich_ha7d0aea_1006  conda-forge         
hdf5                          1.10.4 mpi_openmpi_hac320be_1006  conda-forge         
hdf5                          1.10.4 nompi_h3c11f04_1106  conda-forge         
hdf5                          1.10.5 mpi_mpich_ha7d0aea_1000  conda-forge         
hdf5                          1.10.5 mpi_openmpi_hac320be_1000  conda-forge         
hdf5                          1.10.5 nompi_h3c11f04_1100  conda-forge         
~~~
{: .output}

As you see above, there are quite a number of versions, and builds for the same package. You can install the latest version with:

~~~
(myenv) $ conda install hdf5
~~~
{: .language-bash}
~~~
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /users/username/.conda/envs/myenv

  added / updated specs:
    - hdf5


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    _libgcc_mutex-0.1          |             main           3 KB
    hdf5-1.10.5                |nompi_h3c11f04_1100         5.2 MB  conda-forge
    libgcc-ng-9.1.0            |       hdf63c60_0         5.1 MB
    libgfortran-ng-7.3.0       |       hdf63c60_0        1006 KB
    libstdcxx-ng-9.1.0         |       hdf63c60_0         3.1 MB
    ------------------------------------------------------------
                                           Total:        14.4 MB

The following NEW packages will be INSTALLED:

  _libgcc_mutex      pkgs/main/linux-64::_libgcc_mutex-0.1-main
  hdf5               conda-forge/linux-64::hdf5-1.10.5-nompi_h3c11f04_1100
  libgcc-ng          pkgs/main/linux-64::libgcc-ng-9.1.0-hdf63c60_0
  libgfortran-ng     pkgs/main/linux-64::libgfortran-ng-7.3.0-hdf63c60_0
  libstdcxx-ng       pkgs/main/linux-64::libstdcxx-ng-9.1.0-hdf63c60_0
  zlib               conda-forge/linux-64::zlib-1.2.11-h14c3975_1004


Proceed ([y]/n)? y


Downloading and Extracting Packages
libgcc-ng-9.1.0      | 5.1 MB    | ###################################################################### | 100%
hdf5-1.10.5          | 5.2 MB    | ###################################################################### | 100%
libgfortran-ng-7.3.0 | 1006 KB   | ###################################################################### | 100%
_libgcc_mutex-0.1    | 3 KB      | ###################################################################### | 100%
libstdcxx-ng-9.1.0   | 3.1 MB    | ###################################################################### | 100%
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
~~~
{: .output}

As you notice, conda took the decision of installing the version `conda-forge/linux-64::hdf5-1.10.5-nompi_h3c11f04_1100` and its dependencies.
It could be the case that the actual version that conda installs is not what you want. You can also declare a very specific version and build using the command:

~~~
$ conda install -c conda-forge hdf5=1.10.5=mpi_openmpi_hac320be_1000
~~~
{: .language-bash}
~~~
Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /users/gufranco/.conda/envs/myenv

  added / updated specs:
    - hdf5==1.10.5=mpi_openmpi_hac320be_1000


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    hdf5-1.10.5                |mpi_openmpi_hac320be_1000         5.9 MB  conda-forge
    openmpi-3.1.4              |       hc99cbb1_0         4.0 MB  conda-forge
    ------------------------------------------------------------
                                           Total:        10.0 MB

The following NEW packages will be INSTALLED:

  mpi                conda-forge/linux-64::mpi-1.0-openmpi
  openmpi            conda-forge/linux-64::openmpi-3.1.4-hc99cbb1_0

The following packages will be DOWNGRADED:

  hdf5                           1.10.5-nompi_h3c11f04_1100 --> 1.10.5-mpi_openmpi_hac320be_1000


Proceed ([y]/n)? y


Downloading and Extracting Packages
hdf5-1.10.5          | 5.9 MB    | ###################################################################### | 100%
openmpi-3.1.4        | 4.0 MB    | ###################################################################### | 100%
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
~~~
{: .output}

You can be very granular, declaring the channel, the package, version and build.

To know the list of packages on the current environment execute:

~~~
(myenv) $ conda list
~~~
{: .language-bash}
~~~
# packages in environment at /users/username/.conda/envs/myenv:
#
# Name                    Version                   Build  Channel
_libgcc_mutex             0.1                        main  
hdf5                      1.10.5          mpi_openmpi_hac320be_1000    conda-forge
libgcc-ng                 9.1.0                hdf63c60_0  
libgfortran-ng            7.3.0                hdf63c60_0  
libstdcxx-ng              9.1.0                hdf63c60_0  
mpi                       1.0                     openmpi    conda-forge
openmpi                   3.1.4                hc99cbb1_0    conda-forge
zlib                      1.2.11            h14c3975_1004    conda-forge
~~~
{: .output}

You can install several packages of your interest together, but if you install too many it could be the case that at some point you end up with conflicting packages for which conda cannot find a good solution. Or end up downgrading some packages to satisfy the dependencies of others.

## Adding channels

The file `.condarc` in your `$HOME` folder controls the list of channels that can be searched when requesting new packages. The file is in YAML format and the order of channels matters. For example if your `.condarc` looks like this:

~~~
channels:
  - bioconda
  - conda-forge
  - defaults
~~~
{: .source}

Conda will search for packages first in bioconda channel, the highest priority channel, and go down up to the default channel. This is important if you start adding more and more channels to the list as packages could start conflicting with same packages provided by two or more channels.

## Removing Packages and Environments

Packages can be removed with:

~~~
$ conda remove -n myenv scipy
~~~
{: .language-bash}

If you are inside the environment the command can be simplified as:

~~~
(myenv) $ conda remove scipy
~~~
{: .language-bash}

Remove entire environments with:

~~~
$ conda remove -n myenv --all
~~~
{: .language-bash}

or equivalent

~~~
conda env remove --name myenv
~~~
{: .language-bash}


## Creating recipes for Environments

Instead of creating environment and populate them with packages one by one.
It could be better to have a file that could be used to recreate environments when needed.

If your environment is already created the command to get the file `environment.yml`

~~~
(myenv)$ conda env export > environment.yml
~~~
{: .language-bash}

The file `environment.yml`  can also being created manually. The files exported from the command above are very detailed, with specific indications for version and build. However, you can create simple environment files such as:

~~~
name: stats
dependencies:
  - numpy
  - pandas
~~~
{: .source}

And recreate the environment with:

~~~
$ conda env create -f stats.yml
~~~
{: .language-bash}

{% include links.md %}
