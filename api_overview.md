---

copyright:
  years: 2023, 2024
lastupdated: "2024-01-29"

keywords: api, rest api, cli

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# API and CLI overview
{: #api_reference}

{{site.data.keyword.messagehub_full}} provides a number of API to help use and configure your {{site.data.keyword.messagehub}} Kafka cluster.
{: shortdesc}

![Apache Kafka API and CLI](https://cdnapisec.kaltura.com/html5/html5lib/v2.101/mwEmbedFrame.php/p/1773841/uiconf_id/27941801/entry_id/1_18293q1v?wid=_1773841&iframeembed=true&entry_id=1_18293q1v){: video output="iframe" data-script="none" id="mediacenterplayer" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen}

The following API versions are available:

- [Kafka native API](https://kafka.apache.org/documentation/): Kafka exposes all its functionality over a language independent protocol that has clients available in many programming languages. Kafka includes three protocol APIs (Producer, Consumer, Admin) and two additional framework APIs (Streams, Connect).
- [Kafka console tools](/docs/EventStreams?topic=EventStreams-kafka_console_tools): Apache Kafka comes with various console tools for simple administration and messaging operations.
- [Event Streams Admin REST API](/apidocs/event-streams/adminrest): {{site.data.keyword.messagehub}} provides REST API for administration that you can use to create, delete, list, and update topics.
- [Event Streams REST Producer API](/apidocs/event-streams/restproducer_v2): {{site.data.keyword.messagehub}} provides REST API for producing to your cluster.
- [CLI reference](/docs/EventStreams?topic=EventStreams-cli_reference): {{site.data.keyword.messagehub}} also provides a dedicated CLI plugin that provides a seamless mechanism by which to view and administer the topics, partitions, consumer groups, and configuration of your {{site.data.keyword.messagehub}}'s Kafka instances.
