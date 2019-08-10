---
title: HPC Concepts
keywords: introduction, outline
last_updated: August 12, 2019
summary: "What is high-performance computing?"
sidebar: shortcourse_sidebar
permalink: shortcourse_concepts.html
folder: shortcourse
toc: false
---

## Physical infrastructure

> Our high performance computing (HPC) system, named *Blue Crab* is a supercomputer, formerly ranked in the [top 500 worldwide](https://www.top500.org/site/50589). Capable of `~1.4 PFLOPS` (a petaflop is one quadrillion floating-point operations per second), it is equivalent to `~10,000` laptop computers and consists of over `23,000` cores and over `800` nodes. The purpose of maintaining such a large computer is to performing computations at massive scale while also serving a very large community of academic researchers.

## Terms

We use the following names to describe parts of the machine:

1. `node` a single shelf in a rack. A node is equivalent to a single desktop computer.
2. `cluster` a network of nodes. *Blue Crab* is a cluster.
3. `processor` a single electronic circuit. Also known as a `core`.
4. `memory` random access memory (RAM). Large bandwidth to the `core`.
5. `disk` storage on a spinning disk separate from the `node` and comparatively slower than `memory`.

## Compute hardware

*Blue Crab* has both compute and storage hardware. The compute hardware comes in three flavors:

1. `780` Standard compute nodes. These typically have 24 cores and `128GB` of memory.
2. `50` Large memory nodes. These include 48 cores and `1TB` of memory.
3. `79` Graphical processing unit (GPU) nodes. These typicaly have 24 cores, `128GB` of memory, and either 2 or 4 GPUs, in one of several types.

### GPU hardware

We offer three kinds of GPUs, all from [NVidia](https://www.nvidia.com/en-us/) where `NxM` implies `N` compute nodes with `M` GPUs on each node.

1. `72x4` Tesla [K80](https://www.nvidia.com/en-gb/data-center/tesla-k80/)
2. `5x2` Tesla [P100](https://www.nvidia.com/en-us/data-center/tesla-p100/)
3. `2x2` Volta [V100](https://www.nvidia.com/en-us/data-center/tesla-v100/)

Like all data centers and HPC resources, we use the `Tesla` line of NVidia products. These are distinct from the consumer line in three ways. First, they are more physically robust, which is a requirement for use in a dense, hot cluster. Second, they often have higher-bandwidth networking required to use many GPUs at once. Third, and most importantly, they include error-correcting code ([ECC](https://en.wikipedia.org/wiki/ECC_memory)) memory which is necessary to prevent data corruption and enables binary-reproducible, double-precision calculations. Current trends in artificial intelligence suggest movement towards single- and half-precicion calculations. These are almost always unsuitable for physics-based calculations.

## Storage

*Blue Crab* has two entirely independent file systems. These provide the usual "storage" which holds data on timescales up to months. Storage should not be confused with "memory". The computer memory (i.e. RAM) is physically located on the compute nodes and disappears at the end of your calculation. Memory can swallow a deluge of data coming from a processor. Storage that exists on spinning disks or even solid-state drives can only swallow a tiny trickle in comparison.

### Lustre storage: the "scratch" system

The fastest storage is provided by a `2PB` (two petabyte, or 2000 terabtyes) [Lustre](http://wiki.lustre.org/Main_Page) system which is connected via [Infiniband](https://en.wikipedia.org/wiki/InfiniBand) (IB) to all compute nodes. We call this the **"scratch"** system because it is meant to be used in a manner similar to memory, that is, it is temporary storage (six months maximum) and is designed to enable high-speed I/O operations. Lustre is not optimal for hosting many tiny files, compiling code, or serving programs (binaries) to your calculations. The `~/scratch` and `~/work` folders (the latter is shared within research groups) occupy the Lustre system.

### ZFS storage

Home directories at the `~/` and `~/data` folders are hosted on the much larger `12PB` [ZFS](https://en.wikipedia.org/wiki/ZFS) storage system is designed for longer-term (but not permanent!) storage. It is suitable for hosting shared data sets and both inputs and outputs from your calculations. 

## Physical plant

The components listed above occupy a single physical plant pictured below. Our site has room for five machines, however only one has been built so far.

![A rendering of the physical site.](figs/pic-marcc-render.png)

The machine sits on a concrete palette, including large cooling facilities and a flywheel which supplies power in the event of an outage.

![An image of the physical site.](figs/pic-marcc-view.png)

## Network topology

A more informative map of the system includes the high-speed network, storage components, compute nodes, and access to the public via the internet.

![A diagram of MARCC.](figs/fig-marcc-topology.png)

The figure above includes connections to the national NSF-funded computational infrastructure called [XSEDE](https://www.xsede.org/), the [open-storage network](https://www.openstoragenetwork.org/) (OSN), and [SciServer](http://www.sciserver.org/), which connections are forthcoming.

Not pictured are the "login nodes" which are the primary gateway to the HPC resource.

## Login nodes and the scheduler

All shared HPC resources use login nodes sometimes called the "head node" to host users who otherwise cannot access the compute hardware directly. Users log on to the head node from which they can access storage, perform minor bookkeeping tasks -- for example, writing a script -- and then send instructions to the scheduler. The scheduler allocates resources. Our scheduler is called [SLURM](https://slurm.schedmd.com/documentation.html) and will be described at length.

{% include note.html content="Using a shared HPC resource is more similar to sharing a microscope than using a laptop. We must fairly schedule resources, move data on and off of the machine, and ensure that our calculations do not disrupt other users." %}

Shared HPC resources are called "multi-tenant" because many people use them at once. Since we cannot build perfect safeguards to prevent users from interfering with others or damaging the machine, *all users are collectively responsible for using the machine carefully*. In practice this means that you **cannot run calculations on the shared login nodes** and you must thoughtfully schedule your jobs on the appropriate hardware. We will explain how to do this in the coming sections.

<a class="btn btn-primary" href="shortcourse_allocations.html">Next: allocations</a>