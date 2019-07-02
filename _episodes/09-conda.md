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




{% include links.md %}
