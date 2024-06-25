---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: pattern-pvs-aix-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Resiliency design
{: #resiliency-design}


The following are requirements for the resiliency aspect:

• Backup all essential AIX data

• Provide OS level local high availability

• Replicate {{site.data.keyword.powerSys_notm}} workloads from a protected site to a recovery site in a different region to

enable the failover of workloads if there is a failure in the protected site.

• Failover that meets the required Recovery Time Objectives (RTO) and Recovery Point Objectives

(RPO) of the application.

NOTE: Resiliency needs to be considered for both the infrastructure and application levels. This pattern

does not address application or database specific design.

NOTE: Always validate offerings are available in the regions you are deploying via [IBM Cloud portal](https://cloud.ibm.com/login). Check for paired datacenters and validate if they meet the deployment criteria for client specific requirements.

Secure Automated Backup with Compass - The Backup Offering VPC and the {{site.data.keyword.powerSys_notm}} workspaces should be in the same region and connected using the local Transit Gateway. Check for Secure Automated Backup with Compass [paired datacenters](/docs/power-iaas?topic=power-iaas-backup-strategies#baas)

## Global Replication Services **-** [Locations](/docs/power-iaas?topic=power-iaas-getting-started-GRS) that support global replication service
{: #global-replication}

## Backup Design Considerations
{: #backup-considerations}

Backup Methodology: **Secure Automated Backup with Compass (+ mksysb)**

-   Secure Automated Backup with Compass is a fully IBM managed backup solution for AIX workloads.

    -   File-level backup and restore

    -   Image-level backup and restore

    -   Policy management down to the directory and file object or type levels

    -   Backup and archive features that includes long-term retention of data

-   Backup and restore tool for periodic copies of data and applications to a separate, secondary device or secondary site and then using those copies to recover the data and applications.

-   Compass backup servers are preconfigured in data centers and are also replicated across to the other regions.

-   By default, there are 2 copies of data - one in each MZR, service is setup in pairs; validate Secure Automated Backup with Compass [datacenter pairings](/docs/power-iaas?topic=power-iaas-backup-strategies#baas-dcs).

-   Replication frequency - daily replication based on the local time zone.

-   This third-party product is provided by a vendor outside of IBM and is subject to a separate agreement between you and the third-party if you accept their terms. IBM is not responsible for the product and makes no privacy, security, performance, support, or other commitments regarding the product.

-   For sizing and configuration reach out to Cobalt Iron via [support](https://www.cobaltiron.com/) methods found [here](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass\#about).

-   ROI Estimator can be found [here](https://cobaltiron.valuestoryapp.com/savings-calculator/?mediafly_tco_calculator_conversion_source=site-heroslider).

-   Rootvg restore method is required, such as mksysb stored/retrieved from COS. The restored mksysb image applies the AIX configuration details while preserving the {{site.data.keyword.powerSys_notm}} deployed storage and networking resources. For more information see [mksysb](/docs/power-iaas?topic=power-iaas-restoring-aix-mksysb-image).

-   For more information regarding Secure Automated Backup see [Compass](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass\#about)

-   Check for Secure Automated Backup with Compass [region availability](https://cloud.ibm.com/catalog/services/secure-automated-backup-with-compass).

-   The Compass Commander UI is in the IBM cloud and accessible from the IBM cloud. It is external to the VPC, SSO enabled.

Below is a reference architecture diagram for Secure Automated Backup with Compass

![Alt](images/baas.svg "Baas Diagram"){: caption="Figure X: Secure Automated Backup with Compass" caption-side="bottom"}{: external download="baas.svg"}

## High Availability Design Considerations
{: #ha-considerations}

Local OS High Availability Methodology: **PowerHA Standard Edition**

-   By default, {{site.data.keyword.powerSys_notm}}s are restarted on a different host system if a hardware failure occurs. PowerHA standard edition provides local clustering of “mission critical” workloads.

-   Clustering infrastructure provides the means of creating and managing multiple systems and system resources as a unified entity. Shared resources enable the cluster to continuously provide the essential services to users and applications. PowerHA Cluster Functions:

    -   Heartbeat monitoring for failure detection

    -   Activation/Release of Highly Available Services IP/s

    -   Automatic Activation of Geographically Mirrored Volume Groups

    -   Start/Stop/Monitor of Applications

    -   Failover Resource Groups between Cluster members/sites

-   For more information on POWERHA see https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-ha-dr

-   PowerHA supports resource optimization high availability (ROHA) for AIX instances on {{site.data.keyword.powerSys_notm}}. This is not discussed in this pattern, however, ROHA is another level of automation built into PowerHA that could be considered. It enables clustered instances to automatically adjustment central processing units (CPUs) and memory resources, which allows organizations to be more efficient in their overall use and consumption of those resources. For more information on configuring and using ROHA with {{site.data.keyword.powerSys_notm}}, see [Resource Optimized High Availability in Cloud](/docs/en/powerha-aix/7.2?topic=administering-resources-optimized-high-availability-in-cloud).

The figure below shows a configuration using PowerHA Standard Edition.

![Alt](/images/standardpha.svg "Standard PHA Diagram"){: caption="Figure X: Local PowerHA Architecture" caption-side="bottom"}{: external download="standardpha.svg"}

In this configuration, both nodes have simultaneous access to the shared disks and own the same disk resources. There is no takeover of shared disks if a node leaves the cluster, since the peer node already has the shared volume group varied on.

## Disaster Recovery Design
{: #dr-design}

Disaster Recovery Methodology: **Secondary datacenter with Global Replication Service (GRS)**

The Power Systems Virtual Server service provides a Tier 2 99.95% SLA by default. When a LPAR has an outage within the service, it automatically attempts to restart that lpar on a separate host. For a Tier 3 SLA of 99.99% the workload is distributed across 2 data centers. Supporting a 1 hour RTO and 1 hour RPO, the solution described in this pattern includes

-   Secondary datacenter

-   SAN to SAN replication paired between two IBM {{site.data.keyword.powerSys_notm}} (PowerVS) workspaces in different datacenters.

-   Global Replication services (GRS) is based on industry standards IBM Storwize Global Mirror Change Volume Asynchronous replication technology. For more information see [getting started with GRS](/docs/power-iaas?topic=power-iaas-getting-started-GRS).

-   The GRS involves two sites, over 300 km apart, where storage replication is enabled. These two sites are fixed and mapped into one-to-one relationship mode in both directions. These two sites are fixed and are in replication partnership in both directions. You can create a replication-enabled volume from any site, the site from where the request is initiated contains the master volume and play the role of master. The remote site is auxiliary and contain an auxiliary volume.

-   Key Capabilities

    -   Volume-based async storage replication using Consistency Groups

    -   CG Services to create/delete CGs, add/remove volumes, stop/start, etc.

    -   Volume services (create, delete, retype, etc.) enablement to support mirrored volumes

    -   VM services (deploy, delete, attach, detach, clone, snapshot, etc.) for mirrored volumes.

-   When a write operation is issued to a source volume, the changes are typically propagated to the target volume a few seconds after the data is written to the source volume. However, changes can occur on the source volume before the target volume verifies that it received the change. Because consistent copies of data are formed on the secondary site at set intervals, data loss is determined by the amount of time since the last consistency group was formed. If the system fails, Global Mirror might lose some data that was transmitted when the failure occurred.

-   For additional details regarding GRS see [Global Replication Services Solution using IBM Power Virtual Server](https://cloud.ibm.com/media/docs/downloads/power-iaas/Global_Replication_Services_Solution_using_IBM_Power_Virtual_Server.pdf)
-   Consider the IBM Toolkit for AIX from IBM Technology Expert Labs for disaster recovery automation functions and capabilities on the IBM Cloud by integrating {{site.data.keyword.powerSys_notm}} with the capabilities of GRS. With the Toolkit, simplify and automate operations of the disaster recovery solution. IBM Toolkit for AIX Full System Replication (AIX) enables automate disaster recovery functions and capabilities on the IBM Cloud by integrating {{site.data.keyword.powerSys_notm}} with the capabilities of GRS. Clients can manage their DR environment using their existing AIX skills. Toolkit functions:

    -   Full System Replication for IBM AIX {{site.data.keyword.powerSys_notm}}

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

-   Validate [datacenter pairings](/docs/power-iaas?topic=power-iaas-getting-started-GRS) available for global replication service.

The Figure below illustrates the use of GRS as the DR solution between 2 cloud datacenters

![ALt](/images/grs.svg "GRS Diagram"){: caption="Figure X GRS Architecture" caption-side="bottom"}{: external download="grs.svg"}
