---

copyright:
  years: 2024, 2026
lastupdated: "2026-11-09"

keywords:

subcollection: EventStreams-gen2


---




{{site.data.keyword.attribute-definition-list}}

# Understanding service dependencies for {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}} Gen 2
{: #service-dependencies}

Learn about the service dependencies for {{site.data.keyword.messagehub}} Gen 2, including dependency severity and the impact on the service when a dependency is unavailable.
{: shortdesc}

Critical
:   When the dependency is down, the service is down.

Significant
:   When the dependency is down, service features are affected.

Medium
:   When the dependency is down, the service might be affected and a workaround is possible.

Minimal
:   When the dependency is down, the main service features are not affected.

## Data plane for VPC
{: #data-plane-for-vpc}

The following dependencies apply to these deployment locations: Montreal (ca-mon), Chennai - Airtel (in-che), Mumbai (in-mum), Frankfurt (eu-de), and Washington, D.C. (us-east).

| Dependencies | Dependency impacts | Customer provided | Control or data plane | Location of dependency |
|:---|:---|:---|:---|:---|
| {{site.data.keyword.iamlong}} | Access management, Availability, Change management, Instance control, Security compliance | No | vpc-data-plane | ca-mon \n in-che \n in-mum \n eu-de \n us-east |
| vpc-block-storage | Availability | No | vpc-data-plane | ca-mon \n in-che \n in-mum \n eu-de \n us-east |
| Common Data Platform | Availability, Change management, Configuration management, Instance control, Operations, Security compliance | No | vpc-data-plane | ca-mon \n in-che \n in-mum \n eu-de \n us-east |
{: row-headers}
{: caption="Data plane for VPC service dependency information - Critical dependencies" caption-side="bottom"}
{: tab-title="Critical dependencies"}
{: tab-group="service-dependency-data-for-messagehub-gen2-Data-plane-for-VPC"}
{: class="comparison-tab-table"}
{: #critical-deps-data-plane-for-vpc}
{: summary="Use the buttons for the dependency level to change the context of the table. This table has row and column headers. The row headers detail the specific dependent service. The column headers identify the details about the dependency and its impact. To understand the details about each dependency, navigate to the row to find the dependency that you need more information about."}


## Understanding service dependency data
{: #understand-dependency-data}

Review the following frequently asked questions for guidance on service dependency data:

### What is the expected impact to the functions described?
{: #expected-impact}

Each severity tab in the table indicates the impact on your provisioned service if the dependency goes offline. High availability and disaster recovery configurations influence the severity of the impact. Use this information to understand potential issues if an incident affects the dependency.

Regional services are not affected by a single-zone outage because built-in failover redirects traffic to another zone. During failover, a slight performance impact might occur. Global services can fail over to other regions if necessary, further minimizing potential impact.
{: note}

### What services does my service depend on?
{: #dependent-services}

The **Dependencies** column lists the major service-to-service dependencies, including internal dependencies that might not be visible externally.

### What function does the dependency impact?
{: #function-impact}

Functions include access management, availability, change management, configuration management, customer responsibility, disaster recovery, instance control, operations, security compliance, or none. If the dependency goes offline, these functions might be affected. Definitions for each value include:

Access management
:   Authentication, authorization, and governance of user access to the service and service instances.

Availability
:   Availability of the service and service instances.

Change management
:   Deployment, upgrade, and patching of the service and service instances.

Configuration management
:   Configuration deployment, updates, and settings management for the service and service instances.

Customer responsibility
:   Functions that you provide to support specific service instances, such as {{site.data.keyword.keymanagementservicefull_notm}} instances for customer-managed encryption.

Disaster recovery
:   Backup, recovery, and restart of the service and service instances during a disruption.

Instance control
:   Creation, deletion, start, and stop actions on the lifecycle of service instances.

Operations
:   Monitoring and troubleshooting of the service and service instances.

Security compliance
:   Vulnerability management and compliance management for the service and service instances.

None
:   No function is affected.

### What does customer provided mean for dependencies?
{: #customer-provided-dep}

The **Customer provided** column indicates whether you must provide a dependency to enable specific functionality. For example, to configure customer-managed encryption, you provision an instance of {{site.data.keyword.keymanagementservicefull_notm}}. For more information about how to enable features and provision dependent services, see the service documentation.

### Where do dependency services need to be deployed regarding my service?
{: #deploy-dependencies}

In the **Location of dependency** column, you can view whether the dependency is in the same region or deployed to a specific data center. Use this information with the **Control or data plane** column to identify whether your data leaves the region in a standard setup.

To find where your service can be deployed, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).

The table shows a standard cloud deployment. If you use a specialized deployment model such as FedRAMP, details might differ from the information in the table.
{: note}

### Where are the separate control plane and data plane located, if applicable?
{: #separate-plans}

A dependency might have a separate control plane and data plane. In these cases, separate table rows show the location relative to your deployed service instance. These rows can show different impacts and functions. Review the **Control or data plane** column to understand the impact of an outage.

`Same region` means that the dependent services are in the same region as the provisioned instance. Other values might show `data center` or region names if the service must be used from a specific region or zone. If a service is tied to a specific region, and that region goes offline, the service might go offline as well. Review the high availability and disaster recovery documentation of the dependency to determine steps to mitigate potential risks.

## Additional resources
{: #additional-resources}

For more information about the policies that are related to the services, you can refer to the following resources:

* [Service Level Agreement](https://www.ibm.com/support/customer/csol/terms/?id=i126-6605&lc=en){: external}
* [Shared responsibilities for using {{site.data.keyword.cloud_notm}} products](/docs/overview?topic=overview-shared-responsibilities)
* [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region)
