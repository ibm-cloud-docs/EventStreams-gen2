---

copyright:
  years: 2023
lastupdated: "2023-12-05"

keywords: quotas, quota implementation, mapping quotas, authorization, client metrics

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Setting Kafka quotas
{: #enabling_kafka_quotas}

Kafka quotas enforce limits on produce and consume requests to control the broker resources used by clients. Kafka quotas enable an administrator to enforce limits on the network throughput that can be consumed by individual producer and consumer applications.
{: shortdesc}

## About Kafka quotas
{: #intro_kafka_quotas}

If left unconstrained, it is possible for a small number of consumers or producers to monopolize the available network throughput for your service instance.

Kafka brokers support quotas that enforce rate limits to prevent clients saturating the network or monopolizing broker resources. For more information, see the 
[Apache Kafka documentation](https://kafka.apache.org/documentation/#design_quotas).

Kafka quotas can be configured to limit network bandwidth usage, Kafka measures this throughput in bytes per second. If throughput over a 30 second window is found to 
exceed a configured quota, Kafka calculates a sufficient delay to bring throughput within the quota limit.

Kafka brokers then send the delay information to clients as part of standard Kafka protocol responses. A cooperative client, respecting the protocol contract, waits for this delay before making new requests; an uncooperative client may not respect the throttling request, but in such a case, the broker does not read that client's requests until the throttling delay has elapsed (which may cause timeouts on the uncooperative client).

The following information applies for quotas:

- Separate quotas may be defined for producers and consumers. 
- By default, client quotas are unlimited.
- Quotas are applied per broker, rather than per cluster.
- A quota is applied to all clients that share a single user identity.
- A quota can be applied to the 'default' user, so it applies to any user, even for whom no user-specific quota was set.

## Client metrics
{: #client_metrics}

The Java client also exposes throttling information with the following per-broker metrics:

- produce-throttle-time-max
- produce-throttle-time-avg
- fetch-throttle-time-max
- fetch-throttle-time-avg

## Setting client quotas
{: #setting_quotas}

The {{site.data.keyword.messagehub_full}} Enterprise plan allows the use of the Kafka API to set and describe quotas on Kafka V3.1.x clusters. For more information, see the [Quota Operations section](https://cloud.ibm.com/apidocs/event-streams/adminrest#create-quota) of the {{site.data.keyword.messagehub_full}} Admin REST API.

With reference to the [Kafka documentation on quotas](https://kafka.apache.org/documentation/#quotas), only throughput quota types ("producer_byte_rate" and "consumer_byte_rate" quota types) applied to the "user" entity (or the "default user") are supported. 

The "client-id" entity, the "request", and the "controller-mutation" quota types are not supported as user-settable quotas. In {{site.data.keyword.messagehub}}, an authenticated user identity is represented by an {{site.data.keyword.iamlong}} ID. Because Kafka quotas are applied per {{site.data.keyword.iamshort}} ID, a single quota can be shared by a group of API keys, if these all belong to the same {{site.data.keyword.iamshort}} service ID.

To obtain the {{site.data.keyword.iamshort}} ID of an {{site.data.keyword.iamshort}} (IAM) service ID, the IBM Cloud CLI can be used.

```bash
ibmcloud iam service-id ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab --output json
```
{: codeblock}

See the following example output:

```bash
{

    "active":true,

    "jti":"...",

    "iam_id":"iam-ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab",

    "realmId":"iam",

     ....

}
```
{: codeblock}

## Mapping quotas onto an IBM Event Streams Enterprise cluster
{: #mapping_quotas_enterprise}

The Kafka API quotas are per-broker, however Enterprise plan capacity is described as a [per-cluster throughput](https://cloud.ibm.com/docs/EventStreams?topic=EventStreams-kafka_quotas#enterprise_throughput). Therefore, if you want to limit a user to 10 MB/s in total, you apply a quota of 10/n MB/s to each broker (where n is the number of brokers in the cluster).

To find the number of brokers in a cluster, you can use the `KafkaAdminClient.describeCluster` call.

For more information, see the [Java documentation](https://kafka.apache.org/32/javadoc/org/apache/kafka/clients/admin/KafkaAdminClient.html#describeClusterorg.apache.kafka.clients.admin.DescribeClusterOptions).

The number of brokers can also be found by using the `kafka-configs.sh` shell script bundled in the Apache Kafka distribution.

```bash
bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" --describe --entity-type brokers
```
{: codeblock}

## {{site.data.keyword.messagehub}} authorization
{: #es_authorization}

To be authorized to set client quotas, a user must have the Manager role on the "cluster" resource in {{site.data.keyword.iamshort}}.

A set of credentials created with the {{site.data.keyword.messagehub}} UI with the Manager role on the instance also has the Manager role on cluster. Thus, you are able to create, delete, and alter topics in addition to setting quotas. Any authenticated user has an implied Reader role on cluster and is able to describe quotas.
{: note}

To create a set of credentials that can manage topics, groups, and participate in transactions but are not authrorized to set quotas, an IAM access policy that has 
Manager role on resource types "topic", "group", and "txnid" and Reader role on "cluster" must to be associated with the service ID. 

### Example: Managing quotas with `the kafka-config.sh` script (Apache client)
{: #example_managing_quotas_kafka_script}

1. Download a Kafka *binary* distribution (at least V3.1.0).

2. Create a properties file (named command-config.properties in the following command lines examples), containing the following entries (replacing "myapikey" with the actual API key).

```bash
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="token" password="myapikey";

security.protocol=SASL_SSL

sasl.mechanism=PLAIN

ssl.protocol=TLSv1.2

ssl.enabled.protocols=TLSv1.2

ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

For more information, see [Configuring your Kafka API client](https://cloud.ibm.com/docs/EventStreams?topic=EventStreams-kafka_using#kafka_api_client).

3.  Work with the following usage examples.

    - Alter quotas for user:

    ```bash
    bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" --alter --add-config 'producer_byte_rate=1024,
    consumer_byte_rate=2048' --entity-type users --entity-name iam-ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab
    ```
    {: codeblock}

    Completed updating configuration for user `iam-ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab`.

    - Describe quotas for user:
    
    ```bash
    bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" --describe --entity-type users --entity-name 
    iam-ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab
    ```
    {: codeblock}

    The quota configurations for user-principal `iam-ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab` are `consumer_byte_rate=2048.0` and `producer_byte_rate=1024.0`.

    - Remove quotas for user:

    ```bash
    bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" --alter --delete-config       'producer_byte_rate,consumer_byte_rate' --entity-type users --entity-name iam-ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab
    ```
    {: codeblock}

    Completed updating config for user `iam-ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab`.

    - Describe all quotas that were set to any user, including the default user:

    ```bash
    bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" --describe --entity-type users
    ```
    {: codeblock}

    - Alter quotas for the default user:

    ```bash
    bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" --alter --add-config 'producer_byte_rate=1024,
    consumer_byte_rate=2048' --entity-type users --entity-default
    ```
    {: codeblock}

    Completed updating default configuration for users in the cluster.

    - Remove quotas for the default user:

    ```bash
    bin/kafka-configs.sh --command-config command-config.properties --bootstrap-server "kafka-0.blah.cloud:9093" --alter --delete-config 'producer_byte_rate,
    consumer_byte_rate' --entity-type users --entity-default
    ```
    {: codeblock}

    Completed updating default configuration for users in the cluster.

#### Example: Alter throughput quota usage through the Java API
{: #example_alter_throughput_quota}

The following example is a short sample snippet showing how to invoke the `KafkaAdminClient.alterclientQuotas` method.

```bash
https://kafka.apache.org/32/javadoc/org/apache/kafka/clients/admin/KafkaAdminClient.html#alterClientQuotas(java.util.Collection,org.apache.kafka.clients.admin.AlterClientQuotas
Options)

import java.util.*;

import org.apache.kafka.clients.CommonClientConfigs;

import org.apache.kafka.clients.admin.*;

import org.apache.kafka.common.config.*;

import org.apache.kafka.common.quota.*;

class Snippet {

    public static void main(String[] args) throws Exception {

        // Kafka client configuration properties.

        String mybootstrap = "...";   // from the bootstrap_endpoints of the service credentials

        String myapikey = "...";      // from the apikey of the service credentials

        Properties properties = new Properties();

        properties.put(CommonClientConfigs.BOOTSTRAP_SERVERS_CONFIG, mybootstrap);

        properties.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "sasl_ssl");

        properties.put(SslConfigs.SSL_ENABLED_PROTOCOLS_CONFIG, "TLSv1.2");

        properties.put(SslConfigs.SSL_PROTOCOL_CONFIG, "TLSv1.2");

        properties.put(SaslConfigs.SASL_MECHANISM, "PLAIN");

        properties.put(SaslConfigs.SASL_JAAS_CONFIG,

                "org.apache.kafka.common.security.plain.PlainLoginModule " +

                        "required username=\"token\" password=\"" + myapikey + "\";");

        AdminClient admin = AdminClient.create(properties);

        String iamID = "iam-ServiceId-12345678-aaaa-bbbb-cccc-1234567890ab"; //set iam id of target user to set quotas to

        // if null is used instead of the iamID string, the following quota alteration will be applied to the default user

        // add quotas 

        ClientQuotaEntity entity = new ClientQuotaEntity(

                Collections.singletonMap(ClientQuotaEntity.USER, iamID));

        ClientQuotaAlteration alteration = new ClientQuotaAlteration(entity,

                Arrays.asList(new ClientQuotaAlteration.Op("consumer_byte_rate", 1000.0),

                        new ClientQuotaAlteration.Op("producer_byte_rate", 1000.0)));

        admin.alterClientQuotas(Arrays.asList(alteration)).all().get();

        // describe quotas

        DescribeClientQuotasResult describeClientQuotasFuture = admin.describeClientQuotas(ClientQuotaFilter.all());

        System.out.println(describeClientQuotasFuture.entities().get());

        //remove quotas (set them to null)

        entity = new ClientQuotaEntity(Collections.singletonMap(ClientQuotaEntity.USER, iamID));

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

Whenever throughput quotas are updated, an {{site.data.keyword.messagehub}} configuration event is generated, which can be monitored in {{site.data.keyword.cloudaccesstraillong}}.

For more information about configuring {{site.data.keyword.at_short}} events for {{site.data.keyword.messagehub}}, see the [Activity tracker documentation](https://cloud.ibm.com/docs/EventStreams?topic=EventStreams-at_events).
