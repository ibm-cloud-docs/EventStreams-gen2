---

copyright:
  years: 2026
lastupdated: "2026-11-10"

keywords: quotas, quota implementation, mapping quotas, authorization, client metrics

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Configuring Kafka quotas on {{site.data.keyword.cloud_notm}}
{: #enabling_kafka_quotas}

[Gen 2]{: tag-purple}

Learn how to set Kafka quotas to enforce limits on produce and consume requests, controlling broker resources used by clients on {{site.data.keyword.cloud_notm}}.
{: shortdesc}

## About Kafka quotas
{: #intro_kafka_quotas}

Without constraints, a few consumers or producers can monopolize the available network throughput for your service instance.

Kafka brokers support quotas that enforce rate limits to prevent clients from saturating the network or monopolizing broker resources. For more information, see the [Apache Kafka documentation](https://kafka.apache.org/){: external}.

You can configure Kafka quotas to limit network bandwidth usage. Kafka measures this throughput in bytes per second. If throughput over a 30-second window exceeds a configured quota, Kafka calculates a delay to bring throughput back within the quota limit.

Kafka brokers send throttling delay details to clients in standard Kafka protocol responses. A cooperative client waits for this delay before it makes new requests. If an uncooperative client ignores throttling, the broker halts reading requests from that client until the delay expires, which can cause client timeouts.

The following rules apply to Kafka quotas:

- You can define separate quotas for producers and consumers.
- By default, client quotas are unlimited.
- Quotas are applied per broker, not per cluster.
- A quota applies to all clients that share a single user identity.
- A quota can be applied to the default user, applying to any user without an explicit quota.

## Client metrics
{: #client_metrics}

The Java client also exposes throttling information with the following per-broker metrics:

- produce-throttle-time-max
- produce-throttle-time-avg
- fetch-throttle-time-max
- fetch-throttle-time-avg

## Setting client quotas
{: #setting_quotas}

The {{site.data.keyword.messagehub_full}} Enterprise plan supports using the Kafka API to set and describe quotas.

According to the [Kafka documentation on quotas](https://kafka.apache.org/){: external}, only throughput quota types (`producer_byte_rate` and `consumer_byte_rate`) applied to the `user` entity or the default user are supported.

The `client-id`, `request`, and `controller-mutation` quota types are not supported as user-settable quotas. In {{site.data.keyword.messagehub}}, an authenticated user identity is represented by an {{site.data.keyword.iamlong}} service ID. Because Kafka quotas are applied per {{site.data.keyword.iamshort}} service ID, multiple API keys share a single quota if they belong to the same service ID.

## Mapping quotas onto an IBM Event Streams Enterprise cluster
{: #mapping_quotas_enterprise}

Kafka API quotas apply per broker, while Enterprise plan capacity is specified as total per-cluster throughput. To limit a user to 10 MB/s in total, apply a quota of 10/*n* MB/s to each broker, where *n* is the number of brokers in the cluster.

To find the number of brokers in a cluster, use the `KafkaAdminClient.describeCluster` method. For more information, see the [Kafka Java API reference](https://kafka.apache.org/32/javadoc/org/apache/kafka/clients/admin/KafkaAdminClient.html#describeClusterorg.apache.kafka.clients.admin.DescribeClusterOptions){: external}.

You can also determine the number of brokers by using the `kafka-configs.sh` shell script included with the Apache Kafka distribution:

```bash
bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" --describe --entity-type brokers
```
{: codeblock}

## {{site.data.keyword.messagehub}} authorization
{: #es_authorization}

To be authorized to set client quotas, a user must have the Manager role on the "cluster" resource in {{site.data.keyword.iamshort}}.

A set of credentials created with the {{site.data.keyword.messagehub}} UI with the Manager role on the instance also has the Manager role on cluster. Thus, you can create, delete, and alter topics in addition to setting quotas. Any authenticated user has an implied Reader role on cluster and can describe quotas.
{: note}

To create credentials that can manage topics, consumer groups, and transactions without permission to set quotas, assign an IAM access policy with the Manager role on resource types `topic`, `group`, and `txnid`, and the Reader role on `cluster`.

### Example: Managing quotas with the `kafka-configs.sh` script
{: #example_managing_quotas_kafka_script}

1. Download a Kafka *binary* distribution (4.1.x recommended).

2. Create a properties file (named command-config.properties in the following command lines examples), containing the following entries (replacing "myapikey" with the actual API key).

```bash
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="token" password="myapikey";
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
```
{: codeblock}

For more information, see [Configuring your Kafka API client](/docs/EventStreams-gen2?topic=EventStreams-gen2-using_kafka_api#kafka_api_client).

3.  Work with the following usage examples.

    - Alter quotas for user:

    ```bash
    bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" \
    --alter --add-config 'producer_byte_rate=1024, consumer_byte_rate=2048' \
    --entity-type users --entity-name ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab
    ```
    {: codeblock}

    Completed updating configuration for user `ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab`.

    - Describe quotas for user:

    ```bash
    bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" \
    --describe --entity-type users --entity-name ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab
    ```
    {: codeblock}

    The quota configurations for user-principal `ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab` are `consumer_byte_rate=2048.0` and `producer_byte_rate=1024.0`.

    - Remove quotas for user:

    ```bash
    bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" \
    --alter --delete-config 'producer_byte_rate,consumer_byte_rate' \
    --entity-type users --entity-name ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab
    ```
    {: codeblock}

    Completed updating config for user `ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab`.

    - Describe all quotas that were set to any user, including the default user:

    ```bash
    bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" \
    --describe --entity-type users
    ```
    {: codeblock}

    - Alter quotas for the default user:

    ```bash
    bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" \
    --alter --add-config 'producer_byte_rate=1024,
    consumer_byte_rate=2048' --entity-type users --entity-default
    ```
    {: codeblock}

    Completed updating default configuration for users in the cluster.

    - Remove quotas for the default user:

    ```bash
    bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" \
    --alter --delete-config 'producer_byte_rate,
    consumer_byte_rate' --entity-type users --entity-default
    ```
    {: codeblock}

    Completed updating default configuration for users in the cluster.

#### Example: Alter throughput quota usage through the Java API
{: #example_alter_throughput_quota}

The following example shows how to invoke the `KafkaAdminClient.alterClientQuotas` method. For more information, see the [alterClientQuotas Java reference](https://kafka.apache.org/32/javadoc/org/apache/kafka/clients/admin/KafkaAdminClient.html#alterClientQuotas(java.util.Collection,org.apache.kafka.clients.admin.AlterClientQuotasOptions)){: external}.

```java
import java.util.*;
import org.apache.kafka.clients.CommonClientConfigs;
import org.apache.kafka.clients.admin.*;
import org.apache.kafka.common.config.*;
import org.apache.kafka.common.quota.*;

class Snippet {
    public static void main(String[] args) throws Exception {
        String mybootstrap = "<bootstrap_endpoint>";
        String myapikey = "<api_key>";

        Properties properties = new Properties();
        properties.put(CommonClientConfigs.BOOTSTRAP_SERVERS_CONFIG, mybootstrap);
        properties.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_SSL");
        properties.put(SaslConfigs.SASL_MECHANISM, "PLAIN");
        properties.put(SaslConfigs.SASL_JAAS_CONFIG,
                "org.apache.kafka.common.security.plain.PlainLoginModule " +
                        "required username=\"token\" password=\"" + myapikey + "\";");

        AdminClient admin = AdminClient.create(properties);
        String serviceID = "ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab";

        // Add quotas
        ClientQuotaEntity entity = new ClientQuotaEntity(
                Collections.singletonMap(ClientQuotaEntity.USER, serviceID));
        ClientQuotaAlteration alteration = new ClientQuotaAlteration(entity,
                Arrays.asList(new ClientQuotaAlteration.Op("consumer_byte_rate", 1000.0),
                        new ClientQuotaAlteration.Op("producer_byte_rate", 1000.0)));
        admin.alterClientQuotas(Arrays.asList(alteration)).all().get();

        // Describe quotas
        DescribeClientQuotasResult describeResult = admin.describeClientQuotas(ClientQuotaFilter.all());
        System.out.println(describeResult.entities().get());

        // Remove quotas
        alteration = new ClientQuotaAlteration(entity,
                Arrays.asList(new ClientQuotaAlteration.Op("consumer_byte_rate", null),
                        new ClientQuotaAlteration.Op("producer_byte_rate", null)));
        admin.alterClientQuotas(Arrays.asList(alteration)).all().get();
    }
}
```
{: codeblock}

## {{site.data.keyword.cloudaccesstraillong_notm}} events
{: #activity_tracker_events}

When you update throughput quotas, an {{site.data.keyword.messagehub}} configuration event is generated in {{site.data.keyword.cloudaccesstraillong}}.

For more information about configuring {{site.data.keyword.at_short}} events for {{site.data.keyword.messagehub}}, see [Activity tracker events](/docs/EventStreams-gen2?topic=EventStreams-gen2-at_events).
