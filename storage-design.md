---

copyright:
  years: 2024
lastupdated: "2024-06-19"

subcollection: <repo-name>

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Storage design
{: #storage-design}


The requirements for the compute aspect for the Resiliency for PowerVS for AIX workloads pattern focus on:

-   The storage required to support backup activities.

-   The storage required to support replication activities for disaster recovery.

## 

## Storage Considerations for Backups

**Backup Method: Secure Automated Backup with Compass + MKSYSB**

-   Compass backup servers are preconfigured in data centers and are also replicated across to the other regions.

-   By default, there are 2 copies of data - one in each MZR, service is setup in pairs

-   Validate Secure Automated Backup with Compass is offered in the datacenter where workloads are deployed your workloads. datacenter pairings.

-   Storage sizing considerations: workload and data volumes (source total, unstructured and structured), change and growth rates, retention policies.

-   Engage Cobalt Iron for sizing - [https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass\#about](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass#about)

-   Ensure that there is sufficient file system space to hold the produced mksysb image. Generally, 10 to 15 GB is sufficient depending on additional non-AIX data added to the rootvg. Consider Cloud Object storage for mksysb images.

## Storage Considerations for High Availability

**High Availability Method: PowerHA Standard for Local HA Cluster**

-   Clustered workload LPARS share the same volume group, storage tier and size will be determined by the workload requirements.

-   LUN for Cluster aware AIX (CAA) repository, 1GB recommended.

## Storage Considerations for Disaster Recovery

**Disaster Recovery Method: Global Replication Services (GRS)**

Control LPARS

-   Each GRS control LPAR requires 300gb Tier 1 storage in the primary region and one in the secondary region.

Disaster Recovery Workloads

-   Global Replication Services (GRS) does not support mixed Tiers for the same environment. Storage in the disaster recovery site will be the same Tier as the storage deployed for the primary workload (primary site workload is Tier 1 then secondary site workload storage will be Tier 1, primary workload is Tier 3 then secondary site workload storage will be Tier 3).
