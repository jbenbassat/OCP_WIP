# NetApp ONTAP Comparison

??? abstract "Document Control"

    !!! info "Page Owner: Troy Hess (Solutions Engineer - IBM and RedHat Alliances - NetApp)"

    !!! info "User Story"
        **As a**: Solutions Architect

        **I want to**: understand which ONTAP system is suitable for my deployment model and environment

        **So that**: I can position the most suitable ONTAP system

    !!! info "Status"
        - [ ] Structure
        - [X] Draft
        - [ ] Reviewed:
        - [ ] Ready
        - [ ] Published: _Add the public URL link here once published_

    !!! info "Classification"
        - [ ] IBM Confidential
        - [X] Public

## NetApp ONTAP Comparisons

|  | AFF | FAS | Cloud Volumes ONTAP (CVO) | ONTAP Select (OTS) |
| --- | :---: | :---: | :---: | :---: |
| Can IBM Sell? | Yes | Yes | Yes | Yes |
| Can IBM Support? | Yes | Yes | Yes | Yes |
| NetApp portfolio | ONTAP | ONTAP | ONTAP | ONTAP |
| Offering Type | CapEx/OpEx | CapEx/OpEx | BYOL or Hourly | CapEx SW License |
| GTS Managed | Offering | Offering | Optional | Optional |
| Deployment | On-Prem | On-Prem | AWS, Azure, GCP | IBM Cloud, On-Prem |
| System Type | HW Appliance | HW Appliance | SDS | SDS |
| Required HW/SW | NetApp HW + ONTAP  | NetApp HW + ONTAP  | Servers in AWS, Azure, GCP | Commodity  servers, KVM, vSphere |
| License type | Bundle based + options | Bundle based + options | All Included | All Included |
| Underlying Storage/Media | SSD | SDD/HDD | Cloud Storage Options | DAS based media (SSD included) |
| Supported by NetApp Trident? | Yes | Yes | Yes | Yes |
| DR/Replication<sup>1</sup> | To/From Any ONTAP Listed Here | To/From Any ONTAP Listed Here | To/From Any ONTAP Listed Here | To/From Any ONTAP Listed Here |
| Protoclos | FCP, NFS, CIFS, iSCSI | FCP, NFS, CIFS, iSCSI | iSCSI, NFS, CIFS | iSCSI, NFS, CIFS |
| More Info | [AFF All Flash Storage Arrays](https://www.netapp.com/data-storage/aff-a-series/) | [FAS Hybrid Storage Arrays](https://www.netapp.com/data-storage/fas/) | [Cloud Volumes ONTAP](https://www.netapp.com/cloud-services/cloud-volumes-ontap/) | [ONTAP Select Software Defined Storage ](https://www.netapp.com/data-management/software-defined-storage-ontap-select/) |

*<sup>1</sup> Within the same major release*
