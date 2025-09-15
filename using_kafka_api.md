---

copyright:
  years: 2015, 2024
lastupdated: "2024-05-13"

keywords: api, consumer, producer, admin, streams, connect, client

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Using the Kafka API
{: #kafka_using}

Kafka provides a rich set of APIs and clients across a broad range of languages. APIs include core API, Streams API, and Connect API.
{: shortdesc}

- **Kafka's core API (Consumer, Producer, and Admin API)**  
    Use to send and receive messages directly from one or more Kafka topics.
    The Kafka Admin client provides a simple interface through the Kafka API for managing Kafka resources. You can create, delete, and manage topics. You can also use the Admin client to manage consumer groups and configurations.
- **Streams API**  
    A higher-level stream processing API to easily consume, transform, and produce events between topics.
- **Connect API**  
    A framework that allows reusable or standard integrations to stream events into and out of external systems, such as databases.

The following table summarizes what you can use with {{site.data.keyword.messagehub}}:

|                                              | Enterprise plan                                                                                                                                  | Standard plan                                                                                                                                    | Lite plan                                                                                                                                        |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Kafka version on cluster**                 | Kafka 3.8                                                                                                                                        | Kafka 3.8                                                                                                                                        | Kafka 3.8                                                                                                                                        |
| **Minimum recommended Kafka client version** | Kafka 2.6.0, or later                                                                                                                            | Kafka 2.6.0, or later                                                                                                                            | Kafka 2.6.0, or later                                                                                                                            |
| **Supported client versions**                | See [Support summary for all recommended clients](/docs/EventStreams?topic=EventStreams-kafka_using#client_summary)                              |                                                                                                                                                  |                                                                                                                                                  |
| **Kafka Connect supported**                  | Yes                                                                                                                                              | Yes                                                                                                                                              | No                                                                                                                                               |
| **Kafka Streams supported**                  | Yes                                                                                                                                              | Yes                                                                                                                                              | No                                                                                                                                               |
| **ksqlDB supported**                         | Yes                                                                                                                                              | No                                                                                                                                               | No                                                                                                                                               |
| **Authentication requirements**              | Client must support authentication by using the SASL Plain mechanism and use the Server Name Indication (SNI) extension to the TLSv1.2 protocol. | Client must support authentication by using the SASL Plain mechanism and use the Server Name Indication (SNI) extension to the TLSv1.2 protocol. | Client must support authentication by using the SASL Plain mechanism and use the Server Name Indication (SNI) extension to the TLSv1.2 protocol. |
{: caption="Kafka client support in Standard, Enterprise, and Lite plans." caption-side="bottom"}

## Choosing a Kafka client to use with {{site.data.keyword.messagehub}}
{: #kafka_clients}

The official client for the Kafka API is written in Java, and as such contains the latest features and bug fixes. For more information about this API, see [Kafka Producer API 3.8](https://kafka.apache.org/38/javadoc/org/apache/kafka/clients/producer/KafkaProducer.html){: external} and [Kafka Consumer API 3.8](https://kafka.apache.org/38/javadoc/org/apache/kafka/clients/consumer/KafkaConsumer.html){: external}.

For other languages, run one of the following clients, all of which are tested with {{site.data.keyword.messagehub}}.

### Support summary for all recommended clients
{: #client_summary}

| Client                                                                                   | Language | Recommended version | Minimum version supported [^tabletext1] | Link to sample                                                                                                                                                                                     |
| ---------------------------------------------------------------------------------------- | -------- | ------------------- | --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Official Apache Kafka client:**                                                        |          |                     |                                         |                                                                                                                                                                                                    |
| [Apache Kafka client](http://kafka.apache.org/downloads)                                 | Java     | 3.8.1, or later     | 2.5.0                                   | [Java console sample](/docs/EventStreams?topic=EventStreams-kafka_java_using) \n \n [Liberty sample](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample) |
| **Third-party clients:**                                                                 |          |                     |                                         |                                                                                                                                                                                                    |
| [confluent-kafka-javascript](https://github.com/confluentinc/confluent-kafka-javascript) | Node.js  | Latest              | 1.0.0                                   |                                                                                                                                                                                                    |
| [confluent-kafka-python](https://github.com/confluentinc/confluent-kafka-python)         | Python   | Latest              | 1.4.0                                   | [Kafka Python sample](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-python-console-sample)                                                                              |
| [confluent-kafka-go](https://github.com/confluentinc/confluent-kafka-go)                 | Go       | Latest              | 1.4.0                                   |                                                                                                                                                                                                    |
| [librdkafka](https://github.com/confluentinc/librdkafka)                                 | C or C++ | Latest              | 1.4.0                                   |                                                                                                                                                                                                    |
| [node-rdkafka](https://github.com/Blizzard/node-rdkafka)                                 | Node.js  | Latest              | 2.8.0                                   | [Node.js sample](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-nodejs-console-sample)                                                                                   |
| [sarama](https://github.com/IBM/sarama)                                                  | Go       | Latest              | 1.40.0                                  | [Sarama examples](https://github.com/IBM/sarama/tree/v1.45.0/examples)                                                                                                                             |
{: caption="Client support summary" caption-side="bottom"}

[^tabletext1]: The earliest version that was validated in continual testing. Typically, it is the initial version available within the last 12 months, or newer if significant issues are known to exist. If you can't run any of the clients that are listed, you can use other third-party clients that meet the following minimum requirements (for example, [librdkafka](https://github.com/confluentinc/librdkafka){: external}). 1. Supports Kafka 1.40, or later. 2. Can connect and authenticate by using SASL PLAIN with TLSv1.2. 3. Supports the SNI extensions for TLS where the server's hostname is includes in the TLS handshake. 4. Supports elliptic curve cryptography. In all cases, use the latest version of the client.

### Connecting your client to {{site.data.keyword.messagehub}}
{: #connect_client}

For information about how to configure your Java client to connect to {{site.data.keyword.messagehub}}, see [Configuring your client](/docs/EventStreams?topic=EventStreams-kafka_using#kafka_api_client).

## Configuring your Kafka API client
{: #kafka_api_client}

To establish a connection, clients must be configured to use SASL PLAIN or SASL OAUTHBEARER over TLSv1.2 at a minimum and to require a username, and a list of the bootstrap servers. TLSv1.2 ensures that connections are encrypted and validates the authenticity of the brokers (to prevent man-in-the-middle attacks). SASL enforces authentication on all connections.

To retrieve the username, password, and list of bootstrap servers, a service credentials object, or service key is required for the service instance. For more information about creating these objects, see [Connecting to {{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-connecting).

### Using SASL PLAIN
{: #using_sasl_plain}

Use the following strings and properties.

- Use the `bootstrap_endpoints` string as the list of bootstrap servers and pass this string of host and port pairs to your Kafka client.
- Use the `user` and `api_key` properties as the username and password.

For a Java client, the following example shows the minimum set of properties, where `${USERNAME}`, `${PASSWORD}`, and `${BOOTSTRAP_ENDPOINTS}` are to be replaced by the values that you retrieved previously.

```config
bootstrap.servers=${BOOTSTRAP_ENDPOINTS}
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="${USERNAME}" password="${PASSWORD}";
security.protocol=SASL_SSL
ssl.protocol=TLSv1.2
ssl.enabled.protocols=TLSv1.2
ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

If you use a Kafka client earlier than version 0.10.2.1, the `sasl.jaas.config` property isn't supported, and you must instead provide the client configuration in a JAAS configuration file.
{: note}

### Using SASL OAUTHBEARER
{: #using_sasl_oauthbearer}

Before configuring the SASL mechanism for Java client, there are two prerequisites.

- The minimum supported Kafka Java client version is 3.1.0.
- Additional jar package needs to be downloaded from Maven Central and made available in the classpath.

If Maven is used in build system, add the following information to the file `pom.xml` in the dependencies section.

```xml
<dependency>
    <groupId>com.ibm.cloud.eventstreams</groupId>
    <artifactId>oauth-client</artifactId>
    <version>1.4.0</version>
</dependency>
```

If Gradle is used in build system, add the following information to the file `build.gradle` in the dependencies section.

```gradle
implementation com.ibm.cloud.eventstreams:oauth-client:1.4.0
```

{{site.data.keyword.iamlong}} Identity Service supports multiple ways to generate bearer token, two of which are supported by this oauth client library.

- API key.
- Trusted profile and compute resource token.

#### Using SASL OAUTHBEARER with API key
 Use the following strings and properties.

- Use the `BOOTSTRAP_ENDPOINTS` string as the list of bootstrap servers and pass this string of host and port pairs to your Kafka client.
- The `IAMOAuthBearerLoginCallbackHandler` is provided by the jar package `com.ibm.cloud.eventstreams:oauth-client:+`.
- The {{site.data.keyword.iamlong}}'s token endpoint `https://iam.cloud.ibm.com/identity/token` is configured to generate token from the API key by using specified grant type in jaas config. It is done on client side, thus the API key is never sent to the server side and provides better security than a long-lived API key.
- The {{site.data.keyword.iamshort}}'s key endpoint `https://iam.cloud.ibm.com/identity/keys` is configured to validate the token.
- `grant_type` in `sasl.jaas.config` is `urn:ibm:params:oauth:grant-type:apikey`
- `apikey` in `sasl.jaas.config` is the API key used to generate bearer token at client side. It can be either from a user or service Id.

For a Java client, the following example shows the minimum set of properties, where `${BOOTSTRAP_ENDPOINTS}`, and `${APIKEY}` are to be replaced by the values that you retrieved previously.

```config
bootstrap.servers=${BOOTSTRAP_ENDPOINTS}
security.protocol=SASL_SSL
sasl.mechanism=OAUTHBEARER
sasl.oauthbearer.token.endpoint.url=https://iam.cloud.ibm.com/identity/token
sasl.oauthbearer.jwks.endpoint.url=https://iam.cloud.ibm.com/identity/keys
sasl.login.callback.handler.class=com.ibm.eventstreams.oauth.client.IAMOAuthBearerLoginCallbackHandler
sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required grant_type="urn:ibm:params:oauth:grant-type:apikey" apikey="${APIKEY}";
```

#### Using SASL OAUTHBEARER with trusted profile and compute resource token

All the properties are the same with API key except the `sasl.jaas.config` is different.

- `grant_type` in `sasl.jaas.config` is `urn:ibm:params:oauth:grant-type:cr-token`.
- `profile_id` in `sasl.jaas.config` is a file location storing trusted profile ID. This file can be mounted to a Kubernetes pod running Kafka client code as a read-only volume and made available to the Kafka client code.
- `cr_token` in `sasl.jaas.config` is a file location storing service account token from a Kubernetes pod running Kafka client code. See [What is a service account token](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#launch-a-pod-using-service-account-token-projection){: external}.

See below example
```config
bootstrap.servers=${BOOTSTRAP_ENDPOINTS}
security.protocol=SASL_SSL
sasl.mechanism=OAUTHBEARER
sasl.oauthbearer.token.endpoint.url=https://iam.cloud.ibm.com/identity/token
sasl.oauthbearer.jwks.endpoint.url=https://iam.cloud.ibm.com/identity/keys
sasl.login.callback.handler.class=com.ibm.eventstreams.oauth.client.IAMOAuthBearerLoginCallbackHandler
sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required grant_type="urn:ibm:params:oauth:grant-type:cr-token" profile_id="${TRUSTED_PROFILE_ID_FILE_PATH}" cr_token="${SERVICE_ACCOUNT_TOKEN_FILE_PATH}";
```

More details on [How to setup trusted profile](https://cloud.ibm.com/docs/account?group=administering-trusted-profiles)


The source code of oauth client refer to the [{{site.data.keyword.messagehub}} Java SDK](https://github.com/IBM/eventstreams-java-sdk/tree/main/modules/oauth-client){: external}.


The sample client code refer to [{{site.data.keyword.messagehub}} Sample](https://github.com/IBM/eventstreams-samples){: external}.


For other Kafka client libaries, refer to their documentation about how to implement OAUTHBEARER support. For example:.

- [sarama](https://github.com/IBM/sarama): an implementation of `AccessTokenProvider` interface is required.
- [librdkafka](https://github.com/confluentinc/librdkafka): an implementation of `oauthbearer_token_refresh_cb` callback is required.

For information about how to generate an {{site.data.keyword.Bluemix_notm}} IAM token by using an API key, see {{site.data.keyword.iamlong}}'s [document](https://cloud.ibm.com/docs/account?topic=account-iamtoken_from_apikey).
