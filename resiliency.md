---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: resiliency, cluster resiliency, availability, data corruption, deletion, data management, disaster recovery, responsibilities

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Maximizing {{site.data.keyword.messagehub}} resiliency on {{site.data.keyword.cloud_notm}}
{: #resiliency}

[Gen 2]{: tag-purple}

Learn how to maximize resiliency for your {{site.data.keyword.messagehub}} instance on {{site.data.keyword.cloud}}, including cluster resiliency, application configuration, and monitoring.
{: shortdesc}

By default, {{site.data.keyword.messagehub}} instances include built-in resiliency features to prevent service disruption and data loss. All instances replicate data across three brokers with automated failover. {{site.data.keyword.messagehub}} manages resource utilization and instance limits while proactively monitoring in-sync replicas and disk capacity. The service maintains an SLA of up to 99.99% for multizone regions (MZRs). For more information, see [Understanding your responsibilities when you use {{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-event_streams_responsibilities).

For disaster recovery, you can provision a secondary cluster in an alternate region and restore configuration and data, or replicate data between instances by using tools such as MirrorMaker. You are responsible for maintaining and running disaster recovery procedures.

## Cluster resiliency
{: #cluster_resiliency}

### Instance deletion recovery
{: #instance_deletion_recovery}

{{site.data.keyword.messagehub}} retains deleted instances during a three-day reclamation period, after which the service permanently deletes the data. You can view reclamation status, reclaim, or cancel a pending reclamation by using the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations).

### Client application configuration
{: #resiliency_clientconfig}

Configuring optimal client parameters improves application resiliency and resource efficiency. Balance these settings against your specific workload requirements. For more information, see the [Apache Kafka documentation](https://kafka.apache.org/){: external}.

`fetch.min.bytes=1` and `fetch.max.wait.ms=500`
:   Empty fetch requests consume server CPU without transferring data. The default value of 500 ms for `fetch.max.wait.ms` balances latency and throughput. If consumers experience high CPU usage, increase this value or reduce the number of active consumer instances.

`enable.idempotence=true` and `retries=2147483647`
:   Idempotent producers retry transient failures without producing duplicate records, ensuring exactly-once delivery semantics and higher data durability.

`acks=all`
:   Ensures the broker acknowledges writes only after all in-sync replicas record the message, providing maximum fault tolerance.

### Monitoring and proactive alerting
{: #resiliency_monitoring}

Implement monitoring to identify anomalies early:

Client observability
:   Verify that client applications remain connected and process messages. Instrument producers to monitor unsent record queue depth and request latency.

Consumer lag
:   Monitor consumer group lag. If consumers fall behind and message retention limits are reached, the service deletes unread messages according to the retention policy.

Disk usage
:   Monitor storage utilization. When disk usage reaches 100%, the service blocks topic partition creation and message writes, and only read operations continue. Scale storage capacity or truncate expired records before disk capacity is exhausted.

CPU utilization
:   Maintain normal operational CPU utilization under two-thirds (66%) capacity. Operating near maximum CPU utilization increases latency and risks request timeouts during availability zone failovers.

Throughput management
:   Keep throughput under two-thirds of allocated bandwidth to preserve capacity for maintenance events and failover traffic. For more information, see [Scaling Enterprise plan capacity](/docs/EventStreams-gen2?topic=EventStreams-gen2-ES_scaling_capacity).

### Automation with Terraform
{: #resiliency_terraform}

Use Terraform to manage instances predictably following infrastructure as code (IaC) principles. Store Terraform configuration files in a version-controlled repository as part of your disaster recovery plan.

## Topic and data resiliency
{: #instance_data-resiliency}

To protect individual topics and application data streams:

### Updating client libraries
{: #resiliency_clientlib}

Use the latest supported Kafka client libraries to ensure that your applications include the latest security patches, bug fixes, and protocol improvements.

### Message retention planning
{: #resiliency_retention}

Configure topic retention periods to accommodate extended application downtime. For example, if a consumer application stops over a three-day holiday weekend, configure retention for at least five to seven days so consumers can catch up without losing messages.
