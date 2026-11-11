---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: sla, service level agreement, connectivity, throughput

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Understanding the {{site.data.keyword.messagehub}} service level agreement (SLA)
{: #sla}

[Gen 2]{: tag-purple}

Learn about the service level agreement (SLA) for {{site.data.keyword.messagehub}} availability on {{site.data.keyword.cloud}}, including measurement, connectivity, throughput, and message durability.
{: shortdesc}

The {{site.data.keyword.messagehub}} Enterprise Plan Gen 2 provides a highly available architecture distributed across three availability zones with an availability of 99.99%.

For more information about the SLA for high availability services in {{site.data.keyword.cloud_notm}}, see [Service Level Agreements for {{site.data.keyword.cloud_notm}} (Public Cloud)](https://www.ibm.com/support/customer/csol/terms/?id=i126-9268&lc=en){: external}.

## Measuring availability
{: #sla_measure}

Service instances are continuously monitored for performance, error rates, and their response to synthetic operations. {{site.data.keyword.IBM_notm}} records outages. For more information, see [Service status for {{site.data.keyword.messagehub}}](https://cloud.ibm.com/status?component=messagehub&selected=status){: external}.

Availability refers to the ability of applications to produce and consume messages from Kafka topics.

## What do you need to consider to achieve this availability?
{: #sla_availability}

To achieve high levels of availability from the application perspective, consider [connectivity](/docs/EventStreams-gen2?topic=EventStreams-gen2-sla#connectivity), [throughput](/docs/EventStreams-gen2?topic=EventStreams-gen2-sla#throughput), and [consistency and durability of messages](/docs/EventStreams-gen2?topic=EventStreams-gen2-sla#message_consistency). You are responsible for designing your applications to optimize these three elements for your business.

### Connectivity
{: #connectivity}

Because of the dynamic nature of the cloud, applications must expect connection breakages. A connection breakage is not a failure of service.

#### Retries
{: #sla_retries}

Kafka clients provide reconnect logic, but you must explicitly enable reconnects for producers. For more information, see the [`retries` property](https://kafka.apache.org/){: external}. Kafka clients remake connections within 60 seconds.

#### Duplicates
{: #sla_duplicates}

Enabling retries might result in duplicate messages. Depending on when a connection is lost, the producer might not be able to determine whether the server processed a message successfully. The producer then must send the message again when it reconnects. Design applications to expect duplicate messages.

If duplicates cannot be tolerated, you can use the `idempotent` producer feature (from Kafka 1.1) to prevent duplicates during retries. For more information, see the [`enable.idempotence` property](https://kafka.apache.org/){: external}.

### Throughput
{: #throughput}

Throughput is expressed as the number of bytes per second that can be both sent and received in a cluster.

#### Specific guidance for the Standard plan
{: #sla_standard_specifics}

For throughput guidance information, see [Limits and quotas - Standard](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_quotas).

#### Specific guidance for the Enterprise plan
{: #sla_enterprise_specifics}

For throughput guidance information, see [Limits and quotas - Enterprise](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_quotas#limits_enterprise).


#### Measurement
{: #sla_measurement}

Instrument applications to track how they perform, such as the number of messages sent and received, message sizes, and return codes. Understanding an application's usage helps you configure its resources appropriately, such as the retention time for messages on topics.

#### Saturation
{: #sla_saturation}

As traffic approaches the limit that the cluster can accept, the service throttles producers, latency increases, and eventually errors such as timeouts occur. Depending on the configuration, message consistency and durability might also be affected. For more information, see [Consistency and durability of messages](/docs/EventStreams-gen2?topic=EventStreams-gen2-sla#message_consistency).

### Consistency and durability of messages
{: #message_consistency}

Kafka achieves its availability and durability by replicating the messages it receives across other nodes in the cluster. Kafka can use these replicas if a failure occurs. {{site.data.keyword.messagehub}} uses three replicas (`default.replication.factor = 3`), so each message that a node receives is replicated to two other nodes in different availability zones. In this way, the service tolerates the loss of a node or availability zone without loss of data or function.

#### Producer `acks` mode
{: #sla_acks}

Although all messages are replicated, applications can control how robustly the messages they produce are transferred to the service by using the producer's `acks` mode property. This property provides a choice between speed and the risk of message loss. With Kafka 3.0, the default client setting is `acks=all` (before this version it was `acks=1`). The setting `acks=all` means that the producer returns success, as soon as both the broker it is connected to and at least one further broker in the cluster, has acknowledged receiving the message. The `acks=all` setting offers the highest level of durability and therefore protection against the loss of message data.

#### In-sync replicas
{: #insync_replicas}

In the configuration that {{site.data.keyword.messagehub}} uses, Kafka chooses one broker to be the leader for each partition replica, and two other brokers to be followers. Message data from clients is sent to the leader for the partition, and is replicated to the followers. If the followers keep up with the leader, they are in-sync replicas. By definition, the leader is always in-sync.

If the leader for a partition becomes unavailable, for example, because of maintenance on the broker, Kafka automatically elects one of the other in-sync replicas to become the new leader. This process is quick, and Kafka clients handle it automatically.

#### Unclean leader elections
{: #unclean_leader}

{{site.data.keyword.messagehub}} disables unclean leader elections. You cannot change this setting.

The term _unclean leader election_ describes how Kafka responds when all the in-sync replicas for a partition become unavailable. When unclean leader elections are enabled, Kafka recovers by making the first replica to become available the leader for the partition, regardless of whether it is in-sync. This can cause message data loss if the replica that becomes the leader has not replicated all the message data from the previous leader. When unclean leader elections are disabled, as is the case for {{site.data.keyword.messagehub}}, Kafka waits for an in-sync replica to become available. Kafka then elects that replica as the new leader for the partition. This avoids the potential for message data loss that can occur when unclean leader elections are enabled. The tradeoff is that recovery can take longer. Kafka might have to wait for a specific broker to come back online before clients can produce and consume message data for the partition.
