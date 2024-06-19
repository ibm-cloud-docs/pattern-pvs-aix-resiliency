---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: pattern-pvs-aix-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for storage
{: #storage-decisions}




| **Architecture decision**                     | **Requirement**                                                                                     | **Alternatives**                                                | **Decision**                                                                      | **Rationale**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|-----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Primary Storage for local HA workloads        | Provide highly available storage for local high availability.                                       | Flash Storage from IBM FS9000 series devices Tier 1 - 10 IOPS   | Matches production workload storage tier                                          | LPARS share the same local storage                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Primary Storage for secondary site workloads  | Provide highly available storage for disaster recovery workloads using Global Replication Services  | Match production workload storage tiers                         | Match production workload storage Tiers                                           | Global Replication Services (GRS) does not support mixed Tiers for the same environment. Storage needs to like to like – Tier 1 to Tier 1, Tier 3 to Tier 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Backup Storage                                | Provide highly available storage for backups                                                        | Local Storage COS Managed Service                               | Local storage + COS Storage is deployed as part of the managed backup solution.   | Local + COS storage for mksysb images Storage is deployed as part of the managed backup solution.  Secure Automated Backup with Compass is a fully managed backup solution for AIX and Linux workloads.  backup servers are preconfigured in data centers and replicated to another region.  2 copies of data, one in each MZR, service is setup in pairs; validate Secure Automated Backup with Compass datacenter pairings match your workload locations.  For sizing and configuration reach out to Cobalt Iron via [support](http://support.cobaltiron.com/) https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass\#about |
| Long Term/Backup Storage                      | Provide storage for long term retention                                                             |                                                                 | Managed backup solution storage                                                   | Storage is deployed as part of the managed backup solution.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |


| Architecture decision| Requirement| Option | Decision| Rationale|
|---|---|---|---|---|
|Primary Storage| text | text | text | text |
|Backup Storage| text | text | text | text |
|Archive Storage| text | text | text | text |
|Data Migration| text | text | text | text |
{: caption="Table 1. Architecture decisions for storage" caption-side="bottom"}
