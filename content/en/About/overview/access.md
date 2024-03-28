---
title: Access
linkTitle: Access
type: docs
weight: 3
aliases:
    - /rates.html
    - /rates
    - /manuals_linux-basics_intro.html
    - /manuals_linux-basics_intro
    - /manuals/linux_basics/intro
    - /about/facility/access
---

## User account requests

To create new user or lab accounts please, follow these instructions:

* Please email user account requests to [support@hpcc.ucr.edu](mailto:support@hpcc.ucr.edu). Include the full name, NetID and email address of both users and PI. Users need to be members of the PI's group. Preferentially, user account requests should come from the corresponding PI directly. If the request comes from a new user then the PI needs to be CC'ed in the email exchange. 
* If a PI's lab is not registered yet, please provide in the same email a COA (formerly FAU) required to pay for the annual subscription fee, and optionally for additional data storage (see [here](https://hpcc.ucr.edu/about/overview/access/#recharging-rates)). If additional storage is needed, mention how much and the COA to be used for the additional recharge.  

After receiving the access information for a new account, users want to follow the login instructions [here](/manuals/login).

## Recharging rates

HPCC's recharging rate structure is outlined below. A more formal summary is available in the most recent _Recharging Rate PDF_ [here](/about/facility/rates). 

## Lab-based Registration Fee

An annual registration fee of $1,000 gives all members of a UCR lab access to our high-performance computing infrastructure.
The registration provides access to the following resources: 

  * Over 16,000 CPU cores (60% AMD and 40% Intel), ~230,000 cuda cores (Nvidia A100, P100 and K80 GPUs), ~3PB parallel GPFS-based disk space, 512GB-1TB of memory/node, etc. More details are available on the [hardware pages](https://hpcc.ucr.edu/about/hardware/).
  * Over 1000 software packages and community databases. Details are available on the software page.
  * Free attendance of workshops offered by HPCC staff
  * Free consultation services (up to 1 hour per month)
  * Note: there is no extra charge for CPU usage but each user and lab have CPU quotas of 256 and 512 CPU cores, respectively. Computing jobs exceeding these quotas can be submitted but will stay in a queued state until resources within the quota limits become available. 

## Big data storage

* Standard user accounts have a storage quota of 20 GB. To gain access to much larger storage pools, PIs have the option to rent or own storage space.
* Storage rental option
    
    * $1000 per 10TB of usable and backed up storage space. Storage pool is shared among all user accounts of a registered lab.
 
## Ownership models

* Disk storage
        
    * Lab purchases storage hardware (_e.g._ hard drives) according to the specifications of the facility. Owned hard drives will be added to the facility's parallel GPFS storage system. The annual support fee for owned disk storage is $250 per 10TB of usable and backed-up storage space. The owned storage space is only available to the users of a PI or those a PI wishes to give access to.
    * Owned storage can be attractive for labs with storage needs above 40 TBs.

* Compute nodes
        
    * Lab purchases compatible computer nodes (e.g. with supported network cards). Examples of popular high-density architecture are quad node systems shown here: [HDX XT24-5260V4](http://www.thinkmate.com/system/hdx-xt24-5260v4-sas3) and [HDX XN24-52S1](https://www.thinkmate.com/system/hdx-xn24-52s1). A quad node system includes 4 nodes each configured with two 16 core Intel chips (total physical core count 128), 512GB of RAM, 1.2TB SSD and FDR-IB interconnect. Similar options exist for [AMD EPYC](https://www.gigabyte.com/us/High-Density-Server/H262-Z63-rev-100#ov) and [GPU](https://www.thinkmate.com/systems/servers/gpx) nodes. 
    * Nodes are administered under a priority queueing system that gives users from an owner lab priority and also increases that lab's overall CPU quota (see above) by the number of owned CPU cores.
    * Owned computer nodes are an attractive solution for labs requiring 24/7 access to hundreds of CPU cores with no or only minor waiting times in queue.

## Software install

* Registered users can email software install requests to HPCC's issue tracking system @ [support@hpcc.ucr.edu](mailto:support@hpcc.ucr.edu). Install requests are addressed in the order received. Simple installs are addressed within 1 to a few days. Complex installs may take longer.

<!---
## Startup packages for new PIs
Startup packages are available for variable numbers and architectures of HPC nodes and storage amounts. This includes the following components:

Standard startup packages in the amount of $20K (N=1), $30K (N=2), $40K (N=3)
and so on are available. Note: N refers to the number of HPC nodes below. The
cost for these packages can be covered by the initial complement of new PIs. 

* N HPC node(s): owned by lab for 5 yrs and administered under priority queueing model. After this time the node becomes part of the shared HPCC cluster resources. 

    * Each node with 32* Intel CPU cores (64* logical cores), 512GB RAM and Infiniband interconnect. *The core numbers might nearly double when newer and less expensive Intel chip sets will be released this year. However, the per node cost may be subject to rapid changes (e.g the cost of RAM has increased by several fold in last year).
    * Alternative node architecture (_e.g._ GPU) are available upon request
* Owned HPC nodes with various CPU/GPU architectures, RAM and SSD specifications. Pricing is comptetitive, but will greatly depend on the current market value of HPC components, custom configurations and discounts provided by vendors. 
* Rented big data storage @ $1000 for 10TB per yr covered for 5 yrs; or owned disk storage when storage needs are above 20TB 
* HPCC subscription fee of $1000/yr covered for 5 yrs

To configure a startup HPC package, please contact the facility staff directly.
-->

## Department cluster membership with owned computing nodes

This option addresses the need of department-level HPC access where the standard 
PI-based membership is not practical, _e.g._ provide cluster access to large number of undergraduate
students in classes. Under this model a department purchases computer nodes
that will be administered similarly as described above under the _Ownership
model_. Due to the large number of expected users from departments, the
CPU quota per user is usually lower compared to the PI-based model.

## Using HPCC cluster for classes

To use the HPCC cluster for teaching UCR classes, please coordinate with the
systems administrators (support@hpcc.ucr.edu) at least 4 weeks prior to the
start of a class so that there is enough time for planning. Details that need
be discussed includes the number of user accounts required, special software
requirements, creation of a class-specific Slurm partition, data storage
reservations, as well as other needs that may vary for different classes. 

## External user accounts
Accounts for external customers can only be granted if a lab has a strong
affiliation with UC Riverside, such as a research collaboration with UCR
researchers. Both the corresponding UCR PI and external collaborator need to
maintain an HPCC subscription. External accounts are subject to an annual
review and approval process. To be approved, the external and internal PIs have
to complete this [External Usage Justification](https://bit.ly/32O1lC9).

## Facility description

   * The latest hardware/facility description (_e.g._ for grant applications) is available [here](https://goo.gl/43eOwQ).

