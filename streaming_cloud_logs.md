---

copyright:
  years: 2025
lastupdated: "2025-12-16"

keywords: cloud logs, streaming

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Streaming data from {{site.data.keyword.logs_full_notm}}
{: #streaming_data_cloud_logs}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

{{site.data.keyword.messagehub}} can be used to stream data from {{site.data.keyword.logs_full}} to other corporate tools, such as Security Information and Event Management (SIEM) tools.
{: shortdesc}

When you enable streaming on an {{site.data.keyword.logs_full}} instance, you configure {{site.data.keyword.logs_full}} to send data to an {{site.data.keyword.messagehub}} instance. Then, you can configure Kafka Connect to consume the data and forward it to your destination tool. After the data is persisted within {{site.data.keyword.messagehub}}, you can configure any application, or service to create a subscription and take action on log data that is streamed.

For more information, see [Streaming data from an IBM Cloud Logs instance](/docs/cloud-logs?topic=cloud-logs-streaming){: external}.

For step-by-step instructions for integrating IBM Cloud Logs with Event Streams, see [Integrating IBM Cloud Logs with Event Streams](/docs/cloud-logs?topic=cloud-logs-streaming-config){: external}.
