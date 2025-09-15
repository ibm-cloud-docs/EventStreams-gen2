---

copyright:
  years: 2020, 2024
lastupdated: "2024-03-15"

keywords: reserved disk usage, store data, storage

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Understanding reserved disk usage
{: #ES_understanding_reserved_disk_usage}

Learn how the usable storage of an {{site.data.keyword.messagehub}} instance is used by the topics and partitions that are created and the configuration settings that you apply.
{: shortdesc}

First of all, it's important to note that the defined storage of an {{site.data.keyword.messagehub}} instance is usable storage. That means that you do not need to worry about storage used by replicas (because all topics have their replication factor set to 3) that is not taken from the usable storage. This keeps things simple and you can plan how to map the retention 
of those messages to storage usage.

## Understanding how Kafka stores data
{: #ES_understanding_how_kafka_stores_data}

Kafka allows users to configure retention limits on topics.

The retention.bytes configuration is the total number of bytes allocated for messages for each partition of the topic. If it is exceeded, Kafka deletes oldest messages. So for example, if you are generally sending in 200 MB a day of messages to a single partition topic, and you want to keep them for 5 days, set retention.bytes to 1 GB (200 MB x 5 days). 

If this was over 10 partitions, then you would set retention.bytes = 100 MB (1 GB / 10 partitions).

Internally Kafka splits each partition into log segments. This again is a property that you can set, log.segment.size, the default is 512 MB. When we mentioned that Kafka deletes the oldest messages, it really deletes the oldest log segment. For this reason, Kafka needs to keep space for one extra log segment, in addition to the number of log segments required to satisfy the retention.bytes configuration.

See the following examples.

     retention.bytes = 1 GB
     log.segment.size = 512 MB
     Kafka needs approximately 1.5 GB per partition for the topic storage.

     retention.bytes = 100 MB
     log.segment.size = 512 MB
     Kafka needs approximately 1 GB per partition for the topic storage.

More storage is needed per partition for indexes. For each log segment, Kafka also stores two indexes. Their sizes are defined by segment.index.size, which is also configurable and has a default of 10 MB. For reference, the storage that is used by indexes is calculated as follows:

     2 x number.of.log.segments x segment.index.size

where 

     number.of.log.segments = floor(retention.bytes/log.segment.size) + 1
     
## Managing storage with {{site.data.keyword.messagehub}}
{: #ES_managing_storage_with_event_streams}     

When doing topic administration operations, such as creating topics, creating partitions, or changing topic configurations, {{site.data.keyword.messagehub}} ensures that enough storage is available to satisfy the operation. To do this, for each topic, {{site.data.keyword.messagehub}} computes the "reserved size" for each topic by using the following method.

For topics that have a `cleanup.policy` setting of `compact`, the reserved size consumed for each partition is always 1 GB. When the `cleanup.policy` for a topic is set to `compact`, any values used for the `retention.bytes` or `retention.ms` topic settings are ignored.

For topics that have a `cleanup.policy` setting of `delete`, or `compact, delete`, the reserved size consumed for each partition is calculated as follows:

     Reserved size = retention.bytes + log.segment.size + (2 x segment.index.size x number.of.log.segments)

where 

     number.of.log.segments = floor(retention.bytes/log.segment.size) + 1

The total reserved storage percentage is also displayed in {{site.data.keyword.mon_full_notm}} by the [ibm_eventstreams_instance_reserved_disk_space_percent metric](/docs/EventStreams?topic=EventStreams-metrics#ibm_eventstreams_instance_reserved_disk_space_percent).

Requests to create a new topic, or add partitions to an existing topic are rejected if they would result in the total amount of reserved storage exceeding 90% [^footnote1] of the storage configured for the {{site.data.keyword.messagehub}} instance. Rejected requests receive a PolicyViolation error explaining that the reserved storage limit for the instance has been reached. If the reserved storage limit is reached, you need to either delete topics or increase the amount of storage configured for the instance before further topics can be created.

[^footnote1]: {{site.data.keyword.messagehub}} uses some of the storage assigned to an instance for both internal management functions and as an operational reserve for log segments that are eligible for deletion.


The reserved size calculation can change in the future if Kafka storage requirements are updated.
{: note}  

## Examples
{: #ES_managing_storage_with_event_streams_examples}  

A non-obvious effect is that Kafka can reserve more storage than expected depending on your topic configurations. See the following examples.

1. A topic with retention.bytes of 1 GB, and with a log segment size of 512 MB:

    With one partition, it would reserve about 1.5 GB of storage.
   
    In this case, the reserved size is significantly larger than the retention size.

2. A topic with retention.bytes of 50 GB, and with a log segment size of 512 MB:

    With one partition, it would reserve about 50.5 GB of storage.
    
    In this case, the reserved size is very close to the retention size.

3. A topic with retention.bytes of 1 GB, and with a log segment size of 128 MB:

    With one partition, it would reserve about 1.1 GB of storage.
    
    In this case, the reserved size is very close to the retention size.
