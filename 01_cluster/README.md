# What's an HPC cluster?

[Table of Contents](/hpc_beginning_workshop/)

![Beowulf Cluster](beowulf.png)

## Terminology

* Head Node
  * The server that connects a cluster to the an outside network.
* Compute Node
  * A server that is used for running computations, sometimes managed by a scheduler.
* Cores
  * A shorthand way to refer to the number of
  processor cores (usually physical) of a CPU
  in a node.

**IMPORTANT**: *You may run test jobs on the head nodes that run for up to 10 minutes and use up to 10% of the CPU cores and memory. You will likely disrupt the work of others if you exceed these limits.*

![Nodes to Cores](https://computing.llnl.gov/tutorials/parallel_comp/images/nodesNetwork.gif)

### No Internet Access on the Compute Nodes

For security reasons the compute nodes do not have internet access. You access the compute nodes, and run programs or *jobs* on the compute nodes, via a *scheduler* (an "uber-program" that doles out access to the cores and RAM to various other programs all competing for attention).

Princeton uses a scheduler called *SLURM*.  Running a (non-trivial) program on our clusters consists of writing a "SLURM script" to request computational resources.  And then you wait your turn in a *queue* while SLURM manages all competing requests (yours + those of others on the system).

**NB:** You need to make sure all needed files are present on the cluster before submitting the job to the batch scheduler.

## Reasons to Use the Clusters

- Lots of processing capacity (e.g., you could use 1000 CPU-cores for a single job)  
- Nodes have 100's GB of memory  
- One can store large datasets on `/scratch/gpfs` (or `/scratch/network` on Adroit)
- Lots of software is available and already configured (e.g., MPI, compilers, commercial software)
- Keep your laptop free of use by running the work on the clusters
- GPUs are available in large numbers on TigerGPU and Traverse
- There is a team of people maintaining and supporting the clusters

## Some Clusters At Princeton
For more detail, see:
[Computational Hardware @ Princeton](https://researchcomputing.princeton.edu/systems-and-services/available-systems)

### [Nobel](https://researchcomputing.princeton.edu/systems-and-services/available-systems/nobel)
  This cluster is most appropriate for interactive work. Any member of the
  university community can request access for their work. Two nodes (Davisson and Compton), no distinction between login/compute.

### [Adroit](https://www.princeton.edu/researchcomputing/computational-hardware/adroit/)
  Also available generally to members of the university community. It is composed of 9 Skylake nodes, 7 Ivybridge nodes for MyAdroit and 2 GPU nodes consisting of either Tesla K40c or v100 GPUs.

### [Tiger](https://researchcomputing.princeton.edu/systems-and-services/available-systems/tiger)

![tiger-cluster](https://researchcomputing.princeton.edu/sites/researchcomputing2/files/styles/panopoly_image_original/public/media/tiger-diagram_0.jpg?itok=NCJUQOsC)

### Other Clusters
  These <a href="https://researchcomputing.princeton.edu/systems-and-services/available-systems" target="_blank">other clusters</a> such as Perseus, Tiger, Della and Traverse are research
  clusters with many more nodes and GPFS networked storage between clusters.

  See [https://www.princeton.edu/researchcomputing/access/](https://www.princeton.edu/researchcomputing/access/)
  for details:

  > Proposals for the large  cluster systems should be submitted as PDF or MS Word documents not to exceed 3 pages. The proposal, which can be submitted through an online form . . . etc.

*IMPORTANT*: The Tiger cluster is designed for large parallel jobs. In an effort to dissaude users from running small jobs on Tiger, such as serial or single-node jobs, the scheduler has been configured to penalize these submissions by causing long queue times. Della is ideal for serial jobs. If you only have an account on Tiger and you want to run several small jobs then please write to cses@princeton.edu to request an account on Della. Be sure to explain the situation.

## Where to Store Your Files?

See Storage on RC website: [https://researchcomputing.princeton.edu/storage](https://researchcomputing.princeton.edu/storage)

## Learn More About Adroit by Running Commands

If off-campus then connect via the GlobalProtect VPN</a> on your laptop (<a href="https://www.princeton.edu/vpn">installation directions</a>). Then run this command in a [terminal](https://researchcomputing.princeton.edu/education/training/hardware-and-software-requirements-picscie-workshops):

```
ssh <YourNetID>@adroit.princeton.edu
```

Type each command below and examine the output:

```
hostname                  # get the name of the machine you are on
whoami                    # get username of the account
date                      # get the current date and time
pwd                       # print working directory
cat /etc/os-release       # info about operating system
lscpu                     # info about the CPUs on head node
snodes                    # info about the compute nodes (7 nodes for myadroit)
squeue                    # which jobs are running or waiting to run
qos                       # quality of servce (job partitions and limits)
slurmtop                  # shows a map of cluster usage
who                       # list users on the head node
checkquota                # view your quota and request more space
```

Here is example output from the commands above:

<pre>
$ <b>hostname</b>
adroit4


$ <b>whoami</b>
ceisgrub


$ <b>date</b>
Fri Feb 21 16:20:03 EST 2020


$ <b>pwd</b>
/home/ceisgrub


$ <b>cat /etc/os-release</b> 
NAME="Springdale Linux"
VERSION="7.7 (Verona)"
ID="rhel"
ID_LIKE="fedora"
VERSION_ID="7.7"
PRETTY_NAME="Springdale Linux 7.7 (Verona)"
ANSI_COLOR="0;32"
CPE_NAME="cpe:/o:springdale:linux:7.7:GA"
HOME_URL="http://springdale.princeton.edu/"
BUG_REPORT_URL="https://springdale.math.ias.edu/"

REDHAT_BUGZILLA_PRODUCT="Springdale Linux 7"
REDHAT_BUGZILLA_PRODUCT_VERSION=7.7
REDHAT_SUPPORT_PRODUCT="Springdale Linux"
REDHAT_SUPPORT_PRODUCT_VERSION=7.7



$ <b>lscpu</b>
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                32
On-line CPU(s) list:   0-31
Thread(s) per core:    1
Core(s) per socket:    16
Socket(s):             2
NUMA node(s):          2
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 85
Model name:            Intel(R) Xeon(R) Gold 6142 CPU @ 2.60GHz
Stepping:              4
CPU MHz:               1021.179
CPU max MHz:           3700.0000
CPU min MHz:           1000.0000
BogoMIPS:              5200.00
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              1024K
L3 cache:              22528K
NUMA node0 CPU(s):     0,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30
NUMA node1 CPU(s):     1,3,5,7,9,11,13,15,17,19,21,23,25,27,29,31
Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch epb cat_l3 cdp_l3 intel_ppin intel_pt ssbd mba ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm cqm mpx rdt_a avx512f avx512dq rdseed adx smap clflushopt clwb avx512cd avx512bw avx512vl xsaveopt xsavec xgetbv1 cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local dtherm ida arat pln pts pku ospke md_clear spec_ctrl intel_stibp flush_l1d



$ <b>snodes</b>
HOSTNAMES     STATE    CPUS S:C:T    CPUS(A/I/O/T)   CPU_LOAD MEMORY   GRES     PARTITION          AVAIL_FEATURES
adroit-01     idle     20   2:10:1   0/20/0/20       0.01     128000   (null)   class              ivy
adroit-02     idle     20   2:10:1   0/20/0/20       0.01     64000    (null)   class              ivy
adroit-03     idle     20   2:10:1   0/20/0/20       0.01     64000    (null)   class              ivy
adroit-04     idle     20   2:10:1   0/20/0/20       0.01     64000    (null)   class              ivy
adroit-05     idle     20   2:10:1   0/20/0/20       0.01     64000    (null)   class              ivy
adroit-06     idle     20   2:10:1   0/20/0/20       0.01     64000    (null)   class              ivy
adroit-07     idle     20   2:10:1   0/20/0/20       0.01     64000    (null)   class              ivy
adroit-08     mix      32   2:16:1   31/1/0/32       19.93    384000   (null)   all*               skylake
adroit-09     idle     32   2:16:1   0/32/0/32       0.01     384000   (null)   all*               skylake
adroit-10     mix      32   2:16:1   25/7/0/32       16.58    384000   (null)   all*               skylake
adroit-11     mix      32   2:16:1   30/2/0/32       11.01    384000   (null)   all*               skylake
adroit-12     mix      32   2:16:1   30/2/0/32       10.98    384000   (null)   all*               skylake
adroit-13     mix      32   2:16:1   24/8/0/32       4.20     384000   (null)   all*               skylake
adroit-14     mix      32   2:16:1   20/12/0/32      20.01    384000   (null)   all*               skylake
adroit-15     idle     32   2:16:1   0/32/0/32       0.01     384000   (null)   all*               skylake
adroit-16     mix      32   2:16:1   5/27/0/32       3.01     384000   (null)   all*               skylake
adroit-h11g1  idle     40   2:20:1   0/40/0/40       0.04     770000   gpu:tesl gpu                (null)
adroit-h11g4  idle     16   2:8:1    0/16/0/16       0.61     64000    gpu:tesl gpu                (null)



$ <b>squeue</b>
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            660090       all run_05_0    fanid PD       0:00      1 (QOSMaxJobsPerUserLimit)
            661432       all       KL   mlenel PD       0:00      1 (Dependency)
      661431_[6-7]       all       KL   mlenel PD       0:00      1 (Dependency)
            661439       all sys/dash lclingan  R      37:28      1 adroit-10
          661429_6       all       KL   mlenel  R    1:13:28      1 adroit-08
          661429_7       all       KL   mlenel  R    1:13:28      1 adroit-08
            660093       all run_15_0    fanid  R 3-20:26:13      1 adroit-16
            660092       all run_15_0    fanid  R 3-20:27:14      1 adroit-08
            660091       all run_15_0    fanid  R 3-20:28:42      1 adroit-08
            660087       all run_15_1    fanid  R 9-12:05:41      1 adroit-08
            660999       all BBR_m06-    kyras  R 2-21:31:33      1 adroit-12
            660996       all BMR_m06-    kyras  R 2-21:40:50      1 adroit-11
            660085       all run_1_1_    fanid  R 10-00:38:49      1 adroit-08
            660086       all run_05_1    fanid  R 10-00:38:49      1 adroit-08
            660088       all run_05_0    fanid  R 10-00:38:49      1 adroit-16
            660089       all run_05_0    fanid  R 10-00:38:49      1 adroit-16
            660116       all tBu3PPd_     leit  R 8-05:22:17      1 adroit-11
            660114       all Phdiimin     leit  R 8-14:31:04      1 adroit-14
            660115       all tBu3PPd_     leit  R 8-14:31:04      1 adroit-14
            660113       all Phdiimin     leit  R 9-12:05:41      1 adroit-12
            661298       all job.scri vincenzi  R    6:37:12      1 adroit-13
            661391       all run_cali  hherman  R    3:59:23      1 adroit-10
 
 
 
$ <b>who</b>
bill     pts/0        2020-02-18 08:17 (delta.princeton.edu)
yongickc pts/1        2020-02-20 08:11 (chm-c07y213xjyvx.princeton.edu)
zhengshi pts/2        2020-02-10 01:59 (:pts/25:S.0)
root     pts/3        2020-02-19 13:54 (adroit-nfs2)
keweiz   pts/4        2020-02-21 09:01 (vpn10-client-128-112-69-24.princeton.edu)
haonan   pts/5        2020-02-21 15:23 (myadroit)
jdh4     pts/9        2020-02-21 16:12 (tigressgateway1.princeton.edu)
yongickc pts/10       2020-02-17 08:14 (chm-c07y213xjyvx.princeton.edu)
zhengy   pts/11       2020-02-21 09:26 (nat-oitwireless-inside-vapornet100-10-8-2-157.princeton.edu)
msislam  pts/12       2020-02-21 15:36 (nat-oitwireless-inside-vapornet100-10-9-125-87.princeton.edu)
aidanm   pts/13       2020-02-21 10:03 (pni-10h01410k2euh.princeton.edu)
meggl    pts/15       2020-02-21 10:04 (vpn10-client-128-112-71-193.princeton.edu)
mrasna   pts/16       2020-02-21 14:54 (nat-oitwireless-inside-vapornet100-10-8-5-193.princeton.edu)
nmishra  pts/17       2020-02-21 15:55 (nat-oitwireless-inside-vapornet100-10-9-111-71.princeton.edu)
cfei     pts/19       2020-02-21 13:59 (nat-oitwireless-inside-vapornet100-10-9-154-116.princeton.edu)
jdeobald pts/21       2020-02-21 11:42 (dynamic-oit-swiftnet-128-112-127-249.princeton.edu)
dianw    pts/22       2020-02-21 11:54 (vpn10-client-128-112-69-89.princeton.edu)
xiaoyuel pts/23       2020-02-21 14:26 (vpn10-client-128-112-71-50.princeton.edu)
perezgiz pts/24       2020-02-21 14:07 (josko-eth-dongle.princeton.edu)
zhengshi pts/25       2020-02-12 14:41 (:pts/20:S.0)
keweiz   pts/26       2020-02-21 14:30 (vpn10-client-128-112-69-24.princeton.edu)
keweiz   pts/27       2020-02-21 16:09 (vpn10-client-128-112-69-24.princeton.edu)



$ <b>checkquota</b>
          Storage/size quota filesystem report for user: ceisgrub
Filesystem               Mount                 Used   Limit  MaxLim Comment
Adroit home              /home                9.1GB   9.3GB    10GB 
Adroit scratch           /scratch                 0       0       0 
Adroit scratch network   /scratch/network     1.7GB    93GB   100GB 

          Storage number of files used report for user: ceisgrub
Filesystem               Mount                 Used   Limit  MaxLim Comment
Adroit home              /home                80.5K    975K    1.0M 
Adroit scratch           /scratch                 1       0       0 
Adroit scratch network   /scratch/network     18.9K    9.8M   10.5M 

For quota increase requests please use this website:

         https://forms.rc.princeton.edu/quota
</pre>
