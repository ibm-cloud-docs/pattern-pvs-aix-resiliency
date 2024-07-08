---

copyright:
  years: 2024
lastupdated: "2024-07-08"

subcollection: pattern-pvs-aix-resiliency

keywords: compute, architecture compute

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}

| Architecture decision | Requirement | Decision | Rationale |
|---------------------------------------------|---------------------------------------------------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute: Local high availability workloads  | Provide compute for Logical Partitions (LPARS) supporting local high availability  | Power Virtual Server LPARs | To achieve local high availability, utilize PowerHA SystemMirror. Ensure that the resources can handle workloads during failover scenarios. |
| Compute:  Global Replication Service (GRS) controllers | Provide compute for replication components. | Power Virtual Server LPARs | The compute resources are used for GRS controller workloads. |
| Compute: Edge and management VPCs | Provide compute for workloads in the edge and management VPCs | Virtual Servers for VPC | Deploy virtual servers to handle workloads in both the edge and management VPCs. |
| Compute: Disaster recovery workloads        | Provide compute for LPARs supporting disaster recovery. | Power Virtual Server LPARs | Target an environment to match specific workload requirements. \n \n For your disaster recovery environment, consider leveraging shared processor pools to manage CPU resources efficiently. This approach allows you to allocate compute capacity flexibly while ensuring compliance and minimizing costs.  |
{: caption="Table 1. Architecture decisions for compute" caption-side="bottom"}
