---

copyright:
  years: 2024
lastupdated: "2024-06-28"

subcollection: pattern-pvs-aix-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Network design
{: #network-design}

This resiliency pattern uses a two-region deployment for disaster recovery. The requirements for the network aspect for the resiliency on {{site.data.keyword.powerSysShort}} workloads pattern focus on:

- Resilience for enterprise connectivity to the {{site.data.keyword.powerSysShort}} disaster recovery environment.
- Connectivity between data centers

## Network Design Considerations
{: #network-design-consideratons}

### Network latency

- This Pattern is using Global Replication Services (GRS) which operates between two sites that are over 300 km apart.
- Greater distances typically need asynchronous replication.
- Depending on the application, synchronous mirroring might be the only practical approach.

### Replication Traffic

-   Global replication traffic between {{site.data.keyword.powerSysShort}} regions traverse the {{site.data.keyword.cloud_notm}} backbone.

-   GRS control LPAR traffic traverses the GTGW.

-   Backup replication traverses TGW-\>VPE-\>{{site.data.keyword.cloud_notm}} Backbone-\> Compass Vault System-\> {{site.data.keyword.cloud_notm}} Backbone-\> Secondary Compass vault system

## Virtual Private Cloud (VPC)

Multiple VPCs are used in this pattern. Additional client requirements might require additional VPCs. This pattern includes:

- Baas and backup VPC: Secure Automated Backup with Compass automated deployment deploys a Baas and backup VPC. 

    It's recommended that you don't deploy additional workloads in this VPC.
    {: note}

- Edge VPC: NFGW is deployed in the Edge VPC. To provide isolation and centralized advanced security functions, the network design follows the hub and spoke VPC model. The Edge VPC serves as the hub for which all ingress and egress traffic flows. The Edge is a virtual network VPC that acts as a central point of connectivity to on-premises network and all other VPCs. {{site.data.keyword.powerSysShort}} workspaces are connected to the Edge also know as the Hub by a {{site.data.keyword.tg_short}}, which allows traffic routing between the VPCs and {{site.data.keyword.powerSysShort}} workspaces in the {{site.data.keyword.cloud_notm}} account.

### Secure automated backup with Compass

- When provisioned through the {{site.data.keyword.cloud_notm}} catalog, an automation process deploys the backup solution, which includes:

- {{site.data.keyword.vpc_full}} (VPC) exclusive use of the backup activity (“Baas/Backup vpc”)

- Virtual Private Endpoints (VPE) to establish a secure private network connection to the Compass backup servers.

- A local {{site.data.keyword.tg_short}} if it does not exist.

- The backup offering VPC and the Power Virtual Server workspaces should be in the same region and connected by using the local {{site.data.keyword.tg_short}}.

### High availability clusters

Each {{site.data.keyword.powerSysShort}} needs its own IP, “service IP” typically on the same VLAN as the partner Power VSI. The service IP is an "extra" IP used for the application being HA'd.
