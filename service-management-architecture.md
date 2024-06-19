---

copyright:
  years: 2024
lastupdated: "2024-06-19"

subcollection: <repo-name>

keywords:

---

# Architecture decisions for service management
{: #service}

The following sections summarize the architecture decisions for service management for the web app multi-zone resiliency pattern.


| **Architecture decision**                                   | **Requirement**                                                                                                                                    | **Alternatives**                                                                                    | **Decision**                                                                              | **Rationale**                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Operational Monitoring of Cloud infrastructure and services | Monitor system health to detect issues that might impact the availability of the system and determine the need to failover to an alternative site. | IBM Cloud Monitoring  BYO Tool                                                                      | IBM Cloud Monitoring                                                                      | IBM Cloud Monitoring provides visibility, alerting, and troubleshooting for PowerVS workspaces.  IBM Cloud Monitoring collects and monitors operational metrics for cloud infrastructure as well as the cloud platform and services and provides a single view for all metrics Monitoring of PowerVC metrics with IBM Cloud Monitoring dashboards that are operated by Sysdig in partnership with IBM Cloud Native tools such as NMON can also be used. |
| Logging of Cloud infrastructure and services                | Log Monitoring of Cloud infrastructure and services                                                                                                | [IBM Cloud Logs](https://cloud.ibm.com/docs/cloud-logs?topic=cloud-logs-getting-started) BYO Tool   | [IBM Cloud Logs](https://cloud.ibm.com/docs/cloud-logs?topic=cloud-logs-getting-started)  | Recommended tool for infra Logging for any non-VMWare workloads. Ingestion and integration with other tools for diagnosis and alerts Ingestion and integration with other tools for diagnosis and alerts IBM Cloud Logs collects operational logs from applications, platform resources, and infrastructure and provides interfaces to view and analyze all logs.                                                                                       |
| Alerting                                                    | Provide a mechanism to identify and send notifications about operational issues that are found across application and infrastructure               | IBM Cloud Monitoring + IBM Cloud Logging + Event Notifications BYO Tool                             | IBM Cloud Monitoring + IBM Cloud Logging + Event Notifications                            | IBM Cloud Monitoring and IBM Cloud Logging support the configuration of alerts to detect operational issues and send notifications to targeted channels. Event Notifications are used to route the alert events to service destinations to automate response actions. Full stack observability for application and infrastructure when combined with Pager Duty + ServiceNow (SNOW) + Customer SIEM                                                     |

## Architecture decisions for monitoring
{: #monitoring}

| Architecture decision | Requirement | Option | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Operational monitoring of cloud infrastructure and services | Monitor system health to detect issues that might impact the availability of the system and application. | - IBM Cloud Monitoring \n - BYO Monitoring Tool | IBM Cloud Monitoring | IBM Cloud Monitoring collects and monitors operational metrics for cloud infrastructure as well as the cloud platform and services and provides a single view for all metrics |
| Operational monitoring of applications | Monitor app health to detect issues that might impact the availability of the app. | - IBM Cloud Monitoring \n - Instana (SaaS) \n - BYO Monitoring Tool | IBM Cloud Monitoring + Instana (SaaS) | Instana is used along with IBM Cloud Monitoring to get more application performance metrics and automate application performance management. Instana provides data and actionable insights to monitor the applications and automate root-cause analysis. |
{: caption="Table 1. Architecture decisions for monitoring" caption-side="bottom"}

## Architecture decisions for logging
{: #logging}

| Architecture decision | Requirement | Option | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Log monitoring of cloud infrastructure and services | Monitor operational logs to detect issues that might impact the availability of the system and application. | - IBM Cloud Logging \n - BYO Logging Tool | IBM Cloud Logging | IBM Cloud Logging collects operational logs from applications, platform resources, and infrastructure and provides interfaces to view and analyze all logs. |
| Log monitoring of web app | Monitor application operational logs to detect issues that might impact the availability of the app.| - IBM Cloud Logging \n - Application Logging Tool \n - BYO Logging Tool | IBM Cloud Logging + Application Logging Tool | Use the Application Logging Tool to send application logs to IBM Cloud Logging and aggregate application-specific log details. |
| Log monitoring of DB | Monitor database logs to detect issues that might impact the availability of the database.| - IBM Cloud Logging \n - DB Tools \n - BYO Logging Tool | IBM Cloud Logging + Application Logging Tool | Use the DB tools along with IBM Cloud Logging to get more DB-specific log information. |
{: caption="Table 2. Architecture decisions for logging" caption-side="bottom"}

