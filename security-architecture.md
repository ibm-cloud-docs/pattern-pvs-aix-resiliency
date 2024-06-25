---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: pattern-pvs-aix-resiliency

keywords:

---

# Architecture decisions for security
{: #security-architecture}



The following are security architecture decisions for the pattern name.

| Architecture decision | Requirement | Alternatives | Decision | Rationale |
|------|------|-------|-------|-------|
| Encrypt data at rest - workloads | Ability to encrypt data at rest | N/A | {{site.data.keyword.powerSys_notm}} storage encryption with provider-managed keys | * {{site.data.keyword.powerSys_notm}} uses IBM FlashSystem Storage with AES-256 (Advanced Encryption Standard) hardware-based encryption \n * For customer-managed keys by selecting a Key Management Service (KMS) for the respective storage service |
| Encrypt data at rest - backups | Ability to encrypt backups | N/A | Storage Encryption with provider-managed keys | * All objects that are stored in {{site.data.keyword.cos_full_notm}} are encrypted by using randomly generated keys and an all-or-nothing-transform (AONT). \n * Secure Automated Backup with Compass includes source system Spectrum Protect client encryption at-rest encryption in-cloud with AES 128 encryption. |
| Identity Access & Role Management (IDM)   | Securely authenticate users for platform services and control access to resources consistently across {{site.data.keyword.cloud_notm}}| N/A | {{site.data.keyword.cloud_notm}} IAM | * Use IAM access policies to assign users, service IDs, and trusted profiles access to resources within the {{site.data.keyword.cloud_notm}} account. \n * Secure Automated Backup with Compass is integrated with {{site.data.keyword.cloud_notm}} IAM.                                                                                                |
| Key Management | Provider Managed Keys | Key Protect HPCS | Key Protect | By default, storage at rest is encrypted with provider managed keys. |
| Privileged Identity and Access Management | Privileged access management services for administrative purposes | BYO Bastion Host BYO Bastion Host with Privileged Access Management (PAM) SW | BYO Bastion host (or Privileged Access Gateway) with PAM SW deployed in Edge VPC \n 2FA Authentication though IBM Security Verify | Securely access remote resources over the private network for management purposes; bastion accessed through SSH. Session recording, tracking all activities, successful or not to note any potential threats |
| Core Network Protection | Strict separation of duties Isolated security zones between environments Isolated, private cloud environment | | Separate VPCs, Subnets, ACLs and Security Groups for workloads in VPC. \n \n Use of virtual firewalls deployed to the Edge/Transit VPC to provide advance FW and routing capabilities between VPC and {{site.data.keyword.powerSys_notm}} | * A design combination using: \n * Separate VPCs (edge, management) connected through transit gateway and, the use of edge firewall capabilities. \n * Subnets, Security Groups and ACLs to create an Edge/Transit VPC design along with isolated LPARs on {{site.data.keyword.powerSys_notm}} |
| Threat detection and response | * Boundary protection: highest level of isolation from external network threats \n * IPS/IDS protection at all ingress/egress \n * Unified Threat Management (UTM) Firewall | BYO Virtual Firewall \n [FortiGate](https://cloud.ibm.com/catalog/content/ibm-fortigate-AP-HA-terraform-deploy-5dd3e4ba-c94b-43ab-b416-c1c313479cec-global) \n [Palo Alto](https://cloud.ibm.com/catalog/content/ibmcloud-vmseries-1.9-6470816d-562d-4627-86a5-fe3ad4e94b30-global) | BYO Virtual Firewall -  [FortiGate](https://cloud.ibm.com/catalog/content/ibm-fortigate-AP-HA-terraform-deploy-5dd3e4ba-c94b-43ab-b416-c1c313479cec-global) \n [Palo Alto](https://cloud.ibm.com/catalog/content/ibmcloud-vmseries-1.9-6470816d-562d-4627-86a5-fe3ad4e94b30-global)  | * Virtual FW on VSI in the Transit/Edge VPC \n * Client preference however recommendation is FortiGate \n * FortiGate supports native HA configuration, IPS and IDS |
{: caption="Table 1. Security architecture decisions" caption-side="bottom"}
