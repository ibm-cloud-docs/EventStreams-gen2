---

copyright:
  years: 2026
lastupdated: "2026-11-09"

keywords: api, consumer, producer, admin, streams, connect, client

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Using Kafka APIs with {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}}
{: #using_kafka_api}

[Gen 2]{: tag-purple}

Learn how to use Kafka APIs, including the core, Streams, and Connect APIs, with {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}} to build event-driven applications.
{: shortdesc}

Kafka core API (Consumer, Producer, and Admin API)
:   Send and receive messages directly to and from Kafka topics. The Kafka Admin client manages cluster resources, including creating, configuring, and deleting topics and managing consumer groups.

Streams API
:   A stream-processing library for consuming, transforming, aggregating, and producing event streams between topics.

Connect API
:   A framework for integrating external systems, such as databases and messaging engines, by using reusable connector plug-ins.

The following table summarizes API support on {{site.data.keyword.messagehub}}:

| Feature | Enterprise Gen 2 plan |
| --- | --- |
| Kafka version on cluster | Apache Kafka 4.1 |
| Minimum recommended Kafka client version | Kafka 2.6.0 or later |
| Supported client versions | [Support summary for recommended clients](#client_summary) |
| Kafka Connect supported | Yes |
| Kafka Streams supported | Yes |
| ksqlDB supported | Yes |
| Authentication requirements | SASL PLAIN or SASL OAUTHBEARER over TLS 1.2 with Server Name Indication (SNI) |
{: caption="Kafka client support in Enterprise Gen 2 plan" caption-side="bottom"}

## Choosing a Kafka client to use with {{site.data.keyword.messagehub}}
{: #kafka_clients}

The official client for the Kafka API is written in Java and contains the latest features and bug fixes. For more information about this API, see [Kafka Producer API 4.1](https://kafka.apache.org/41/javadoc/org/apache/kafka/clients/producer/KafkaProducer.html){: external} and [Kafka Consumer API 4.1](https://kafka.apache.org/41/javadoc/org/apache/kafka/clients/consumer/KafkaConsumer.html){: external}.

For other languages, use one of the following clients, which are tested with {{site.data.keyword.messagehub}}:

### Support summary for all recommended clients
{: #client_summary}

| Client | Language | Recommended version | Minimum version supported [^tabletext1] | Link to sample |
| --- | --- | --- | --- | --- |
| **Official Apache Kafka client:** | | | | |
| [Apache Kafka client](https://kafka.apache.org/community/downloads/){: external} | Java | 4.1.1 or later | 2.5.0 | [Java console sample](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_java_using) \n \n [Liberty sample](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample){: external} |
| **Third-party clients:** | | | | |
| [confluent-kafka-javascript](https://github.com/confluentinc/confluent-kafka-javascript){: external} | Node.js | Latest | 1.0.0 | |
| [confluent-kafka-python](https://github.com/confluentinc/confluent-kafka-python){: external} | Python | Latest | 1.4.0 | [Kafka Python sample](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample){: external} |
| [confluent-kafka-go](https://github.com/confluentinc/confluent-kafka-go){: external} | Go | Latest | 1.4.0 | |
| [librdkafka](https://github.com/confluentinc/librdkafka){: external} | C or C++ | Latest | 1.4.0 | |
| [node-rdkafka](https://github.com/Blizzard/node-rdkafka){: external} | Node.js | Latest | 2.8.0 | [Node.js sample](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample){: external} |
| [sarama](https://github.com/IBM/sarama){: external} | Go | Latest | 1.40.0 | [Sarama examples](https://github.com/IBM/sarama/tree/v1.45.0/examples){: external} |
{: caption="Client support summary" caption-side="bottom"}

[^tabletext1]: The earliest version validated in continuous integration testing. If you use an alternative client library, verify that it supports: 1. Kafka protocol 2.5 or later. 2. SASL PLAIN or SASL OAUTHBEARER authentication over TLS 1.2. 3. Server Name Indication (SNI) in TLS handshakes. 4. Elliptic curve cryptography (ECC).

### Connecting your client to {{site.data.keyword.messagehub}}
{: #connect_client}

For information about how to configure your Java client to connect to {{site.data.keyword.messagehub}}, see [Configuring your client](/docs/EventStreams-gen2?topic=EventStreams-gen2-using_kafka_api#kafka_api_client).

## Configuring your Kafka API client
{: #kafka_api_client}

To establish a connection, clients must use SASL PLAIN or SASL OAUTHBEARER over TLS 1.2 at a minimum, with a username and a list of bootstrap servers. TLS 1.2 ensures that connections are encrypted and validates the authenticity of the brokers. SASL enforces authentication on all connections.

To retrieve the username, password, and list of bootstrap servers, obtain a service credentials object for the service instance. For more information about creating credentials, see [Connecting to {{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-connecting).

### Using SASL PLAIN
{: #using_sasl_plain}

Use the following strings and properties:

- Use the `bootstrap_endpoints` string as the list of bootstrap servers and pass this string of host and port pairs to your Kafka client.
- Use the `user` and `api_key` properties as the username and password.

For a Java client, the following example shows the minimum set of properties. Replace `${USERNAME}`, `${PASSWORD}`, and `${BOOTSTRAP_ENDPOINTS}` with your retrieved values.

```config
bootstrap.servers=${BOOTSTRAP_ENDPOINTS}
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="${USERNAME}" password="${PASSWORD}";
security.protocol=SASL_SSL
ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

If you use a Kafka client earlier than version 0.10.2.1, the `sasl.jaas.config` property isn't supported, and you must instead provide the client configuration in a JAAS configuration file.
{: note}

### Using SASL OAUTHBEARER with Java clients version 3.4 - 4.0
{: #using_sasl_oauthbearer}

Before you configure the SASL mechanism for a Java client, verify the following prerequisites:

- The minimum supported Kafka Java client version is 3.4 (3.6 or later is recommended).
- An additional JAR package must be downloaded from Maven Central and made available in your classpath.

If you use Maven, add the following dependency to `pom.xml`:

```xml
<dependency>
    <groupId>com.ibm.cloud.eventstreams</groupId>
    <artifactId>oauth-client</artifactId>
    <version>1.4.0</version>
</dependency>
```
{: codeblock}

If you use Gradle, add the following dependency to `build.gradle`:

```gradle
implementation com.ibm.cloud.eventstreams:oauth-client:1.4.0
```
{: codeblock}

{{site.data.keyword.iamlong}} supports multiple methods to generate a bearer token. Two methods are supported by this OAuth client library:

- API key
- Trusted profile and compute resource token

#### Using SASL OAUTHBEARER with API key
{: #using_sasl_oauthbearer_api_key}

Use the following strings and properties:

- Use the `BOOTSTRAP_ENDPOINTS` string as the list of bootstrap servers and pass this string of host and port pairs to your Kafka client.
- The `IAMOAuthBearerLoginCallbackHandler` is provided by the JAR package `com.ibm.cloud.eventstreams:oauth-client:+`.
- The {{site.data.keyword.iamlong}} token endpoint `https://iam.cloud.ibm.com/identity/token` generates a token from the API key by using the specified grant type in JAAS configuration. Token generation occurs on the client side, so the API key is never sent to the server side.
- The {{site.data.keyword.iamshort}} key endpoint `https://iam.cloud.ibm.com/identity/keys` validates the token.
- `grant_type` in `sasl.jaas.config` is `urn:ibm:params:oauth:grant-type:apikey`.
- `apikey` in `sasl.jaas.config` is the API key used to generate the bearer token at the client side.

For a Java client, the following example shows the minimum set of properties. Replace `${BOOTSTRAP_ENDPOINTS}` and `${APIKEY}` with your retrieved values.

```config
bootstrap.servers=${BOOTSTRAP_ENDPOINTS}
security.protocol=SASL_SSL
sasl.mechanism=OAUTHBEARER
sasl.oauthbearer.token.endpoint.url=https://iam.cloud.ibm.com/identity/token
sasl.oauthbearer.jwks.endpoint.url=https://iam.cloud.ibm.com/identity/keys
sasl.login.callback.handler.class=com.ibm.eventstreams.oauth.client.IAMOAuthBearerLoginCallbackHandler
sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required grant_type="urn:ibm:params:oauth:grant-type:apikey" apikey="${APIKEY}";
```
{: codeblock}

#### Using SASL OAUTHBEARER with trusted profile and compute resource token
{: #using_sasl_oauthbearer_trusted_profile}

The properties are the same as with the API key, except for the `sasl.jaas.config` property:

- `grant_type` in `sasl.jaas.config` is `urn:ibm:params:oauth:grant-type:cr-token`.
- `profile_id` in `sasl.jaas.config` is a file location storing the trusted profile ID. This file can be mounted to a Kubernetes pod running Kafka client code as a read-only volume and made available to the Kafka client code.
- `cr_token` in `sasl.jaas.config` is a file location storing the service account token from a Kubernetes pod running Kafka client code. For more information, see [What is a service account token](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#launch-a-pod-using-service-account-token-projection){: external}.

The following example shows the configuration properties:

```config
bootstrap.servers=${BOOTSTRAP_ENDPOINTS}
security.protocol=SASL_SSL
sasl.mechanism=OAUTHBEARER
sasl.oauthbearer.token.endpoint.url=https://iam.cloud.ibm.com/identity/token
sasl.oauthbearer.jwks.endpoint.url=https://iam.cloud.ibm.com/identity/keys
sasl.login.callback.handler.class=com.ibm.eventstreams.oauth.client.IAMOAuthBearerLoginCallbackHandler
sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required grant_type="urn:ibm:params:oauth:grant-type:cr-token" profile_id="${TRUSTED_PROFILE_ID_FILE_PATH}" cr_token="${SERVICE_ACCOUNT_TOKEN_FILE_PATH}";
```
{: codeblock}

For more information, see [Administering trusted profiles](/docs/iam?topic=iam-trusted-profiles-overview).

For source code, see the [{{site.data.keyword.messagehub}} Java SDK](https://github.com/IBM/eventstreams-java-sdk/tree/main/modules/oauth-client){: external}.

### Using SASL OAUTHBEARER with Java clients version 4.1 and later
{: #using_sasl_oauthbearer_java_4.1}

For Kafka Java clients 4.1 and later, use version 2.0.0 or later of the `oauth-client` library, which uses the default Kafka callback handler with `IAMTokenRetriever`.

If you use Maven, add the following dependency to `pom.xml`:

```xml
<dependency>
    <groupId>com.ibm.cloud.eventstreams</groupId>
    <artifactId>oauth-client</artifactId>
    <version>2.0.0</version>
</dependency>
```
{: codeblock}

If you use Gradle, add the following dependency to `build.gradle`:

```gradle
implementation 'com.ibm.cloud.eventstreams:oauth-client:2.0.0'
```
{: codeblock}

{{site.data.keyword.iamlong}} supports two methods to generate bearer tokens with this library:

- API key
- Trusted profile and compute resource token

#### Using SASL OAUTHBEARER with API key for Kafka 4.1 and later
{: #using_sasl_oauthbearer_api_key_41}

Use the following strings and properties in addition to the mandatory `bootstrap.servers` and any specific producer, consumer, and admin settings:

```properties
security.protocol=SASL_SSL
sasl.mechanism=OAUTHBEARER
sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required \
    grant_type="urn:ibm:params:oauth:grant-type:apikey" \
    apikey="${YOUR_IBM_CLOUD_API_KEY}";
sasl.login.callback.handler.class=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginCallbackHandler
sasl.oauthbearer.jwt.retriever.class=com.ibm.cloud.eventstreams.oauth.client.IAMTokenRetriever
sasl.oauthbearer.token.endpoint.url=https://private.iam.cloud.ibm.com/identity/token
sasl.oauthbearer.jwks.endpoint.url=https://private.iam.cloud.ibm.com/identity/keys
```
{: codeblock}

#### Using SASL OAUTHBEARER with trusted profile and compute resource token in containerized environments
{: #using_sasl_oauthbearer_trusted_containerized}

For more information, see [Generating an IAM token for a compute resource](/docs/iam?topic=iam-trusted-profile-iam-token).

```properties
security.protocol=SASL_SSL
sasl.mechanism=OAUTHBEARER
sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required \
    grant_type="urn:ibm:params:oauth:grant-type:cr-token" \
    cr_token="/path/to/cr-token-file" \
    profile_id="/path/to/profile-id-file";
sasl.login.callback.handler.class=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginCallbackHandler
sasl.oauthbearer.jwt.retriever.class=com.ibm.cloud.eventstreams.oauth.client.IAMTokenRetriever
sasl.oauthbearer.token.endpoint.url=https://private.iam.cloud.ibm.com/identity/token
sasl.oauthbearer.jwks.endpoint.url=https://private.iam.cloud.ibm.com/identity/keys
```
{: codeblock}

#### Using SASL OAUTHBEARER with trusted profile and compute resource token in Virtual Servers for VPC
{: #using_sasl_oauthbearer_trusted_vpc}

For more information, see [Generating an IAM token for a compute resource](/docs/iam?topic=iam-trusted-profile-iam-token) and [Identity operations](/docs/vpc?topic=vpc-imd-identity-operations&interface=api#imd-token-exchange).

```properties
security.protocol=SASL_SSL
sasl.mechanism=OAUTHBEARER
sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required \
    grant_type="urn:ibm:params:oauth:grant-type:cr-token" \
    cr_token="/path/to/cr-token-file" \
    profile_id="/path/to/profile-id-file" \
    vpc_metadata="true";
sasl.login.callback.handler.class=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginCallbackHandler
sasl.oauthbearer.jwt.retriever.class=com.ibm.cloud.eventstreams.oauth.client.IAMTokenRetriever
sasl.oauthbearer.token.endpoint.url=https://api.metadata.cloud.ibm.com/identity/v1/iam_tokens?version=2025-10-14
sasl.oauthbearer.jwks.endpoint.url=https://private.iam.cloud.ibm.com/identity/keys
```
{: codeblock}

#### System property `org.apache.kafka.sasl.oauthbearer.allowed.urls`
{: #system_property}

Starting in Kafka 4.0, the client requires a system property to set the allowed URLs of SASL OAUTHBEARER token and JWKS endpoints.

For more information, see [System properties](https://kafka.apache.org/42/configuration/system-properties/){: external}.

When you run CLI scripts included in the Apache Kafka distribution, set the system property by using the `KAFKA_OPTS` environment variable:

```sh
export KAFKA_OPTS="-Dorg.apache.kafka.sasl.oauthbearer.allowed.urls=https://private.iam.cloud.ibm.com/identity/keys,https://private.iam.cloud.ibm.com/identity/token,https://api.metadata.cloud.ibm.com/identity/v1/iam_tokens"
```
{: pre}

### Using SASL OAUTHBEARER with non-Java clients
{: #using_sasl_oauthbearer_non_Java}

For other Kafka client libraries, refer to their documentation for guidance on implementing OAUTHBEARER support. For example:

- [sarama](https://github.com/IBM/sarama){: external}: Implement the `AccessTokenProvider` interface.
- [librdkafka](https://github.com/confluentinc/librdkafka){: external}: Implement the `oauthbearer_token_refresh_cb` callback.

For information about generating an IAM token, see [Generating an IBM Cloud IAM token by using an API key](/docs/iam?topic=iam-iamtoken_from_apikey).
