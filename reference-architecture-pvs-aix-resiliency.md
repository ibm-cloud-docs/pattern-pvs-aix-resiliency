---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: pattern-pvs-aix-resiliency

keywords:

authors:
- name: Doug Eppard

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

![](/images/refarchsummary.svg)
{: caption="Figure X: Reference Architecture Summary for Deploying Resilient AIX workloads on Power Virtual Server" caption-side="bottom"}

## Architecture Diagram
{: #architecture-diagram}

![Alt](/images/resiliencypvsarch.svg "Resiliency Arch Diagram")
{: caption="Figure X: Deploying Resilient AIX workloads on Power Virtual Server Reference Architecture" caption-side="bottom"}

Environments related to this reference architecture:

-   Primary Environment – added to the primary datacenter

    -   BaaS VPC deployed as part of the backup service automation; not for workloads

-   Secondary IBM Cloud datacenter for disaster recovery workloads

    -   Edge VPC for firewall

    -   Management VPC for management tool stacks to manage VPC, PowerVS environments

    -   PowerVS Environment - workload PowerVS workspace, workload/GRS controller LPARs

## Design Scope
{: #design-scope}

The PowerVS resiliency for AIX workloads architecture covers design considerations and architecture decisions for the following aspects and domains (as defined in the [Architecture Design Framework](https://cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-intro)):

-   Compute: Virtual Servers

-   Storage: Primary Storage, Backup Storage, Archive Storage

-   Networking: Enterprise Connectivity, BYOIP/Edge gateways, Segmentation, Isolation, Cloud Native Connectivity, Domain name system

-   Security: Data security, Identity and access management, Infrastructure and endpoint security

-   Resiliency: Backups and Restore, High Availability, Disaster Recovery

The Architecture Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. See [Introduction to the Architecture Design Framework](https://cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-intro) for more details.

Following the Architecture Design Framework, Resiliency for PowerVS covers design considerations and architecture decisions for the following aspects and domains:

![Alt](/images/aixheatmap.svg "AIX Heatmap")

Figure x Resiliency for PowerVS AIX Workloads heat map

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
| Networking         | IBM Cloud Direct Link                                                                                                         | Enterprise to cloud network connectivity                                                                                            |
|                    | Transit Gateway (TGW)                                                                                                         | Connectivity between PowerVS and VPCs                                                                                               |
|                    | Service Endpoints                                                                                                             | Private network access to cloud services such IBM Cloud Logs, Cloud Object Storage.                                                 |
|                    | [Global Transit Gateway (GTGW)](https://cloud.ibm.com/docs/transit-gateway?topic=transit-gateway-about)                       | Provides PowerVS and VPC connectivity in different regions (global routing)                                                         |
|                    | [DNS Services](https://cloud.ibm.com/docs/dns-svcs?topic=dns-svcs-about-dns-services)                                         | Private DNS resolution                                                                                                              |
| Security           | Next Generation Firewall (NFGW)                                                                                               | Provide IDS/IPS and edge firewall capabilities                                                                                      |
| Resiliency         | Secure Automated Backup with Compass                                                                                          | Backups for AIX workloads                                                                                                           |
|                    | PowerHA Standard                                                                                                              | Local OS level between two LPARS                                                                                                    |
|                    | Global Replication Service + IBM Toolkit for AIX Full System Replication                                                      | SAN to SAN replication between two IBM cloud data centers                                                                           |
| Service Management | IBM Cloud Logs IBM Cloud Monitoring                                                                                           | Apps, Audit, and operational logs Monitor platform metrics                                                                          |
{: caption="Table 2. Resiliency for PowerVS components" caption-side="bottom"}

## Architecture diagram 
{: #architecture-diagram}



Figure 1 illustrates...overview text here

![A diagram of a computer network Description automatically
generated](./image1.svg)

Figure 1 Pattern-name solution architecture

Figure 1 detailed description goes here


Figure 2 illustrates ....overview text here.

![A diagram of a computer network Description automatically
generated](./image2.svg)

Figure 2 Pattern-name solution architecture

figure 2 detailed diagram description here

## Design scope 
{: #design-scope}

Following the [Architecture Framework](/docs/architecture-framework?topic=architecture-framework-taxonomy), pattern-name covers design considerations and architecture decisions for the following aspects and domains:


- **Compute:** Bare Metal and Virtual infrastructure

- **Storage:** Primary, Backup, Archive and Migration

- **Networking:** Enterprise Connectivity, Edge Gateways, Segmentation and Isolation, Cloud Native Connectivity and Load Balancing

- **Security:** Data, Identity and Access Management, Infrastructure and Endpoint, Threat Detection and Response

- **Resiliency:** Backup and Restore, Disaster Recovery, High Availability

- **Service Management:** Monitoring, Logging, Alerting, Management/Orchestration

The Architecture Framework, described in [Introduction to the Architecture Framework](/docs/architecture-framework?topic=architecture-framework-intro), provides a consistent approach to design cloud solutions by addressing requirements across a pre-defined set of aspects and domains, which are technology-agnostic architectural areas that need to be considered for any enterprise solution. It can be used as a guide to make the necessary design and component choices to ensure you have considered applicable requirements for each aspect and domain. After you have identified the applicable requirements and domains that are in scope, you can evaluate and select the best fit for purpose components for your enterprise cloud solution.

The Figure 3 shows the domains that are covered in this solution.


![A diagram of a computer network Description automatically
generated](./image3.svg)

Figure 3 design scope

## Requirements 
{: #requirements}



The following represents a baseline set of requirements which we believe are applicable to most clients and critical to successful SAP deployment.

| Aspect | Requirement |
|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Network| Enterprise connectivity to customer data center(s) to provide access to applications from on-prem|
| | Map and convert existing customer SAP Network functionality into IBM Cloud and VPC networking services|
| | Migrate/Redeploy customer IP addressing scheme within the IBM Cloud environment |
| | Provide network isolation with the ability to segregate applications based on attributes such as data classification, public vs internal apps and function  |
| Security | Provide data encryption in transit and at rest|
| | Migrate customer IDS/IAM Services to target IBM Cloud environment |
| | Retain the same firewall rulesets across existing DCs |
| | Firewalls must be restrictively configured to provide advanced security features and prevent all traffic, both inbound and outbound, except that which is specifically required, documented, and approved, and include IPS/IDS services |
| Resiliency | Multi-site capability to support a disaster recovery strategy and solution leveraging IBM Cloud infrastructure DR capabilities|
| | Provide backups for data retention|
| | RTO/RPO = 4 hours/15 minutes; Rollback to original environments should occur no later than specified RTOs |
| | 99.95 Availability|
| | Backups |
| | - Prod: Daily Full, logs per SAP product standard, 30 days retention time \n - Non-Prod: Weekly full, logs per SAP product standard, 14 days retention time|
| Service management | Provide Health and System Monitoring with ability to monitor and correlate performance metrics and events and provide alerting across applications and infrastructure |
| | Ability to diagnose issues and exceptions and identify error sources|
| | Automate management processes to keep applications and infrastructure secure, up to date, and available |
| Other| Migrate SAP workloads from existing data center to IBM Cloud VPC |
| | Customer's SAP systems and applications run on NetWeaver (application) & HANA (DB), AnyDB or S/4 HANA |
| | Provide an Image Replication migration solution that will minimize disruption during cut-over|
| | Cloud infrastructure for the proposed IAAS solution must be SAP Certified |
| | IBM Cloud IaaS will be deployed to support SAP and surrounding non-SAP workloads |
| | Customer does not want to adopt [RISE](https://www.ibm.com/consulting/rise-with-sap?utm_content=SRCWW&p1=Search&p4=43700077624079785&p5=e&gclid=EAIaIQobChMIr9bRlt7LgQMVJdHCBB0cewwcEAAYASAAEgIVgfD_BwE&gclsrc=aw.ds) at this time but wants to consider Cloud deployment solution that would facilitate a future RISE transformation|
{: caption="Table 1. Pattern requirements" caption-side="bottom"}

## Components 
{: #components}



| Aspect| Component| How the component is used|
|-------------------|--------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| Compute|[VPC VSIs](docs/vpc?topic=vpc-vsi_best_practices&interface=ui)|NetWeaver and HANA DB|
|Storage|[VPC Block Storage](/docs/openshift?topic=openshift-vpc-block)|NetWeaver and HANA DB servers primary storage. Backup storage|
| |[Cloud Object Storage](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage)|Backup and archive, application logs, operational logs and audit logs
|Networking|[VPC Virtual Private Network (VPN)](/docs/iaas-vpn?topic=iaas-vpn-getting-started)|Remote access to manage resources in private network|
| |[Virtual Private Gateway & Virtual Private Endpoint (VPE)](/docs/vpc?topic=vpc-about-vpe)| For private network access to Cloud Services, e.g. Key Protect, COS, etc.|
| | [VPC Load Balancers](/docs/vpc?topic=vpc-load-balancers) | Application Load Balancing for web servers, app servers, and database servers|
| | Public gateway| For web server access to the internet|
| | [Cloud Internet Services (CIS)](/docs/cis?topic=cis-getting-started) | Public Load balancing and DDoS of web servers traffic across zones in the region|
| | [DNS Services](/docs/dns-svcs?topic=dns-svcs-about-dns-services) | |
| | [VPCs and subnets](/docs/vpc?topic=vpc-about-subnets-vpc&interface=ui) | Network Segmentation/Isolation |
| | [Transit Gateway](/docs/transit-gateway?topic=transit-gateway-about) | Connect across multiple VPCs |
| | [IBM Cloud Application Load Balancer](/docs/vpc?topic=vpc-load-balancers-about) (ALB) \n SAP Web Dispatcher | Load balancing workloads across multiple workload instances over the private network |
|Security |[Block Storage encryption](/docs/vpc?topic=vpc-mng-data&interface=ui) with provider keys | Block Storage Encryption at rest |
| | Cloud Object Storage Encryption | Cloud Object Storage Encryption at rest|
| | HANA Data Volume Encryption (DVE) | HANA Database Encryption at rest |
| | [IAM](/docs/account?topic=account-cloudaccess) | IBM Cloud Identity & Access Management |
| | Privileged Identity and Access Management | BYO Bastion host (or Privileged Access Gateway) with PAM SW deployed in Edge VPC |
| | [BYO Bastion Host on VPC VSI with PAM SW](/docs/framework-financial-services?topic=framework-financial-services-vpc-architecture-connectivity-bastion-tutorial-teleport) | Remote access with Privileged Access Management|
| | [Virtual Private Clouds (VPCs), Subnets, Security Groups, ACLs](/docs/vpc?topic=vpc-getting-started) | Core Network Protection|
| | [Cloud Internet Services (CIS)](/docs/cis?topic=cis-getting-started) | DDoS protection and Web App Firewall |
| | One of the following: \n - [Fortigate](https://cloud.ibm.com/catalog/content/ibm-fortigate-AP-HA-terraform-deploy-5dd3e4ba-c94b-43ab-b416-c1c313479cec-global) \n - [Juniper vSRX](https://cloud.ibm.com/catalog/content/juniper-vsrx-catalog-deploy-1.4-dc1e707c-33dd-4321-b2a5-c22dbf0dd0ee-global) \n -[Palo Alto](https://cloud.ibm.com/catalog/content/ibmcloud-vmseries-1.9-6470816d-562d-4627-86a5-fe3ad4e94b30-global)| - IPS/IDS protection at all ingress/egress \n- Unified Threat Management (UTM) Firewall|
| Resiliency | HANA System Replication (HSR) | Provide 99.95% availability for HANA DB|
| | [Veeam](/docs/vpc?topic=vpc-about-veeam) | Controls both the backups and restores of all VSIs or BMs. Veeam Backup & Replication 12 |
| Service Management (Observability) | [IBM Cloud Monitoring](/docs/monitoring?topic=monitoring-about-monitor)| Apps and operational monitoring|
| | [IBM Log Analysis](/docs/log-analysis?topic=log-analysis-getting-started) | Apps and operational logs |
{: caption="Table 2. Pattern components" caption-side="bottom"} <!-- each table MUST have a caption attribute>

As mentioned earlier, the Architecture Framework is used to guide and determine the applicable aspects and domains for which architecture decisions need to be made. The following sections contain the considerations, and architecture decisions for the aspects and domains that are in play in this solution pattern.
