---

copyright:
  years: 2025
lastupdated: "2025-12-15"

keywords: limits, quotas, network throughput, retention, consumer groups, schema

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# How {{site.data.keyword.messagehub}} uses limits and quotas
{: #kafka_quotas

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

{{site.data.keyword.messagehub}} uses quotas to control the resources, such as network bandwidth, that a service can consume. The types and levels of quotas depend on the plan being used.
{: shortdesc}

## Enterprise plan
{: #limits_enterprise}

### Network throughput
{: #enterprise_throughput}

The figure represents the minumim throughput that can be expected to be continually available for a typical workload and should be used for capacity planning. It takes in to account the possible impact of operational actions, such as internal updates or failure modes (for example, the loss of an availability zone). In normal operation, up to 30% more may be possible, but should not be assumed to be continuously available. The maximum achievable throughput can also vary according to the configuration of the workload.

| Throughput (continously available) |
|------------------------------------|
|100 MB/s (50 MB/s producing and 50 MB/s consuming)|
{: caption="Network throughput capacity on Enterprise Gen2" caption-side="top"}

Throughput is expressed as the number of bytes per second that can be both sent and received in a service instance. This capacity can be selected when the service instance is created.

Throughput capacity cannot be scaled down. To move to a lower throughput capacity would require creating a new {{site.data.keyword.messagehub}} service instance at the lower capacity unit.

For more information, see [Scaling {{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-ES_scaling_capacity).

### Partitions
{: #enterprise_partitions}

The maximum number of partitions scales in proportion to the selected throughput. For 100MB/s, 3000 partitions can be used.

### Retention
{: #enterprise_retention}

The storage capacity can be selected when the service instance is created, and later scaled as demands increase. Storage capacity is dependent upon the configured throughput capacity. For more information, see [Scaling {{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-ES_scaling_capacity) on storage capacity options.

Storage capacity cannot be scaled down. To move to a lower storage capacity would require creating a new {{site.data.keyword.messagehub}} service instance at the lower capacity unit.

### Other limits
{: #enterprise_limits}

* Maximum message size: 1 MB
* Maximum concurrently active Kafka clients: 10,000
* Maximum concurrent connections: 100,000
