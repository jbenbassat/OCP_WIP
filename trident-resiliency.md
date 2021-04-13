# NetApp DR for OpenShift/Kubernetes


??? abstract "Document Control"

    !!! info "Page Owner: Jamal Boudi (Solutions Architect - NetApp)"

    !!! info "User Story"
        **As a**: Solutions Architect

        **I want to**: understand NetApp's persistent storage DR topologies

        **So that**: I can properly incorporate NetApp's persistent storage and ONTAP technologies into OpenShift/Kubernetes based solution

    !!! info "Status"
        - [ ] Structure
        - [X] Draft
        - [ ] Reviewed:
        - [ ] Ready
        - [ ] Published: _Add the public URL link here once published_

    !!! info "Classification"
        - [X] IBM Confidential
        - [ ] Public


## Trident Disaster Recovery Scenarios

The following section outlines three most common procedures to recover Trident with ONTAP configurations when there is only storage failure. Kubernetes has its own procedures for recovering Kubernetes clusters, which is out of this scope.

In Trident version prior to 19.07, Trident metadata was stored in ***etcd*** volumes on the storage cluster, so it was necessary to protect those volumes in order to preserve the Trident metadata.  
Beginning in version 19.07 Trident adopted Custom Resource Definitions (CRDs) to maintain its stateful information within the Kubernetes cluster, eliminating the need for etcd volumes on the storage.  
The following procedures contain extra steps listed in italics that apply ONLY to Trident versions prior to 19.04 which utilize the external etcd files.

## Three Outlined Scenarios:
There are three recovery scenarios that will depend on the configuration of the cluster.

![](./img/Trident_and_3_ONTAP_Configs.png)

**Storage Failure Scenario 1: MetroCluster with mirrored aggregates**

In a MetroCluster switch-over event, the SVM name(s) will change but the data LIF remains the same therefore K8s will be able to access existing PVs in the fail-over site, however, new PVs cannot be created until the back-end is updated with new SVM name(s)
- Create new backend.json file with the new SVM name (this must be done for each Trident install with multiple K8s clusters)
- Update Trident with new back-end information

**Storage Failure Scenario 2: SVM SnapMirror**

There are two options with SVM SnapMirror:
1) **Identity-preserve=“TRUE”** – This method replicates the entire SVM configuration (IP addresses and data LIFs do not change)
2) **Identity-preserve=“FALSE”** – This method replicates only the volumes and authentication/authorization configurations (IP addresses and data LIFs will change)


**Option 1:** Identity-preserve TRUE (SVM name changes, data LIF(s) remain the same):

Since IPs don’t change, Trident will know about existing volumes, but the backend must be updated with the new SVM name to ensure new PV requests will work.

- Break the SnapMirror relationship(s) making the SnapMirror destination volume(s) active (read/write)
- Uninstall Trident by doing a **tridentctl uninstall -n** command. Do not use the “-a” option when uninstalling, as it will completely remove Trident [1]

```csharp
tridentctl uninstall -n <trident-namespace>
```

- Update the backend.json file present in the “setup” dir to reflect the new SVM name which contains Trident’s etcd volume [1]
- Install trident by running a **tridentctl install -n** command [1]
```csharp
tridentctl install -n <same-namespace> --volume-name <name-of-Trident-etcd-volume>
```

- Create new backend.json file(s) for each backend on the secondary cluster with the SVM name (this must be done for each Trident install with multiple K8s clusters)
- Update Trident with new backend information

[1] To be done if Trident’s etcd volume is also mirrored to a secondary storage cluster (Recommended)


**Option 2:** Identity-preserve FALSE (SVM and data LIF(s) change):

In this scenario the backend must be updated so Trident is aware of the new SVM name and Data LIF IP changes, and existing Trident volumes must be imported.

- Break the SnapMirror relationship(s) making the SnapMirror destination volume(s) active (read/write)
- Uninstall Trident by doing a **tridentctl uninstall -n** command. Do not use the “-a” option when uninstalling, as it will completely remove Trident [1]
```csharp
tridentctl uninstall -n <trident-namespace>
```

- Update the backend.json file present in the “setup” dir to reflect the new SVM name and data LIF IP which contains Trident’s etcd volume [1]
- Install trident by running a **tridentctl install -n** command [1]
```csharp
tridentctl install -n <same-namespace> --volume-name <name-of-Trident-etcd-volume>
```

- Create new backend.json file(s) for each backend on the secondary cluster with the SVM name AND data LIF IP (this must be done for each Trident install with multiple K8s clusters)
- Update Trident with new backend information
- Import volume(s) into Trident [2]
- Delete old PVCs [2]

[1]  To be done if Trident’s etcd volume is also mirrored to a secondary storage cluster (Recommended)

[2]  These steps may require scripting



**Storage Failure Scenario 3: Individual FlexVol SnapMirror**

This configuration uses traditional Data Protection SnapMirror for replication of individual volumes.

- Break the SnapMirror relationship(s) making the SnapMirror destination volume(s) active (read/write)
- Uninstall Trident by doing a **tridentctl uninstall -n** command. Do not use the “-a” option when uninstalling, as it will completely remove Trident [1]
```csharp
tridentctl uninstall -n <trident-namespace>
```

- Update the backend.json file present in the “setup” dir to reflect the new SVM name which contains Trident’s etcd volume [1]
- Install trident by running a **tridentctl install -n** command [1]
```csharp
tridentctl install -n <same-namespace> --volume-name <name-of-Trident-etcd-volume>
```

- Create new backend.json file(s) on the secondary cluster with the SVM name and data LIF IP (this must be done for each Trident install with multiple K8s clusters)
- Update Trident with new backend information
- Import volume(s) into Trident [2]
- Delete old PVCs [2]

[1]  To be done if Trident’s etcd volume is also mirrored to a secondary storage cluster (Recommended)

[2]  These steps may require scripting

If Kubernetes Cluster is lost and Trident is pre-v19.07 (etcd files used), proceed with the following:
- Bring up Kubernetes replica following established procedures
- Reinstall Trident in the Kubernetes replica adding the flag “**-volume_name=<original_etcd_vol>**”, where <original_etcd_vol> references the original etcd configuration volume from the original cluster
- Follow the procedures for the specific configuration that applies from the previously described scenarios.


Link to [NetApp Trident 19.07 Backup and Disaster Recovery Page](https://netapp-trident.readthedocs.io/en/stable-v19.07/dag/kubernetes/backup_disaster_recovery.html)
