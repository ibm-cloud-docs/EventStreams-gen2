---

copyright:
  years: 2025
lastupdated: "2025-12-15"

keywords: resiliency, cluster resiliency, availability, data corruption, deletion, data management, disaster recovery, responsibilities

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Maximizing resiliency for your {{site.data.keyword.messagehub}} instance
{: #resiliency}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

By default, {{site.data.keyword.messagehub}} instances are built with resiliency, with numerous safeguards to prevent loss of availability or data. All instances are replicated across 3 brokers in addition to automated failover for maximum availability. In addition, {{site.data.keyword.messagehub}} is responsible for managing resource utilisation and instance limits across the estate as well as proactively monitoring (for example, of in-sync replicas and disk space utilization) to ensure high availability and data durability out of the box. {{site.data.keyword.messagehub}} proactively manages the security and compliance of customer clusters through threat detection and vulnerability scanning and guarantees an SLA of up to 99.99% for MZRs. [Understanding your responsibilities when you use {{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-event_streams_responsibilities) highlights customer responsibilities which include backup of message payload data and instance data including topic names, data and schema registry data if this is required.

If a disaster recovery plan including {{site.data.keyword.messagehub}} on {{site.data.keyword.cloud}} is required, this plan can include provisioning a new cluster in a new region if a disaster occurs and restoring any configuration or data to that cluster, or, pre-provisioning a cluster in another region and using an external tool, such as MirrorMaker, to mirror data between the two instances. You are responsible for maintaining and executing this disaster recovery plan if the service is lost. This page outlines additional steps users can take to maximize the resiliency of their {{site.data.keyword.messagehub}} instance at both the cluster and cluster data level.

## Cluster resiliency
{: #cluster_resiliency}

### 1. Instance deletion recovery
{: #instance_deletion_recovery}

All {{site.data.keyword.messagehub}} plans can recover a deleted instance within reclamation period of three days, after which the data is irreversibly destroyed.  You can check the status of a reclamation, and force or cancel a scheduled reclamation by using  the [IBM Cloud CLI](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_reclamations).

### 2. Application/client configuration
{: #resiliency_clientconfig}

Applying these parameters to your applications can significantly improve resiliency and optimize resource usage. These settings are a guidance and ultimately should be traded off against your application needs. The [open source Apache Kafka documentation](https://kafka.apache.org/documentation/) provides a more detailed explanation into these parameters and their importance.

| Setting | Explanation |
|----------|-----------------------|
|‘fetch.min.bytes=1 fetch.max.wait.ms=500’  |  Empty fetch requests are wasteful because they consume server resources without actually retrieving meaningful data. The default value of 500ms is recommended for fetch.max.wait.ms however, if your application is experiencing high CPU , then consider running less clients, or increasing this.|
|‘enable.idempotence=true retries=Integer.MAX_VALUE’   |  Idempotent producers enable retries and prevent duplicate messages, ensuring exactly-once semantics during data production and improving data durability.  |
|‘acks=all’    |  This guarantees that messages are delivered and acknowledged across all replicas for maximum fault tolerance.    |

### 3. Monitoring and alerting for early anomaly detection
{: #resiliency_monitoring}

Client observability: It is important to validate that your clients are active and not network isolated. In some cases, customers pump a lot of data into the clients but clients don’t receive the data therefore it is advisable to instrument your clients, check whether the data reaches the server or not and establish a baseline, for example using throughput and number of unsent messages queued up. The ways you can instrument your clients will vary depending on which client you use.

Consumer lag: If consumers fall significantly behind and the retention period is approaching its limit, consumers risk missing data because log segments may be deleted in line with the retention policy.

Disk usage: It is important to monitor disk usage in your {{site.data.keyword.messagehub}} instance. When Disk usage reaches the maximum level, you won’t be able to create anymore topic partitions and only reading (not write) of data will be possible. This risk’s a loss of application availability if it cannot tolerate this.  You must either scale your instance or truncate messages when you get to the maximum disk usage.

CPU utilization: {{site.data.keyword.messagehub}} advise operating at a maximum of 2/3 CPU utilisation to protect against the loss of an availability zone. An {{site.data.keyword.messagehub}} cluster which is working close or near its recommended max CPU usage is at risk of inefficient processing and may lead to some loss of data.

Throughput/Capacity management: Similar to CPU Utilization, {{site.data.keyword.messagehub}} recommend not to exceed the 2/3rds of the maximum throughput, even when working at Peak capacity. This is buffer is crucial to safeguard the instance during maintenance periods or in the event of an availability zone failure. See more in [Scaling enterprise plan capacity](/docs/EventStreams-gen2?topic=EventStreams-gen2-ES_scaling_capacity).

### 4. Automation via Terraform
{: #resiliency_terraform}

{{site.data.keyword.messagehub}} recommend using Terraform which enables you to rapidly and predictably create and change your instance following Infrastructure as Code (IaC) principles. You must store all terraform configurations in a safe and easily accessibly repository as part of a comprehensive disaster recovery plan.

## Topics and instance data resiliency
{: #instance_data-resiliency}

The recommendations for maximizing cluster resiliency will help with maximizing resiliency of cluster data but in some cases, a single topic, set of topics or a single application can be affected and the following methods can help in prevention and/or recovery of cluster data.

### 1. Keeping client libraries up to date for vulnerabilities and fixes
{: #resiliency_clientlib}

Using the latest client libraries ensures you are up to date with latest testing and you are not missing out on fixes which may harm your clients.  The latest client library will provide the best usability and better error messages, maximizing the health and reliability of your client and client data.

### 2. Application configuration
{: #resiliency_retention}

To maximize topic resiliency, the retention period should be configured for potential downtime to ensure consumers can catch up without missing data. We advise configuring the retention period to exceed the maximum expected downtime for consumers. For example, if a retention period is less than 3 days and applications experience an extended shut own e.g over a long weekend, there is significant risk of missing data upon restart.
