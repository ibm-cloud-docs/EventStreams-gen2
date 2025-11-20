---

copyright:
  years: 2025
lastupdated: "2025-11-20"

keywords: HA for Event Streams, DR for Event Streams, Event Streams recovery time objective, high availability, disaster recovery

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}



# Understanding high availability and disaster recovery for {{site.data.keyword.messagehub}}
{: #eventstreams-ha-dr}





[High availability](#x2284708){: term} (HA) is the ability for a service to remain operational and accessible in the presence of unexpected failures. [Disaster recovery](#x2113280){: term} is the process of recovering the service instance to a working state.
{: shortdesc}



{{site.data.keyword.messagehub_full}} is a global service and you can find the available region and data center locations in the [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region) documentation. As a global service, {{site.data.keyword.messagehub}}} fulfills the defined [Service Level Objectives (SLO)](/docs/resiliency?topic=resiliency-slo) using the Enterprise Gen2 plans. **N.B. during Beta no SLOs apply**. The SLO is not a warranty and {{site.data.keyword.IBM}} will not issue credits for failure to meet an objective.

## High availability architecture
{: #ha-architecture}



### High availability features
{: #ha-features}

{{site.data.keyword.messagehub}} supports the following high availability features:



| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| Multi-zone region reployment  | Distributed across three availability zones for fault tolerance and high availability | In {{site.data.keyword.messagehub}}, each partitions data is distributed across three availability zones (for MZR deployments) to ensure business continuity in the event of the loss of an availability zone data. |
| Minimum in-sync replicas | A minimum of two in-sync replicas is required at all times | {{site.data.keyword.messagehub}} continuosly monitors and ensures that at least two replicas are synchronized across availability to make sure that messages are not lost in the case of a broker or zone failure, ensuring that critical data remains durable.|
{: caption="HA features for {{site.data.keyword.messagehub}}" caption-side="bottom"}


## Disaster recovery architecture
{: #disaster-recovery-intro}



NEED disaster recovery architecture diagram! DESIGN TEAM




### Disaster recovery features
{: #dr-features}

The Enterprose Gen2 initial release does not support Mirroring as a managed feature, although you can set up your own Kafka Mirroring solution.



| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| Mirroring | Mirroring for cluster replication | {{site.data.keyword.messagehub}} Enterprose Gne2 initial release does not provide provides a mirroring feature, however a customer can choose to manager their own mirroring solution to enable messages to be continuously copied into a second instance. |
{: caption="DR features for {{site.data.keyword.messagehub}}" caption-side="bottom"}

#### Mirroring for {{site.data.keyword.messagehub}}
{: #dr-feature-1}

Mirroring enables messages in one {{site.data.keyword.messagehub}} service instance to be continuously copied to a second instance. Application resilience can be improved by using mirroring, so if the first service instance becomes unavailable, applications can reconnect to the second instance and continue their normal operation.

GREY THIS SECTION OUT or REMOVE UNTIL SUPPORTED???

This feature is part of the fully managed service and can only be used between service instances that use the {{site.data.keyword.messagehub}} Enterprise plan.

1. Features of mirroring:
- Mirror topics, message data, and consumer group offsets between two {{site.data.keyword.messagehub}} service instances, which can be provisioned in different {{site.data.keyword.cloud}} accounts.
- SLA of 99.99% availability, consistent with the {{site.data.keyword.messagehub}} service.
- Can be monitored using {{site.data.keyword.monitoringlong}}.
2. Limitations of mirroring:
- Unidirectional: Data can only be mirrored in one direction at a time between a pair of service instances. This means that mirroring offers an "active-passive" style of high availability, not an "active-active" style.
- Asynchronous: Messages must be successfully produced to the source instance before they can be mirrored to the target instance. This means that when a failure occurs, some message data may be lost.
- At-least-once message consumption: When a consumer moves between instances, it may need to reprocess messages that it has already processed.-->



### Planning for disaster recovery
{: #features-for-disaster-recovery}

The disaster recovery steps must be practiced regularly. As you build your plan, consider the following failure scenarios and resolutions.



| Failure | Resolution |
| -------------- | -------------- |
| Hardware failure (single point) | {{site.data.keyword.messagehub}} is resilient to a single point of hardware failure within a zone - no configuration required. |
| Zone failure | An {{site.data.keyword.messagehub}} instance that is deployed in a multi-zone region is resilient to the failure of a single zone - no configuration required.|
| Data corruption | {{site.data.keyword.messagehub}} does not include any built-in mechanisms to recover from data corruption. You are required to plan for such circumstances as a part of a disaster recovery plan and may need to use a mirroring feature or configure a new instance. |
| Regional failure | If you configured your {{site.data.keyword.messagehub}} instance in a multi-zone region, a regional disaster is unlikely. If a regional failure does occur, you are required to configure a new instance in another region. For more information, see [Understanding your responsibilities](/docs/EventStreams?topic=EventStreams-event_streams_responsibilities). |
{: caption="Disaster recovery scenarios for {{site.data.keyword.messagehub}}" caption-side="bottom"}





## Change management
{: #change-management}



It is important to understand the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.messagehub}}. The [customer responsibilities](/docs/EventStreams?topic=EventStreams-event_streams_responsibilities) page helps as a starting point to create a plan for high availability and disaster recovery.

As part of disaster recovery, it is recommended that you grant users and processes the IAM roles and actions with the least privilege required for their work. For more information, see [How can I prevent accidental deletion of services?](/docs/resiliency?topic=resiliency-dr-faq#prevent-accidental-deletion).

All {{site.data.keyword.messagehub}} plans can recover a deleted instance within reclamation period of three days, after which the data is irreversibly destroyed. You can check the status of a reclamation, and force or cancel a scheduled reclamation by using  the [IBM Cloud CLI](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations).



If {{site.data.keyword.messagehub}} can’t restore the service instance, you must restore as described in [Mirroring in a disaster recovery scenario](/docs/EventStreams?topic=EventStreams-disaster_recovery_scenario).

## How {{site.data.keyword.IBM_notm}} maintains services
{: #ibm-service-maintenance}


All upgrades follow the {{site.data.keyword.IBM_notm}} service best practices and have a recovery plan and rollback process in-place. Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that are handled by [client availability retry logic](/docs/resiliency?topic=resiliency-high-availability-design#client-retry-logic-for-ha). Changes are rolled out sequentially, region by region and zone by zone within a region. Updates are backed out at the first sign of a defect.


Complex changes are enabled and disabled with feature flags to control exposure.


Changes that impact customer workloads are detailed in notifications. For more information, see [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and [release notes](/docs/EventStreams?topic=EventStreams-event-streams-relnotes) that impact Event Streams.
