---
title: Allocations
keywords: introduction, outline
last_updated: August 12, 2019
summary: "How is MARCC funded and how is it shared between users?"
sidebar: shortcourse_sidebar
permalink: shortcourse_allocations.html
folder: shortcourse
toc: false
---

## Funding

MARCC was created thanks to a grant from the state of Maryland. It is currently shared between several schools across Johns Hopkins University and the University of Maryland. Students at either institution must be members of a research group that has access to the machine. MARCC serves the following five entities, with the following distribution of total number of hours or "service units" (SUs) per quarter:

1. The Whiting school of Engineering (14M SU/q)
2. The Krieger school of Arts and Sciences (14M SU/q)
3. The University of Maryland (7M SU/q)
4. The School of Medicine (7M SU/q)
5. The Bloomberg School of Public Health (3M SU/q)

## Allocations

Principal investigators (PIs) must first [request an allocation](https://www.marcc.jhu.edu/request-access/marcc-allocation-request/). Members of their groups can then [request an account](https://www.marcc.jhu.edu/request-access/request-an-account/). PIs at the University of Maryland must request an allocation from the [allocations and advisory committee](http://hpcc.umd.edu/hpcc/bluecrab.html).

## Shared HPC versus Condominiums

The dominant model for academic HPC is similar to the provision of other equipment: research groups purchase computer hardware for nearly exclusive use by their members. Most research-oriented universities organize these purchases into larger groups and offer a condominium or "condo" model in which the university supplies the physical infrastructure in exchange for rent, much like it provides offices and wet labs, while the research groups purchase the hardware and have exclusive access to it.

> A shared HPC system uses compute hours known as "service units" (SUs) as a currency for purchasing exclusive access to the hardware. The scheduler charges down your account when it allocates hardware to you. A service unit is equivalent to 1 hour on 1 core. For example, one day on a 24-core compute node costs 576 SUs.

Larger-scale computation offers two advantages to this model, however these come at the cost of a few minor social and engineering challenges.

1. Users can access massively parallel hardware that they could not otherwise purchase. They are free to spend their SUs all at once.
2. Compute resources are not idle when a research group does not need them.
3. Larger HPC resources distribute the high cost of fast networks, fast storage, unique hardware, and expert support across many users. Scale improves cost.
4. Shared resources must protect users from each other (and themselves).
5. Shared resources are uniquely vulnerable to systematic hardware problems because they are more interconnected than a set of computer labs.
6. Fairly sharing the resource requires negotiation and compromise between all tenants.

In our opinion, the advantages to operating a shared resource outweigh the costs associated with maintaining the system, particularly because they provide a useful and unique resource to all members of the academic communities at Johns Hopkins and the University of Maryland.

## Costs

A startup allocation on MARCC is free. The schools pay for operations and the state of Maryland provided capital costs. This funding model means that there are **no refunds**, however, in contrast to fee-for-service cloud providers, we have generous allocation policies and "mistakes" do not generally incur additional fees. 

> As a rule of thumb, a typical startup allocation of `50,000 SUs` for one quarter is equivalent to one node, with 24 cores, running continuously for 90 days (`24x24x90=51840`). It is important to consider the costs of hardware, network connectivity, software support, and electricity that would be required to replace such a resource.

We encourage users to be mindful of the fact that the resource is *free but not infinite.*

<a class="btn btn-primary" href="shortcourse_help.html">Next: extra help</a>
