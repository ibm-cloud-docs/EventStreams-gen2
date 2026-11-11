---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: reserved disk usage, store data, storage

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Managing reserved disk space in {{site.data.keyword.messagehub}} on {{site.data.keyword.cloud_notm}}
{: #ES_understanding_reserved_disk_usage}

[Gen 2]{: tag-purple}

Understand how {{site.data.keyword.messagehub}} uses usable storage for topics and partitions, and how configuration settings affect disk usage on {{site.data.keyword.cloud}}.
{: shortdesc}

The defined storage of an {{site.data.keyword.messagehub}} instance is usable storage. All topics have a replication factor of 3, and the storage that replicas use is not taken from the usable storage. This approach keeps planning simple because you can map the retention of messages directly to storage usage.

## Understanding how Kafka stores data
{: #ES_understanding_how_kafka_stores_data}

You can configure retention limits on Kafka topics.

The retention.bytes configuration is the total number of bytes allocated for messages for each partition of the topic. If the partition exceeds this value, Kafka deletes the oldest messages. For example, if you send 200 MB of messages a day to a single partition topic, and you want to keep them for 5 days, set retention.bytes to 1 GB (200 MB x 5 days).

If the topic has 10 partitions, set retention.bytes to 100 MB (1 GB / 10 partitions).

Internally, Kafka splits each partition into log segments. You can set the size of a log segment with the log.segment.size property. The default is 512 MB. Kafka deletes the oldest messages by deleting the oldest log segment. For this reason, Kafka needs to keep space for one extra log segment, in addition to the number of log segments required to satisfy the retention.bytes configuration.

See the following examples.

     retention.bytes = 1 GB
     log.segment.size = 512 MB
     Kafka needs approximately 1.5 GB per partition for the topic storage.

     retention.bytes = 100 MB
     log.segment.size = 512 MB
     Kafka needs approximately 1 GB per partition for the topic storage.

Each partition needs more storage for indexes. For each log segment, Kafka also stores two indexes. The segment.index.size property defines their sizes. This property is also configurable and has a default of 10 MB. For reference, the storage that is used by indexes is calculated as follows:

     2 x number.of.log.segments x segment.index.size

where

     number.of.log.segments = floor(retention.bytes/log.segment.size) + 1

## Managing storage with {{site.data.keyword.messagehub}}
{: #ES_managing_storage_with_event_streams}

When you do topic administration operations, such as creating topics, creating partitions, or changing topic configurations, {{site.data.keyword.messagehub}} ensures that enough storage is available to satisfy the operation. To do this, for each topic, {{site.data.keyword.messagehub}} computes the "reserved size" for each topic by using the following method.

For topics that have a `cleanup.policy` setting of `compact`, the reserved size consumed for each partition is always 1 GB. When the `cleanup.policy` for a topic is set to `compact`, any values used for the `retention.bytes` or `retention.ms` topic settings are ignored.

For topics that have a `cleanup.policy` setting of `delete`, or `compact, delete`, the reserved size consumed for each partition is calculated as follows:

     Reserved size = retention.bytes + log.segment.size + (2 x segment.index.size x number.of.log.segments)

where

     number.of.log.segments = floor(retention.bytes/log.segment.size) + 1

The total reserved storage percentage is also displayed in {{site.data.keyword.mon_full_notm}} by the [ibm_eventstreams_instance_reserved_disk_space_percent metric](/docs/EventStreams-gen2?topic=EventStreams-gen2-metrics#ibm_eventstreams_instance_reserved_disk_space_percent).

{{site.data.keyword.messagehub}} rejects requests to create a new topic or add partitions to an existing topic if they cause the total amount of reserved storage to exceed 90% [^footnote1] of the storage configured for the instance. Rejected requests receive a PolicyViolation error that explains that the reserved storage limit for the instance is reached. If you reach the reserved storage limit, delete topics or increase the amount of storage configured for the instance before you create further topics.

[^footnote1]: {{site.data.keyword.messagehub}} uses some of the storage assigned to an instance for both internal management functions and as an operational reserve for log segments that are eligible for deletion.


The reserved size calculation can change if Kafka storage requirements change.
{: note}

## Examples
{: #ES_managing_storage_with_event_streams_examples}

Depending on your topic configurations, Kafka can reserve more storage than you expect. See the following examples.

1. A topic with retention.bytes of 1 GB, and with a log segment size of 512 MB:

    With one partition, it reserves about 1.5 GB of storage.
 
    In this case, the reserved size is significantly larger than the retention size.

2. A topic with retention.bytes of 50 GB, and with a log segment size of 512 MB:

    With one partition, it reserves about 50.5 GB of storage.

    In this case, the reserved size is very close to the retention size.

3. A topic with retention.bytes of 1 GB, and with a log segment size of 128 MB:

    With one partition, it reserves about 1.1 GB of storage.

    In this case, the reserved size is very close to the retention size.
