---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: pattern-pvs-aix-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Network design
{: #network-design}


This resiliency pattern leverages a 2-region deployment for disaster recovery. The requirements for the network aspect for the Resiliency on {{site.data.keyword.powerSysShort}} workloads pattern focus on:

-   Resilience for Enterprise connectivity to {{site.data.keyword.powerSysShort}} Disaster Recovery Environment

-   Connectivity between datacenters

Network Design Considerations

-   **Network Latency**

    -   Greater distances typically necessitate asynchronous replication.

    -   Depending on the application, synchronous mirroring maybe the only practical approach

    -   This Pattern is using Global Replication Services (GRS) which operates between two sites that are over 300 km apart.

-   **Replication Traffic**

    -   Global replication traffic between {{site.data.keyword.powerSysShort}} regions will traverse the {{site.data.keyword.cloud_notm}} backbone.

    -   GRS control LPAR traffic traverse the GTGW.

    -   Backup replication traverses TGW-\>VPE-\>{{site.data.keyword.cloud_notm}} Backbone-\> Compass Vault System-\> {{site.data.keyword.cloud_notm}} Backbone-\> Secondary Compass vault system

-   **VPC**

    -   Multiple VPCs are utilized in this pattern (additional client requirements may require additional VPCs). This pattern includes:

    -   “Baas/Backup VPC” - Secure Automated Backup with Compass automated deployment will deploy a “Baas/Backup vpc”. It is recommended to not deploy additional workloads in this VPC.

    -   Edge VPC - NFGW will be deployed in the Edge VPC. To provide isolation and centralized advanced security functions, the network design follows the Hub and spoke VPC model. The Edge VPC serves as the hub for which all ingress and egress traffic flows. The Edge is a virtual network (VPC) that acts as a central point of connectivity to on-premises network and all other VPCs. {{site.data.keyword.powerSysShort}} workspaces are connected to the Edge ("Hub") by a {{site.data.keyword.tg_short}}, which allows traffic routing between the VPCs and {{site.data.keyword.powerSysShort}} workspaces in the {{site.data.keyword.cloud_notm}} account.

-   **Secure Automated Backup with Compass**

    -   When provisioned through the {{site.data.keyword.cloud_notm}} catalog, an automation process deploys the backup solution which includes:

        -   {{site.data.keyword.vpc_full}} (VPC) exclusive use of the backup activity (“Baas/Backup vpc”)

        -   Virtual Private Endpoints (VPE) to establish secure private network connection to the Compass backup servers.

        -   A local {{site.data.keyword.tg_short}} if it does not already exist.

    -   The Backup Offering VPC and the Power Virtual Server workspaces should be in the same region and connected using the local {{site.data.keyword.tg_short}}.

-   **HA Clusters**

    -   Each {{site.data.keyword.powerSysShort}} will need its own IP, “service IP” typically on the same VLAN as the partner Power VSI. The service IP is an "extra" IP used for the application being HA'd.
