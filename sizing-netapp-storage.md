# Sizing NetApp Storage

??? abstract "Document Control"

    !!! info "Page Owner: Troy Hess (Solutions Engineer - IBM and Red Hat Alliances - NetApp)"

    !!! info "User Story"
        **As a**: Solutions Architect

        **I want to**: accurately size a storage solution based on my customer's capacity and performance requirements

        **So that**: I can architect a recommend a NetApp storage solution that will accurately meet my customer's storage needs

    !!! info "Status"
        - [ ] Structure
        - [X] Draft
        - [ ] Reviewed:
        - [ ] Ready
        - [ ] Published: _Add the public URL link here once published_

    !!! info "Classification"
        - [ ] IBM Confidential
        - [X] Public

## NetApp Fusion Sizing Tool

The NetApp Fusion sizing tool is a very powerful, comprehensive tool designed for storage architects to accurately scope and size storage solutions based on customers' applications, workloads, etc. Although Fusion is designed to be intuitive with minimal training required, ***it is highly recommended that you consult with your NetApp Sales Team when designing and sizing a solution using NetApp Fusion.***

Access to Fusion will require a NetApp Support Site (NSS) account associated with your IBM email address. If you need to create a NSS account, the instructions can be found [below](#register-for-a-netapp-support-site-account) or they are also available in the NetApp Training document [here](netapp-training.md).

-   [Getting Started](https://netapp.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=3f7db070-a32c-403b-8cf5-a8f1015dcebb)
-   [System Design Workflow](https://netapp.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=371e44e8-4561-49e8-90d9-a903003f46cc)
-   [Size and Recommend Workflow](https://netapp.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=8de9c413-04f3-4847-b41f-a8f200e928f1)
-   [Tech Refresh Workflow](https://netapp.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=8aba031b-1714-4de5-a8b5-a8f200edd459)
-   [Upgrade/Modify Workflow](https://netapp.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=e8ec13c2-d6aa-48be-ba6c-a90a00762a1f)
-   [Sizing](https://netapp.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=261116e6-60e9-449f-9e49-ab2700709957)    
-   [AFF Workload Automation](https://netapp.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=d354eeca-5c66-48c6-8356-ab41017e7b46)

## Fusion Workflows
A workflow is a series of steps/tasks required to complete a project in Fusion. There are different workflows available in Fusion depending on the type of sales opportunity you are working on (e.g. new system, upgrade, tech refresh, etc...)

Fusion supports the following workflows:

-   **Size and Recommend** - provides the ability to define workloads with capacity and performance requirements and size recommended solutions that satisfy these requirements. (this is often referred to as "forward sizing").  This workflow also provides users the ability to modify/refine a new recommended system configuration.  This workflow is recommended when sizing new systems.  
-   **System Design** - provides users the ability to manually design storage systems, model storage layouts, and assign workloads to return capacity, performance, and environmental details.  This workflow is recommended when designing new storage solutions.
-   **Tech Refresh** - provides the ability to define an existing install base and size a recommended replacement solution based on performance and capacity requirements.
-   **Upgrade/Modify** - provides the ability to import and/or manually design AFF and FAS install base systems and model different upgrade scenarios such as shelf add-ons and cluster expansions.

## AFF & FAS Sizing

### Sizing Options and Preferences

**How does the Auto-Suggest feature pick the controller platform?**
Fusion uses an extensive data set of the throughput and capacity needed to determine which platforms are the best fits. The user can override this default approach at any time by turning off Auto-Suggest and selected the preferred platforms manually.

**How does the Auto-Suggest feature pick the disk type or flash type to use?**
Fusion uses the ratio of the throughput to the capacity needed to decide which disk architectures and which flash acceleration methods to try.

Most ratios involve trying more than one combination. Those results are then ranked and presented according to the sorting options. The default sort is least cost to greater cost. A high throughput and low capacity would tend to suggest SSD-based configurations. A low throughput and high capacity would tend to suggest SATA-based configurations

**Must the user try all shelf types and drive types to find a combination that works? That would be many combinations to try.** Fusion has developed the hardware Auto-Suggest feature to meet these
needs and reduce time spent.

**What does "Degraded Performance OK on HA takeover Event" mean?**
The Degraded Failover Performance OK selection allows you to set the performance expectations during a failover scenario when only one controller is operational. There are two possibilities:

-   **Checked (default)** - Performance degradation is expected and acceptable during failover.
-   **Unchecked** - Performance is expected to be the same as during normal operation (no performance degradation).

This setting affects sizing. If no degradation is expected, then both controllers in the high-availability (HA) pair must operate below 50% utilization during normal mode. The selection of the System Headroom Percentage value also applies to the 50% utilization target.  For example, if failover degradation is not allowed and a 30% headroom is set, each controller is sized to consume only 35% (.5 \* .7 = .35) of its resources. Selecting this setting would make the recommended controller much larger than if those two options were not set.

**What does "Map to Full Shelves" mean?**
This option determines whether Fusion should add extra disks to the recommended configuration to fully populate disk shelves.

**What does "System Age" mean?**
System age is a parameter that considers the effect of aging on system behavior. It represents the expected fragmentation level of the system being sized. If the value is set to Empty System, it reflects a clean file system, which results in perfect chain lengths for reads and writes and very few CP reads. The Half Full setting represents a partially fragmented file system in which chain lengths are about half as long as optimal. The 90% Full setting represents a heavily fragmented file system in which chain lengths are extremely short.  CP reads are modeled as a function of chain lengths based on data derived from install-base systems using AutoSupport™ data.

**The System Age selections (empty, half full, 90% full) sound like capacity choices, not age choices.  What does this mean?**

Fusion asks you to specify the system age as empty, half full, or 90% full. These choices do have something to do with used capacity; however, with the NetApp WAFL® (Write Anywhere File Layout) file system, the number of blocks captured by NetApp Snapshot® technology and the number of blocks overwritten also matter. If there are enough random writes to a particular aggregate, the number of contiguous free blocks available to write to is lower on average. On a fresh volume, WAFL writes in stripes of 64 blocks.  As more and more blocks are written and overwritten (written somewhere else because WAFL does not write in place), on average the number of contiguous free blocks gets lower. Therefore, it takes more write requests to the disk to write the same amount of data.

-   On a half-aged system, 32-block chains are written (potentially ~2x write requests to disk from a non-aged system).
-   On a 90%-aged system, 6-block write chains are written (potentially ~10x write requests to disk).

Not many workloads or environments reach 90% aging. This limitation is mostly a factor of how random-write-heavy a workload is and how much capacity is used. If in doubt, start with a half-aged system because this age tends to match most environments.

**Does Fusion support sizing of MetroCluster?**  
Fusion supports sizing FAS and AFF MetroCluster in ONTAP 9.1 and later.
 To size MetroCluster solutions, you must disable Auto-Suggest and
enable the option on the hardware screen. Currently Fusion supports
sizing of:

-   2-Node Stretch MetroCluster
-   2-Node Fabric MetroCluster
-   4-Node Fabric MetroCluster

At this time, Fusion does not provide sizing recommendations for
MetroCluster IP. 

**How do I size MetroCluster IP**  
To size MetroCluster IP, you must design the solution manually using the
Manual Design workflow in Fusion and validate the workloads that will be
run on the system.  For more information, [read the following
article](https://forums.netapp.com/t5/fusion-documents/How-to-size-MetroCluster-IP-in-Fusion/ba-p/438452). 

**When sizing MetroCluster solutions, do I need to add capacity for remote mirrors?**  
No, Fusion will account for the storage required to host local and
remote mirrors.

### Workloads

**Can I size for multiple heterogeneous workloads with Fusion?**  
Yes. Beyond the convenience of the consolidated user interface, the
capability of sizing for multiple concurrent heterogeneous workloads is
one of the most compelling features of Fusion. Key NetApp strengths
include multiprotocol support and unified architecture. Fusion was
designed from the ground up to provide comprehensive support for these
attributes. Fusion inherently provides the ability to size for
heterogeneous workloads, and it allows you to store and dynamically
modify those workloads to suit your sizing needs

**How do I determine the active working set size (WSS)?**  
WSS is a principle that applies primarily to applications with random
I/O, determining how much of the file set is read or written randomly at
a given point in time. WSS is highly application dependent. If you don't
know the size of the working set, refer to the guidelines in the table
below:  

| **Application**           | **WSS as a Percentage of File Set or Storage**           |
| :---                      | :---                                                     |
| Home directories          | 3% to 10% of total file set                              |
| Databases (OLTP)          | 5% to 20% of database size                               |
| Microsoft Exchange        | Approximately 80% of database size                       |
| Other e-mail applications | Approximately 20% of storage space                       |
| Other applications        | Working set as a percentage of file set or storage space |

Whenever possible, characterize the storage environment and determine
the application WSS either by using the Automated Workload Analyzer
(AWA).  For more information, refer to the "Data Import" section.

**How does Fusion consider working set size (WSS)?**  
For sizings that do not use NetApp Flash Cache™ or NetApp Flash
Pool™ intelligent caching technology, the models assume that all user
data is read from the drives. For sizings based on Flash Cache and Flash
Pool, the metadata and user data in the working set should fit into
flash space for optimal throughput and minimal latency.

**Why do I need to specify working set sizes only for random read and random write workloads?**  
The number of metadata blocks (particularly L1 and active map blocks)
that random-read and random-write workloads read or write is
proportional to the size of the active working set size (WSS). The WSS
is extremely important when using Flash Cache and Flash Pool. If most of
the active data can fit in the Flash Cache or Flash Pool, the number of
IOPS that the system can satisfy with the same number of disks can
potentially be increased by a large amount. Therefore, setting the WSS
correctly is extremely important. This value defaults to 5% of the
capacity required, which represents a reasonable value, but not the true
value for most environments. For very large databases, you should also
look at this value closely because the 5% default becomes a very large
WSS when the database capacity is very large.

**How can I size workloads that have larger I/O sizes than Fusion supports?**  
Fusion has determined that the performance characteristics of workloads
with very large I/O are very similar to those of the largest I/O size
supported for each protocol (64K for FC and FC-NVMe, and 32K for NFS,
CIFS, and iSCSI).  In addition, these I/Os depend on the throughput
value, not on IOPS. Therefore, they can be modeled by using the
throughput value (IOPS \* I/O size) and setting them to sequential I/O
in Fusion. For example, in sequential FC reads, a value of 10,000 256KB
FC read IOPS becomes 2500 MB/sec.

**How do I account for metadata operations in my workloads?**  
To account for metadata operations, create a separate custom workload
and model metadata operations as 4K random reads but reduce the amount
by 20%.  For example: if you a workload with 10,000 metadata IOPS,
create a custom workload in Fusion consisting of the following:

-   8,000 IOPS
-   100% random reads
-   4K Block Size
-   0.1 TiB of capacity

**For a Home Directory workload, what's the difference between a light, medium, and heavy user?**  
Below is the performance profile for each of these user types:

-   Light is 4 operations per second per user
-   Medium is 5 operations per second per user
-   Heavy is 8 operations per second per user

**Why do I receive the "No Performance Data Available" error when importing workload data from Active IQ and why is the workload imported with 25/25/25/25 I/O Mixture?**  
There are few reasons this occurs, including:

-   The customer's system may not be configured to include performance
    data with AutoSupport.  To fix this, have your customer enable
    the autosupport.performance_data.enable option. 
-   The AutoSupport data may be truncated during transmission back to
    NetApp.  See KB article 1005262 for more information on fixing this
    issue.
-   The customer's system may be running a non-standard version of
    ONTAP, system, or hardware configuration which prevents proper
    parsing of the AutoSupport data.  In these cases, please contact
    NetApp support for assistance.

**What is the CIFS (SMB2) home directory op-mix distribution?**  
The operations-mix (op-mix) distribution is based on the Netmist Lite
version of home directory mix. The table below lists the file operations
and provides their respective percentage breakdowns.

| **CIFS (SMB2) Operation** &nbsp;&nbsp; | &nbsp;&nbsp; **Percentage of Workload** |
| :---                      | :---:|
| GetAttr                   |  14% |
| SetAttr                   |   &nbsp; 2% |
| Read                      |  15% |
| Write                     |   &nbsp; 6% |
| Open/Close                |  61% |
| Other                     |  &nbsp; 1% |

**How can I get data from an existing NetApp storage environment into Fusion?**  
Fusion currently supports importing AutoSupport data from Active IQ as
well as host performance data captured using OneCollect.

**Where can I get more information about using OneCollect with Fusion?**  
Please refer to the OneCollect Tech Report: <https://fieldportal.netapp.com/content/906246>

**How can I get data from an existing NetApp storage environment if I cannot use AutoSupport or OneCollect?**  
You will have to gather data manually to enter into a custom workload.
See the table below for information on how to do that.

| **Parameter**                         | **Command**                | **Counter**                                |
| :---                                  | :---                       | :---                                       |
| ONTAP Version                         | version                    |                                            |
|    |    |
| Aggregate to Raid Group mapping       | sysconfig -r               |                                            |
|    |    |
| Raid Group size                       | aggr status -v             | raidsize                                   |
|    |    |
| Raid type                             | aggr status -v             | raidtype                                   |
|    |    |
| Volume to aggregate mapping           | aggr status -v             | Volumes and Plex                           |
|    |    |
| Platform                              | sysconfig -av              | Model Name                                 |
|    |    |
| List of aggregates and type           | aggr status -v             |                                            |
|    |    |
| List of volumes                       | stats stop -I perfstat_vol | volume                                     |
|    |    |
| Root volume                           | vol status -v              | root                                       |
|    |    |
| Disk type (Size, RPM, DriveType)      | sysconfig -r               | Type, RPM, Used                            |
|    |    |
| Number of disks                       | sysconfig -r               | RAID Disk                                  |
|    |    |
| Hybrid Aggregates                     | sysconfig -r               | Type                                       |
|    |    |
| Shelf Type                            | sysconfig -av              |                                            |
|    |    |
| Spares                                | sysconfig -r               | spare disks                                |
|    |    |
| Flash Cache, number, and type.        | sysconfig -av              | Performance Accelerator Module/Flash Cache |
|    |    |
| HA Pair                               | sysconfig -av, /etc/rc     | System ID, partner ID, hostname            |
|    |    |
| Throughput (Volume level)             | stats stop -I perfstat_vol | <protocol>\_read_ops + <protocol>\_write_ops|
|    |    |
| Random IO Size                        | stats stop -I perfstat\_\<protocol>|max(<protocol>:<protocol>:<protocol>\_read_size_histo.0-511, .512-1023,.1K-2047,.2K-4095,.4K-8191,.8K-16383, .16K-32767)|
|   |            | and                                                        |
|   |            | max(<protocol>:<protocol>:<protocol>\_write_size_histo.0-511,.512-1023,.1K-2047,.2K-4095,.4K-8191,.8K-16383,.16K-32767) |
|    |    |
| Sequential IO Size | stats stop -I perfstat\_\<protocol>  |max(<protocol>:<protocol>:<protocol>\_read_size_histo.32K-65535,.64K-131071,.131071) |
|                      |                      | and                                        |
|                      |                      | max(<protocol>:<protocol>:<protocol>\_write_size_histo.32K-65535,.64K-131071) |
|    |    |
| Read WSS | wafl_susp -w , wafl bufstats  | ((blocks read by type for normal file in level 1 + (bufCount\*0.4)) * protocolRndRdOps/totalRndRdOps) * 510 * 4 |
|    |    |
| Write WSS | wafl_susp -w , wafl bufstats  | ((blocks read by type for normal file in level 1 + (bufCount*0.4)) * protocolRndWrOps/totalRndWrOps) * 510 * 4 * (Max random write percentage)|
| Workload - Aggregate Mapping (Protocol -Vol mapping and Vol to Aggr Mapping)| stats stop -I  perfstat_vol | Protocol to Vol mapping / Vol - aggr mapping |
|    |    |
| Read Latency        | stats stop -I perfstat_vol | max(<protocol> random read % * avg_latency) in selected iterations |
|    |    |
| Data Set Size        | df                   | max(used) in selected iterations |
|    |    |
| IO Type % (Random read %, Random write %, Seq read %, Seq write %)| stats stop -I perfstat\_\<protocol> | total=sum of \<protocol>:\<protocol>:\<protocol>\_read/write\_size\_histo.<size>                                |
|   |   | if (total \> 0.0) |
|   |   | seqRd% = seqRd * 100 / total seqWr% = seqWr * 100 / total rndRd% = rndRd * 100 / total rndWr% = 100.0 - seqRd% - seqWr% - rndRd% |
|    |    |
| Total Ops in Volume  | stats stop -I perstat_vol | total_ops            |
|    |    |
| Protocol Ops in Volume | stats stop -I perfstat_vol | \<protocol\>\_read_ops + \<protocol\>\_write_ops |
|    |    |
| Metadata ops         | stats stop -I perfstat_vol\_\<protocol> | protocol\_op\_percent or \<optype\>\_percent  |
|    |    |
| SnapMirror           | snapmirror status -l | SnapMirror is ON or OFF                  |

**How can I get data from an existing third-party storage environment into Fusion?**.
The OneCollect tool can gather performance data from VMware
environments. Start the data gathering session from within Fusion to get
a unique token to identify the project.

**Exactly what workload is imported into Fusion?**  
Fusion imports the front-end protocol-based workload from the host.
Fusion does not import back-end workloads such as SnapMirror, SnapVault,
deduplication, compression, antivirus, or deswizzling. 

### Sizing Results

**What is the default sort order of the sizing results?**  
For AFF and FAS solutions, the first result listed in the sizing
recommendations should be the lowest cost solution with the fewest
cluster nodes.

**What does "Quote Ready" mean?**  
A "Quote Ready" sizing recommendation matches configuration known to
be configurable in the NetApp Quoting System.

**What are Express Packs and why do the show in sizing results?**  
Express Packs are predefined configurations with predetermined pricing
that can flexibly scale.  Express Packs are designed to accelerate the
opportunity-to-quote process. Express Packs will appear in sizing
results when a recommendation matches an Express Pack configuration
available in the NetApp Quoting System.

**How are Express Packs recommended?**  
After calculating a configuration to meet the workload requirements, the
sizing engine uses an algorithm that compares the calculated
configuration to available Express Pack definitions.  If increasing the
number of drives results in a match with the Express Pack, then sizing
engine will use it and suggest Expansion Packs for additional capacity.

**Does "% system utilization" mean the same thing as CPU busy?**  
No. Fusion uses platform test data to calculate a cost per IOP to
various I/O subsystems within ONTAP.  Not only CPU, but also RAID
processing, WAFL, network processing, and consistency point processing,
are considered among other items. System utilization is the current
workload's percentage of use before the first bottleneck is reached.
This bottleneck could be CPU, RAID, WAFL, or other.  System utilization
is the percent of way to knee of the curve of the latency vs throughput
graph.

**Why does maximum throughput not correspond to the maximum throughput I
can achieve in Size and Recommend before it adds nodes? What's the
latency at maximum throughput?**  

- **Short Answer:** For the Size and Recommend and Tech Refresh workflows,
the latency is an input used as a constraint. For maximum throughput,
the latency in the inputs is ignored.
- **Long Answer:** The throughputs given in a Size and Recommend and Tech
Refresh workflow is constrained by the latencies entered in those same
workloads. In other words, if you imagine it as a throughput versus
latency graph, the input latencies act as a "cut-off" point at which
more nodes need to be added to the configuration to meet the input
criteria. The maximum throughput shown throughout Fusion is at either
the knee of the curve of the throughput versus latency graph or the
maximum measured latency for that controller and protocol, which is
often a much higher latency than the desired latency input in each
workload.

**Why don't I see higher max throughput or fewer HDDs when adding Flash Cache™ or Flash Pool™?**  
There are several possible reasons for this:

-   The solution is capacity bound. The number of HDDs in your sizing
    might have been determined by capacity. Flash Cache and Flash Pool
    will reduce the HDD count requirement only when the HDD count is
    determined by the throughput requirement. If the HDD count is
    determined by capacity, no reduction is possible with Flash Cache or
    Flash Pool.
-   The workload is write intensive. Your workload might contain a large
    write component. Flash Cache does not reduce disk operations for
    write-intensive workloads.
-   The workload is sequential-read intensive. Fusion supports sizing
    with Flash Cache in default caching mode only. In the default
    caching mode, sequentially read blocks are not cached. Therefore, in
    the default mode, Flash Cache is not expected to reduce latency or
    spindle count for sequential-read workloads.

**When SnapMirror is enabled, why does my sizing seem to need more disks or controllers than I would estimate?**  
SnapMirror consumes more resources. It is a workload that cannot be
cached (WSS is in essence 100%), which makes it very hard on the disks.
In addition, often the daily change rate is set too high. On a large
dataset, the default 10% daily change rate is usually much too high. On
a 100TB capacity aggregate, for example, that change rate would create
more than 10TB of net changes in every 24-hour period

**Are there limits on the number of cluster nodes in sizing recommendation?**  
Yes.  For AFF and FAS, the cluster size is limited to 24 nodes for
workloads using NAS protocols and 12 nodes for workloads using SAN
protocols.  These are the platform cluster size limits.

**Why do sizing results for AFF All SAN Array not match sizing results for unified AFF running the same workloads?**  
Sizing results for All SAN Array (ASA) reflect a default sizing
parameter that does not allow performance degradation during a HA
failover event.  This means that the sizing reserves 50% performance
headroom on each controller to handle such events.  This change was made
for All SAN Array to target Tier 1 SAN applications.

### Manual Design (Reverse Sizing)

**Are there any reasons not to use reverse sizing in a pre-sales context?**  
Yes. Size and Recomend tries to recommend all the hardware you would
need to sell. In Manual Design, Fusion assumes that you already have a
known configuration and therefore doesn't report on all the hardware
components that you might need.

**Why does reverse sizing for maximum throughput ask for the workload details?**  
The maximum throughput that a system can deliver varies with a given
combination of workload and hardware. For example, the I/O mix and size
can strongly affect the maximum throughput.

**Why is the system utilization value not 100% for the maximum throughput?**  
The system utilization and latency values calculated in a reverse sizing
may correspond to the maximum throughput value. The system utilization
and latency values are calculated from the given workload. The maximum
throughput value is calculated by pushing those workloads higher and
higher until a bottleneck is hit. Therefore, the system utilization and
latency would be different values at the maximum throughput level.

**How do I adjust the Raid Group size?**  
Fusion supports RAID 1,5,6,10 and DDP groups. The number of drives per
group will vary depending on RAID selection, RAID 5 adjusts up to 9
disks and RAID 6 will adjust up to10 disks per group. RAID 10 and DDP
will allow up to the maximum number of disks supported by the
configuration selected. The number of disks allowed per group size is
what was tested with. Express Packs

**What do the Configured and Single System lines represent in the
performance charts?**  
The Configured System line represents the performance of the selected
system with the number of configured drives. Single System line is the
maximum performance that can be achieved for the selected platform
configuration factoring the sizing options: Workload, Host Interface
Card, and Drive type. The difference between the Configured and Single
System lines shows the headroom available to add more drives to the
configuration until you reach the maximum system performance.


### Register for a Netapp Support Site Account

In order to access NetApp training you must have a NetApp Support Site ID associated with your IBM email address. Following are instructions for creating a NSS account:

1. 	Go to [http://support.netapp.com](http://support.netapp.com) and click **Register Now** or go
directly to the registration page at [http://support.netapp.com/eservice/public/now.do](http://support.netapp.com/eservice/public/now.do).  

2. On the Welcome to Account Registration page, enter your IBM email address.  The approval process will reject personal email addresses.  

3. On the second page, select the third option, *NetApp Reseller / Service Provider
/ System Integrator / Partner*. In the boxes to the right of this selection, My company is one (or both) of the following, check the box for *Reseller, Service Provider or System Integrator*.  

4. For the Company (licensee) field, enter company name and address (IBM Global Services).

5. On the final page, create a login name, password and password hint. Click the submit button to start the approval process. You should be granted access within 48 hours.

6. After you receive a confirmation via email that your account has been created, log-in to the [NetApp Learning Center](https://learningcenter.netapp.com) to activate your account.

If you need any support, please contact NetApp Support Site administration [nssadmin@netapp.com](mailto:nssadmin@netapp.com)
