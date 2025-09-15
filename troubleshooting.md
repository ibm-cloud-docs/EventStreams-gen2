---

copyright:
  years: 2023, 2024
lastupdated: "2024-03-01"

keywords: troubleshooting, question, problem

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting {{site.data.keyword.messagehub}}
{: #troubleshooting}

Use the troubleshooting tips to learn how to troubleshoot issues with {{site.data.keyword.messagehub_full}}.
{: shortdesc}

## Network related issues
{: #network_related}

If you have an Apache Kafka application that is unable to connect to a topic or unable to consume from or produce to a topic due to network issues, it could be manifested in following different ways.

- Unable to resolve host. Use nslookup or dig to check if the broker address could be resolved. If nslookup or dig fails, check your DNS server. Normally, the public endpoint resolves to three public IPs, as in the following examples.

    ```
    $ nslookup broker-0-93g2bbjd0pz8ghkj.kafka.svc08.us-south.eventstreams.cloud.ibm.com
    Server:		172.31.255.241
    Address:	172.31.255.241#53
    Non-authoritative answer:
    broker-0-93g2bbjd0pz8ghkj.kafka.svc08.us-south.eventstreams.cloud.ibm.com	canonical name = kafka.svc08.us-south.eventstreams.cloud.ibm.com.
    Name:	kafka.svc08.us-south.eventstreams.cloud.ibm.com
    Address: 169.48.243.82
    Name:	kafka.svc08.us-south.eventstreams.cloud.ibm.com
    Address: 169.61.217.150
    Name:	kafka.svc08.us-south.eventstreams.cloud.ibm.com
    Address: 52.117.238.34
    ```

    Private endpoint resolves to three private IPs that start with 166.9.
    
    ```
    $ nslookup kafka-0-mh-int-pipe-mzr-cse-kjh.private.us-south.messagehub.test.appdomain.cloud
    Server:		172.31.255.241
    Address:	172.31.255.241#53
    ```

-  Connection timeout. Use netcat (nc) to check the connectivity to the broker.

  ```
  $ nc -zv broker-0-93g2bbjd0pz8ghkj.kafka.svc08.us-south.eventstreams.cloud.ibm.com 9093
  Connection to broker-0-93g2bbjd0pz8ghkj.kafka.svc08.us-south.eventstreams.cloud.ibm.com port 9093 [tcp/*] succeeded!
  ```
  
  An nc command timeout means that the network where your client application is running could not connect to the broker addresses. It could be caused by a  number of reasons, such as firewall rules, calico network policies, and service mesh setup. Work with your network administrator to get netcat working. If the service instance is on the Enterprise plan and the cluster has a private-only service endpoint, check if your client application’s IPs were added to the IP allowlist or CBR rules. For more information, see [Restricting network access](/docs/EventStreams?topic=EventStreams-restrict_access).
  
- Connection reset or client disconnected. This may not necessarily be a network issue, rather than an application level issue. 

   During the maintenance window, brokers and proxies are restarted in a rolling manner and the Kafka client may see disconnect exception. With the correct configuration, the Kafka client reconnects to a different broker immediately, so that availability is not affected. If your application experiences a disconnect for an extended period of time, share your Kafka client library version, client configuration, and logs with debug enabled, and we will review and suggest appropriate values to configure the client to be resilient to broker restart. For supported Kafka client libraries, refer to the [list of recommended clients](/docs/EventStreams?topic=EventStreams-kafka_java_using). 

   If the issue still persists, raise a [support ticket](/docs/EventStreams?topic=EventStreams-report_problem_enterprise) for further investigation.

## HTTP error codes for REST APIs and how to fix them
{: #http_error_codes}

HTTP error codes apply to REST APIs, including [Admin REST](/apidocs/event-streams/adminrest), [REST Producer](/apidocs/event-streams/restproducer), and [Schema Registry](/apidocs/event-streams/restproducer_v2).

| HTTP error code  | Admin REST  | REST Producer  | Schema Registry  |
| --- | --- | --- |--- |
| 400 | Bad request - not a valid request. Correct your request. | Bad request - not a valid request. This error might be caused by a malformed payload, or unsupported key type or exceeded key/message size. | Bad request - not a valid request. Correct your request. |
| 403 | Not authorized to perform the operation. The API key used is missing a certain role. The ID that was used to create the resource key does not have an IAM policy. For more information, see [Managing authentication to your Event Streams instances](/docs/EventStreams?topic=EventStreams-security). | Forbidden. The token that you have provided does not have sufficient permissions to produce to a topic. Check if you have provided the token with the required access role. For more information, see [Managing authentication to your Event Streams instances](/docs/EventStreams?topic=EventStreams-security). | Forbidden. The client is not authorized to perform this request. The service ID is not authorized to access a schema resource. For more information, see [Managing authentication to your Event Streams instances](/docs/EventStreams?topic=EventStreams-security). |
| 404 | Not found. Unable to find the topic with the topic name you specified. {{site.data.keyword.messagehub}} set `auto.create.topics.enable` to `false`, thus the topic must be explicitly created before using it. | Not found. The topic does not exist. {{site.data.keyword.messagehub}} set `auto.create.topics.enable` to `false`, thus topic must be explicitly created before producing message to it. | Not found. Either the registry does not contain a schema with the specified schema ID, or the schema identified by the schema ID does not contain a version corresponding to the specified version number, or the schema is not configured with the specified type of rule. |
| 415 |  | Unsupported media type. |  |
| 422 | Semantically invalid request. You have a malformed request. If you receive this error when you try to create a new topic, it might be due to the maximum number of allowed partitions for your plan. For more information, see [How Event Streams uses limits and quotas for verifying the limits](/docs/EventStreams?topic=EventStreams-kafka_quotas). |  |  |
| 503 | Service unavailable. The request failed due to {{site.data.keyword.messagehub}} brokers being unavailable. An error occurred while handling the request. The service is unavailable. If you receive this error when creating a new topic fails, verify the [limits and quotas](/docs/EventStreams?topic=EventStreams-kafka_quotas). |Service unavailable. The request failed due to {{site.data.keyword.messagehub}} brokers being unavailable.  |  |
{: caption="HTTP error codes for REST APIs and how to fix them" caption-side="bottom"}

The Kafka API is a TCP-based API. For more information, refer to its [protocol guide](https://kafka.apache.org/protocol.html#protocol_error_codes) regarding the error codes.
  
## Unable to create a Kafka topic
{: #unable_create_kafka_topic}

If you are not able to create topics, it might be due to the following reasons: 

- You do not have the [Administration role on the cluster](/docs/EventStreams?topic=EventStreams-security#security_resources).
- There is insufficient disk space. Check how the [disk space is reserved according to the topic configuration](/docs/EventStreams?topic=EventStreams-ES_understanding_reserved_disk_usage). To solve this issue, delete unused topics and update topic's config `retention.ms` and `retention.bytes` to be smaller. For the Enterprise instance, scale the clusters. For more information see [Scaling Enterprise plan capacity](/docs/EventStreams?topic=EventStreams-ES_scaling_capacity).

## Error "Failed to send message"
{: #error_failed_send_message}

An Event Streams target returns an error, and you are unable to send events to it.  When you view the logs, the error is "Failed to send message", and the entry contains information such as the following:

> "Failed to send message","error":"kafka server: Request was for a topic or partition that does not exist on this broker","reason":"invalid_or_unreachable_topic"

The error can occur if the topic has not been created or has been deleted, or if you are sending data to a broker that is not the current leader for the topic partition. Leadership of topic partitions can change over time, because if a broker if offline for any reason, leadership of its partitions gets moved to other brokers until it recovers.

To resolve the problem, verify that the topic exists. With the correct configuration, the Kafka client refreshes the metadata or reconnects to a different broker quickly, so that this type of error situation often recovers on its own. If the issue is persistent, [contact Cloud Support](/docs/EventStreams?topic=EventStreams-gettinghelp) for problem investigation.
