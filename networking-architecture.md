---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: pattern-pvs-aix-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for networking
{: #networking-architecture}




| Architecture decision |Requirement | Alternatives | Decision | Rationale |
|-----|-----|-----|-----|------|
| BYOIP/Edge Gateway             | Capability that is needed for customers to provide isolation, security, and edge routing services | Edge Gateways:  Palo Alto, Fortinet, F5 with the client choice | Gateway: Client choice \n {{site.data.keyword.vpc_short}} facilitates BYOIP | Edge Gateway is client choice based on the requirements \n \n Client can [bring their own subnet](/docs/vpc?topic=vpc-configuring-address-prefixes) IP address range to an {{site.data.keyword.vpc_full}} \n \n GRE (Generic Routing Encapsulation) Tunnel connecting the {{site.data.keyword.powerSysShort}} to VPC for routes to be advertised across on-premises environment |
|Network Segmentation/Isolation | Deploy workloads in an isolated environment and enforce information flow policies. | Subnets, Security Groups, ACLs, Workspaces | VPCs and subnets \n \n Separate {{site.data.keyword.powerSysShort}} LPARs | Native VPC isolation by using separate VPCs and subnets environments for separation of workload \n \n {{site.data.keyword.powerSysShort}} isolation \n \n Security group with inbound rule, address prefix, and subnet for Secure Automated Backup with Compass. |
| Cloud Native Connectivity (to cloud services) | Provide secure connection to Cloud Services | VPC Gateway + Virtual Private Endpoints (VPE) \n Private Cloud Service endpoints \n Public Cloud Service Endpoints | VPC Gateway + Virtual Private Endpoints (VPE) | VPC Gateway + Virtual Private Endpoints enable connectivity to {{site.data.keyword.cloud_notm}} services by using private IP addresses allocated from a VPC subnet.|
| Cloud Landing-zone Connectivity | Connect across multiple VPCs and to {{site.data.keyword.cloud_notm}} Classic, {{site.data.keyword.powerSysShort}} environments | {{site.data.keyword.tg_short}} (TGW) \n Power Edge Router (PER) \n Global {{site.data.keyword.tg_short}} (GTGW) | {{site.data.keyword.tg_short}} \n \n Power Edge Router (PER) | {{site.data.keyword.tg_short}}s (TGW) are used for interconnectivity between {{site.data.keyword.powerSysShort}} and VPCs. {{site.data.keyword.tg_short}}s have built in redundancy. TGWs are regional and are deployed 2 per AZ within the same region. \n \n Power Edge Routers (PER) are also deployed as 2 per region. PER is used for interconnectivity between {{site.data.keyword.powerSysShort}} and the TGW. For more information see [getting started with PER] (/docs/power-iaas?topic=power-iaas-per)|
| Cloud Landing-zone Connectivity across regions | Connect across regions | Global {{site.data.keyword.tg_short}} (GTGW) | Global {{site.data.keyword.tg_short}} (GTGW) | Interconnects classic, VPCs and {{site.data.keyword.powerSysShort}} resources across regions. \n \n Connect to environments in other regions for resiliency data replication purposes.|
| Domain Name System (DNS) | Ability to resolve DNS names on site | {{site.data.keyword.dns_full_notm}} | IBM continues to forward or relay the DNS to client DNS Servers onsite | This is the default option in the absence of a specific customer requirement to manage DNS \n \n Name resolution for the backup server connections is required. |
{: caption="Table 1. Architecture decisions for network" caption-side="bottom"}
