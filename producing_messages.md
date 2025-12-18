---

copyright:
  years: 2015, 2023
lastupdated: "2023-11-28"

keywords: producer, producing messages, message ordering, batching, throttling, compression

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Producing messages
{: #producing_messages }

A producer is an application that publishes streams of messages to Kafka topics. This information focuses on the Java programming interface that is part of the Apache Kafka project. The concepts apply to other languages too, but the names are sometimes a little different.
{: shortdesc}

In the programming interfaces, a message is called a record. For example, the Java class org.apache.kafka.clients.producer.ProducerRecord is used to represent a message from the point of view of the producer API. The terms _record_ and _message_ can be used interchangeably, but essentially a record is used to represent a message.

When a producer connects to Kafka, it makes an initial bootstrap connection. This connection can be to any of the servers in the cluster. The producer requests the partition and leadership information about the topic that it wants to publish to. Then, the producer establishes another connection to the partition leader and can publish messages. These actions happen automatically internally when your producer connects to the Kafka cluster.

To ensure availability, the Kafka brokers replicate messages, so that if one broker is unavailable, the others can still receive messages from producers and send them to consumers. {{site.data.keyword.messagehub}} uses a replication factor of 3, meaning that each message is stored on three brokers. When a message is sent to the partition leader, that message is not immediately available to consumers. The leader appends the record for the message to the partition, assigning it the next offset number for that partition. After all the followers for the in-sync replicas replicated the record and acknowledged that they wrote the record to their replicas, the record is now *committed* and becomes available for consumers.

Each message is represented as a record that comprises two parts: key and value. The key is commonly used for data about the message and the value is the body of the message. Because many tools in the Kafka ecosystem (such as connectors to other systems) use only the value and ignore the key, it is best to put all of the message data in the value and  use the key for partitioning or log compaction. Do not rely on everything that reads from Kafka to use the key.

Many other messaging systems also have a way of carrying other information along with the messages. Kafka version 0.11 introduces record headers for this purpose.

You might find it useful to read this information along with [consuming messages](/docs/EventStreams-gen2?topic=EventStreams-gen2-consuming_messages) in {{site.data.keyword.messagehub}}.

## Configuration settings
{: #config_settings}

Many configuration settings exist for the producer. You can control aspects of the producer, including batching, retries, and message acknowledgment. Here are the most important ones:

| Name     |Description   | Valid values   | Default   |
|----------|---------|----------|---------|
|key.serializer     | The class used to serialize keys. | Java class that implements Serializer interface, such as org.apache.kafka.common.serialization.StringSerializer. |No default - you must specify a value.|
|value.serializer     | The class used to serialize values. | Java class that implements Serializer interface, such as org.apache.kafka.common.serialization.StringSerializer.  | No default - you must specify a value. |
|acks     | The number of servers required to acknowledge each message published. This controls the durability guarantees that the producer requires. | 0, 1, all (or -1)  | all (Kafka 3.0 and later) 1 (earlier than Kafka 3.0) |
|retries     | The number of times that the client resends a message when the send encounters an error. | 0,...  | 0 |
|max.block.ms     | The number of milliseconds that a send or metadata request can block waiting. | 0,...  | 60000 (1 minute) |
|max.in.flight.requests.per.connection     | The maximum number of unacknowledged requests that the client sends on a connection before it blocks further requests.| 1,...  | 5 |
|request.timeout.ms     | The maximum amount of time the producer waits for a response to a request. If the response is not received before the timeout elapses, the request is retried, or fails if the number of retries was exhausted.| 0,...  | 30000 (30 seconds) |
{: caption="Producer configuration settings" caption-side="top"}

Many more configuration settings are available, but ensure that you read the [Apache Kafka documentation](https://kafka.apache.org/){: external} thoroughly before you experiment with them.

## Partitioning
{: #partitioning}

With Kafka, partitions are the unit of scalability. Therefore, partitioning is an effective way to increase your throughput because it allows topic data to flow in multiple parallel streams.

When the producer publishes a message on a topic, the producer can choose which partition to use. If ordering is important, note that a partition is an ordered sequence of records, but a topic comprises one or more partitions. If you want a set of messages to be delivered in order, ensure that they all go on the same partition. The most straightforward way to achieve this is to give all of those messages the same key.

The producer can explicitly specify a partition number when it publishes a message. This gives direct control, but it makes the producer code more complex because it takes on the responsibility for managing the partition selection. For more information, see the method call Producer.partitionsFor. For example, the call is described for
[Kafka version 2.2.0](https://kafka.apache.org/22/javadoc/org/apache/kafka/clients/producer/KafkaProducer.html).{: external}

If the producer does not specify a partition number, the selection of partition is made by a partitioner. The default partitioner that is built into the Kafka producer works as follows:

* If the record does not have a key, select the partition in a round-robin fashion.
* If the record does have a key, select the partition by calculating a hash value for the key. This has the effect of selecting the same partition for all messages with the same key.

You can also write your own custom partitioner. A custom partitioner can choose any scheme to assign records to partitions. For example, use just a subset of the information in the key or an application-specific identifier.


## Message ordering
{: #message_ordering}

Kafka generally writes messages in the order that they are sent by the producer. However, in certain situations retries can cause messages to be duplicated or reordered. If you want a sequence of messages to be sent in order, it is important to ensure that they are all written to the same partition because this is the only way to guarantee message ordering.

The producer is also able to retry to send messages automatically. It is a good idea to enable this retry feature because the alternative is that your application code must  perform any retries itself. The combination of batching in Kafka and automatic retries can have the effect of duplicating messages and reordering them.

For example, if you publish a sequence of three messages &lt;M1, M2, M3&gt; on a topic. The records might all fit within the same batch, so they're all sent to the partition leader together. The leader then writes them to the partition and replicates them as separate records. If a failure occurs, it is possible that M1 and M2 are added to the partition, but M3 is not. The producer doesn't receive an acknowledgment, so it retries sending &lt;M1, M2, M3&gt;. The new leader writes M1, M2 and M3 onto the partition, which now contains &lt;M1, M2, M1, M2, M3&gt;, where the duplicated M1 follows the original M2. If you restrict the number of requests in flight to each broker to just one, you can prevent this reordering. You might still find that a single record is duplicated such as &lt;M1, M2, M2, M3&gt;, but you never get out of order sequences. In Kafka version 0.11 or later, you can also use the idempotent producer feature to prevent the duplication of M2.

It is normal practice with Kafka to write the applications to handle occasional message duplicates because the performance impact of having only a single request in flight is significant.

## Message acknowledgments
{: #message_acknowledgments}

When you publish a message, you can choose the level of acknowledgments that are required by using the `acks` producer configuration. The choice represents a balance between throughput and reliability. The following three levels exist.


acks=0 (least reliable)
:   The message is considered sent as soon as it was written to the network. There is no acknowledgment from the partition leader. As a result, messages can be lost if the partition leadership changes. This level of acknowledgment is fast, but comes with the possibility of message loss in some situations.

acks=1 (the default)
:   The message is acknowledged to the producer as soon as the partition leader successfully wrote its record to the partition. Because the acknowledgment occurs before the record reached the in-sync replicas, the message might be lost if the leader fails, but the followers do not yet have the message. If partition leadership changes, the old leader informs the producer, which can handle the error and retry to send the message to the new leader. Because messages are acknowledged before their receipt was confirmed by all replicas, messages that were acknowledged but not yet fully replicated can be lost if the partition leadership changes.

acks=all (most reliable)
:   The message is acknowledged to the producer when the partition leader successfully wrote its record and all in-sync replicas did the same. The message is not lost if the partition leadership changes provided that at least one in-sync replica is available.


Even if you do not wait for messages to be acknowledged to the producer, messages are still only available to be consumed when committed, and that means replication to the in-sync replicas is complete. In other words, the latency of sending the messages from the point of view of the producer is lower than the end-to-end latency measured from the producer sending a message to a consumer receiving the message.

If possible, avoid waiting for the acknowledgment of a message before you publish the next message. Waiting prevents the producer from being able to batch together messages and also reduces the rate that messages can be published to below the round-trip latency of the network.

## Batching, throttling, and compression
{: #batching}

For efficiency purposes, the producer collects batches of records together for sending to the servers. If you enable compression, the producer compresses each batch, which can improve performance by requiring less data to be transferred over the network.

If you try to publish messages faster than they can be sent to a server, the producer automatically buffers them up into batched requests. The producer maintains a buffer of unsent records for each partition. There comes a point when even batching does not allow the rate that you want to be achieved.

There is another factor that has an impact. To prevent individual producers or consumers from swamping the cluster, {{site.data.keyword.messagehub}} applies throughput quotas. The rate that each producer is sending data at is calculated and any producer that attempts to exceed its quota is throttled. The throttling is applied by slightly delaying the sending of responses to the producer. Usually, this just acts as a natural brake.

For more information about throughput guidance, see [Limits and quotas]/docs/EventStreams-gen2?topic=EventStreams-gen2-how-event-streams-uses-limits-and-quotas).

In summary, when a message is published, its record is first written into a buffer in the producer. In the background, the producer batches up and sends the records to the server. The server then responds to the producer, possibly applying a throttling delay if the producer is publishing too fast. If the buffer in the producer fills up, the producer's send call is delayed, but ultimately might fail with an exception.

## Delivery semantics
{: #delivery_semantics}

Kafka offers the following multiple different message delivery semantics:

* *At most once*: Messages might get lost and do not get redelivered.
* *At least once*: Messages are never lost but there might be duplicates.
* *Exactly once*: Messages are never lost and there are no duplicates.

The delivery semantics are determined by the following settings:

* `acks`
* `retries`
* `enable.idempotence`

By default, Kafka uses *at least once* semantics.

To enable *exactly once* semantics, you must use the idempotent or transactional producers. The idempotent producer is enabled by setting `enable.idempotence` to `true` and guarantees that exactly one copy of each message is written to Kafka, even if it retries. The transactional producer enables the sending of data to multiple partitions such that either all messages are successfully delivered, or none of them are. That is, a transaction is either fully committed or fully discarded. You can also include offsets in transactions to build applications that read, process, and write messages to Kafka.

## Code snippets
{: #code_snippets}

These code snippets are at a high level to illustrate the concepts involved. For complete examples, see the {{site.data.keyword.messagehub}} samples in [GitHub](https://github.com/ibm-messaging/event-streams-samples){: external}.

To connect a consumer to {{site.data.keyword.messagehub}}, you must create service credentials. For more information, see [Connecting to {{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-connecting).

In the producer code, you first need to build the set of configuration properties. All connections to {{site.data.keyword.messagehub}} are secured by using TLS and user and password authentication, so you need these properties at a minimum. Replace BOOTSTRAP_ENDPOINTS, USER, and PASSWORD with those from your own service credentials:

```text
Properties props = new Properties();
 props.put("bootstrap.servers", BOOTSTRAP_ENDPOINTS);
 props.put("sasl.jaas.config", "org.apache.kafka.common.security.plain.PlainLoginModule required username=\"USER\" password=\"PASSWORD\";");
 props.put("security.protocol", "SASL_SSL");
 props.put("sasl.mechanism", "PLAIN");
 props.put("ssl.protocol", "TLSv1.2");
 props.put("ssl.enabled.protocols", "TLSv1.2");
 props.put("ssl.endpoint.identification.algorithm", "HTTPS");
```

To send messages, you also need to specify serializers for the keys and values, for example:

```text
 props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
 props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
```

These serializers must match the deserializers used by the consumers.

Then, use a KafkaProducer to send messages, where each message is represented by a ProducerRecord. Don't forget to close the KafkaProducer when you're finished. This code just sends the message but it doesn't wait to see whether the send succeeded. The message is sent to topic `T1`, with key the string `key` and value the string `value`.

```text
 Producer<String, String> producer = new KafkaProducer<>(props);
 producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
 producer.close();
 ```

The `send()` method is asynchronous and returns a Future that you can use to check its completion:

```text
 Future<RecordMetadata> f = producer.send(new ProducerRecord<String, String>("T1", "key", "value"));
// Do some other stuff
// Now wait for the result of the send
 RecordMetadata rm = f.get();
 long offset = rm.offset;
```

Alternatively, you can supply a callback when you send the message:

```text
producer.send(new ProducerRecord<String,String>("T1","key","value", new Callback() {
          public void onCompletion(RecordMetadata metadata, Exception exception) {
                     // This is called when the send completes, either successfully or with an exception
          }
});
```

For more information, see the [Javadoc for the Kafka client](https://kafka.apache.org/11/javadoc/index.html?overview-summary.html).{: external}.
