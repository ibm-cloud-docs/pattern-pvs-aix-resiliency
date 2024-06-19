---

copyright:
  years: 2024
lastupdated: "2024-06-19"

subcollection: <repo-name>

keywords:

---

# Architecture decisions for resiliency
{: #resiliency-architecture}

The following sections summarize the resiliency architecture decisions for workloads deployed on IBM Cloud VPC infrastructure.

| **Architecture decision**                                | **Requirement**                                   | **Alternatives**                                                                                                                             | **Decision**                                                                     | **Rationale**                                                                                                                                                                                                                                                                     |
|----------------------------------------------------------|---------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Backups                                                  | Backup AIX workloads with a managed service       | Image capture Snapshots/Flash Copy Secure Automated Backup with Compass  Veeam IBM Storage Protect Falconstor VTL Roll/Bring Your Own Backup | Secure Automated Backup with Compass + mksysb                                    | Managed service supporting AIX operating system Rootvg restore method is required, such as mksysb stored/retrieved from COS. The restored mksysb image applies the AIX configuration details while preserving the Power Virtual Server deployed storage and networking resources. |
| High Availability                                        | Local OS level high availability                  | “Different Server” placement group  PowerHA Standard Edition                                                                                 | PowerHA Standard Edition                                                         | local availability optimization by allowing for the dynamic reconfiguration of running clusters.  minimize unscheduled downtime in response to unplanned cluster component failures.                                                                                              |
| Disaster Recovery PowerVS Workloads                      | Secondary datacenter with SAN-to-SAN replication  | Global Replication Services (GRS) + IBM Toolkit for AIX Full System Replication                                                              | Global Replication Services (GRS) + AIX Toolkit for AIX Full System Replication  | DR capability for RPO \< 1 hours, RTO \< 1 hours. IBM Toolkit for AIX from Technology Services enables automate disaster recovery functions and capabilities on the IBM Cloud by integrating PowerVS with the capabilities of GRS.                                                |
| Disaster Recovery PowerVS Workloads – Cost Optimization  |                                                   | Implement shared processor Pool  DR and non-production systems to share infrastructure.                                                      | Implement shared processor Pool                                                  | Set up shared processor pool to reserve capacity in the secondary region. Set up DR systems on minimum sized VMs to save operating cost. This is a Power Systems virtual server special feature.                                                                                  |


## Architecture decisions for high availability
{: #high-availability}

| Architecture decision | Requirement | Option | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| High availability deployment | * Ensure availability of resources if outages occur. \n * Support SLA targets for availability. | - Single zone, single region \n - Multi zone, single region \n - Multi-zone, multi region | text | text|
| High availability infrastructure | * Ensure availability of infrastructure resources if outages occur. \n * Support SLA targets for infrastructure availability. | text | text | text|
| High availability application and database | * Ensure availability of application resources if outages occur. \n * Support SLA targets for application availability. | text | text | text|
{: caption="Table 1. High availability architecture decisions" caption-side="bottom"}

