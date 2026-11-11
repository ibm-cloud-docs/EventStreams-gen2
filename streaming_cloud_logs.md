---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: cloud logs, streaming

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Stream {{site.data.keyword.logs_full_notm}} data with {{site.data.keyword.messagehub}}
{: #streaming_data_cloud_logs}

[Gen 2]{: tag-purple}

Use {{site.data.keyword.messagehub}} to stream {{site.data.keyword.logs_full_notm}} data to Security Information and Event Management (SIEM) tools for security and log management.
{: shortdesc}

When you enable streaming on an {{site.data.keyword.logs_full}} instance, you configure {{site.data.keyword.logs_full}} to send data to an {{site.data.keyword.messagehub}} instance. Then, you can configure Kafka Connect to consume the data and forward it to your destination tool. After {{site.data.keyword.messagehub}} stores the data, you can configure any application or service to create a subscription and take action on the streamed log data.

For more information, see [Streaming data from an {{site.data.keyword.logs_full_notm}} instance](/docs/cloud-logs?topic=cloud-logs-streaming){: external}.

For step-by-step instructions for integrating {{site.data.keyword.logs_full_notm}} with {{site.data.keyword.messagehub}}, see [Integrating {{site.data.keyword.logs_full_notm}} with {{site.data.keyword.messagehub}}](/docs/cloud-logs?topic=cloud-logs-streaming-config){: external}.
