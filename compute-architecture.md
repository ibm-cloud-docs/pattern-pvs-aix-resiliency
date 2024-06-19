---

copyright:
  years: 2024
lastupdated: "2024-06-19"

subcollection: <repo-name>

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}




| **Architecture decision**                   | **Requirement**                                               | **Alternatives**  | **Decision**                 | **Rationale**                                                                                                                                                                                                          |
|---------------------------------------------|---------------------------------------------------------------|-------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute: Local high availability workloads  | Provide compute for LPARs supporting local high availability  | Not Applicable    | Power Virtual Server LPAR(s) | Local High Availability via PowerHA System Mirror, resources should be equal to support workloads in a failover situation.                                                                                             |
| Compute:  GRS Controllers                   | Provide compute for replication components.                   | Not Applicable    | Power Virtual Server LPAR(s) | Compute for GRS controller workloads                                                                                                                                                                                   |
| Compute: Edge and Management VPCs           | Provide compute for workloads in the edge and management VPCs | Not Applicable    | Virtual Servers for VPC      | Virtual servers for workloads in the Edge and Management VPCs                                                                                                                                                          |
| Compute: Disaster Recovery Workloads        | Provide compute for LPARs supporting disaster recovery.       | Not Applicable    | Power Virtual Server LPAR(s) | Target an environment to match specific workload requirements. Consider Shared Processor Pool configuration as an option to provision the disaster recovery environment at the lowest possible compute configuration.  |
{: caption="Table 2. Architecture decisions for compute" caption-side="bottom"}

