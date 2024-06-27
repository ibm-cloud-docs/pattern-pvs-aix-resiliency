---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: pattern-pvs-aix-resiliency

keywords:

authors:
- name: Jose Ocasio and Anne Mcdermaid

# The release that the reference architecture describes
version: 1.0

# Use if the reference architecture has deployable code.
# Value is the URL to land the user in the IBM Cloud catalog details page for the deployable architecture.
# See https://test.cloud.ibm.com/docs/get-coding?topic=get-coding-deploy-button
deployment-url:

docs: /docs/pattern-pvs-aix-resiliency

content-type: reference-architecture

---

{{site.data.keyword.attribute-definition-list}}



# Power Virtual Server Resiliency on AIX
{: #power-virtual-server-on-AIX}
{: toc-content-type="reference-architecture"}
{: toc-version="1.0"}



This is a baseline solution pattern containing the design and architecture decisions for a PowerVS resiliency solution for AIX workloads to meet common requirements as noted in this use case. Actual solutions depend on the specific requirements that are set by the client. Below is a summary of the use case for this reference architecture:

![AIX resiliency summary](/images/usecase.svg "Reference Summary"){: caption="Figure 1. Reference Architecture Summary for Deploying Resilient AIX workloads on Power Virtual Server" caption-side="bottom"}{: external download="usecase.svg"}

## Architecture Diagram
{: #architecture-diagram}

![AIX reference architecture](/images/resiliencypvsarch.svg "Resiliency Architecture Diagram"){: caption="Figure 2. Deploying Resilient AIX workloads on Power Virtual Server Reference Architecture" caption-side="bottom"}{: external download="resiliencypvsarch.svg"}

**Environments related to this reference architecture:**

    1. Provider would connect the environment using a direct link for private connectivity
    2. The direct link then connects to a Local Transit Gateway. This advertises and routes on-premises traffic to VPC for gateway or firewall inspection.
    3. The transit gateway connects to Managment VPC which hosts your NGFW, Managment Subnets for your Bastion Hosts, and your Virtual Private Endpoint.
    4. BaaS VPC deployed as part of the backup service automation; not for workloads.
    5. The Cobalt iron VPE instance then communicates to the Cobalt Iron SAAS IBM Cloud Service. 
    6. PowerVS Workspace is deployed within the Power Virtual Server Environment and connects to the Power Edge Router (PER).
    7. A local Power HA Standard cluster is then deployed within the workspace to provde local clustering. 
    8. The Managment and Workload VPC mentioned from the primary site is also deployed in DR.
    9. Global Replication Service (GRS) is deployed as part of DR SAN to SAN replication.
    10. There is also a GRS controller lpar that is also deployed at both the primary and the DR site. 
    11. Communication for GRS SAN to SAN traffic between sites occurs over the IBM private backbone. 
    12. Replication of the controller lpars occurs over the Global Transit gateway.

## Design Scope
{: #design-scope}

The PowerVS resiliency for AIX workloads architecture covers design considerations and architecture decisions for the following aspects and domains (as defined in the [Architecture Design Framework](https://cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-intro)):

-   **Compute:** Virtual Servers

-   **Storage:** Primary Storage, Backup Storage, Archive Storage

-   **Networking:** Enterprise Connectivity, BYOIP/Edge gateways, Segmentation, Isolation, Cloud Native Connectivity, Domain name system

-   **Security:** Data security, Identity and access management, Infrastructure and endpoint security

-   **Resiliency:** Backups and Restore, High Availability, Disaster Recovery

The Architecture Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. See [Introduction to the Architecture Design Framework](https://cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-intro) for more details.

Following the Architecture Design Framework, Resiliency for PowerVS covers design considerations and architecture decisions for the following aspects and domains:

![heatmap](/images/aixheatmap.svg "AIX Heatmap"){: caption="Figure 3. Resiliency for PowerVS AIX Workloads Heat Map" caption-side="bottom"}{: external download="aixheatmap.svg"}

## Requirements
{: #requirements-list}

| **Aspect**         | **Requirements**                                                                                                                                                                                                                                                                                  |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute            | Provide CPU and RAM to support resiliency components.                                                                                                                                                                                                                                             |
| Storage            | Provide storage to support replication activities. Provide storage to support customer retention schedules.                                                                                                                                                                                       |
| Networking         | Provide enterprise to cloud network connectivity to recovery site.  Provide private connectivity between workloads across protected and recovery sites. Deploy workloads in an isolated environment and enforce information flow policies. Provide BYOIP, Edge Routing, VLAN segmentation and DNS |
| Security           | Ensure data encryption at rest and in transit for the storage layer. Protect the boundaries of the application against denial-of-service and application-layer attacks.                                                                                                                           |
| Resiliency         | Provide local OS level high availability between two AIX LPARs  Provide backups for data retention for AIX workloads. Recovery Time Objective (RTO) and Recovery Point Objective(/RPO) = 1 hours/1 hours.  99.99% Infrastructure Availability                                                     |
| Service Management | Monitor the usage and performance of the resiliency components                                                                                                                                                                                                                                    |
{: caption="Table 1. Resiliency for PowerVS requirements" caption-side="bottom"}




## Components
{: #component-list}

| **Category**       | **Solution Components**                                                                                                       | **How it is used in solution**                                                                                                      |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Compute            | PowerVS LPARs                                                                                                                 | High Availability workload virtual servers  Disaster Recovery workload virtual servers Global Replication Service (GRS) Controllers |
|                    | VPC VSI                                                                                                                       | Compute for NFGW, Management Tools                                                                                                  |
| Storage            | Flash storage from IBM FS9500 series devices                                                                                  | Web, Application, Database storage Storage for GRS                                                                                  |
|                    | [Cloud Object Storage](https://cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage) | Long term backup archive, logs                                                                                                      |
| Networking         | {{site.data.keyword.dl_full_notm}} Direct Link                                                                                                         | Enterprise to cloud network connectivity                                                                                            |
|                    | Transit Gateway (TGW)                                                                                                         | Connectivity between PowerVS and VPCs                                                                                               |
|                    | Service Endpoints                                                                                                             | Private network access to cloud services such {{site.data.keyword.logs_full_notm}}, Cloud Object Storage.                                                 |
|                    | [Global Transit Gateway (GTGW)](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-about)                       | Provides PowerVS and VPC connectivity in different regions (global routing)                                                         |
|                    | [DNS Services](https://cloud.ibm.com/docs/dns-svcs?topic=dns-svcs-about-dns-services)                                         | Private DNS resolution                                                                                                              |
| Security           | Next Generation Firewall (NFGW)                                                                                               | Provide IDS/IPS and edge firewall capabilities                                                                                      |
| Resiliency         | Secure Automated Backup with Compass                                                                                          | Backups for AIX workloads                                                                                                           |
|                    | PowerHA Standard                                                                                                              | Local OS level between two LPARS                                                                                                    |
|                    | Global Replication Service + IBM Toolkit for AIX Full System Replication                                                      | SAN to SAN replication between two {{site.data.keyword.cloud_notm}} data centers                                                                           |
| Service Management | {{site.data.keyword.logs_full_notm}} {{site.data.keyword.monitoringlong_notm}}                                                | Apps, Audit, and operational logs Monitor platform metrics                                                                          |
{: caption="Table 2. Resiliency for PowerVS components" caption-side="bottom"}

As mentioned earlier, the Architecture Framework is used to guide and determine the applicable aspects and domains for which architecture decisions need to be made. The following sections contain the considerations, and architecture decisions for the aspects and domains that are in play in this solution pattern.
