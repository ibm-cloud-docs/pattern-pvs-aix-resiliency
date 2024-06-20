---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: <repo-name>

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Compute design 
{: #compute-design}


The requirements for the compute aspect for the Resiliency for PowerVS AIX workloads pattern focus on:

-   The compute required for the backup components.

-   The compute required for the disaster recovery workloads.

-   The compute required to support high availability activities.

## Compute Considerations for Backups
{: #design-considerations-backups}

Backup Methodology: **Secure Automated Backup with Compass**

-   Fully managed backup as a service solution for any AIX workloads. Ordered and configured via the [IBM Cloud catalog](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2c%2FY2F0ZWdvcnk9c3RvcmFnZQ%3D%3D). Compute is deployed as a service and connected to the PowerVS workloads via a “BaaS/Backup VPC” and a transit gateway in the client IBM Account.

-   Compass backup servers are preconfigured in data centers and are also replicated across to the other regions. When a customer provisions the Backup Offering through IBM Cloud catalog, an automation process deploys the Backup Offering, Virtual Private Cloud (VPC) and necessary Virtual Private Endpoints (VPE) to establish secure private network connection to the Compass backup servers. It is highly recommended that you refrain from deploying any additional resources to Backup Offering VPC.

## Compute Considerations for High Availability
{: #design-considerations-ha}


Local OS High Availability Methodology:

**PowerHA Standard**

-   Adequate LPAR compute for clustered AIX LPARs in one IBM Cloud datacenter for local HA.

## Compute Considerations for Disaster Recovery
{: #design-considerations-dr}

Disaster Recovery Methodology: **Secondary datacenter with Global Replication Service (GRS)**

-   Control LPARS for GRS replication

    -   one control LPAR (.25 cpu x 16GB x300GB) per datacenter per OS type

-   LPARS for Disaster Recovery Workloads

-   Ensure adequate LPARS are provisioned in the recovery location to support replicas of critical workloads if there is a disaster.

-   Consider workload optimizations for secondary site LPARS such as Shared Process Pools (SPP)

-   pool of processor capacity shared between a group of virtual server instances. Unlike a virtual server instance that has a dedicated and defined maximum amount of processing capacity, set the reserved cores in SPP that are guaranteed to be available at the pool level. Provides the ability to have lower cost standby processor core resources for HA and DR secondary environments.

-   Minimum Shared processor virtual machines can be deployed upfront to maintain replication workloads and grow (and shrink) the capacity of these VMs when needed.

-   SPP will allow provisioning the disaster recovery environment at the lowest possible compute configuration. At the time of the DR event, the LPARs are rehydrated from the reserved compute capacity.

-   SPP cost optimization by paying for compute capacity when needed. Please note SPP only reserves compute capacity, not the memory. More about SPP can be found [here] (/docs/power-iaas?topic=power-iaas-manage-SPP) and in [Managing shared processor pools](/docs/en/power9?topic=systems-managing-shared-processor-pools)
