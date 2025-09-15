---

copyright:
  years: 2015, 2023
lastupdated: "2023-10-27"

keywords: stand-alone, source connector, sink connector, distributed worker

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Using Kafka Connect with {{site.data.keyword.messagehub}}
{: #kafka_connect}

Kafka Connect is part of the Apache Kafka project and allows connecting external systems to Kafka. It consists of a runtime that can run [connectors](https://ibm.github.io/event-automation/connectors/) to copy data to and from a cluster. Its main characteristics are:
{: shortdesc}

- Scalability: It can easily scale from a single worker to many. 
- Reliability: It automatically manages offsets and the lifecycle of connectors.
- Extensibility: The community built connectors for most popular systems. {{site.data.keyword.IBM}} has connectors for [MQ](/docs/EventStreams?topic=EventStreams-mq_connector) and [Cloud Object Storage](/docs/EventStreams?topic=EventStreams-cos_connector).
 
You can use Kafka Connect with {{site.data.keyword.messagehub_full}} and can run the workers inside or outside {{site.data.keyword.cloud}}. {{site.data.keyword.IBM_notm}} has an extensive list of over 50 connectors that are supported either by {{site.data.keyword.IBM_notm}} or the community. You find these connectors in the [connector catalog](https://ibm.github.io/event-automation/connectors/).

Kafka Connect can run in either stand-alone or distributed mode. Stand-alone mode is intended for testing and temporary connections between systems. Distributed mode is more appropriate for production use. The configuration required to use {{site.data.keyword.messagehub}} with these two modes is slightly different.

## Stand-alone worker configuration
{: #standalone_worker notoc}

The stand-alone worker does not use any internal topics. Instead, it uses a file for storing offset information.

You must provide the bootstrap servers and SASL credentials information in the worker properties file that you supply when you start a Kafka Connect stand-alone worker. The following example lists the properties that you must provide in your properties file:

```config
    bootstrap.servers=BOOTSTRAP_ENDPOINTS
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    ssl.protocol=TLSv1.2
    ssl.enabled.protocols=TLSv1.2
    ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

Replace BOOTSTRAP_ENDPOINTS, USER, and PASSWORD with the values from your {{site.data.keyword.messagehub}} **Service Credentials** tab in the {{site.data.keyword.Bluemix_notm}} console.

### Source connector
{: #source_connector notoc}

The following example lists the properties that you must provide in your properties file:

```config
    bootstrap.servers=BOOTSTRAP_ENDPOINTS
    producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
    producer.security.protocol=SASL_SSL
    producer.sasl.mechanism=PLAIN
    producer.ssl.protocol=TLSv1.2
    producer.ssl.enabled.protocols=TLSv1.2
    producer.ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

Replace BOOTSTRAP_ENDPOINTS, USER, and PASSWORD with the values from your {{site.data.keyword.messagehub}} **Service Credentials** tab in the {{site.data.keyword.Bluemix_notm}} console.

### Sink connector
{: #sink_connector}

The following example lists the properties that you must provide in your properties file:

```config
    bootstrap.servers=BOOTSTRAP_ENDPOINTS
    consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
    consumer.security.protocol=SASL_SSL
    consumer.sasl.mechanism=PLAIN
    consumer.ssl.protocol=TLSv1.2
    consumer.ssl.enabled.protocols=TLSv1.2
    consumer.ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

Replace BOOTSTRAP_ENDPOINTS, USER, and PASSWORD with the values from your {{site.data.keyword.messagehub}} **Service Credentials** tab in the {{site.data.keyword.Bluemix_notm}} console.

## Distributed worker configuration
{: #distributed_worker notoc}

You must provide the bootstrap servers and SASL credentials information in the properties file that you supply when you start the Kafka Connect distributed workers. The following example lists the properties that you must provide in your properties file:

```config
    bootstrap.servers=BOOTSTRAP_ENDPOINTS
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    ssl.protocol=TLSv1.2
    ssl.enabled.protocols=TLSv1.2
    ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

Replace BOOTSTRAP_ENDPOINTS, USER, and PASSWORD with the values from your {{site.data.keyword.messagehub}} **Service Credentials** tab in the {{site.data.keyword.Bluemix_notm}} console.

If you want to use a source connector, you must also specify the SSL and SASL configuration for the producer as follows:

```config
    producer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
    producer.security.protocol=SASL_SSL
    producer.sasl.mechanism=PLAIN
    producer.ssl.protocol=TLSv1.2
    producer.ssl.enabled.protocols=TLSv1.2
    producer.ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

If you want to use a sink connector, you must also specify the SSL and SASL configuration for the consumer as follows:

```config
    consumer.sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USER" password="PASSWORD";
    consumer.security.protocol=SASL_SSL
    consumer.sasl.mechanism=PLAIN
    consumer.ssl.protocol=TLSv1.2
    consumer.ssl.enabled.protocols=TLSv1.2
    consumer.ssl.endpoint.identification.algorithm=HTTPS
````
{: codeblock}

In addition, Kafka Connect in distributed mode uses three topics internally. These topics are created automatically when a worker starts up, if you use Kafka Connect in Apache Kafka version 0.11 or later. You provide the names of the topics as configuration parameters. Ensure that the values are the same for all workers with the same `group.id` configuration value.

| Configuration               | Description                                                         |
| --------------------------- | ------------------------------------------------------------------- |
| `offset.storage.topic`      | Connector offsets topic                                             |
| `offset.storage.partitions` | Number of partitions for connector offsets topic (default 25)       |
| `config.storage.topic`      | Connector configuration topic                                       |
| `status.storage.topic`      | Connector status topic                                              |
| `status.storage.partitions` | Number of partitions for connector status topic (default 5)         |
{: caption="Topics in Kafka Connect" caption-side="bottom"}

For example, you can use the following key-value pairs in your properties file:

```config
    offset.storage.topic=connect-offsets
    config.storage.topic=connect-configs
    status.storage.topic=connect-status
```
{: codeblock}

Consider reducing the number of partitions if you are making only light use of Kafka Connect.

For more information about Kafka Connect, see [Kafka Connect overview](http://kafka.apache.org/documentation/#connect_overview){: external}.
