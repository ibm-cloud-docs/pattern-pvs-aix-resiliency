---

copyright:
  years: 2024
lastupdated: "2024-06-20"

subcollection: pattern-pvs-aix-resiliency

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Service Managment Design
{: #service-managment-design}


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

References for service management for IBM Power Virtual server:

  [Monitoring metrics for IBM Power Virtual Server](/docs/power-iaas?topic=power-iaas-monitor-sysdig)

  [Activity tracker](/docs/power-iaas?topic=power-iaas-at-events)

Attention: IBM Cloud Activity Tracker services are deprecated and will no longer be supported as of 30 March 2025. The replacement service, IBM Cloud Logs is planned to be generally available late second quarter 2024. For more information, see [Getting started with IBM Cloud Logs](https://cloud.ibm.com/docs/cloud-logs?topic=cloud-logs-getting-started).
