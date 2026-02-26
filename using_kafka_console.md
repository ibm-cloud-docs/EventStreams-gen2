---

copyright:
  years: 2026
lastupdated: "2026-02-26"

keywords: console tools, console producer, console consumer, consumer groups

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Using Kafka console tools with {{site.data.keyword.messagehub}}
{: #kafka_console_tools}

[Gen 2]{: tag-purple}

Apache Kafka comes with various console tools for simple administration and messaging operations. You can use many of them with {{site.data.keyword.messagehub}}, although {{site.data.keyword.messagehub}} does not permit connection to its KRaft cluster.
{: shortdesc}

### Download the console tools
{: #download_tools}

The console tools are distributed as part of the Kafka binary download and can be downloaded from the [Apache Kafka downloads](https://kafka.apache.org/community/downloads/){: external}. The most recent supported release is recommended.

For example, to download version 4.1.1 to a Linux machine:

```bash
$ wget https://downloads.apache.org/kafka/4.1.1/kafka_2.13-4.1.1.tgz
```
{: codeblock}

Extract what you’ve downloaded and change directory:

```bash
$ tar -xzf kafka_2.13-4.1.1.tgz
$ cd kafka_2.13-4.1.1/
```
{: codeblock}

These console tools are in the `bin` directory.

### Install Java
{: #install_java}

Install java with the following commands:

```bash
$ sudo apt update
$ sudo apt install openjdk-17-jdk
```
{: codeblock}

Next, you can verify the installation:

```bash
$ java -version
```
{: codeblock}

### Create a `client.properties` file
{: #create_properties}

Create a properties file with the information needed to connect to your instance.

```bash
$ nano client.properties
```
{: codeblock}

Copy the following snippet into the `client.properties` file and edit with your details.

```properties
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="token" password="<API_KEY>";
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
```
{: codeblock}

Replace the API_KEY variable in the snippet with the value from your {{site.data.keyword.messagehub}} **Service credentials** tab in the {{site.data.keyword.cloud_notm}} console.

Save and then exit nano.

## Topics
{: #topics_tool}

To create a topic:

```bash
$ ./bin/kafka-topics.sh --create --topic <TOPIC_NAME> --bootstrap-server <BOOTSTRAP_SERVERS> --command-config client.properties --partitions 1 --replication-factor 3 --config retention.ms=604800000
```
{: codeblock}

Replace the variables in the example with your own values:

- BOOTSTRAP_SERVERS with the value from your {{site.data.keyword.messagehub}} **Service credentials** tab in the {{site.data.keyword.cloud_notm}} console.
- TOPIC_NAME with the name of the topic to be created.

The topics tool can also be used to find out information about your topics and their configuration in an existing service instance.

```bash
./bin/kafka-topics.sh --bootstrap-server <BOOTSTRAP_SERVERS> --command-config client.properties --describe

Topic:sample-topic	PartitionCount:3	ReplicationFactor:3	 Configs:min.insync.replicas=2,unclean.leader.election.enable=true,retention.bytes=1073741824,segment.bytes=536870912,retention.ms=86400000
    Topic: sample-topic    Partition: 0    Leader: 0    Replicas: 0,2,1    Isr: 0,2,1
    Topic: sample-topic    Partition: 1    Leader: 1    Replicas: 1,2,0	   Isr: 0,2,1
    Topic: sample-topic    Partition: 2    Leader: 2    Replicas: 2,1,0	   Isr: 0,2,1
Topic:testtopic	 PartitionCount:1	 ReplicationFactor:3	Configs:min.insync.replicas=2,unclean.leader.election.enable=true,retention.bytes=1073741824,segment.bytes=536870912,retention.ms=86400000
    Topic: testtopic    Partition: 0    Leader: 0    Replicas: 0,2,1   Isr: 0,2
```
{: codeblock}

From the sample, you can see that topic `sample-topic` has three partitions and a replication factor of three. The example also shows which broker the leader of each partitions is on and which replicas are in sync (`Isr`). For example, the leader of partition `0` is on broker `0`, the followers are on brokers `2` and `1` and all three replicas are in sync. If you look at the second topic `testtopic`, it has only one partition, which is replicated on brokers `0`, `2`, and `1` but the in-sync replica list shows only `0` and `2`. This means that the follower on broker `1` is falling behind and is therefore not in the `Isr` list.

## Console producer
{: #console_producer}

To produce a message to a topic, run the following command. A prompt will be shown where some text can be entered and sent by pressing return. To quit, type <CTRL> + C.

```bash
$ ./bin/kafka-console-producer.sh --topic <TOPIC_NAME> --bootstrap-server <BOOTSTRAP_SERVERS> --producer.config client.properties
```
{: codeblock}

Replace the variables in the example with your own values:

- BOOTSTRAP_SERVERS with the value from your {{site.data.keyword.messagehub}} **Service credentials** tab in the {{site.data.keyword.Bluemix_notm}} console.
- TOPIC_NAME with the name of the topic to be created.

## Console consumer
{: #console_consumer}

To consume a message from a topic, run the following. To quit, type <CTRL> + C. Note that the `--from-beginning` option will consume all messages on the topic. If this option is omitted, only messages produced after the consumer was run will be retrieved.

```bash
$ ./bin/kafka-console-consumer.sh --topic <TOPIC_NAME> --bootstrap-server <BOOTSTRAP_SERVERS> --consumer.config client.properties --from-beginning
```
{: codeblock}

Replace the variables in the example with your own values:

- BOOTSTRAP_SERVERS with the value from your {{site.data.keyword.messagehub}} **Service credentials** tab in the {{site.data.keyword.cloud_notm}} console.
- TOPIC_NAME with the name of the topic to be created.

## Consumer groups
{: #consumer_groups_tool}

You can use the Kafka consumer groups tool with {{site.data.keyword.messagehub}}.

```bash
$ ./bin/kafka-consumer-groups.sh --bootstrap-server <BOOTSTRAP_SERVERS> --command-config client.properties --list --timeout 60000
```
{: codeblock}

Replace the BOOTSTRAP_SERVERS variable in the example with the value from your {{site.data.keyword.messagehub}} **Service credentials** tab in the {{site.data.keyword.cloud_notm}} console.

Using this tool, you can also display details, such as the current positions of the consumers, their lag, and client-id for each partition for a group. For example:

```bash
$ kafka-consumer-groups.sh --bootstrap-server <BOOTSTRAP_SERVERS> --command-config client.properties --describe --group <GROUP> --timeout 60000
```
{: codeblock}

Replace GROUP in the example with the group name that you want to retrieve details for.

See the following sample output from running the **kafka-consumer-groups** tool:

```bash
GROUP              TOPIC    PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG         CONSUMER-ID      HOST          CLIENT-ID
consumer-group-1   foo        0          264             267            3          client-1-abc    example.com    client-1
consumer-group-1   foo        1          124             124            0          client-1-abc    example.com    client-1
consumer-group-1   foo        2          212             212            0          client-2-def    example.com    client-2
```
{: codeblock}

From the example, you can see that consumer group `consumer-group-1` has two consumer members that consume messages from topic `foo` with three partitions. It also shows that the consumer `client-1-abc` that is consuming from partition `0` is three messages behind because the current offset of the consumer is `264` but the offset of the last message on partition `0` is `267`.

## Kafka Streams reset
{: #kafka_streams_reset}

You can use this tool with {{site.data.keyword.messagehub}} to reset the processing state of a Kafka Streams application, so you can reprocess its input from scratch. Before you run this tool, ensure that your Streams application is fully stopped.

For example:

```bash
./bin/kafka-streams-application-reset.sh --bootstrap-servers <BOOTSTRAP_SERVERS> --config-file client.properties --application-id <APP_ID>
```
{: codeblock}

Replace the following variables in the example with your own values:

- BOOTSTRAP_SERVERS with the value from your {{site.data.keyword.messagehub}} **Service credentials** tab in the {{site.data.keyword.Bluemix_notm}} console.
- APP_ID with your Streams application ID.
