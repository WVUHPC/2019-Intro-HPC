---
title: "Software Containers (Singularity)"
teaching: 30
exercises: 30
questions:
- "How to send files in and out the cluster"
objectives:
- "Transfer files using rsync and Globus Online"
keypoints:
- "Transfer files using rsync and Globus Online"
---

Singularity Containers
======================

Containers are a software technology that allows us to keep control of
the environment where a given code runs. Consider for example that you
want to run a code in such a way the same code runs on several machines
or clusters ensuring that the same libraries are loaded and the same
general environment is present. Different clusters could come installed
with different compilers, different Linux distributions and different
libraries in general. Containers can be used to package entire
scientific workflows, software and libraries, and even data and move
them to several compute infrastructures with complete reproducibility.

Containers are similar to Virtual Machines, however, the differences are
enough to consider them different technologies and those differences are
very important for HPC. Virtual Machines takes up a lot of system
resources. Each Virtual Machine (VM) runs not just a full copy of an
operating system, but a virtual copy of all the hardware that the
operating system needs to run. This quickly adds up to a lot of precious
RAM and CPU cycles, valuable resources for HPC.

In contrast, all that a container requires is enough of an operating
system, supporting programs and libraries, and system resources to run a
specific program. From the user perspective, a container is in most
cases a single file that contains the file system, ie a rather complete
Unix filesystem tree with all libraries, executables, and data that are
needed for a given workflow or scientific computation.

There are several container solutions, one prominent example is Docker,
however, the main issue with containers is security, despite the name,
containers do not actually contain the powers of the user who executes
code on them. That is why you do not see Docker installed on an HPC
cluster. Using dockers requires superuser access something that on
shared resources like an HPC cluster is not widely offered.

Singularity offers an alternative solution to Docker, users can run the
prepared images that we are offering on Spruce or bring their own.

For more information about Singularity and complete documentation see:
<https://singularity.lbl.gov/quickstart>


{% include links.md %}
