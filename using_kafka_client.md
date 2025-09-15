---

copyright:
  years: 2015, 2023
lastupdated: "2023-11-21"

keywords: java client, api, admin api, liberty, migrate client, sasl.jaas.config property

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Using the Kafka Java client
{: #kafka_java_using}

The Java&trade; Kafka API sample is an example producer and consumer that is written in Java, which directly uses the Kafka API. You can run this sample locally or in {{site.data.keyword.Bluemix_short}}.
{: shortdesc}

For optimal performance with the Java client, you are recommended to use version 2.6.0, or later. For more information, see [known restriction about Java Kafka calls](/docs/EventStreams?topic=EventStreams-restrictions#calls_timeout_problem).

The sample code is in the [event-streams-samples GitHub project](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){: external}. Although the sample uses the Kafka API to send and receive messages, the sample uses the {{site.data.keyword.messagehub}} Administration API to create the topic it sends messages to and receives messages from.

For more information about the setup and running of the sample, see the [README.md](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-console-sample){: external}.

For a detailed walkthrough of how to run the sample, see [Getting started with {{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-getting-started#getting_started_steps).

## How to use, download, and run the {{site.data.keyword.runtime_liberty_notm}} sample
{: #liberty_sample}

The {{site.data.keyword.runtime_liberty_full}} sample implements a simple application that is deployed onto the Liberty runtime. The application uses the Kafka API for {{site.data.keyword.messagehub}} to produce and consume messages. The application also serves up a web front end that you can use for administration.

You can find the sample code in the [event-streams-samples GitHub project](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample){: external}.

## Using the sasl.jaas.config property
{: #sasl_prop}

If you use a Kafka client at 0.10.2.1 or later, you can use the `sasl.jaas.config` property for client configuration instead of a JAAS file. To connect to {{site.data.keyword.messagehub}}, set `sasl.jaas.config` as in the following example:

```config
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
    username="USERNAME" \
    password="PASSWORD";</code>
```
{: codeblock}

`USERNAME` and `PASSWORD` are the values from your {{site.data.keyword.messagehub}} **Service Credentials** tab in {{site.data.keyword.Bluemix_notm}}.

If you use `sasl.jaas.config`, clients that run in the same JVM can use different credentials. For more information, see [Configuring Kafka clients](http://kafka.apache.org/documentation/#security_sasl_plain_clientconfig){: external}.

For an earlier Kafka client, you must use a JAAS configuration file to specify the credentials. This mechanism is less convenient, therefore use the `sasl.jaas.config` property instead.

## Migrating a Kafka client from version 0.9.X or 0.10.X to later client versions
{: #kafka_migrate}

If you use the Java clients, you can use the publicly available Kafka clients version 0.10 or later. 

You are encouraged to move from version 0.9.X to the latest version. You can download a Kafka client from [https://kafka.apache.org/downloads](https://kafka.apache.org/downloads){: external}.

### Migrating a Kafka client to version 0.10.2.X or later versions
{: #kafka_migrate_later}

From version 0.10.2, you can configure SASL authentication directly in the client's properties instead of using a JAAS file. This simplification lets you to run multiple clients in the same JVM by using different sets of credentials, which is not possible with a JAAS file.

Complete the following steps:

1. Delete the JAAS file. The JVM property `java.security.auth.login.config=<PATH TO JAAS>` is also no longer required.
2. Add the following to the client's properties:

    ```config
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="USERNAME" password="PASSWORD";
    ```

    `USERNAME` and `PASSWORD` are the values from your {{site.data.keyword.messagehub}} **Service Credentials** tab in {{site.data.keyword.Bluemix_notm}}.
