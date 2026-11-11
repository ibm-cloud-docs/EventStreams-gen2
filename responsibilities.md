---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: responsibilities, incident, operations, cluster management, security, compliance, infrastructure, disaster recovery, client-side libraries, app orchestration

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Your responsibilities when using {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}}
{: #event_streams_responsibilities}

[Gen 2]{: tag-purple}

Learn about management responsibilities and terms when you use {{site.data.keyword.cloud}} {{site.data.keyword.messagehub}}. Understand the breakdown of responsibilities between you and {{site.data.keyword.IBM_notm}}.
{: shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.messagehub_full}}. For the overall terms of use, see [{{site.data.keyword.Bluemix}} Terms and Notices](/docs/overview?topic=overview-terms).

## Incident, operations, and cluster management
{: #incident_ops_cluster}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|Monitor environment|  {{site.data.keyword.messagehub}} performs continuous review, service improvements, code updates, and operational monitoring. This includes automatic, no downtime upgrades of the environment. |  |
|High availability|  {{site.data.keyword.messagehub}} provides high availability by multi-zone region deployment to protect against single points of failure, up to and including a data center loss to achieve the IBM SLA detailed in the {{site.data.keyword.Bluemix}} terms and conditions.  |  |
|Deploy {{site.data.keyword.messagehub}} environment|  {{site.data.keyword.messagehub}} is deployed with IBM recommended best practice configuration options. For example, replication factor, minimum in sync replicas, throttling, and rack awareness.  | |
|Supported client|   | You are responsible for maintaining a supported version of the Kafka client. For more information, see [Support summary for all recommended clients](/docs/EventStreams-gen2?topic=EventStreams-gen2-using_kafka_api#client_summary).|
|Client configuration, deployment, and lifecycle|   | You are responsible for managing client configuration, deployment, and lifecycle by following [IBM best practice documentation](/docs/overview?topic=overview-shared-responsibilities#software-packages).|
|Cluster management|  | You are responsible for managing the provided resource capacity of your clusters across your organizational user base. |
{: caption="Responsibilities for incident, operations, and cluster management" caption-side="bottom"}

## Security and regulation compliance
{: #security_compliance}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|Maintain controls| {{site.data.keyword.messagehub}} maintains controls commensurate with various industry compliance standards for which the service is certified.  |  |
|IBM Cloud Identity and Access Management (IAM)| {{site.data.keyword.messagehub}} provides security and access control service with IBM Cloud Identity and Access Management (IAM).  |  |
|Security and vulnerability patch updates to cluster| {{site.data.keyword.messagehub}} applies the provided security and vulnerability patch updates to the client cluster, according to IBM X-Force timeframes.  | |
|Manage users and access|   | You are responsible for managing your organizational account users and related access to the {{site.data.keyword.messagehub}} instance.|
|Compliance controls|  | You are responsible for maintaining your organizational compliance controls.|
{: caption="Responsibilities for security and regulation compliance" caption-side="bottom"}

## {{site.data.keyword.cloud_notm}} infrastructure and managing the environment
{: #cloud_infrastructure}

| Task | {{site.data.keyword.cloud_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|Deploy| {{site.data.keyword.messagehub}} deploys an instance that consists of all required {{site.data.keyword.messagehub}} components and storage.  | You are responsible for deciding which region to deploy, selecting the cluster capacity, and setting any cluster configuration parameters available at deployment, for example private and public endpoints, IP allowlisting. |
|Monitor and repair| {{site.data.keyword.messagehub}} monitors and repairs infrastructure nondisruptively.  | |
|Manage and configure|   | You are responsible for using the provided APIs, CLI, or console to manage topics and configuration. |
{: caption="Responsibilities for {{site.data.keyword.IBM_notm}} infrastructure and managing the environment" caption-side="bottom"}

## Disaster recovery
{: #disaster-recovery}

If you configured your {{site.data.keyword.messagehub}} instance in a multi-zone region, a regional disaster is unlikely. However, plan for such circumstances. If such an event makes your instance unavailable, and you did not set up a remote DR instance, consider configuring a new instance in a new region.

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|Disaster recovery|   | You are responsible for maintaining and running a disaster recovery plan if the service is lost. The plan might include provisioning a new cluster in a new region if a disaster occurs and restoring any configuration or data to that cluster, or, pre-provisioning a cluster in another region and using the {{site.data.keyword.messagehub}} [Mirroring feature](/docs/EventStreams-gen2?topic=EventStreams-gen2-event_streams_responsibilities#disaster-recovery).|
|Mirroring|   | You can use the {{site.data.keyword.messagehub}} [Mirroring feature](/docs/EventStreams-gen2?topic=EventStreams-gen2-event_streams_responsibilities#disaster-recovery), or manage your own mirroring solution.|
|Message payload data backup|   | You are also responsible for the backup of message payload data. Although this data is replicated across multiple Kafka brokers within a cluster, which protects against most failures, this replication does not cover a location-wide failure.|
|Topic name and data backup|   | Store your topic names and topic configuration data in the same repository as your application source code. You can then restore the topics into a new cluster if a disaster occurs. |
|Schema Registry|   | Store your schema in the same repository as your application source code. You can then restore the schema into a new cluster if a disaster occurs. |
{: caption="Responsibilities for disaster recovery" caption-side="bottom"}

## App orchestration
{: #app_orchestration}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
|Integrate with select third-party partnership technologies| {{site.data.keyword.messagehub}} provides integrations with select third-party partnership technologies, such as IBM Cloud Activity Tracker with LogDNA.   |
|Provide service binding to other {{site.data.keyword.IBM_notm}} services| {{site.data.keyword.messagehub}} provides the capability for service binding to other {{site.data.keyword.IBM_notm}} services.  | |
|Manage, integrate, and monitor|   | You are responsible for using the provided tools and features to manage the lifecycle of your applications, to integrate with other services, and to monitor the health of the application (for example, Availability Monitoring).|
{: caption="Responsibilities for app orchestration" caption-side="bottom"}

## Support of client-side libraries
{: #support_client_libraries}

{{site.data.keyword.IBM_notm}} does not own Kafka client-side libraries and cannot commit to provide fixes if issues are found in them. Client-side libraries include the official Apache Kafka clients (Consumer, Producer, Connect, and Streams), as well as third-party clients, libraries, tools, and connectors.

As an active member of the Kafka community, {{site.data.keyword.IBM_notm}} works, on a best effort basis, with communities that own client-side libraries to help develop and release fixes for critical issues.
