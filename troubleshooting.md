---

copyright:
  years: 2026
lastupdated: "2026-11-10"

keywords: troubleshooting, question, problem

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshoot {{site.data.keyword.messagehub}} issues on {{site.data.keyword.cloud_notm}}
{: #troubleshooting}

[Gen 2]{: tag-purple}

Learn how to resolve common issues with {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}}, including network-related problems.
{: shortdesc}

## Network related issues
{: #network_related}

If an Apache Kafka client cannot connect or produce and consume records, review the following troubleshooting steps:

- **Unable to resolve host**: Use `nslookup` or `dig` to check whether the broker DNS hostname resolves. If hostname resolution fails, verify your VPC DNS resolver configuration. A private endpoint resolves to allocated IP addresses across your VPC subnets.

    ```sh
    nslookup kafka-0-mh-int-pipe-mzr-cse-kjh.private.us-south.messagehub.test.appdomain.cloud
    ```
    {: pre}

- **Connection timeout**: Use `nc` (netcat) to test TCP connectivity to port 9093 on the broker hostname:

    ```sh
    nc -zv kafka-0-mh-int-pipe-mzr-cse-kjh.private.us-south.messagehub.test.appdomain.cloud 9093
    ```
    {: pre}

    A timeout indicates that network firewalls, security groups, or routing policies are blocking traffic on port 9093. Ensure your VPC security groups and subnet network ACLs permit outbound and inbound TCP traffic on port 9093.

- **Connection reset or client disconnect**: During scheduled maintenance windows, brokers undergo rolling restarts. A properly configured Kafka client automatically reconnects to an alternate broker without interrupting message processing. If your client disconnects for an extended period, verify that your client retry and timeout settings follow recommended guidelines. For more information, see [Using Kafka Java client](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_java_using).

If issues persist, open a [support case](/docs/EventStreams-gen2?topic=EventStreams-gen2-report_problem_enterprise).

## HTTP error codes for REST APIs and how to fix them
{: #http_error_codes}

HTTP error codes apply to REST APIs, including [Admin REST](/docs/apis/event-streams/adminrest), [REST Producer](/docs/apis/event-streams/restproducer), and [Schema Registry](/docs/apis/event-streams/restproducer_v2).

| HTTP error code | Admin REST | REST Producer | Schema Registry |
| --- | --- | --- | --- |
| 400 | Bad request. The request payload or syntax is invalid. | Bad request. The message payload is malformed or exceeds maximum message size (1 MB). | Bad request. The schema definition payload is invalid. |
| 403 | Forbidden. The IAM token lacks the required permissions. For more information, see [Managing access](/docs/EventStreams-gen2?topic=EventStreams-gen2-security). | Forbidden. The IAM identity lacks Writer permissions on the topic. | Forbidden. The IAM identity lacks access to the schema resource. |
| 404 | Not found. The requested topic does not exist. Automatic topic creation is disabled. | Not found. The target topic does not exist. | Not found. The specified schema ID or version does not exist. |
| 415 | Not applicable. | Unsupported media type. Ensure `Content-Type` is set to `application/json`. | Unsupported media type. |
| 422 | Unprocessable entity. The request exceeds partition or configuration limits. For more information, see [Limits and quotas](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_quotas). | Not applicable. | Unprocessable entity. Schema validation failed. |
| 503 | Service unavailable. Temporary broker unavailability. Retry with backoff. | Service unavailable. Temporary broker unavailability. Retry with backoff. | Service unavailable. |
{: caption="HTTP error codes for REST APIs" caption-side="bottom"}

The Kafka native client API uses TCP protocol error codes. For more information, see the [Apache Kafka protocol guide](https://kafka.apache.org/protocol.html){: external}.

## Unable to create a Kafka topic
{: #unable_create_kafka_topic}

If you are not able to create topics, it might be due to the following reasons:

- Your identity lacks Manager or Administrator permissions on the cluster resource. For more information, see [Managing access](/docs/EventStreams-gen2?topic=EventStreams-gen2-security).
- Storage capacity is exhausted. To resolve this issue, delete unused topics or decrease `retention.ms` and `retention.bytes`. For Enterprise instances, scale storage capacity as described in [Scaling Enterprise plan capacity](/docs/EventStreams-gen2?topic=EventStreams-gen2-ES_scaling_capacity).

## Error: Failed to send message
{: #error_failed_send_message}

When producer applications cannot send records to {{site.data.keyword.messagehub}}, logs can report:

```text
"Failed to send message", "error": "kafka server: Request was for a topic or partition that does not exist on this broker", "reason": "invalid_or_unreachable_topic"
```
{: codeblock}

This error occurs when the destination topic does not exist, or when the client produces to a broker that is not the partition leader. If a broker goes offline, partition leadership transitions to another broker in the replica set.

To resolve the problem, verify that the target topic exists. Ensure your Kafka producer client refreshes metadata (`metadata.max.age.ms`) and reconnects to the new partition leader. If the issue persists, open an [IBM Cloud support case](/docs/EventStreams-gen2?topic=EventStreams-gen2-gettinghelp).
