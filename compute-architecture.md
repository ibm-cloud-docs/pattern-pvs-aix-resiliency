---

copyright:
  years: 2024
lastupdated: "2024-06-28"

subcollection: pattern-pvs-aix-resiliency

keywords: compute, architecture compute

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}

| Architecture decision | Requirement | Decision | Rationale |
|---------------------------------------------|---------------------------------------------------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute: Local high availability workloads  | Provide compute for LPARs supporting local high availability  | Power Virtual Server LPARs | Local high availability by using the PowerHA System Mirror. Resources should be equal to support workloads in a failover situation. |
| Compute:  GRS controllers | Provide compute for replication components. | Power Virtual Server LPAR(s) | Compute for GRS controller workloads |
| Compute: Edge and management VPCs | Provide compute for workloads in the edge and management VPCs | Virtual Servers for VPC | Virtual servers for workloads in the edge and management VPCs |
| Compute: Disaster recovery workloads        | Provide compute for LPARs supporting disaster recovery. | Power Virtual Server LPAR(s) | Target an environment to match specific workload requirements. \n \n Consider using the shared processor pool configuration as an option to provision the disaster recovery environment at the lowest possible compute configuration.  |
{: caption="Table 1. Architecture decisions for compute" caption-side="bottom"}
