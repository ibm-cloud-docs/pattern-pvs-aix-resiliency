---

copyright:
  years: 2024
lastupdated: "2024-07-08"

subcollection: pattern-pvs-aix-resiliency

keywords:

---

# Architecture decisions for resiliency
{: #resiliency-architecture}

The following sections summarize the resiliency architecture decisions for workloads deployed on {{site.data.keyword.vpc_short}} infrastructure.

| Architecture decision | Requirement | Alternatives | Decision | Rationale |
|------|-------|-------|-------|-----|
| Backups | Backup AIX workloads with a managed service | Image capture snapshots and flash copy \n \n Secure Automated Backup with Compass \n \n Veeam \n \n {{site.data.keyword.IBM_notm}} Storage Protect \n \n Falconstor VTL \n \n Bring Your Own Backup | Secure Automated Backup with Compass and Make System Backup (mksysb) | Managed service that supports the AIX operating system \n \n To restore rootvg, you need a method like mksysb, which is stored and retrieved from Cloud Object Storage. The restored mksysb image then applies the AIX configuration details while preserving the {{site.data.keyword.powerSys_notm}} deployed storage and networking resources. |
| High availability | Local OS level high availability | Different server placement group \n \n PowerHA standard edition | PowerHA standard edition | Local availability optimization by allowing for the dynamic reconfiguration of running clusters. \n \n Minimize unscheduled downtime in response to unplanned cluster component failures. |
| Disaster recovery {{site.data.keyword.powerSys_notm}} workloads                      | Secondary data center with SAN-to-SAN replication  | Global Replication Services (GRS) and {{site.data.keyword.IBM_notm}} toolkit for AIX full system replication                                                              | Global Replication Services (GRS) and AIX toolkit for AIX full system replication  | Disaster recovery capability for RPO \< 1 hours, RTO \< 1 hours. \n \n {{site.data.keyword.IBM_notm}} toolkit for AIX from technology services enables automate disaster recovery functions and capabilities on the {{site.data.keyword.cloud_notm}} by integrating {{site.data.keyword.powerSys_notm}} with the capabilities of GRS. |
| Disaster recovery {{site.data.keyword.powerSys_notm}} workloads: Cost optimization | | Implement shared processor pool \n Disaster recovery and non-production systems to share infrastructure. | Implement shared processor pool | Set up shared processor pool to reserve capacity in the secondary region. Set up Disaster recovery systems on minimum sized VMs to save operating cost.                                                                               |
{: caption="Table 1. Resliency architecture decisions" caption-side="bottom"}
