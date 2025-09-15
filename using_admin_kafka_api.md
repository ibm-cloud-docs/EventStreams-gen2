---

copyright:
  years: 2015, 2023
lastupdated: "2023-07-24"

keywords: java client, api, settings, create topics, delete topics

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Using the administration Kafka Java client API
{: #kafka_java_api}

If you use a Kafka client version 0.11 or later, or Kafka Streams version 0.10.2.0 or later, you can use APIs to create and delete topics. We put some restrictions on the settings that are allowed when you create topics. See the following settings that you can modify.
{: shortdesc}

cleanup.policy
:   Set to `delete` (default), `compact` or `delete,compact`.

retention.ms
:   The default retention period is 24 hours. The minimum is 1 hour and the maximum is 30 days. Specify this value as multiples of hours.

    In the Enterprise plan, you can set retention to any value.
    {: note}

retention.bytes
:   The maximum size a partition (which consists of log segments) can grow to before we discard old log segments to free up space.

    Enterprise plan only. Set to any value larger than 10 MB.
    {: note}
    
segment.bytes
:   The segment file size for the log.

    Enterprise plan only. Set to any value larger than 100 kB.
    {: note}

segment.index.bytes
:   The size of the index that maps offsets to file positions. 

    Enterprise plan only. Set to any value between 100 kB and 1 GB.
    {: note}

segment.ms
:   The period after which Kafka forces the log to roll even if the segment file isn't full. 

    Enterprise plan only. Set to any value between 5 minutes and 30 days.
    {: note}

