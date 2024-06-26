---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: pattern-pvs-aix-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}




| **Architecture decision**                   | **Requirement**                                               | **Decision**                 | **Rationale**                                                                                                                                                                                                          |
|---------------------------------------------|---------------------------------------------------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute: Local high availability workloads  | Provide compute for LPARs supporting local high availability  | Power Virtual Server LPAR(s) | Local High Availability via PowerHA System Mirror, resources should be equal to support workloads in a failover situation.                                                                                             |
| Compute:  GRS Controllers                   | Provide compute for replication components.                   | Power Virtual Server LPAR(s) | Compute for GRS controller workloads                                                                                                                                                                                   |
| Compute: Edge and Management VPCs           | Provide compute for workloads in the edge and management VPCs | Virtual Servers for VPC      | Virtual servers for workloads in the Edge and Management VPCs                                                                                                                                                          |
| Compute: Disaster Recovery Workloads        | Provide compute for LPARs supporting disaster recovery.       | Power Virtual Server LPAR(s) | - Target an environment to match specific workload requirements. \n \n - Consider Shared Processor Pool configuration as an option to provision the disaster recovery environment at the lowest possible compute configuration.  |
{: caption="Table 1. Architecture decisions for compute" caption-side="bottom"}
