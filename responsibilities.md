---

copyright:
  years: 2015, 2025
lastupdated: "2025-09-15"

keywords: responsibilities, incident, operations, cluster management, security, compliance, infrastructure, disaster recovery, client-side libraries, app orchestration

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Understanding your responsibilities when you use {{site.data.keyword.messagehub}}
{: #event_streams_responsibilities}

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.messagehub_full}}. For a high-level view of the service types in {{site.data.keyword.Bluemix}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{: shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.messagehub_full}}. For the overall terms of use, see [{{site.data.keyword.Bluemix}} Terms and Notices](/docs/overview?topic=overview-terms).
  
## Incident, operations, and cluster management 
{: #incident_ops_cluster}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|Monitor environment|  {{site.data.keyword.messagehub}} performs continuous review, service improvements, code updates, and operational monitoring. This includes automatic, no downtime upgrades of the environment. |  |
|High availability|  {{site.data.keyword.messagehub}} provides high availability by multi-zone region deployment  to protect against single points of failure, up to and including a data center loss to achieve the IBM SLA detailed in the {{site.data.keyword.Bluemix}} terms and conditions.  |  |
|Deploy {{site.data.keyword.messagehub}} environment|  {{site.data.keyword.messagehub}} is deployed with IBM recommended best practice configuration options.  For example, replication factor, minimum in sync replicas, throttling, and rack awareness.  | |
|Supported client|   | Customer is responsible for maintaining a supported version of the Kafka client. For more information, see [Support summary for all recommended clients](/docs/EventStreams?topic=EventStreams-kafka_using#client_summary).|
|Client configuration, deployment, and lifecycle|   | Customer is responsible for managing client configuration, deployment, and lifecycle by following [IBM best practice documentation](/docs/overview?topic=overview-shared-responsibilities#software-packages).|
|Cluster management|  | Customer is responsible for managing the provided resource capacity of their clusters across their organizational user base. |
{: caption="Responsibilities for incident, operations, and cluster management" caption-side="bottom"}

## Security and regulation compliance
{: #security_compliance}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|Maintain controls| {{site.data.keyword.messagehub}} maintains controls commensurate with various industry compliance standards for which we are certified.  |  |
|IBM Cloud Identity and Access Management (IAM)| {{site.data.keyword.messagehub}} provides security and access control service with IBM Cloud Identity and Access Management (IAM).  |  |
|Security and vulnerability patch updates to cluster| {{site.data.keyword.messagehub}} applies the provided security and vulnerability patch updates to the client cluster, according to IBM X-Force timeframes.  | |
|Manage users and access|   | Customer is responsible for managing your organizational account users and related access to the {{site.data.keyword.messagehub}} instance.|
|Compliance controls|  | Customer is responsible for maintaining your organizational compliance controls.|
{: caption="Responsibilities for security and regulation compliance" caption-side="bottom"}

## {{site.data.keyword.cloud_notm}} infrastructure and managing the environment
{: #cloud_infrastructure}

| Task | {{site.data.keyword.cloud_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|Deploy| {{site.data.keyword.messagehub}} deploys an instance that consists of all required {{site.data.keyword.messagehub}} components and storage.  | Customer is responsible for deciding which region to deploy, selecting capacity of cluster and setting any cluster configuration parameters available at deployment, for example private and public endpoints, IP allowlisting. |
|Monitor and repair| {{site.data.keyword.messagehub}} monitors and repairs infrastructure nondisruptively.  | |
|Manage and configure|   | Customer is responsible for using the provided APIs, CLI, or console to manage topics and configuration. |
{: caption="Responsibilities for {{site.data.keyword.IBM_notm}} infrastructure and managing the environment" caption-side="bottom"}

## Disaster recovery
{: #disaster-recovery}

If you configured your {{site.data.keyword.messagehub}} instance in a multi-zone region, a regional disaster is unlikely. However, we recommend that customers plan for such circumstances. If due to such an event, a customer's instance is no longer available (and a remote DR instance was not already set up), the customer should consider configuring a new instance in a new region.  

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|Disaster recovery|   | Customer is responsible for maintaining and executing a disaster recovery plan if the service is lost. The plan might include provisioning a new cluster in a new region if a disaster occurs and restoring any configuration or data to that cluster, or, pre-provisioning a cluster in another region and using the {{site.data.keyword.messagehub}} [Mirroring feature](/docs/EventStreams?topic=EventStreams-event_streams_responsibilities#mirroring_responsibilities).|
|Mirroring|   | Customer can use the {{site.data.keyword.messagehub}} [Mirroring feature](/docs/EventStreams?topic=EventStreams-event_streams_responsibilities#mirroring_responsibilities), or choose to manage their own mirroring solution.|
|Message payload data backup|   | The customer is also responsible for the backup of message payload data. Although this data is replicated across multiple Kafka brokers within a cluster, which protects against most failures, this replication does not cover a location-wide failure.|
|Topic name and data backup|   | It is recommended good practice that a customer stores their topic names and configuration data for those topics in the same repository as their application source code. This way the topics can be restored into a new cluster if a disaster occurs. |
|Schema Registry|   | It is recommended good practice that a customer stores their schema in the same repository as their application source code. This way the schema can be restored into a new cluster if a disaster occurs. |
{: caption="Responsibilities for disaster recovery" caption-side="bottom"}

## App orchestration
{: #app_orchestration}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|Integrate with select third-party partnership technologies| {{site.data.keyword.messagehub}} provides integrations with select third-party partnership technologies, such as IBM Cloud Activity Tracker with LogDNA.   |
|Provide service binding to other {{site.data.keyword.IBM_notm}} services| {{site.data.keyword.messagehub}} provides the capability for service binding to other {{site.data.keyword.IBM_notm}} services.  | |
|Manage, integrate, and monitor|   | Customer is responsible for using the provided tools and features to manage the lifecycle of customer-owned applications, for integrating with other services, and monitoring the health of the application (for example, Availability Monitoring).|
{: caption="Responsibilities for app orchestration" caption-side="bottom"}

## Mirroring
{: #mirroring_responsibilities}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|Clusters| Checking clusters are viable mirroring pairs|Provisioning both clusters. |
|Setup  | Setting up mirroring    |  Setting up service to service binding. |
|Enablement and monitoring | Monitoring health and SLA of mirroring link.  |  Requesting enablement by using the **service-instance-update** CLI comamnd.  |
|Metrics and applications  | Providing metrics to enable customer to understand the current recovery point objective (RPO).   |  Enabling applications to switch clusters.  |
|IAM  |    | Setting up required access policies.    |
|Failover  |  Disabling any existing mirroring configurations.  | Deciding when to fail over and failover applications. |
|Failback  | Reconfiguring mirroring |Developing and executing failback plan. Coordinating with {{site.data.keyword.IBM_notm}} to reconfigure mirroring. |
{: caption="Responsibilities for mirroring" caption-side="bottom"}

## Support of client-side libraries
{: #support_client_libraries}

{{site.data.keyword.IBM_notm}} does not own Kafka client-side libraries and cannot commit to provide fixes in case issues are found in them. Client-side libraries include the official Apache Kafka clients (Consumer, Producer, Connect, and Streams), as well as third-party clients, libraries, tools, and connectors.

As an active member of the Kafka community, {{site.data.keyword.IBM_notm}} works, on a best effort basis, with communities that own clients-side libraries to help develop and release fixes for critical issues.
