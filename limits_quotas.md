---

copyright:
  years: 2026
lastupdated: "2026-11-10"

keywords: limits, quotas, network throughput, retention, consumer groups, schema

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Understanding {{site.data.keyword.messagehub}} limits and quotas on {{site.data.keyword.cloud_notm}}
{: #kafka_quotas}

[Gen 2]{: tag-purple}

Learn how {{site.data.keyword.messagehub}} uses quotas to control resources like network bandwidth on {{site.data.keyword.cloud_notm}}, and understand the limits for your plan.
{: shortdesc}

## Enterprise plan
{: #limits_enterprise}

### Network throughput
{: #enterprise_throughput}

Throughput represents the sum of bytes per second that can be sent (produced) and received (consumed) in a service instance.

Enterprise Gen 2 provides a baseline allocation of 100 MB/s (50 MB/s producing and 50 MB/s consuming). This represents the minimum continuously available throughput for a typical workload during normal operations and availability zone failovers.

Throughput capacity cannot be scaled down after provisioning. To reduce throughput capacity, you must create a new {{site.data.keyword.messagehub}} service instance.

For more information, see [Scaling {{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-ES_scaling_capacity).

### Partitions
{: #enterprise_partitions}

The maximum number of partitions scales with your allocated capacity. For a standard 100 MB/s Enterprise Gen 2 instance, you can create up to 3000 partitions across all topics.

### Retention and storage
{: #enterprise_retention}

You select storage capacity when provisioning the instance (2 TB, 4 TB, or 6 TB) and can scale storage upward as demands increase.

Storage capacity cannot be scaled down. To reduce storage capacity, you must provision a new instance with less capacity and migrate your data. For more information, see [Scaling {{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-ES_scaling_capacity).

### Other limits
{: #enterprise_limits}

Maximum message size
:   1 MB per message.

Maximum concurrently active Kafka clients
:   1000 connected client applications.

Maximum concurrent network connections
:   10,000 connections.
