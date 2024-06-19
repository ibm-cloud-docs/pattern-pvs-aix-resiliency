


# Overview

The Deploying Resilient AIX workloads on Power Virtual Server pattern deploys a multi-region solution on PowerVS that includes backup, high availability, and disaster recovery. This pattern can be used to provide an all-in approach for deploying a resilient power architecture within the PowerVS for AIX workloads. The solution does not address application or database level high available design.

• It can support up to 99.99% infrastructure availability when deployed as a multi-region.

• It can support local high availability to protect from immediate lpar failure.

• It can support backups for AIX workloads to protect against data loss and support disaster recovery scenarios.

• It can support out-of-region disaster recovery.

The PowerVS resiliency pattern is intended to:

• Accelerate and simplify solution design by providing a standard IBM Cloud deployment architecture reference following the IBM Architecture Design Framework.

• Provide a prescriptive, end-to-end enterprise-class solution design, with diagrams, component architecture decisions along with rationale for cloud component selection for a resilient architecture.

• Ensure that requirements can be met from a performance, system availability, and security perspective.

**Pattern Considerations**

• Backup AIX data utilizing a managed backup service.

• Provide OS level local high availability between two AIX LPARs.

• Provide a disaster recovery solution utilizing SAN to SAN replication between two regions.

Note: Always validate offerings are available in the regions you are deploying via [IBM Cloud portal](https://cloud.ibm.com/login).

Note: Resiliency needs to be considered for both the infrastructure and application levels. This pattern does not address application or database specific designs. Consider latency between environments before deciding on a replication method. Consider options for synchronous and asynchronous replication when designing database replication to meet the requirements.

# Reference Architecture

This is a baseline solution pattern containing the design and architecture decisions for a PowerVS resiliency solution for AIX workloads to meet common requirements as noted in this use case. Actual solutions depend on the specific requirements that are set by the client. Below is a summary of the use case for this reference architecture:

![](13bf14fbf3a3c8d6ab330529aced2ca9.png)

Figure X: Reference Architecture Summary for Deploying Resilient AIX workloads on Power Virtual Server

## Architecture Diagram

![](945b2b29becc276207e96ccd02a4432d.png)

![](5c764216c16fcc02f2d7b0d7bdb6a6cd.png)

![](2a66ddf06756f3e6b9c00dbed09c72e8.png)

Figure X: Deploying Resilient AIX workloads on Power Virtual Server Reference Architecture

Environments related to this reference architecture:

-   Primary Environment – added to the primary datacenter

    -   BaaS VPC deployed as part of the backup service automation; not for workloads

-   Secondary IBM Cloud datacenter for disaster recovery workloads

    -   Edge VPC for firewall

    -   Management VPC for management tool stacks to manage VPC, PowerVS environments

    -   PowerVS Environment - workload PowerVS workspace, workload/GRS controller LPARs

## Design Scope

The PowerVS resiliency for AIX workloads architecture covers design considerations and architecture decisions for the following aspects and domains (as defined in the [Architecture Design Framework](https://cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-intro)):

-   Compute: Virtual Servers

-   Storage: Primary Storage, Backup Storage, Archive Storage

-   Networking: Enterprise Connectivity, BYOIP/Edge gateways, Segmentation, Isolation, Cloud Native Connectivity, Domain name system

-   Security: Data security, Identity and access management, Infrastructure and endpoint security

-   Resiliency: Backups and Restore, High Availability, Disaster Recovery

The Architecture Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. See [Introduction to the Architecture Design Framework](https://cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-intro) for more details.

Following the Architecture Design Framework, Resiliency for PowerVS covers design considerations and architecture decisions for the following aspects and domains:

![](6d17f81955f76621d66bf28354e36d8d.png)

Figure x Resiliency for PowerVS AIX Workloads heat map

## Requirements

| **Aspect**         | **Requirements**                                                                                                                                                                                                                                                                                  |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute            | Provide CPU and RAM to support resiliency components.                                                                                                                                                                                                                                             |
| Storage            | Provide storage to support replication activities. Provide storage to support customer retention schedules.                                                                                                                                                                                       |
| Networking         | Provide enterprise to cloud network connectivity to recovery site.  Provide private connectivity between workloads across protected and recovery sites. Deploy workloads in an isolated environment and enforce information flow policies. Provide BYOIP, Edge Routing, VLAN segmentation and DNS |
| Security           | Ensure data encryption at rest and in transit for the storage layer. Protect the boundaries of the application against denial-of-service and application-layer attacks.                                                                                                                           |
| Resiliency         | Provide local OS level high availability between two AIX LPARs  Provide backups for data retention for AIX workloads. Recovery Time Objective (RTO) and Recovery Point Objective(/RPO) = 1 hours/1 hours.  99.99% Infrastructure Availability                                                     |
| Service Management | Monitor the usage and performance of the resiliency components                                                                                                                                                                                                                                    |

Table 1. Resiliency for PowerVS requirements

## 

## Components

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

Table 2. Resiliency for PowerVS components

# Compute Design

The requirements for the compute aspect for the Resiliency for PowerVS AIX workloads pattern focus on:

-   The compute required for the backup components.

-   The compute required for the disaster recovery workloads.

-   The compute required to support high availability activities.

## Compute Considerations for Backups

Backup Methodology: **Secure Automated Backup with Compass**

-   Fully managed backup as a service solution for any AIX workloads. Ordered and configured via the [IBM Cloud catalog](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2c%2FY2F0ZWdvcnk9c3RvcmFnZQ%3D%3D). Compute is deployed as a service and connected to the PowerVS workloads via a “BaaS/Backup VPC” and a transit gateway in the client IBM Account.

-   Compass backup servers are preconfigured in data centers and are also replicated across to the other regions. When a customer provisions the Backup Offering through IBM Cloud catalog, an automation process deploys the Backup Offering, Virtual Private Cloud (VPC) and necessary Virtual Private Endpoints (VPE) to establish secure private network connection to the Compass backup servers. It is highly recommended that you refrain from deploying any additional resources to Backup Offering VPC.

## Compute Considerations for High Availability

Local OS High Availability Methodology: **PowerHA Standard**

-   Adequate LPAR compute for clustered AIX LPARs in one IBM Cloud datacenter for local HA.

## 

## Compute Considerations for Disaster Recovery

Disaster Recovery Methodology: **Secondary datacenter with Global Replication Service (GRS)**

-   Control LPARS for GRS replication

    -   one control LPAR (.25 cpu x 16GB x300GB) per datacenter per OS type

-   LPARS for Disaster Recovery Workloads

-   Ensure adequate LPARS are provisioned in the recovery location to support replicas of critical workloads if there is a disaster.

-   Consider workload optimizations for secondary site LPARS such as Shared Process Pools (SPP)

-   pool of processor capacity shared between a group of virtual server instances. Unlike a virtual server instance that has a dedicated and defined maximum amount of processing capacity, set the reserved cores in SPP that are guaranteed to be available at the pool level. Provides the ability to have lower cost standby processor core resources for HA and DR secondary environments.

-   Minimum Shared processor virtual machines can be deployed upfront to maintain replication workloads and grow (and shrink) the capacity of these VMs when needed.

-   SPP will allow provisioning the disaster recovery environment at the lowest possible compute configuration. At the time of the DR event, the LPARs are rehydrated from the reserved compute capacity.

-   SPP cost optimization by paying for compute capacity when needed. Please note SPP only reserves compute capacity, not the memory. More about SPP can be found here <https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-manage-SPP> and here Managing shared processor pools <https://www.ibm.com/docs/en/power9?topic=systems-managing-shared-processor-pools>

# Storage Design

The requirements for the compute aspect for the Resiliency for PowerVS for AIX workloads pattern focus on:

-   The storage required to support backup activities.

-   The storage required to support replication activities for disaster recovery.

## 

## Storage Considerations for Backups

**Backup Method: Secure Automated Backup with Compass + MKSYSB**

-   Compass backup servers are preconfigured in data centers and are also replicated across to the other regions.

-   By default, there are 2 copies of data - one in each MZR, service is setup in pairs

-   Validate Secure Automated Backup with Compass is offered in the datacenter where workloads are deployed your workloads. datacenter pairings.

-   Storage sizing considerations: workload and data volumes (source total, unstructured and structured), change and growth rates, retention policies.

-   Engage Cobalt Iron for sizing - [https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass\#about](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass#about)

-   Ensure that there is sufficient file system space to hold the produced mksysb image. Generally, 10 to 15 GB is sufficient depending on additional non-AIX data added to the rootvg. Consider Cloud Object storage for mksysb images.

## Storage Considerations for High Availability

**High Availability Method: PowerHA Standard for Local HA Cluster**

-   Clustered workload LPARS share the same volume group, storage tier and size will be determined by the workload requirements.

-   LUN for Cluster aware AIX (CAA) repository, 1GB recommended.

## Storage Considerations for Disaster Recovery

**Disaster Recovery Method: Global Replication Services (GRS)**

Control LPARS

-   Each GRS control LPAR requires 300gb Tier 1 storage in the primary region and one in the secondary region.

Disaster Recovery Workloads

-   Global Replication Services (GRS) does not support mixed Tiers for the same environment. Storage in the disaster recovery site will be the same Tier as the storage deployed for the primary workload (primary site workload is Tier 1 then secondary site workload storage will be Tier 1, primary workload is Tier 3 then secondary site workload storage will be Tier 3).

# Network Design

This resiliency pattern leverages a 2-region deployment for disaster recovery. The requirements for the network aspect for the Resiliency on PowerVS workloads pattern focus on:

-   Resilience for Enterprise connectivity to PowerVS Disaster Recovery Environment

-   Connectivity between datacenters

Network Design Considerations

-   Network Latency

    -   Greater distances typically necessitate asynchronous replication.

    -   Depending on the application, synchronous mirroring maybe the only practical approach

    -   This Pattern is using Global Replication Services (GRS) which operates between two sites that are over 300 km apart.

-   Replication Traffic

    -   Global replication traffic between PowerVS regions will traverse the IBM Cloud backbone.

    -   GRS control LPAR traffic traverse the GTGW.

    -   Backup replication traverses TGW-\>VPE-\>IBM Cloud Backbone-\> Compass Vault System-\> IBM Cloud Backbone-\> Secondary Compass vault system

-   VPC

    -   Multiple VPCs are utilized in this pattern (additional client requirements may require additional VPCs). This pattern includes:

    -   “Baas/Backup VPC” - Secure Automated Backup with Compass automated deployment will deploy a “Baas/Backup vpc”. It is recommended to not deploy additional workloads in this VPC.

    -   Edge VPC - NFGW will be deployed in the Edge VPC. To provide isolation and centralized advanced security functions, the network design follows the Hub and spoke VPC model. The Edge VPC serves as the hub for which all ingress and egress traffic flows. The Edge is a virtual network (VPC) that acts as a central point of connectivity to on-premises network and all other VPCs. PowerVS workspaces are connected to the Edge ("Hub") by a transit gateway, which allows traffic routing between the VPCs and PowerVS workspaces in the IBM Cloud account.

-   Secure Automated Backup with Compass

    -   When provisioned through the IBM Cloud catalog, an automation process deploys the backup solution which includes:

        -   Virtual Private Cloud (VPC) exclusive use of the backup activity (“Baas/Backup vpc”)

        -   Virtual Private Endpoints (VPE) to establish secure private network connection to the Compass backup servers.

        -   A local Transit Gateway if it does not already exist.

    -   The Backup Offering VPC and the Power Virtual Server workspaces should be in the same region and connected using the local Transit Gateway.

-   HA Clusters

    -   Each PowerVS will need its own IP, “service IP” typically on the same VLAN as the partner Power VSI. The service IP is an "extra" IP used for the application being HA'd.

-   For additional key network areas to keep in mind while designing Resiliency for PowerVS workloads on IBM Cloud see - \<pending white paper link\>

# Security Design

The following are requirements for the security aspect for the Resiliency for PowerVS workloads pattern:

-   Ensure data encryption at rest and in transit for the storage layer.

-   Protect the boundaries of the application against denial-of-service and application-layer attacks.

Security Design Considerations

Security should be applied at all layers of the solution for defense in depth depending on requirements, for example at the network edge, VPCs, and at every compute instance as well as at the application and database layers. In addition, data should be protected both in transit and at rest according to data classification and controls should be in place to eliminate the need for direct access to the environment, and when direct access is needed, all access should be logged and monitored.

It is a best practice to isolate, secure, manage, and monitor all ingress and egress traffic to the environment and to centralize these functions in a hub and spoke model through an Edge VPC. As a result, it's recommended that security and isolation are established by using a combination of Security Groups (SG), Network Access Control Lists (NACL) and routing/FW rules defined in the Edge VPC. This is in addition to the cloud account-level protection provided in IBM Cloud Identity Services.

-   Implementation of firewalls in the Transit/Edge can secure and route traffic to the IBM Cloud environment as well as from the Customer network.

-   Enable logging to facilitate the firewall activity analysis if needed to meet client security IPS/IDS requirements.

-   All traffic, both public and private, should route through the Edge/Transit VPC for routing, isolation, and logging.

-   To restrict, log and monitor administrative access to the environment, a bastion (jump) host is recommended in the Edge VPC for all administrative access. In addition, all Bastion access and activity should be logged and monitored through Privileged Access Management (PAM) services.

-   For additional key security areas to keep in mind while designing Resiliency for PowerVS workloads on IBM Cloud see - \<pending white paper link\>

# Resiliency Design

The following are requirements for the resiliency aspect:

• Backup all essential AIX data

• Provide OS level local high availability

• Replicate PowerVS workloads from a protected site to a recovery site in a different region to

enable the failover of workloads if there is a failure in the protected site.

• Failover that meets the required Recovery Time Objectives (RTO) and Recovery Point Objectives

(RPO) of the application.

NOTE: Resiliency needs to be considered for both the infrastructure and application levels. This pattern

does not address application or database specific design.

NOTE: Always validate offerings are available in the regions you are deploying via [IBM Cloud portal](https://cloud.ibm.com/login). Check for paired datacenters and validate if they meet the deployment criteria for client specific requirements.

Secure Automated Backup with Compass - The Backup Offering VPC and the Power Virtual Server workspaces should be in the same region and connected using the local Transit Gateway. Check for Secure Automated Backup with Compass [paired datacenters](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-backup-strategies#baas)

## Global Replication Services **-** [Locations](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-getting-started-GRS) that support global replication service

## Backup Design Considerations

Backup Methodology: **Secure Automated Backup with Compass (+ mksysb)**

-   Secure Automated Backup with Compass is a fully IBM managed backup solution for AIX workloads.

    -   File-level backup and restore

    -   Image-level backup and restore

    -   Policy management down to the directory and file object or type levels

    -   Backup and archive features that includes long-term retention of data

-   Backup and restore tool for periodic copies of data and applications to a separate, secondary device or secondary site and then using those copies to recover the data and applications.

-   Compass backup servers are preconfigured in data centers and are also replicated across to the other regions.

-   By default, there are 2 copies of data - one in each MZR, service is setup in pairs; validate Secure Automated Backup with Compass [datacenter pairings]().

-   Replication frequency - daily replication based on the local time zone.

-   This third-party product is provided by a vendor outside of IBM and is subject to a separate agreement between you and the third-party if you accept their terms. IBM is not responsible for the product and makes no privacy, security, performance, support, or other commitments regarding the product.

-   For sizing and configuration reach out to Cobalt Iron via [support](http://support.cobaltiron.com/) methods found here https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass\#about.

-   ROI Estimator can be found here <https://cobaltiron.valuestoryapp.com/savings-calculator/?mediafly_tco_calculator_conversion_source=site-heroslider>.

-   Rootvg restore method is required, such as mksysb stored/retrieved from COS. The restored mksysb image applies the AIX configuration details while preserving the Power Virtual Server deployed storage and networking resources. For more information see <https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-restoring-aix-mksysb-image>.

-   For more information regarding Secure Automated Backup with Compass see [https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass\#about](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass#about).

-   Check for Secure Automated Backup with Compass [region availability](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass).

-   The Compass Commander UI is in the IBM cloud and accessible from the IBM cloud. It is external to the VPC, SSO enabled.

Below is a reference architecture diagram for Secure Automated Backup with Compass

![](8a889a67695c1fe1c9d738d330524456.png)

Figure X: **Secure Automated Backup with Compass**

## High Availability Design Considerations

Local OS High Availability Methodology: **PowerHA Standard Edition**

-   By default, Power Virtual servers are restarted on a different host system if a hardware failure occurs. PowerHA standard edition provides local clustering of “mission critical” workloads.

-   Clustering infrastructure provides the means of creating and managing multiple systems and system resources as a unified entity. Shared resources enable the cluster to continuously provide the essential services to users and applications. PowerHA Cluster Functions:

    -   Heartbeat monitoring for failure detection

    -   Activation/Release of Highly Available Services IP/s

    -   Automatic Activation of Geographically Mirrored Volume Groups

    -   Start/Stop/Monitor of Applications

    -   Failover Resource Groups between Cluster members/sites

-   For more information on POWERHA see https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-ha-dr

-   PowerHA supports resource optimization high availability (ROHA) for AIX instances on PowerVS. This is not discussed in this pattern, however, ROHA is another level of automation built into PowerHA that could be considered. It enables clustered instances to automatically adjustment central processing units (CPUs) and memory resources, which allows organizations to be more efficient in their overall use and consumption of those resources. For more information on configuring and using ROHA with Power Virtual Server, see [Resource Optimized High Availability in Cloud](https://www.ibm.com/docs/en/powerha-aix/7.2?topic=administering-resources-optimized-high-availability-in-cloud) - <https://www.ibm.com/docs/en/powerha-aix/7.2?topic=administering-resources-optimized-high-availability-in-cloud>.

The figure below shows a configuration using PowerHA Standard Edition.

![](dee6336bac9f91b2e6dee934bc0b4b03.png)

Figure X: Local PowerHA Architecture

In this configuration, both nodes have simultaneous access to the shared disks and own the same disk resources. There is no takeover of shared disks if a node leaves the cluster, since the peer node already has the shared volume group varied on.

## 

## Disaster Recovery Design

Disaster Recovery Methodology: **Secondary datacenter with Global Replication Service (GRS)**

The Power Systems Virtual Server service provides a Tier 2 99.95% SLA by default. When a LPAR has an outage within the service, it automatically attempts to restart that lpar on a separate host. For a Tier 3 SLA of 99.99% the workload is distributed across 2 data centers. Supporting a 1 hour RTO and 1 hour RPO, the solution described in this pattern includes

-   Secondary datacenter

-   SAN to SAN replication paired between two IBM Power Virtual Server (PowerVS) workspaces in different datacenters.

-   Global Replication services (GRS) is based on industry standards IBM Storwize Global Mirror Change Volume Asynchronous replication technology. For more information on getting started with GRS see https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-getting-started-GRS

-   The GRS involves two sites, over 300 km apart, where storage replication is enabled. These two sites are fixed and mapped into one-to-one relationship mode in both directions. These two sites are fixed and are in replication partnership in both directions. You can create a replication-enabled volume from any site, the site from where the request is initiated contains the master volume and play the role of master. The remote site is auxiliary and contain an auxiliary volume.

-   Key Capabilities

    -   Volume-based async storage replication using Consistency Groups

    -   CG Services to create/delete CGs, add/remove volumes, stop/start, etc.

    -   Volume services (create, delete, retype, etc.) enablement to support mirrored volumes

    -   VM services (deploy, delete, attach, detach, clone, snapshot, etc.) for mirrored volumes.

-   When a write operation is issued to a source volume, the changes are typically propagated to the target volume a few seconds after the data is written to the source volume. However, changes can occur on the source volume before the target volume verifies that it received the change. Because consistent copies of data are formed on the secondary site at set intervals, data loss is determined by the amount of time since the last consistency group was formed. If the system fails, Global Mirror might lose some data that was transmitted when the failure occurred.

-   For additional details regarding GRS see [Global Replication Services Solution using IBM Power Virtual Server](https://cloud.ibm.com/media/docs/downloads/power-iaas/Global_Replication_Services_Solution_using_IBM_Power_Virtual_Server.pdf) https://cloud.ibm.com/media/docs/downloads/power-iaas/Global_Replication_Services_Solution_using_IBM_Power_Virtual_Server.pdf

-   Consider the IBM Toolkit for AIX from IBM Technology Expert Labs for disaster recovery automation functions and capabilities on the IBM Cloud by integrating PowerVS with the capabilities of GRS. With the Toolkit, simplify and automate operations of the disaster recovery solution. IBM Toolkit for AIX Full System Replication (AIX) enables automate disaster recovery functions and capabilities on the IBM Cloud by integrating PowerVS with the capabilities of GRS. Clients can manage their DR environment using their existing AIX skills. Toolkit functions:

    -   Full System Replication for IBM AIX PowerVS

        -   Replicate your data from between IBM Cloud sites

        -   Replicate the AIX OS volumes

    -   Disaster Recovery Services

        -   Perform a VM switch by deactivating a VM from one site and activating its replicated VM on another site

    -   Administrative Functions

        -   Reduce outage time by activating your application on another IBM site while performing required maintenance

    -   How to get started: [technologyservices@ibm.com](mailto:technologyservices@ibm.com)

-   To fully automate the site/volume relationship requires PowerHA Enterprise Edition. PowerHA GLVM Functions:

    -   Automate all rpvserver / rpvclient relationships

    -   Ensure that all LV copies are kept in sync

    -   If half of the disks are missing perform a "force" varyon

    -   Handle Data Divergence dominant site

-   Validate [datacenter pairings](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-getting-started-GRS) available for global replication service.

The Figure below illustrates the use of GRS as the DR solution between 2 cloud datacenters

![A diagram of a cloud computing system Description automatically generated with medium confidence](9c5f9ed49a38dfebf6d2cef46aa98934.png)

Figure X GRS Architecture

# Service Management Design

The following are requirements for the service management aspect for the Resiliency for PowerVS workloads pattern:

-   Monitor the usage and performance of the backup, high availability, and disaster recovery components.

Typically, service management tools are integrated with a centralized Service Management Ticketing System to provide a single pane of glass for all operations activities. The list of tools is based on client's needs and requirements and what level of management one wants to perform. There are general guidelines and some recommendations in the Service management Architecture decision section.

Operations management is a key aspect of building resilient applications. To support the application availability targets:

-   Continuously monitor the application and platform infrastructure to detect failures and degradations.

-   Integrate automated monitoring with rich notification tools to automate problem resolution and enable a timely response to incidents.

It is important to monitor the health of all the components of the solution, including infrastructure, cloud services, and application as well as operational logs to detect and correct issues that might affect the availability of enterprise applications. Proper operational monitoring can help you determine whether you need to fail over to an alternative site or whether operations have returned to normal after a system disruption.

Equally important is to track and monitor all activity that is performed on the IBM Cloud to detect changes and potential security threats that might impact the availability of the applications that are deployed on IBM Cloud.

Implement incident detection, notification, escalation, discovery, and declaration to provide realistic, achievable objectives that add business value.

-   Use [IBM Cloud Monitoring](https://cloud.ibm.com/docs/monitoring?topic=monitoring-about-monitor) and [IBM Log Analysis](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-getting-started) to monitor operational metrics and logs. Operational metrics include CPU and memory usage, API response times, and so on.

-   Monitor platform metrics from resources in your IBM® Power® Virtual Server workspace by using IBM Cloud® Monitoring dashboards. To monitor platform metrics for a service instance, provision the IBM Cloud Monitoring instance in the same region where the IBM Cloud service instance to be monitored is provisioned.

-   Use [IBM Cloud Activity Tracker](https://cloud.ibm.com/docs/activity-tracker?topic=activity-tracker-getting-started) to monitor audit logs for the application, IBM Cloud Infrastructure, and other IBM Cloud services.

-   Configure IBM Cloud Monitoring, IBM Logging, and Activity Tracker to set up alerts, send notifications, and trigger actions in response to the alerts.

-   Use [IBM Cloud Event Notifications](https://cloud.ibm.com/docs/event-notifications?topic=event-notifications-en-about) to route events associated with IBM Cloud resources (event sources) to a destination (delivery target for a notification) to trigger actions and help automate the response to critical incidents. Define and build event notifications by linking event sources and destinations. As an example, select event sources to detect cloud provider level (for example, region, zone, services), network level (for example load balancers, global load balancers), security level, and application level critical events and integrate them with destination targets. Select destinations such as ServiceNow to collect all events and assign owners and AIOps tool to automate response to events like the file system is full.

-   PowerHA - The primary task of PowerHA® SystemMirror® is to recognize and respond to failures. PowerHA SystemMirror uses the Cluster Aware AIX® infrastructure to monitor the activity of its network interfaces, devices, and IP labels. Monitoring connections are necessary because they enable PowerHA SystemMirror to recognize the difference between a network failure and a node failure. For instance, if connectivity on the PowerHA SystemMirror network (this network's IP labels are used in a resource group) is lost, and another TCP/IP based network, PowerHA SystemMirror recognizes the failure of its cluster network and takes recovery actions that prevent the cluster from becoming partitioned. PowerHA SystemMirror automatically monitors interfaces on TCP/IP networks, Storage area networks and Repository disks.

-   Global Replication Services

-   Secure automated backup with Compass - Alerting, notifications, and ticketing features and integration with IBM Cloud Monitoring, IBM Cloud Logs and Client tools.

-   Alternatively, third party software such as Splunk and Datadog can be integrated with IBM Cloud to provide security monitoring, compliance reporting, and operational intelligence.

References for service management for IBM Power Virtual server

-   <https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-monitor-sysdig>

-   <https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-at-events>

Attention: IBM Cloud Activity Tracker services are deprecated and will no longer be supported as of 30 March 2025. The replacement service, IBM Cloud Logs is planned to be generally available late second quarter 2024. For more information, see [Getting started with IBM Cloud Logs](https://cloud.ibm.com/docs/cloud-logs?topic=cloud-logs-getting-started).

# Architecture Decisions

## Architecture Decisions for Compute

| **Architecture decision**                   | **Requirement**                                               | **Alternatives**  | **Decision**                 | **Rationale**                                                                                                                                                                                                          |
|---------------------------------------------|---------------------------------------------------------------|-------------------|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Compute: Local high availability workloads  | Provide compute for LPARs supporting local high availability  | Not Applicable    | Power Virtual Server LPAR(s) | Local High Availability via PowerHA System Mirror, resources should be equal to support workloads in a failover situation.                                                                                             |
| Compute:  GRS Controllers                   | Provide compute for replication components.                   | Not Applicable    | Power Virtual Server LPAR(s) | Compute for GRS controller workloads                                                                                                                                                                                   |
| Compute: Edge and Management VPCs           | Provide compute for workloads in the edge and management VPCs | Not Applicable    | Virtual Servers for VPC      | Virtual servers for workloads in the Edge and Management VPCs                                                                                                                                                          |
| Compute: Disaster Recovery Workloads        | Provide compute for LPARs supporting disaster recovery.       | Not Applicable    | Power Virtual Server LPAR(s) | Target an environment to match specific workload requirements. Consider Shared Processor Pool configuration as an option to provision the disaster recovery environment at the lowest possible compute configuration.  |

## Architecture Decisions for Storage

| **Architecture decision**                     | **Requirement**                                                                                     | **Alternatives**                                                | **Decision**                                                                      | **Rationale**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|-----------------------------------------------|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|-----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Primary Storage for local HA workloads        | Provide highly available storage for local high availability.                                       | Flash Storage from IBM FS9000 series devices Tier 1 - 10 IOPS   | Matches production workload storage tier                                          | LPARS share the same local storage                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Primary Storage for secondary site workloads  | Provide highly available storage for disaster recovery workloads using Global Replication Services  | Match production workload storage tiers                         | Match production workload storage Tiers                                           | Global Replication Services (GRS) does not support mixed Tiers for the same environment. Storage needs to like to like – Tier 1 to Tier 1, Tier 3 to Tier 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Backup Storage                                | Provide highly available storage for backups                                                        | Local Storage COS Managed Service                               | Local storage + COS Storage is deployed as part of the managed backup solution.   | Local + COS storage for mksysb images Storage is deployed as part of the managed backup solution.  Secure Automated Backup with Compass is a fully managed backup solution for AIX and Linux workloads.  backup servers are preconfigured in data centers and replicated to another region.  2 copies of data, one in each MZR, service is setup in pairs; validate Secure Automated Backup with Compass datacenter pairings match your workload locations.  For sizing and configuration reach out to Cobalt Iron via [support](http://support.cobaltiron.com/) https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass\#about |
| Long Term/Backup Storage                      | Provide storage for long term retention                                                             |                                                                 | Managed backup solution storage                                                   | Storage is deployed as part of the managed backup solution.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

## 

## Architecture Decisions for Network

| **Architecture decision**      | **Requirement**                                                                                   | **Alternatives**                                                  | **Decision**                                               | **Rationale**                                                                                                                                                                                                                                                                                                                                                  |
|--------------------------------|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| BYOIP/Edge Gateway             | Capability that is needed for customers to provide isolation, security, and edge routing services | Edge Gateways:  Palo Alto, Fortinet, F5 with the client choice    | Gateway: Client choice   IBM Cloud VPC facilitates BYOIP   | Edge Gateway is client choice based on the requirements Client can [bring their own subnet](https://cloud.ibm.com/docs/vpc?topic=vpc-configuring-address-prefixes) IP address range to an IBM Cloud® Virtual Private Cloud GRE (Generic Routing Encapsulation) Tunnel connecting the PowerVS to VPC for routes to be advertised across on-premises environment |
| Network Segmentation/Isolation | Deploy workloads in an isolated environment and enforce information flow policies.                | Subnets, Security Groups, ACLs, Workspaces                        | VPCs and subnets Separate PowerVS LPARs                    | Native VPC isolation by using separate VPCs and subnets environments for separation of workload PowerVS isolation                                                                                                                                                                                                                                              |

-   Security group with inbound rule, address prefix, and subnet for Secure Automated Backup with Compass.

Cloud Native Connectivity (to cloud services)

Provide secure connection to Cloud Services

VPC Gateway + Virtual Private Endpoints (VPE)

Private Cloud Service endpoints

Public Cloud Service Endpoints

VPC Gateway + Virtual Private Endpoints (VPE)

-   VPC Gateway + Virtual Private Endpoints enable connectivity to IBM Cloud services by using private IP addresses allocated from a VPC subnet.

Cloud Landing-zone Connectivity

Connect across multiple VPCs and to IBM Cloud Classic, PowerVS environments

Transit Gateway (TGW)

Power Edge Router (PER)

Global Transit Gateway (GTGW)

Transit Gateway

Power Edge Router (PER)

-   Transit gateways (TGW) are used for interconnectivity between PowerVS and VPCs. Transit Gateways have built in redundancy. TGWs are regional and are deployed 2 per AZ within the same region.

-   Power Edge Routers (PER) are also deployed as 2 per region. PER is used for interconnectivity between PowerVS and the TGW. For more information see getting started with PER https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-per

Cloud Landing-zone Connectivity across regions

Connect across regions

Global Transit Gateway (GTGW)

Global Transit Gateway (GTGW)

-   interconnects classic, VPCs and PowerVS resources across regions.

-   connect to environments in other regions for resiliency data replication purposes.

Domain Name System (DNS)

Ability to resolve DNS names on site

IBM Cloud DNS

IBM continues to forward or relay the DNS to client DNS Servers onsite

IBM continues to forward or relay the DNS to client DNS Servers onsite

-   This is the default option in the absence of a specific customer requirement to manage DNS

-   Name resolution for the backup server connections is required.

## Architecture Decisions for Security

| **Architecture decision**                 | **Requirement**                                                                                                                                                 | **Alternatives**                                                                                                                                                                                                                                                                  | **Decision**                                                                                                                                                                                                                                                                      | **Rationale**                                                                                                                                                                                                                                                                                      |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Encrypt data at rest - workloads          | Ability to encrypt data at rest                                                                                                                                 | N/A                                                                                                                                                                                                                                                                               | PowerVS storage encryption with provider-managed keys                                                                                                                                                                                                                             | PowerVS uses IBM FlashSystem Storage with AES-256 (Advanced Encryption Standard) hardware-based encryption For customer-managed keys by selecting a Key Management Service (KMS) for the respective storage service                                                                                |
| Encrypt data at rest - backups            | Ability to encrypt backups                                                                                                                                      | N/A                                                                                                                                                                                                                                                                               | Storage Encryption with provider-managed keys                                                                                                                                                                                                                                     | All objects that are stored in IBM Cloud Object Storage are encrypted by using randomly generated keys and an all-or-nothing-transform (AONT). Secure Automated Backup with Compass includes source system Spectrum Protect client encryption at-rest encryption in-cloud with AES 128 encryption. |
| Identity Access & Role Management (IDM)   | Securely authenticate users for platform services and control access to resources consistently across IBM Cloud                                                 | N/A                                                                                                                                                                                                                                                                               | IBM Cloud IAM                                                                                                                                                                                                                                                                     | Use IAM access policies to assign users, service IDs, and trusted profiles access to resources within the IBM Cloud account. Secure Automated Backup with Compass is integrated with IBM Cloud IAM.                                                                                                |
| Key Management                            | Provider Managed Keys                                                                                                                                           | Key Protect HPCS                                                                                                                                                                                                                                                                  | Key Protect                                                                                                                                                                                                                                                                       | By default, storage at rest is encrypted with provider managed keys.                                                                                                                                                                                                                               |
| Privileged Identity and Access Management | Privileged access management services for administrative purposes                                                                                               | BYO Bastion Host BYO Bastion Host with Privileged Access Management (PAM) SW                                                                                                                                                                                                      | BYO Bastion host (or Privileged Access Gateway) with PAM SW deployed in Edge VPC  2FA Authentication though IBM Security Verify                                                                                                                                                   | Securely access remote resources over the private network for management purposes; bastion accessed through SSH. Session recording, tracking all activities, successful or not to note any potential threats                                                                                       |
| Core Network Protection                   | Strict separation of duties Isolated security zones between environments Isolated, private cloud environment                                                    |                                                                                                                                                                                                                                                                                   | Separate VPCs, Subnets, ACLs and Security Groups for workloads in VPC.   Use of virtual firewalls deployed to the Edge/Transit VPC to provide advance FW and routing capabilities between VPC and PowerVS                                                                         | A design combination using: Separate VPCs (edge, management) connected through transit gateway and, the use of edge firewall capabilities.   Subnets, Security Groups and ACLs to create an Edge/Transit VPC design along with isolated LPARs on PowerVS                                           |
| Threat detection and response             | Boundary protection: highest level of isolation from external network threats IPS/IDS protection at all ingress/egress Unified Threat Management (UTM) Firewall | BYO Virtual Firewall - [FortiGate](https://cloud.ibm.com/catalog/content/ibm-fortigate-AP-HA-terraform-deploy-5dd3e4ba-c94b-43ab-b416-c1c313479cec-global) [Palo Alto](https://cloud.ibm.com/catalog/content/ibmcloud-vmseries-1.9-6470816d-562d-4627-86a5-fe3ad4e94b30-global)   | BYO Virtual Firewall -  [FortiGate](https://cloud.ibm.com/catalog/content/ibm-fortigate-AP-HA-terraform-deploy-5dd3e4ba-c94b-43ab-b416-c1c313479cec-global) [Palo Alto](https://cloud.ibm.com/catalog/content/ibmcloud-vmseries-1.9-6470816d-562d-4627-86a5-fe3ad4e94b30-global)  | Virtual FW on VSI in the Transit/Edge VPC Client preference however recommendation is FortiGate FortiGate supports native HA configuration, IPS and IDS                                                                                                                                            |

## Architecture Decisions for Resiliency

| **Architecture decision**                                | **Requirement**                                   | **Alternatives**                                                                                                                             | **Decision**                                                                     | **Rationale**                                                                                                                                                                                                                                                                     |
|----------------------------------------------------------|---------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Backups                                                  | Backup AIX workloads with a managed service       | Image capture Snapshots/Flash Copy Secure Automated Backup with Compass  Veeam IBM Storage Protect Falconstor VTL Roll/Bring Your Own Backup | Secure Automated Backup with Compass + mksysb                                    | Managed service supporting AIX operating system Rootvg restore method is required, such as mksysb stored/retrieved from COS. The restored mksysb image applies the AIX configuration details while preserving the Power Virtual Server deployed storage and networking resources. |
| High Availability                                        | Local OS level high availability                  | “Different Server” placement group  PowerHA Standard Edition                                                                                 | PowerHA Standard Edition                                                         | local availability optimization by allowing for the dynamic reconfiguration of running clusters.  minimize unscheduled downtime in response to unplanned cluster component failures.                                                                                              |
| Disaster Recovery PowerVS Workloads                      | Secondary datacenter with SAN-to-SAN replication  | Global Replication Services (GRS) + IBM Toolkit for AIX Full System Replication                                                              | Global Replication Services (GRS) + AIX Toolkit for AIX Full System Replication  | DR capability for RPO \< 1 hours, RTO \< 1 hours. IBM Toolkit for AIX from Technology Services enables automate disaster recovery functions and capabilities on the IBM Cloud by integrating PowerVS with the capabilities of GRS.                                                |
| Disaster Recovery PowerVS Workloads – Cost Optimization  |                                                   | Implement shared processor Pool  DR and non-production systems to share infrastructure.                                                      | Implement shared processor Pool                                                  | Set up shared processor pool to reserve capacity in the secondary region. Set up DR systems on minimum sized VMs to save operating cost. This is a Power Systems virtual server special feature.                                                                                  |

## 

## Architecture Decisions for Service Management

| **Architecture decision**                                   | **Requirement**                                                                                                                                    | **Alternatives**                                                                                    | **Decision**                                                                              | **Rationale**                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Operational Monitoring of Cloud infrastructure and services | Monitor system health to detect issues that might impact the availability of the system and determine the need to failover to an alternative site. | IBM Cloud Monitoring  BYO Tool                                                                      | IBM Cloud Monitoring                                                                      | IBM Cloud Monitoring provides visibility, alerting, and troubleshooting for PowerVS workspaces.  IBM Cloud Monitoring collects and monitors operational metrics for cloud infrastructure as well as the cloud platform and services and provides a single view for all metrics Monitoring of PowerVC metrics with IBM Cloud Monitoring dashboards that are operated by Sysdig in partnership with IBM Cloud Native tools such as NMON can also be used. |
| Logging of Cloud infrastructure and services                | Log Monitoring of Cloud infrastructure and services                                                                                                | [IBM Cloud Logs](https://cloud.ibm.com/docs/cloud-logs?topic=cloud-logs-getting-started) BYO Tool   | [IBM Cloud Logs](https://cloud.ibm.com/docs/cloud-logs?topic=cloud-logs-getting-started)  | Recommended tool for infra Logging for any non-VMWare workloads. Ingestion and integration with other tools for diagnosis and alerts Ingestion and integration with other tools for diagnosis and alerts IBM Cloud Logs collects operational logs from applications, platform resources, and infrastructure and provides interfaces to view and analyze all logs.                                                                                       |
| Alerting                                                    | Provide a mechanism to identify and send notifications about operational issues that are found across application and infrastructure               | IBM Cloud Monitoring + IBM Cloud Logging + Event Notifications BYO Tool                             | IBM Cloud Monitoring + IBM Cloud Logging + Event Notifications                            | IBM Cloud Monitoring and IBM Cloud Logging support the configuration of alerts to detect operational issues and send notifications to targeted channels. Event Notifications are used to route the alert events to service destinations to automate response actions. Full stack observability for application and infrastructure when combined with Pager Duty + ServiceNow (SNOW) + Customer SIEM                                                     |

# References

IBM Redbooks High Availability and Disaster Recovery Options for IBM Power Cloud and On-Premises - https://www.redbooks.ibm.com/redpapers/pdfs/redp5656.pdf

IBM Power Virtual Server Guide for IBM AIX and Linux - https://www.redbooks.ibm.com/redbooks/pdfs/sg248512.pdf
