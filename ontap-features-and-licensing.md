# ONTAP Features and Licensing

??? abstract "Document Control"

    !!! info "Page Owner: Troy Hess (Solutions Engineer - IBM and RedHat Alliances - NetApp)"

    !!! info "User Story"
        **As a**: Solutions Architect

        **I want to**: Understand what features are available for each ONTAP system and what licenses may be required

        **So that**: I can position the right ONTAP system

    !!! info "Status"
        - [ ] Structure
        - [X] Draft
        - [ ] Reviewed:
        - [ ] Ready
        - [ ] Published: _Add the public URL link here once published_

    !!! info "Classification"
        - [ ] IBM Confidential
        - [X] Public

## NetApp ONTAP Software Features & Licensing Model

ONTAP is the software operating system for NetApp Fabric Attached Storage (FAS) and All-Flash FAS (AFF)Following is a brief description of NetApp ONTAP software features, how NetApp systems with ONTAP are licensed, and the software bundles available with ONTAP.  ***For more detailed information on ONTAP software licensing or pricing, please contact your NetApp sales representative.***

## ONTAP Software Features

The table below highlights the many features and benefits that are unique to ONTAP software.

|	|**Function**	|**Benefits**|
|---	|---	|---|
|**Balance Placement**	|Automates loading of new workloads onto a cluster	|Increase cluster utilization by performance adding by a new workload to the optimal node|
|**Data compaction**	|Packs more data into each storage block for greater data reduction	|Works with compression to reduce the amount of storage you need to purchase and operate|
|**Data compression**	|Provides transparent inline and postprocess data compression for data reduction	|Reduces the amount of storage you need to purchase and maintain|
|**Deduplication**	|Performs general-purpose deduplication for removal of redundant data	|Reduces the amount of storage you need to purchase and maintain|
|**Flash Pool™**	|Creates a mixed-media storage pool by using SSDs and HDDs	|Increases the performance and efficiency of HDD pools with flash acceleration|
|**FlexClone®**	|Instantaneously creates file, LUN, and volume clones without requiring additional storage"	|Saves you time in testing and development and increases your storage capacity|
|**FlexGroup**	|Enable a single namespace to scale up to 20PB and 400 billion files	|Support compute-intensive workload and data repositories require a massive NAS container while maintaining consistent high performance and resiliency|
|**FlexVol®**	|Creates flexibly sized volumes across a large pool of disks and one or more RAID groups	|Enables storage systems to be used at maximum efficiency and reduces hardware investment|
|**Headroom**	|Provides visibility of performance capacity available for deploying new workloads on storage nodes	|Simplifies management and enables more effective provisioning of new workloads to the optimal node|
|**MetroCluster**	|Combines array-based clustering with synchronous mirroring to deliver continuous availability and zero data loss; up to 300km distance between nodes	|Maintains business continuity for critical enterprise applications and workloads in the event of a data center disaster|
|**QoS (adaptive)**	|Creates a performance limit for a storage workload	|Can prevent one workload or tenant from affecting the performance of another in multiworkload and multitenant environments|
|**RAID DP® and RAID-TEC™**	|Provides a double-parity and triple parity RAID 6 implementation that prevents data loss when two or three drives fail	|Protects your data without the performance impact of other RAID 6 implementations; reduces risks during long rebuilds of large-capacity HDDs|
|**SnapCenter® and SnapManager®**	|Provides host-based data management of NetApp storage for databases and business applications	|Offers application-aware backup and disaster recovery; automates error-free data restores|
|**SnapLock**	|Provides WORM file-level locking	|Supports regulatory compliance and organizational data retention requirements|
|**SnapMirror**	|Enables automatic, incremental asynchronous data replication between systems	|Provides you with flexibility and efficiency when mirroring for data distribution and disaster recovery|
|**SnapRestore®** |Rapidly restores single files, directories, or entire LUNs and volumes from any Snapshot copy backup |Instantaneously recovers files, databases, and complete volumes from your backup|
|**Snapshot**	|Makes incremental data-in-place, point-in-time copies of a LUN or volume with minimal performance impact"	|Enables you to create frequent space-efficient backups with no disruption to data traffic|
|**Volume encryption**	|Provides data-at-rest encryption that is built into ONTAP	|Lets you easily and efficiency protect your-at-last data by encrypting any volume on an AFF or FAS system; no special encrypting disks are required|


s## ONTAP Licensing Model

ONTAP 9.x licensed software entitlement is to a hardware product’s unique serial number which is associated with a specific customer for the "Life of Controller", meaning ONTAP and ONTAP features are licensed to a specific controller for the original purchaser only.

Platforms sold with ONTAP 9.x are priced based on storage capacity configured with the system. Both ONTAP and the feature software is sold based on storage capacity, however the license keys are still locked to the controller serial number and do not control capacity usage.
ONTAP Software can be ordered with the following bundles:

* **Core Bundle** *(included with all ONTAP systems)*
	* CIFS/SMB
	* FCP
	* iSCSI
	* NFS
	* FlexClone
	* SnapRestore
* **Data Protection Bundle**
	* SnapCenter
	* SnapManager
	* SnapMirror
	* SnapVault
	* SnapMirror Synchronous
* **Security & Compliance Bundle**
	* Multi-tenant Key Management
	* SnapLock
	* Hybrid Cloud:
	* Fabric Pool Subscription
* **Premium Software Bundle** - Includes all protocols and features from the Core, Data Protection and Security & Compliance Bundles.

**\* NOTE:** NetApp includes **Volume Encryption** and **Data-at-Rest Encryption** free-of-charge, however, certain countries require special authorizations, permits, or licenses to import, export, re-export or use this software. For example, a state license for importing encryption equipment is required to import ONTAP 9.1 (or later) with NetApp Volume Encryption into Member States of the Eurasian Economic Union: Russia, Belarus, Kazakhstan, Armenia, and Kyrgyzstan. For countries on the "Restricted" list, NetApp offers a **NODAR** (NO Data-At-Rest) version of ONTAP. If you have a question about which countries are restricted, or if you need the system installed with a NODAR version of ONTAP please consult with your NetApp Representative.

**You can find more information about ONTAP Software [here](https://www.netapp.com/pdf.html?item=/media/7413-ds-3231.pdf).**
