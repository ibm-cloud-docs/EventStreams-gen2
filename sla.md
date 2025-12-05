---

copyright:
  years: 2025
lastupdated: "2025-12-05"

keywords: sla, service level agreement, connectivity, throughput

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Service Level Agreement (SLA) for {{site.data.keyword.messagehub}} availability 
{: #sla}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

The {{site.data.keyword.messagehub}} Enterprise Plan Gen 2 provides a highly available architecture distributed across three availability zones with an availability of 99.99%.

For more information about the SLA for high availability services in {{site.data.keyword.Bluemix_notm}}, see [Service Level Agreements for {{site.data.keyword.cloud}} (Public Cloud)](https://www.ibm.com/support/customer/csol/terms/?id=i126-9268&lc=en){: external}.

## How do we measure it?
{: #sla_measure}

Service instances are continuously monitored for performance, error rates, and their response to synthetic operations. Outages are recorded. For more information, see [Service status for Event Streams](https://cloud.ibm.com/status?component=messagehub&selected=status){: external}.

Availability refers to the ability of applications to produce and consume messages from Kafka topics.

## What do you need to consider to achieve this availability?
{: #sla_availability}

To achieve high levels of availability from the application perspective, you should consider [connectivity](/docs/EventStreams?topic=EventStreams-sla#connectivity), [throughput](/docs/EventStreams?topic=EventStreams-sla#throughput), and [consistency and durability of messages](/docs/EventStreams?topic=EventStreams-sla#message_consistency). Users are responsible for designing their applications to optimize these three elements for their business.

### Connectivity
{: #connectivity}

Because of the dynamic nature of the cloud, applications must expect connection breakages. A connection breakage is not considered a failure of service.

#### Retries
{: #sla_retries}

Kafka clients provide reconnect logic, but you must explicitly enable reconnects for producers. For more information, see the [`retries` property](http://kafka.apache.org/documentation.html#producerconfigs){: external}. Connections are remade within 60 seconds.

#### Duplicates
{: #sla_duplicates}

Enabling retries might result in duplicate messages. Depending on when a connection is lost, the producer might not be able to determine if a message was successfully processed by the server and therefore must send the message again when reconnected. Design applications to expect duplicate messages.

If duplicates cannot be tolerated, you can use the `idempotent` producer feature (from Kafka 1.1) to prevent duplicates during retries. For more information, see the [`enable.idempotence` property](http://kafka.apache.org/documentation.html#producerconfigs){: external}.

### Throughput
{: #throughput}

Throughput is expressed as the number of bytes per second that can be both sent and received in a cluster.

#### Specific guidance for the Standard plan
{: #sla_standard_specifics}

For throughput guidance information, see [Limits and quotas - Standard](/docs/EventStreams?topic=EventStreams-kafka_quotas#standard_throughput).

#### Specific guidance for the Enterprise plan
{: #sla_enterprise_specifics}

For throughput guidance information, see [Limits and quotas - Enterprise](/docs/EventStreams?topic=EventStreams-kafka_quotas#enterprise_throughput).

#### Measurement
{: #sla_measurement}

Instrument applications to be aware of how they are performing. For example, the number of messages sent and received, message sizes, and return codes. Understanding an application's usage helps you configure its resources appropriately, such as the retention time for messages on topics.

#### Saturation
{: #sla_saturation}

As the limit of the traffic that can be produced in to the cluster is approached, producers start to be throttled, latency increases, and ultimately errors such as timeout errors occur. Depending on the configuration, message consistency and durability might also be impacted. For more information, see [Consistency and durability of messages](/docs/EventStreams?topic=EventStreams-sla#message_consistency).

### Consistency and durability of messages
{: #message_consistency}

Kafka achieves its availability and durability by replicating the messages it receives across other nodes in the cluster, which can then be used in case of failure. {{site.data.keyword.messagehub}} uses three replicas (default.replication.factor = 3) meaning that each message received by a node is replicated to two other nodes in different availability zones. In this way, the loss of a node or availability zone can be tolerated without loss of data or function.

#### Producer `acks` mode
{: #sla_acks}

Although all messages are replicated, applications can control how robustly the messages they produce are transferred to the service by using the producer's `acks` mode property. This property provides a choice between speed and the risk of message loss. With Kafka 3.0, the default client setting is `acks=all` (before this version it was `acks=1`). The setting `acks=all` means that the producer returns success, as soon as both the broker it is connected to and at least one further broker in the cluster, has acknowledged receiving the message. The advantage of using `acks=all` is that it offers the highest level of durability and hence protection against the loss of message data.

#### In-sync replicas
{: #insync_replicas}

In the configuration that {{site.data.keyword.messagehub}} uses, Kafka chooses one broker to be the leader for each partition replica, and two other brokers to be followers. Message data from clients is sent to the leader for the partition, and is replicated to the followers. If the followers are keeping up with the leader, they are considered "in-sync" replicas. The leader, by definition, is always considered to be in-sync.

If the leader for a partition becomes unavailable, perhaps because of maintenance being applied to the broker, Kafka automatically elects one of the other in-sync replicas to become the new leader. This process occurs swiftly and is automatically handled by Kafka clients.

#### Unclean leader elections
{: #unclean_leader}

{{site.data.keyword.messagehub}} disables unclean leader elections. This setting cannot be changed.

The term *unclean leader election* describes how Kafka responds in a situation where all the in-sync replica for a partition becomes unavailable. When unclean leader elections are enabled, Kafka recovers by making the first replica to become available the leader for the partition, regardless of whether it is in-sync. This can cause message data to be lost if the replica picked to be leader has not replicated all the message data from the previous leader. When unclean leader elections are disabled (as is the case for {{site.data.keyword.messagehub}}), Kafka waits for an in-sync replica to become available and elects it to be the new leader for the partition. This avoids the potential for message data loss that can occur when unclean leader elections are enabled. The tradeoff is that recovery can take longer because Kafka might have to wait for a specific broker to be brought back online before a partition is available for clients to produce and consume message data.

## Single zone location deployments
{: #sla_szr}

For the highest availability we recommend our high availability Public environments, which are built in our multizone locations. In a multizone location, our Kafka clusters are distributed across 3 availability zones, which means that the cluster is resilient to the failure of a single zone or any component within that zone. Some customers require geographic locality and therefore want to provision an {{site.data.keyword.messagehub}} cluster in a geographically local but single zone location. {{site.data.keyword.messagehub}} supports this deployment model, however be aware of the following availability tradeoffs:

- In a single zone location, there are categories of single failures that might lead to the cluster going offline for a period of time. For example, the failure of an entire data center or the update or failure of a shared component such as the underlying hypervisor, SAN, or network. These failures are reflected in the reduced SLA for single zone locations.

- An advantage of spreading Kafka across many zones is to minimize the chance of a failure that might bring down an entire cluster. In contrast, there is the small possibility that a single failure might bring down the entire cluster within one zone. In extreme cases there is also the potential of data loss. For example, even if `acks=all` is used by the producers, if all Kafka nodes went down simultaneously, there might be some messages that the brokers had acknowledged receipt for, but the underlying file system had not completed the flush to disk. Potentially, those un-flushed messages might be lost.

For more information, see [message acknowledgments](/docs/EventStreams?topic=EventStreams-producing_messages#message_acknowledgments). In many use cases this is not necessarily an issue. However, if message loss is unacceptable under any circumstance, consider other strategies such as using a multi-availability zone cluster, cross region replication, or producer side message checkpointing.

For more information, see [single zone clusters](/docs/containers?topic=containers-regions-and-zones#regions_single_zone) and [multizone clusters](/docs/containers?topic=containers-regions-and-zones#regions_multizone).
