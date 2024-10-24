---
title: Queue Policies
linkTitle: Queue Policies
type: docs
weight: 4
aliases:
    - /manuals_linux-cluster_queue.html
    - /manuals_linux-cluster_queue
---

## Start Times
Start times are a great way to track your jobs:
```bash
squeue -u $USER --start
```
Start times are rough estimates based on the current state of the queue.

## Partition Quotas

Each partition has a specific usecase. Below outlines each partition, it's usecase, as well as any job/user/group limits that are in place.
Empty boxes imply no limit, but is still limited by the next higher limit. Job limits are capped by user limits, and user limits are capped by group limits.

| Partition Name   | Usecase                                                   | Per-Group Limit | Per-User Limit         | Per-Job Limit                             | Max Job Time |
|------------------|-----------------------------------------------------------|-----------------|------------------------|-------------------------------------------|--------------|
| epyc (2021 CPU)  | CPU Intensive Workloads, Multithreaded, MPI, OpenMP       |                 | 384 Cores, 1TB memory  | 64GB memory per Core [^1],[^4]            | 30 Days      |
| intel (2016 CPU) | CPU Intensive Workloads, Multithreaded, MPI, OpenMP       |                 | 384 Cores, 1TB memory  | 64GB memory per Core [^1],[^3]            | 30 Days      |
| batch (2012 CPU) | CPU Intensive Workloads, Multithreaded, MPI, OpenMP       |                 | 384 Cores, 1TB memory  | 64GB memory per Core [^1],[^2]            | 30 Days      |
| short            | Short CPU Intensive Workloads, Multithreaded, MPI, OpenMP |                 | 384 Cores, 1TB memory  | 64GB memory per Core, 2-hour time limit   | 2 Hours      |
| highmem          | Memory Intensive Workloads                                |                 | 32 Cores, 1TB memory   |                                           | 30 Days      |
| gpu              | GPU-Enabled Workloads                                     | 8 GPUs          | 4 GPUs[^6],48 Cores, 512GB memory | 16 Cores, 256GB memory [^1],[^5]          | 7 Days      |

 [^1]: A 64GB-per-core limit is placed to prevent over allocating memory compared to CPUs. If more than a 64GB-per-core ratio is requested, the core count will be increased to match.  
 [^2]: Allocatable memory per-node in the **batch** partition is limited to **~500GB** to allow for system overhead.  
 [^3]: Allocatable memory per-node in the **intel** partition is limited to **~450GB** to allow for system overhead.  
 [^4]: Allocatable memory per-node in the **epyc** partition is limited to **~950GB** to allow for system overhead.  
 [^5]: Allocatable memory per-node in the **gpu** partition is dependent on the node. 115GB for gpu[01-02], 500GB for gpu[03-04], 200GB for gpu05, 922GB for gpu06, 950GB for gpu[07-08]  
 [^6]: If a user needs more than 4 GPUs, please contact support@hpcc.ucr.edu with a short justification for a temporary increase.
 
 Attempting to allocate more member than a node can support, eg 500GB on an Intel node, will cause the job to immediately fail.  

Limits are for actively running jobs, and any newly queued job that exceeds a limit will be queued until resources become available. In addition
to the above limits, there is also a 768 core group limit that spans across all users in a group across all partitions. Upon request, a user or
group’s quota can be extended temporarily, but only if sufficient CPU resources are available and with adequate justification. If you wish to
request additional resources, please contact us at support@hpcc.ucr.edu with your justification.

Partition quotas can also be viewed on the cluster using the `slurm_limits` command.

### Example Scenarios

#### Per-Job Limit

A job is submitted on the gpu partition. The job requests 32 cores.

> This job will not be able to be submitted, as 32 cores is above the partition's 16 core per-job limit.

#### Per-User Limit

You submit a job the highmem partition, requesting 32 cores.

> This job will start successfully, as it is within the partition's core limit.

You submit a second job while the first job is still running. The new job is requesting 32 cores.

> Because you are at your per-user core limit on the highmem partition, the second job will be queued until the first job finishes.

#### Per-Lab Limit

User A submits a job requesting 384 cores. User B submits a job requesting 384 cores.

> Because each user is within their per-user limits and the lab is within their limit, the jobs will run in parallel.

User C submits a job, requesting 16 cores.

> Because User A and User B are using all 768 cores within the lab, User C's job will be queued until either User A's or User B's jobs finishes.

## Changing Partitions

In `srun` commands and `sbatch` scripts, the `-p` or `--partition` flag controls which partition/queue a job will run on. For example,
using `-p epyc` will have your job queued and ran on the `epyc` partition. For more examples and information on running jobs,
see the [Managing Jobs](https://hpcc.ucr.edu/manuals/hpc_cluster/jobs/) page of our documentation.

## Fair-Share
Users that have not submitted any jobs in a long time usually have a higher priority over others that have ran jobs recently.
Thus the estimated start times can be extended to allow everyone their fair share of the system.
This prevents a few large groups from dominating the queuing system for long periods of time.

You can see with the `sqmore` command what priority your job has (list is sorted from lowest to highest priority).
You can also check to see how your group's priority is compared to other groups on the cluster with the "sshare" command.

For example:
```bash
sshare
```

It may also be useful to see your entire group's fairshare score and who has used the most shares:
```bash
sshare -A $GROUP --all
```

Lastley, if you only want to see your own fairshare score:
```bash
sshare -A $GROUP -u $USER
```

The fairshare score is a number between 0 and 1. The best score being 1, and the worst being 0.
The fairshare score approches zero the more resource you (or your group) consume.
Your individual consumption of resources (usage) does affect your entire group's fiarshare score.
The affects of your running/completed jobs on your fairshare score are halved each day (half-life).
Thus, after waiting several days without running any jobs, you should see an improvment in your fairshare score.

Here is a very good [explaination of fairshare](https://www.rc.fas.harvard.edu/fairshare/).

## Priority
The fairshare score and jobs queue wait time is used to calculate your job's priority.
You can use the `sprio` command to check the priority of your jobs:

```
sprio -u $USER
```

Even if your group has a lower fairshare score, your job may still have a very high priority.
This would be likely due to the job's queue wait time, and it should start as soon as possible regardless of fairshare score.
You can use the `sqmore` command to see a list of all jobs sorted by priority.

## Backfill
Some small jobs may start before yours, only if they can complete before yours starts and thus not negatively affecting your start time.

## Priority Partition
Some groups on our system have purchased additional hardware. These nodes will not be affected by the fairshare score.
This is because jobs submitted to the group's partition will be evaluated first before any other jobs that have been submitted to those nodes from a different partition.
