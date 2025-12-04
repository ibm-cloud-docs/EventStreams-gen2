---

copyright:
  years: 2025
lastupdated: "2025-12-04" 

keywords: activity , cloud logs

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cloudaccesstrailshort}} events
{: #at_events}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

Use the {{site.data.keyword.logs_full}} service to track how users and applications interact with the {{site.data.keyword.messagehub}} service on the Standard and Enterprise plans in {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

The {{site.data.keyword.logs_full_notm}} service records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. For more information, see [{{site.data.keyword.logs_full_notm}}](/docs/cloud-logs?topic=cloud-logs-at_events){: external}.

Events are formatted according to the Cloud Auditing Data Federation (CADF) standard. For further details of the information they include, see [CADF standard](/docs/atracker?topic=atracker-about).

## Topic events
{: #topic-events}

{{site.data.keyword.messagehub}} instances that are on the Enterprise plan or the Standard plan automatically generate topic events.

The following table lists the topic events:

| Action | Description |
|:-------|:------------|
| event-streams.topic.create | An event is created when you create a topic.|
| event-streams.topic.delete | An event is created when you delete a topic.|
| event-streams.topic.update | An event is created when you update a topic's configuration or increase partitions.|
{: caption="{{site.data.keyword.messagehub}} topic events" caption-side="top"}

Additional information about topic configuration is logged in the update and delete events,  for example partitions, `retentionMs`, and `segmentMs`.

## Message audit events
{: #message-events}

You can enable message audit events on a per topic basis for {{site.data.keyword.messagehub}} instances that are on the `Enterprise plan`.
Also, see [How to enable message audit events](/docs/EventStreams?topic=EventStreams-at_events#enable-message-events).

The following table lists the message audit events:

| Action | Description |
|:-------|:------------|
| event-streams.message.read | An event is created when message audit is enabled on a topic and a consumer is reading data from the topic.|
| event-streams.message.write | An event is created when message audit is enabled on a topic and a producer is writing data to the topic.|
| event-streams.message.delete | An event is created when message audit is enabled on a topic and records are deleted from the topic. Records deletion because of retention policy does not generate.|
{: caption="{{site.data.keyword.messagehub}} message events" caption-side="top"}

{{site.data.keyword.messagehub}} can sustain high request rates, so not every request triggers an event. Instead, events are aggregated by initiator (user ID or service ID), host (IP address), operation (read, write, delete), outcome (success or failure), and topic over a 1-hour period.

## Other events
{: #other-events}

{{site.data.keyword.messagehub}} instances that are on the `Enterprise plan` automatically generate events so that you can track activity on your service.

| Action | Description |
|:-------|:------------|
| event-streams.storage-key.read | An event is created when access to the disk encryption key in {{site.data.keyword.keymanagementserviceshort}} changed. If the outcome of this event is `success`, access to the disk encryption key is restored, and the {{site.data.keyword.messagehub}} instance is available for use. If the outcome is `failure`, access to the disk encryption key was withdrawn, and the {{site.data.keyword.messagehub}} instance is not available for use. |
| event-streams.storage-key.update | The disk encryption key in {{site.data.keyword.keymanagementserviceshort}} was rotated and the {{site.data.keyword.messagehub}} instance was updated to use the new key. |
| event-streams.schema.create | A schema  or schema version was created or updated in the {{site.data.keyword.messagehub}} schema registry for the enterprise instance either through the administration API or through the Confluent Serdes.
| event-streams.schema.delete | A schema or schema version was deleted from the {{site.data.keyword.messagehub}} schema registry for the enterprise instance.|
| event-streams.schema-rule.create | A new rule or global rule was created in the {{site.data.keyword.messagehub}} schema registry for the enterprise instance.|
| event-streams.schema-rule.update | An existing rule or global rule was updated in the {{site.data.keyword.messagehub}} schema registry for the enterprise instance.|
| event-streams.schema-rule.delete | A rule was deleted in the {{site.data.keyword.messagehub}} schema registry for the enterprise instance.|
{: caption="{{site.data.keyword.messagehub}} events" caption-side="top"}

## Where to view the events
{: #ui}

{{site.data.keyword.logs_full_notm}} events are available in the {{site.data.keyword.logs_full_notm}} **account domain** that is available in the {{site.data.keyword.cloud_notm}} location (region) where the events are generated.

Events that are generated by an instance of the {{site.data.keyword.messagehub}} service are automatically forwarded to the {{site.data.keyword.logs_full_notm}} service instance that is available in the same location.

{{site.data.keyword.logs_full_notm}} can have only one instance per location. To view events, you must access the web UI of the {{site.data.keyword.logs_full_notm}} service in the same location where your service instance is available. For more information, see [Launch the Cloud Logs UI](/docs/cloud-logs?topic=cloud-logs-getting-started#gs-access-ui){: external}.

## How to enable message audit events
{: #enable-message-events}

Message audit events can be enabled on a per topic basis. To do so, complete the following steps:

1. Install {{site.data.keyword.messagehub}} CLI plug-in v2.3 or later:

   ```sh
   ibmcloud plugin install event-streams
   ```
   {: pre}

2. Enable message audit on an existing topic:

   ```sh
   ibmcloud es topic-update <topic-name> --config message.audit.enable=true
   ```
   {: pre}

   Or create a new topic with message audit enabled:

   ```sh
   ibmcloud es topic-create <topic-name> --partitions <number-of-partitions> --config message.audit.enable=true
   ```
   {: pre}

After the topic's message audit config is updated, it takes about 5 minutes for message audit events to show up in {{site.data.keyword.cloudaccesstrailshort}}.
{: important}

Additionally, be aware of the implications of enabling message audit events:

1. An internal Kafka topic is used for streaming events to {{site.data.keyword.logs_full_notm}}, thus it uses a small amount of the cluster's network bandwidth and storage. Typically throughput is less than 1 KB/s, and storage does not exceed 1 GB.

2. Because more events are sent to {{site.data.keyword.logs_full_notm}}, enabling message audit events incurs extra storage costs for {{site.data.keyword.logs_full_notm}}. Each event's size is about 1 KB, see the following rough estimation of how much storage it takes. Assuming that the cluster has 100 topics, each topic has 10 clients actively producing and consuming, and each client runs on three different locations. It then generates 100x10x3=3000 events per hour, so 2 GB per month.
