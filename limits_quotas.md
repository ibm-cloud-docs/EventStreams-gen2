---

copyright:
  years: 2015, 2025
lastupdated: "2025-09-15"

keywords: limits, quotas, network throughput, retention, consumer groups, schema

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# How {{site.data.keyword.messagehub}} uses limits and quotas
{: #kafka_quotas}

{{site.data.keyword.messagehub}} uses quotas to control the resources, such as network bandwidth, that a service can consume. The types and levels of quotas depend on whether you use the Lite, Standard, or Enterprise plan.
{: shortdesc}

## Lite plan
{: #limits_lite}

### Network throughput
{: #lite_throughput}

A recommended maximum for network throughput is 100 KB per second. Throughput is expressed as the number of bytes per second that can be both sent and received in a cluster.

The recommended figure is based on a typical workload and considers the possible impact of operational actions such as internal updates or failure modes, like the loss of an availability zone. If the average throughput exceeds the recommended figure, a loss in performance might be experienced during these conditions.

### Partitions
{: #lite_partitions}

One partition for each service instance.

### Retention
{: #lite_retention}

A maximum of 100 MB for the partition. 

### Consumer groups
{: #lite_consumer_groups}

A maximum of 10 consumer groups. When the limit is exceeded, the `GROUP_MAX_SIZE_REACHED` error is returned to the client.

### Other limits
{: #lite_limits}

* Maximum message size: 1 MB
* Maximum concurrently active Kafka clients: 5
* Maximum request rate [HTTP Produce API]: 5 per second
* Maximum request rate [HTTP Admin API]: 10 per second
* Maximum record key size when you use REST Producer API is 4 K.
* Maximum record value size when you use REST Producer API is 64 K.

## Standard plan
{: #limits_standard}

### Network throughput
{: #standard_throughput}

The maximum throughput for each service instance equates to 1 MB per second per partition up to a maximum of 20 MB per second. For example, for a service instance with 10 partitions, the maximum throughput is 10 MB per second and for 30 partitions it is 20 MB per second.

The throughput is measured separately for producers and consumers. When exceeded, throttling is applied by slightly delaying the responses to requests, effectively applying a gentle brake to producers and consumers until the bandwidth is reduced.

### Partitions
{: #standard_partitions}

One hundred partitions for each service instance.

### Retention
{: #standard_retention}

A maximum of 1 GB for each partition.

### Consumer groups
{: #standard_consumer_groups}

A maximum of 1000 consumer groups. When the limit is exceeded, the `GROUP_MAX_SIZE_REACHED` error is returned to the client.

### Other limits
{: #standard_limits}

* Maximum message size: 1 MB
* Maximum concurrently active Kafka clients: 500
* Maximum concurrent connections: 3000
* Maximum request rate [HTTP Produce API]: 100 per second
* Maximum request rate [HTTP Admin API]: 10 per second
* Maximum record key size when you use REST Producer API is 4 K. 
* Maximum record value size when you use REST Producer API is 64 K.

## Enterprise plan
{: #limits_enterprise}

### Network throughput
{: #enterprise_throughput}

Network throughput capacity is based on the peak maximum. Each peak maximum has a recommended maximum for typical production workloads.

| Peak Maximum | Recommended maximum | 
|--------------|-----------------------|
|150 MB/s (75 MB/s producing and 75 MB/s consuming)| 100 MB/s (50 MB/s producing and 50 MB/s consuming) |
|300 MB/s (150 MB/s producing and 150 MB/s consuming)|200/s (100 MB/s producing and 100 MB/s consuming) |
|450 MB/s (225 MB/s producing and 225 MB/s consuming)|300 MB/s (150 MB/s producing and 150 MB/s consuming)|
{: caption="Network throughput capacity on Enterprise" caption-side="top"}

Throughput is expressed as the number of bytes per second that can be both sent and received in a service instance. The peak maximum throughput capacity can be selected when the service instance is created, and later scaled as demands increase. 

Throughput capacity cannot be scaled down. To move to a lower throughput capacity would require creating a new {{site.data.keyword.messagehub}} service instance at the lower capacity unit.

The recommended maximum figure is based on a typical workload and considers the possible impact of operational actions such as internal updates or failure modes, like the loss of an availability zone. If the average throughput exceeds the recommended figure, a loss in performance might be experienced during these conditions. It is recommended to plan your maximum throughput capacity as two-thirds of the peak maximum. For example, two-thirds of the 150 MB/s peak maximum with one capacity unit are 100 MB/s.

For more information, see [Scaling {{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-ES_scaling_capacity).

### Partitions
{: #enterprise_partitions}

The maximum number of partitions increases in line with the number of capacity units, so 3000 for 150 MB/s, 6000 for 300 MB/s and 9000 for 450 MB/s in Enterprise. 

You can change the number of capacity units by using the self-service option as described in [Scaling Enterprise plan capacity](/docs/EventStreams?topic=EventStreams-ES_scaling_capacity#ES_how_to_scale_capacity).

### Retention
{: #enterprise_retention}

The storage capacity can be selected when the service instance is created, and later scaled as demands increase. Storage capacity is dependent upon the configured throughput capacity. For more information, see [Scaling {{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-ES_scaling_capacity) on storage capacity options.

Storage capacity cannot be scaled down. To move to a lower storage capacity would require creating a new {{site.data.keyword.messagehub}} service instance at the lower capacity unit.

### Schema Registry
{: #enterprise_schema_registry}

#### Schemas
{: #schemas}

* Maximum number of schemas that can be stored: 1000
* Maximum number of schema versions for each schema that can be stored: 100
* Maximum schema size: 64 kB

#### Limits
{: #limits}

* Maximum request rate [HTTP Schema Admin] 10 per second
* Maximum request rate [HTTP Serdes] 100 per second

### Other limits
{: #enterprise_limits}

* Maximum message size: 1 MB
* Maximum concurrently active Kafka clients: 10,000
* Maximum concurrent connections: 100,000
* Maximum record key size when you use REST Producer API is 4 K.
* Maximum record value size when you use REST Producer API is 64 K.
* Maximum messages per second when you use REST Producer API is 200.
