---

copyright:
  years: 2015, 2023
lastupdated: "2023-10-23"

keywords: managed kafka, kafka as a service, managed apache kafka, kafka on cloud

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.messagehub}} overview
{: #about}

{{site.data.keyword.messagehub_full}} is a high-throughput message bus that is built with Apache Kafka. This managed Kafka service is optimized for event ingestion into {{site.data.keyword.cloud}} and event stream distribution between your services and applications. {{site.data.keyword.messagehub}} was previously known as Message Hub.
{: shortdesc}

![Leverage Kafka to build intelligent applications with IBM Cloud Event Streams](https://cdnapisec.kaltura.com/html5/html5lib/v2.101/mwEmbedFrame.php/p/1773841/uiconf_id/27941801/entry_id/1_g98laaak?wid=_1773841&iframeembed=true&entry_id=1_g98laaak){: video output="iframe" data-script="none" id="mediacenterplayer" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen}

You can use {{site.data.keyword.messagehub}} to complete the following tasks:

- Offload work to back-end worker applications.
- Connect {{site.data.keyword.messagehub}} to streaming analytics to realize powerful insights.
- Publish event data to multiple applications to react in real time.

{{site.data.keyword.messagehub}} offers a fully managed Apache Kafka service, ensuring durability and high availability for our clients. By using {{site.data.keyword.messagehub}}, you have support around the clock from our team of Kafka experts.

By being built with Apache Kafka, our offering directly benefits from all the innovation that occurs in the community and supports Kafka client APIs, Kafka Streams, and Kafka Connect. Kafka is highly reliable and lets you build scalable data pipelines for many use cases, including connecting microservices, doing log aggregation, event sourcing, and stream processing.

Apache Kafka tools usually work directly with {{site.data.keyword.messagehub}}, although you do need to provide more configuration because connections to {{site.data.keyword.messagehub}} always authenticate by using credentials.

In {{site.data.keyword.messagehub}}, applications send data by creating a message and sending it to a topic. To receive messages, applications subscribe to a topic
and choose to either receive all the topic's messages, or to share the messages between them. {{site.data.keyword.messagehub}} hosts and maintains the messages in an ordered sequence. 
