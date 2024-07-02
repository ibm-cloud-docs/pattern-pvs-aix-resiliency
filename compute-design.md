---

copyright:
  years: 2024
lastupdated: "2024-07-02"

subcollection: pattern-pvs-aix-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Compute design
{: #compute-design}

The requirements for the resiliency for {{site.data.keyword.powerSysFull}} AIX workloads pattern focus on:

-  The compute aspects required for the backup components.
-  The compute aspects required for the disaster recovery workloads.
-  The compute aspects required to support high availability activities.

## Compute considerations for backups
{: #design-considerations-backups}

Review the following backup method for a Secure Automated Backup with Compass: 

This is a fully managed backup as a service solution for any AIX workloads. It is ordered and configured by using the [Managing shared processor pools](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass){: external}. Compute is deployed as a service and connected to the {{site.data.keyword.powerSysFull}} workloads through a Backup as a Service VPC and a transit gateway in the client {{site.data.keyword.Bluemix_notm}} account.

Compass backup servers are preconfigured in data centers and are also replicated across to the other regions. When a customer provisions the backup offering through {{site.data.keyword.cloud_notm}} catalog, an automation process deploys the backup offering, Virtual Private Cloud (VPC), and necessary Virtual Private Endpoints (VPE) to establish secure private network connection to the Compass backup servers. 

It is advisable to avoid allocating any additional resources to the backup VPC, as this VPC is exclusively dedicated to the compass solution.
{: tip}

## Compute considerations for high availability
{: #design-considerations-ha}

The local operating system high availability method is PowerHA Standard.

- This allows the failover of 2 LPAR sharing the same storage volume

- If one LPAR were to fail the second LPAR would resume the primary role and the system would continue to function as usual. 

- This method offers adequate LPAR compute for clustered AIX LPARs in one {{site.data.keyword.cloud_notm}} data center for local high availability.

## Compute considerations for disaster recovery
{: #design-considerations-dr}

The disaster recovery method is a secondary data center with Global Replication Service (GRS).

- The control LPARS for GRS replication: One control LPAR (.25 cpu x 16 GB x300GB) per data center and per OS type.

- LPARS for Disaster Recovery Workloads

- Ensure that adequate LPARS are provisioned in the recovery location to support replicas of critical workloads if there is a disaster.

- Consider workload optimizations for secondary site LPARS such as shared processor pools.

- The pool of processor capacity is shared between a group of virtual server instances. Unlike a virtual server instance that has a dedicated and defined maximum amount of processing capacity, set the reserved cores in the shared processor pool that are guaranteed to be available at the pool level. This provides the ability to have lower-cost standby processor core resources for high availability and disaster recovery secondary environments.

- Minimum shared processor virtual machines can be deployed upfront to maintain replication workloads to grow and shrink the capacity of the virtual machines when needed.

- The shared processor pool allows the provisioning of the disaster recovery environment at the lowest possible compute configuration. At the time of the disaster recovery event, the LPARs are rehydrated from the reserved compute capacity.

- Optimize the shared processor pool costs by paying for compute capacity when needed.

The shared processor pool (SPP) reserves only compute capacity, not the memory. For more information, see [The shared processor pool](/docs/power-iaas?topic=power-iaas-manage-SPP) and in [Managing shared processor pools](https://www.ibm.com/docs/en/power9?topic=systems-managing-shared-processor-pools){: external}.
