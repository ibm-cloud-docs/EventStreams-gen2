---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: monitoring, metrics

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with Kafka client metrics on {{site.data.keyword.cloud_notm}}
{: #getting-started-metrics}

[Gen 2]{: tag-purple}

Learn how to monitor Kafka client metrics for optimal functionality and health of your data pipeline on {{site.data.keyword.cloud}} {{site.data.keyword.messagehub}}.
{: shortdesc}

With Kafka, monitoring typically involves various metrics that are related to topics, partitions, brokers, and consumer groups. Standard Kafka metrics include information on throughput, latency, replication, and disk usage. See the [Kafka documentation](https://kafka.apache.org/){: external} and relevant monitoring tools to understand the specific metrics available for your version of Kafka and how to interpret them effectively.

## Why monitor Kafka clients
{: #why-monitor}

Monitoring your {{site.data.keyword.messagehub}} instance helps you maintain optimal functionality and health of your data pipeline. Client-side metrics help you detect issues such as resource contention, consumer lag, and broker bottlenecks before they cause downtime.

Kafka producers and consumers generate client-side metrics to monitor application performance and stability. The {{site.data.keyword.messagehub}} service also emits broker-side metrics. For more information, see [Monitoring {{site.data.keyword.messagehub}} service metrics by using {{site.data.keyword.mon_full_notm}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-metrics).

## Client metrics to monitor
{: #what-monitor}

### Producer metrics
{: #producer-metrics}

`record-error-rate`
:   Measures the average number of record produce requests per second that result in errors. An increasing error rate indicates potential data loss or rejected records. Monitor this metric to verify that producers record messages reliably.

`request-latency-avg`
:   Measures the average latency for each produce request in milliseconds. Spikes in request latency indicate broker bottlenecks or network contention. If request latency increases, consider adjusting producer batch sizes or scaling instance capacity.

`byte-rate`
:   Measures the average number of bytes sent per second to a topic. A sudden drop in throughput can signal application or network failures. For effective capacity planning, keep normal throughput under two-thirds of your maximum allocated bandwidth to accommodate traffic spikes and availability zone failovers.

### Consumer metrics
{: #consumer-metrics}

`fetch-rate`
:   Measures the number of fetch requests per second. A high fetch rate with low message throughput indicates that consumers are polling frequently without retrieving sufficient data.

`fetch-size-avg`
:   Measures the average number of bytes fetched per request. Tune `fetch.min.bytes`, `fetch.max.bytes`, and `fetch.max.wait.ms` to optimize throughput and minimize broker CPU load.

`commit-latency-avg`
:   Measures the average time in milliseconds to commit offsets. High commit latency delays message processing and can cause duplicate message delivery if uncommitted offsets are reprocessed after a consumer restart.

`bytes-consumed-rate`
:   Measures the average number of bytes consumed per second. Unexpected drops in consumed byte rate can indicate downstream application processing bottlenecks or stalled consumer threads.

`rebalance-rate-per-hour`
:   Measures the number of consumer group rebalances per hour. Frequent rebalances halt message consumption while the service reassigns partitions, which increases latency. Maintain a low and stable rebalance rate for consistent consumer throughput.
