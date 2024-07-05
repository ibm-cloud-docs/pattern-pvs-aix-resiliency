---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: pattern-pvs-aix-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Storage design
{: #storage-design}



Ensuring storage resiliency for {{site.data.keyword.powerSysFull}} for AIX involves having sufficient storage capacity to accommodate high availability, backup, and disaster recovery workloads. End users must carefully consider and understand the storage types and requirements that need to be met.

The requirements for the compute aspect for the Resiliency for PowerVS for AIX workloads pattern focus on:

-   The storage required to support backup activities.

-   The storage required to support replication activities for disaster recovery.


## Storage Considerations for Backups
{: #storage-considerations}

The backup method chosen for this pattern was **Secure Automated Backup with Compass + Make System Backup (MKSYSB)** . There are several storage considerations when it comes to sizing and configuring this method.

Compass backup servers are preconfigured in data centers and are also replicated across other regions. By default, there are two copies of data—one in each MZR (Multi-Zone Region), and the service is set up in pairs. Validate that Secure Automated Backup with Compass is offered in the datacenter where your workloads are deployed. Consider storage sizing considerations, including workload and data volumes (both structured and unstructured), change and growth rates, and retention policies. For sizing guidance, [Engage Cobalt Iron for sizing](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass\#about). Additionally, ensure that there is sufficient file system space to hold the produced mksysb image. Generally, 10 to 15 GB is sufficient, depending on any additional non-AIX data added to the rootvg. You may also want to consider Cloud Object Storage for storing mksysb images .

## Storage Considerations for High Availability
{: #storage-ha-considerations}

The High Availblity method chosen for this pattern was **PowerHA SystemMirror Standard Edition for Local HA Cluster** . This method meets the requirements of local clustering. There are several HA considerations when it comes to storage with this solution. 

In PowerHA SystemMirror, ensure that the clustered workload LPARs share the same volume group. The storage tier and size should be determined based on the workload requirements. Additionally, make sure that the Storage Logical Unit Number (LUN) for the Cluster Aware AIX (CAA) repository is set to the recommended 1GB.

## Storage Considerations for Disaster Recovery
{: #dr-storage-considerations}

The Disaster Recovery method chosen for this pattern was **Global Replication Services (GRS)** . The storage considerations for this pattern concern the contoller lpars and the types of disaster recovery workloads

Please note that each GRS control LPAR requires 300GB of Tier 1 storage in both the primary and secondary regions. Global Replication Services (GRS) does not support mixed tiers within the same environment. Additionally, the storage in the disaster recovery site should match the tier deployed for the primary workload. For instance, if the primary site workload uses Tier 1 storage, the secondary site workload storage should also be Tier 1. Similarly, if the primary workload is on Tier 3 storage, the secondary site workload storage should align with Tier 3.

