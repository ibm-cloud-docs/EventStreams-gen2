---

copyright:
  years: 2015, 2023
lastupdated: "2023-07-24"

keywords: asl.jaas.config

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Using Kafka Streams with {{site.data.keyword.messagehub}}
{: #kafka_streams }

Kafka Streams is a stream processing library that is part of the Apache Kafka project. It simplifies building applications that process and analyze data in Kafka. 
{: shortdesc}

The topic APIs work with {{site.data.keyword.messagehub}} with no setup required. Specify your SASL credentials by using ```sasl.jaas.config``` or a JAAS file and set ```replication.factor``` to 3.

Ensure that you use Streams version 0.10.2, or later.   

See the following example:

```text
    props.put(StreamsConfig.REPLICATION_FACTOR_CONFIG, "3");
    props.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, BOOTSTRAP_SERVERS);
    props.put("security.protocol","SASL_SSL");
    props.put("sasl.mechanism","PLAIN");
    props.put("ssl.protocol","TLSv1.2");
    props.put("ssl.enabled.protocols","TLSv1.2");
    props.put("sasl.jaas.config","org.apache.kafka.common.security.plain.PlainLoginModule required username=\"USERNAME\" password=\"PASSWORD\";");
```
{: codeblock}

Where `BOOTSTRAP_SERVERS`, `USERNAME`, and `PASSWORD` are the values from your {{site.data.keyword.messagehub}} **Service Credentials** tab in
{{site.data.keyword.Bluemix_notm}}.


