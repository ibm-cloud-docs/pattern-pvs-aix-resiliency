---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: pattern-pvs-aix-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Overview
{: #overview}


The Deploying Resilient AIX workloads on {{site.data.keyword.powerSys_notm}} pattern deploys a multi-region solution on PowerVS that includes backup, high availability, and disaster recovery. This pattern can be used to provide an all-in approach for deploying a resilient power architecture within the PowerVS for AIX workloads. The solution does not address application or database level high available design. Test.

• It can support up to 99.99% infrastructure availability when deployed as a multi-region.

• It can support local high availability to protect from immediate lpar failure.

• It can support backups for AIX workloads to protect against data loss and support disaster recovery scenarios.

• It can support out-of-region disaster recovery.

The PowerVS resiliency pattern is intended to:

• Accelerate and simplify solution design by providing a standard IBM Cloud deployment architecture reference following the IBM Architecture Design Framework.

• Provide a prescriptive, end-to-end enterprise-class solution design, with diagrams, component architecture decisions along with rationale for cloud component selection for a resilient architecture.

• Ensure that requirements can be met from a performance, system availability, and security perspective.


## Pattern Considerations
{: #considerations}

• Backup AIX data utilizing a managed backup service.

• Provide OS level local high availability between two AIX LPARs.

• Provide a disaster recovery solution utilizing SAN to SAN replication between two regions.

Note: Always validate offerings are available in the regions you are deploying via [IBM Cloud portal](https://cloud.ibm.com/login).

Note: Resiliency needs to be considered for both the infrastructure and application levels. This pattern does not address application or database specific designs. Consider latency between environments before deciding on a replication method. Consider options for synchronous and asynchronous replication when designing database replication to meet the requirements.





Following the Architecture Framework, the `<Pattern Name>` covers design considerations and architecture decisions for the following aspects and domains:

- Compute: Virtual Servers
- Storage: Primary Storage, Backup Storage
- Networking: Enterprise Connectivity, Segmentation and Isolation, Cloud Native Connectivity, Load Balancing, DNS
- Security: Data Security, Identity and Access Management, Application Security, Infrastructure and Endpoint Security
- Resiliency: High Availability, Backup and Restore
- Service Management: Monitoring, Logging, Auditing, Alerting

The Architecture Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. For more details, see [Introduction to the Architecture Framework](/docs/architecture-framework).
