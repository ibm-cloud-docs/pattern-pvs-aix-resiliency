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
| BYOIP/Edge Gateway             | Capability that is needed for customers to provide isolation, security, and edge routing services | Edge Gateways:  Palo Alto, Fortinet, F5 with the client choice | Gateway: Client choice \n IBM Cloud VPC facilitates BYOIP   | Edge Gateway is client choice based on the requirements \n Client can [bring their own subnet](https://cloud.ibm.com/docs/vpc?topic=vpc-configuring-address-prefixes) IP address range to an IBM Cloud® Virtual Private Cloud \n GRE (Generic Routing Encapsulation) Tunnel connecting the PowerVS to VPC for routes to be advertised across on-premises environment |
|Network Segmentation/Isolation | Deploy workloads in an isolated environment and enforce information flow policies. | Subnets, Security Groups, ACLs, Workspaces | VPCs and subnets \n Separate PowerVS LPARs | Native VPC isolation by using separate VPCs and subnets environments for separation of workload \n PowerVS isolation \n Security group with inbound rule, address prefix, and subnet for Secure Automated Backup with Compass. |
| Cloud Native Connectivity (to cloud services) | Provide secure connection to Cloud Services | VPC Gateway + Virtual Private Endpoints (VPE) \n Private Cloud Service endpoints \n Public Cloud Service Endpoints | VPC Gateway + Virtual Private Endpoints (VPE) | VPC Gateway + Virtual Private Endpoints enable connectivity to IBM Cloud services by using private IP addresses allocated from a VPC subnet.|
| Cloud Landing-zone Connectivity | Connect across multiple VPCs and to IBM Cloud Classic, PowerVS environments | Transit Gateway (TGW) \n Power Edge Router (PER) \n Global Transit Gateway (GTGW) | Transit Gateway \n Power Edge Router (PER) | Transit gateways (TGW) are used for interconnectivity between PowerVS and VPCs. Transit Gateways have built in redundancy. TGWs are regional and are deployed 2 per AZ within the same region. \n Power Edge Routers (PER) are also deployed as 2 per region. PER is used for interconnectivity between PowerVS and the TGW. For more information see getting started with PER https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-per|
| Cloud Landing-zone Connectivity across regions | Connect across regions | Global Transit Gateway (GTGW) | Global Transit Gateway (GTGW) | Interconnects classic, VPCs and PowerVS resources across regions. /n Connect to environments in other regions for resiliency data replication purposes.|
| Domain Name System (DNS) | Ability to resolve DNS names on site | IBM Cloud DNS | IBM continues to forward or relay the DNS to client DNS Servers onsite | This is the default option in the absence of a specific customer requirement to manage DNS \n Name resolution for the backup server connections is required. |
{: caption="Table 1. Architecture decisions for network" caption-side="bottom"}
