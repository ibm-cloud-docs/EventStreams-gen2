---

copyright:
  years: 2025, 2026
lastupdated: "2026-11-11"

keywords: java client, api, settings, create topics, delete topics

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Administering Kafka with Java client API on {{site.data.keyword.cloud_notm}}
{: #kafka_java_api}

Use Kafka client version 0.11 or later to create and delete topics with the Java administration API on {{site.data.keyword.cloud}} {{site.data.keyword.messagehub}}.
{: shortdesc}

If you use a Kafka client version 0.11 or later, or Kafka Streams version 0.10.2.0 or later, you can use APIs to create and delete topics. {{site.data.keyword.messagehub}} restricts the settings that you can use when you create topics. You can modify the following settings.

cleanup.policy
:   Set to `delete` (default), `compact`, or `delete,compact`.

retention.ms
:   The default retention period is 24 hours. The minimum is 1 hour and the maximum is 30 days. Specify this value as multiples of hours.

    In the Enterprise plan, you can set retention to any value.
    {: note}

retention.bytes
:   The maximum size that a partition (which consists of log segments) can grow to before Kafka discards old log segments to free up space.

    Enterprise plan only. Set to any value greater than 10 MB.
    {: note}
    
segment.bytes
:   The segment file size for the log.

    Enterprise plan only. Set to any value greater than 100 kB.
    {: note}

segment.index.bytes
:   The size of the index that maps offsets to file positions. 

    Enterprise plan only. Set to any value between 100 kB and 1 GB.
    {: note}

segment.ms
:   The period after which Kafka forces the log to roll even if the segment file is not full. 

    Enterprise plan only. Set to any value between 5 minutes and 30 days.
    {: note}
