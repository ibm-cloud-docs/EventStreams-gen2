---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: managed kafka, kafka as a service, managed apache kafka, kafka on cloud

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}}: Scalable messaging for cloud-native applications
{: #about}

[Gen 2]{: tag-purple}

Learn about {{site.data.keyword.cloud}} {{site.data.keyword.messagehub}}, a high-throughput message bus built with Apache Kafka and optimized for event ingestion and distribution between services.
{: shortdesc}

![Use Kafka to build applications with {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}}](https://cdnapisec.kaltura.com/html5/html5lib/v2.101/mwEmbedFrame.php/p/1773841/uiconf_id/27941801/entry_id/1_g98laaak?wid=_1773841&iframeembed=true&entry_id=1_g98laaak){: video output="iframe" data-script="none" id="mediacenterplayer" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen}

{{site.data.keyword.messagehub_full}} offers two deployment generations:

Generation 1 (Classic)
:   Runs on the original IBM platform, supporting public and private endpoints with multi-tenant Standard and single-tenant Enterprise plans.

Generation 2 (VPC)
:   Built on the {{site.data.keyword.IBM_notm}} VPC platform with software-defined networking and virtual private endpoints (VPE). Only the Enterprise plan is available on Gen 2, designed for cloud-native workloads that require network isolation and consistent performance.

You can use {{site.data.keyword.messagehub}} to complete the following tasks:

- Offload asynchronous work to back-end worker applications.
- Connect streaming data to analytics engines for real-time stream processing.
- Publish event data to multiple subscriber applications simultaneously.

{{site.data.keyword.messagehub}} provides a managed Apache Kafka service that delivers durability and high availability. When you use {{site.data.keyword.messagehub}}, you receive continuous support from IBM Kafka specialists.

Built on Apache Kafka, the service incorporates open-source community enhancements and supports standard Kafka client APIs, Kafka Streams, and Kafka Connect. You can build scalable data pipelines for diverse use cases, including microservices integration, log aggregation, event sourcing, and stream processing.

Apache Kafka open-source tools connect directly with {{site.data.keyword.messagehub}} by using standard authentication credentials over secure SASL connections.

In {{site.data.keyword.messagehub}}, applications produce messages to topics. Consumer applications subscribe to topics to receive records individually or share processing across a consumer group. {{site.data.keyword.messagehub}} partitions and stores messages in immutable, ordered sequences.
