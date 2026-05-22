---

copyright:
  years: 2026
lastupdated: "2026-05-22"

keywords: monitoring, metrics, cost, billing, opting in

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring {{site.data.keyword.messagehub}} service metrics by using {{site.data.keyword.mon_full_notm}}
{: #metrics}

[Gen 2]{: tag-purple}

[{{site.data.keyword.mon_full}}](/docs/monitoring?topic=monitoring-getting-started) is a third-party cloud-native, and container-intelligence management system that you can include as part of your {{site.data.keyword.cloud_notm}} architecture. Use it to gain operational visibility into the performance and health of your applications, services, and platforms. It offers administrators, DevOps teams, and developers full stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards.
{: shortdesc}

While you monitor service metrics with {{site.data.keyword.mon_full_notm}}, Kafka clients (producers and consumers) have their own set of [metrics](/docs/EventStreams-gen2?topic=EventStreams-gen2-getting-started-metrics) to monitor their performance and health.

## Opting in to and enabling {{site.data.keyword.messagehub}} service metrics
{: #opt_in_metrics}

{{site.data.keyword.messagehub}} service metrics can broadly be categorized into two different groups: **Default** and **Enhanced**.

### Enabling default {{site.data.keyword.messagehub}} service metrics
{: #enabling_default_metrics}

Before you can start to use {{site.data.keyword.messagehub}} {{site.data.keyword.mon_full_notm}} metrics, you must first opt in, and then enable platform metrics by completing the following steps:

1. Enable platform metrics for {{site.data.keyword.messagehub}}. For more information, see [Enabling platform metrics](/docs/monitoring?topic=monitoring-platform_metrics_enabling){: external}.

   The owner of the account has full access to this metrics data. For more information about managing access for other users, see [Getting started with {{site.data.keyword.mon_full_notm}} - manage user access](/docs/monitoring?topic=monitoring-getting-started#getting-started-step1){: external}.

2. To navigate from the {{site.data.keyword.messagehub}} instance page to the {{site.data.keyword.monitoringshort}} dashboard, click **Actions** on the instance page and select **Monitoring**.

   On your first usage, you might see a welcome wizard. To advance to the dashboard selection menu, select **Next** and then **Skip** on the **Choosing an installation method** page. Accept the prompts that follow. You can then select the **IBM {{site.data.keyword.messagehub}}** or **IBM {{site.data.keyword.messagehub}} (Enterprise)** dashboard, depending on the plan that you use.

### Enabling enhanced {{site.data.keyword.messagehub}} metrics
{: #opt_in_enhanced_metrics}

The enhanced {{site.data.keyword.messagehub}} metrics consist of three groups; `topic`, `partition` and `consumers`. You can opt in to either one, two, or all. The metrics available are described in the [topic](#metrics-topic), [partition](#metrics-partition) and [consumers](#metrics-consumers) tables.

Enabling enhanced metrics introduces more global gauge metrics and therefore increases the costs.

Before you can start to use enhanced {{site.data.keyword.messagehub}} metrics, you must first enable them by completing the following step:

* Run the following command to update the service instance to start using enhanced metrics:

   ```bash
   ibmcloud resource service-instance-update <instance-name> -p '{"dataservices": {"options":{"metrics":["topic","partition","consumers"]}}}'
   ```
   {: codeblock}

When enhanced metrics are enabled, depending on the selection, the following new dashboards are available; **IBM {{site.data.keyword.messagehub}}(Topic)**, **IBM {{site.data.keyword.messagehub}}(Partitions)** and **IBM {{site.data.keyword.messagehub}}(Consumers)**.

To opt out of enhanced metrics, run the following command:

   ```bash
   ibmcloud resource service-instance-update <instance-name> -p '{"dataservices": {"options":{"metrics":[]}}}'
   ```
   {: codeblock}

Dashboards are available only after metrics started to be recorded; it might take a few minutes to initialize.
{: note}

## {{site.data.keyword.messagehub}} service metrics cost information
{: #metric_costs}

Before you opt in to using {{site.data.keyword.monitoringshort}} metrics, be aware of the cost of doing so. The estimated cost depends on the following considerations:

- The {{site.data.keyword.messagehub}} plan that you use.
- How many unique time series are sent for each plan.
- The number of topics that you created.
- The number of partitions that you created.
- Whether you have topics, partitions, consumers, or all enabled.

For more information, see [{{site.data.keyword.monitoringshort}} pricing](/docs/monitoring?topic=monitoring-pricing_plans).


## {{site.data.keyword.messagehub}} service metrics details
{: #metric_details}

The following tables describe the specific metrics that are provided by {{site.data.keyword.messagehub}} for each plan.

## Service metrics available by service plan
{: #metrics-by-plan}

| Metric name | Enterprise Gen2 |
|-----------|--------|
| [Authentication failures](#ibm_eventstreams_kafka_authentication_failure_total) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | | |
| [Consume message conversion time](#ibm_eventstreams_instance_consume_conversions_time_quantile) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Inactive consumer groups](#ibm_eventstreams_instance_inactive_consumergroups) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Instance bytes in per second](#ibm_eventstreams_instance_bytes_in_per_second) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Instance bytes out per second](#ibm_eventstreams_instance_bytes_out_per_second) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Instance utilization](#ibm_eventstreams_instance_utilization) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Number of partitions](#ibm_eventstreams_instance_partitions) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Produce message conversion time](#ibm_eventstreams_instance_produce_conversions_time_quantile) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [IAM ID bytes in per second](#ibm_eventstreams_iam_id_bytes_in_per_second) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | | |
| [IAM ID bytes out per second](#ibm_eventstreams_iam_id_bytes_out_per_second) |  ![Checkmark icon](../icons/checkmark-icon.svg) |   |   |
| [Rebalancing consumer groups](#ibm_eventstreams_instance_rebalancing_consumergroups) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Reserved disk space percentage](#ibm_eventstreams_instance_reserved_disk_space_percent) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Stable consumer groups](#ibm_eventstreams_instance_stable_consumergroups) |   ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Topic bytes in per second](#ibm_eventstreams_instance_topic_bytes_in_per_second) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Topic bytes out per second](#ibm_eventstreams_instance_topic_bytes_out_per_second) | ![Checkmark icon](../icons/checkmark-icon.svg) |![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Number of topics](#ibm_eventstreams_instance_topics) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Number of offline partitions](#ibm_eventstreams_kafka_offline_partitions) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Number of under in-sync replica partitions](#ibm_eventstreams_kafka_under_minisr_partitions) |   ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Used disk space percentage](#ibm_eventstreams_instance_utilised_disk_space_percent) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Service metrics available by service plan" caption-side="bottom"}

## Enhanced service metrics available with topic enabled
{: #metrics-topic}

| Metric name |Enterprise Gen2 |
|-----------|--------|
| [Topic size](#ibm_eventstreams_instance_topic_size) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Metrics available for topic" caption-side="bottom"}

## Enhanced service metrics available with partition enabled
{: #metrics-partition}

| Metric name |Enterprise Gen2 |
|-----------|--------|
| [Message rate per partition](#ibm_eventstreams_instance_message_rate_per_partition) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Metrics available for partition" caption-side="bottom"}

This information is useful for detecting if the distribution of message activity across the partitions in a topic is unbalanced and if the number of partitions a topic is scaled appropriately.

## Enhanced service metrics available with quotas enabled
{: #metrics-quotas}

| Metric name | Enterprise | Lite | Standard |
|-----------|--------|--------|--------|
| [IAM ID bytes in quota used percentage](#ibm_eventstreams_iam_id_bytes_in_quota_used_percentage) |  ![Checkmark icon](../icons/checkmark-icon.svg) | | |
| [IAM ID bytes out quota used percentage](#ibm_eventstreams_iam_id_bytes_out_quota_used_percentage) |  ![Checkmark icon](../icons/checkmark-icon.svg) | | |
{: caption="Metrics available for quotas" caption-side="bottom"}

Kafka quotas use sampling to determine how long clients should be paused before they can send or receive more data. For unpredictable workloads, or configurations that result in quota decisions being made using only a few samples, the percentage quota used metric could go above 100%.

### Authentication failures
{: #ibm_eventstreams_kafka_authentication_failure_total}

This metric tracks the total number of authentication failures.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_kafka_authentication_failure_total`|
| `Metric Type` | `counter` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Authentication failures metric metadata" caption-side="bottom"}

A value of 0 is expected under normal conditions. A nonzero value indicates that one or more clients are attempting to connect using invalid credentials. Verify that all clients are configured with correct authentication credentials and that no outdated or revoked credentials are in use.

### Consume message conversion time
{: #ibm_eventstreams_instance_consume_conversions_time_quantile}

Indicates that the accumulated time spent performing message conversion from clients that are consuming by using older protocol versions.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_consume_conversions_time_quantile`|
| `Metric Type` | `gauge` |
| `Value Type`  | `second` |
| `Segment By` | `Service instance, Quantile, Service instance name` |
{: caption="Consume message conversion time metric metadata" caption-side="bottom"}

Ideally zero, as nonzero indicates that clients are experiencing more latency because of using an older protocol level. Those clients are down-level and must be upgraded. Ensure that all clients are at the latest levels.

### IAM ID bytes in per second
{: #ibm_eventstreams_iam_id_bytes_in_per_second}

This metric tracks the rate of data, in bytes per second, that is sent to the service by each IAM ID.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_iam_id_bytes_in_per_second`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance name, IBM IAM Id, Service instance` |
{: caption="IAM ID bytes in per second metric metadata" caption-side="bottom"}

Use this metric to monitor usage patterns and identify trends in data ingestion across IAM IDs. Higher than expected values for a specific IAM ID can indicate disproportionate throughput usage. Compare activity across IAM IDs to understand traffic distribution and detect anomalies. This metric can also help inform decisions about rate limits or quotas if certain IAM IDs consistently consume more throughput than intended.

### IAM ID bytes in quota used percentage
{: #ibm_eventstreams_iam_id_bytes_in_quota_used_percentage}

The percentage of bytes in quota used per IAM ID.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_iam_id_bytes_in_quota_used_percentage`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance name, IBM IAM ID, Service instance` |
{: caption="IAM ID bytes in quota used percentage metric metadata" caption-side="bottom"}

This metric is for information to help you monitor trends in your usage, particularly if any IAM IDs are producing close to their quota limits.

Quota metrics might sometimes exceed 100%. Kafka quotas use sampling and are applied asynchronously. For some workloads, especially where data is sent in large batches, this might result in small deviations from the limit.

### IAM ID bytes out per second
{: #ibm_eventstreams_iam_id_bytes_out_per_second}

This metric tracks the rate of data, in bytes per second, that is sent from the service to each IAM ID.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_iam_id_bytes_out_per_second`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance name, IBM IAM Id, Service instance` |
{: caption="IAM ID bytes out per second metric metadata" caption-side="bottom"}

Use this metric to monitor usage patterns and track how data is distributed to IAM IDs. Higher than expected values for a specific IAM ID can indicate disproportionate data consumption. Compare activity across IAM IDs to understand traffic patterns and identify potential anomalies. This metric can also help inform decisions about rate limits or quotas if certain IAM IDs consistently consume more outbound throughput than intended.

### IAM ID bytes out quota used percentage
{: #ibm_eventstreams_iam_id_bytes_out_quota_used_percentage}

The percentage of bytes out quota used per IAM ID.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_iam_id_bytes_out_quota_used_percentage`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance name, IBM IAM ID, Service instance` |
{: caption="IAM ID bytes out quota used percentage metric metadata" caption-side="bottom"}

This metric is for information to help you monitor trends in your usage, particularly if any IAM IDs are consuming close to their quota limits.

Quota metrics might sometimes exceed 100%. Kafka quotas use sampling and are applied asynchronously. For some workloads, especially where data is sent in large batches, this might result in small deviations from the limit.

### Inactive consumer groups
{: #ibm_eventstreams_instance_inactive_consumergroups}

The number of inactive consumer groups in an {{site.data.keyword.messagehub}} instance.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_inactive_consumergroups`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Inactive consumer groups metric metadata" caption-side="bottom"}

This is for information only and is not an issue. Spikes indicate that a set of consumer groups stopped sending messages.

### Instance bytes in per second
{: #ibm_eventstreams_instance_bytes_in_per_second}

This metric tracks the rate of data, in bytes per second, that is produced to an {{site.data.keyword.messagehub}} instance.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_bytes_in_per_second`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Instance bytes in per second metric metadata" caption-side="bottom"}

Use this metric to monitor data ingestion rates and identify trends in client-produced throughput.
Compare observed values against the recommended limits for your plan and instance to ensure that your workload remains within supported thresholds. For more information, see [{{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-plan_choose){: external}.

### Instance bytes out per second
{: #ibm_eventstreams_instance_bytes_out_per_second}

This metric tracks the rate of data, in bytes per second, that is consumed from an {{site.data.keyword.messagehub}} instance.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_bytes_out_per_second`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Instance bytes out per second metric metadata" caption-side="bottom"}

This is for information to help you monitor trends in your usage of how many incoming or outgoing MB/s your clients are transferring to and from your cluster. To determine what the recommended limits for your plan and instance are, see [{{site.data.keyword.messagehub}}](/docs/EventStreams?topic=EventStreams-plan_choose){: external}.

### Instance utilization
{: #ibm_eventstreams_instance_utilization}

The level of utilization of an {{site.data.keyword.messagehub}} instance. This is a numeric value between zero and two (inclusive):

- `0` indicates that the workload being processed by this instance is within the capacity of the instance. More precisely, the utilization level is under 80%.
- `1` indicates that the workload being processed by this instance is approaching the capacity limit for the instance. Review whether it is appropriate to scale the service instance. More precisely, the utilization level is over 80% and under 95%.
- `2` indicates the workload being processed by this instance is at the capacity limit for the instance. As a result of this, messaging latency might increase. Review whether it is appropriate to scale the service instance. More precisely, the utilization level is over 95%.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_utilization`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Instance utilization metric metadata" caption-side="bottom"}

### Message rate per partition
{: #ibm_eventstreams_instance_message_rate_per_partition}

The rate of change of this metric gives the message per second that is incoming in to a partition of a {{site.data.keyword.messagehub}} instance topic.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_message_rate_per_partition`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name, IBM {{site.data.keyword.messagehub}} Kafka topic, IBM {{site.data.keyword.messagehub}} Kafka partition` |
{: caption="Message rate per partition metric metadata" caption-side="bottom"}

### Number of offline partitions
{: #ibm_eventstreams_kafka_offline_partitions}

The number of partitions offline in an {{site.data.keyword.messagehub}} instance.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_kafka_offline_partitions`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Number of offline partitions metric metadata" caption-side="bottom"}

Ideally this value should be zero. A nonzero value might indicate to a temporary issue with the cluster. It might also indicate to a Kafka partition leader election difficulty.

### Number of partitions
{: #ibm_eventstreams_instance_partitions}

The number of leader partitions in an {{site.data.keyword.messagehub}} instance.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_partitions`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Number of partitions metric metadata" caption-side="bottom"}

This is for information to help you monitor trends in your usage. Refer to [{{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose){: external} to determine what the recommended limits are for your plan and instance.

### Number of topics
{: #ibm_eventstreams_instance_topics}

The number of topics in an {{site.data.keyword.messagehub}} instance.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_topics`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Number of topics metric metadata" caption-side="bottom"}

### Number of under min ISR in-sync replica partitions
{: #ibm_eventstreams_kafka_under_minisr_partitions}

The number of partitions with fewer than two in-sync replicas.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_kafka_under_minisr_partitions`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Number of under in-sync replica partitions metric metadata" caption-side="bottom"}

Ideally this value should be zero. A nonzero value might highlight a temporary issue with the cluster.

### Produce message conversion time
{: #ibm_eventstreams_instance_produce_conversions_time_quantile}

Indicates that the accumulated time spent performing message conversion from clients that are producing by using older protocol versions.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_produce_conversions_time_quantile`|
| `Metric Type` | `gauge` |
| `Value Type`  | `second` |
| `Segment By` | `Service instance, Quantile, Service instance name` |
{: caption="Produce message conversion time metric metadata" caption-side="bottom"}

Ideally zero. A consistent growth in this indicates that some clients are down-level and should be upgraded. Ensure that all clients are at the latest levels.

### Rebalancing consumer groups
{: #ibm_eventstreams_instance_rebalancing_consumergroups}

The number of rebalancing consumer groups in an {{site.data.keyword.messagehub}} instance.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_rebalancing_consumergroups`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Rebalancing consumer groups metric metadata" caption-side="bottom"}

While it is expected that this figure is occasionally >0 (as broker restarts happen frequently,) sustained high levels suggest that consumers might be restarting frequently and leaving or rejoining the consumer groups. Check you client logs.

### Reserved disk space percentage
{: #ibm_eventstreams_instance_reserved_disk_space_percent}

Shows the percentage of reserved disk space that is required if all partitions are fully utilized.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_reserved_disk_space_percent`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Reserved disk space percentage metric metadata" caption-side="bottom"}

Shows the percentage of disk space that would be used if data reaches the configured retention limits.

### Stable consumer groups
{: #ibm_eventstreams_instance_stable_consumergroups}

The number of stable consumer groups in an {{site.data.keyword.messagehub}} instance.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_stable_consumergroups`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Stable consumer groups metric metadata" caption-side="bottom"}

Use along with rebalancing consumer groups. If this is consistently zero and rebalancing high, then it indicates a cluster problem. If this is nonzero and rebalancing high, it indicates a consumer group issue.

### Topic bytes in per second
{: #ibm_eventstreams_instance_topic_bytes_in_per_second}

This metric tracks the rate of data, in bytes per second, that is produced to each topic.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_topic_bytes_in_per_second`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance, IBM {{site.data.keyword.messagehub}} Kafka topic, Service instance name` |
{: caption="Topic bytes in per second metric metadata" caption-side="bottom"}

Use this metric to monitor data ingestion at the topic level and identify trends in producer activity. Compare throughput across topics to detect imbalances and potential anomalies.

### Topic bytes out per second
{: #ibm_eventstreams_instance_topic_bytes_out_per_second}

This metric tracks the rate of data, in bytes per second, that is consumed from each topic.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_topic_bytes_out_per_second`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance, IBM {{site.data.keyword.messagehub}} Kafka topic, Service instance name` |
{: caption="Topic bytes out per second metric metadata" caption-side="bottom"}

Use this metric to monitor data consumption at the topic level and understand how throughput is distributed across consumers. Compare activity across topics to identify bottlenecks, imbalances, or anomalies.

### Topic size
{: #ibm_eventstreams_instance_topic_size}

Total disk size currently being used by partitions of a topic e.g if a topic has two partitions, one with 2MB of data and one with 4MB of data, the metric will report the size as 6MB. This can be used to monitor storage usage and optimise partitioning.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_topic_size`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance, Service instance name, IBM {{site.data.keyword.messagehub}} Kafka topic` |
{: caption="Topic size metric metadata" caption-side="bottom"}

### Used disk space percentage
{: #ibm_eventstreams_instance_utilised_disk_space_percent}

Shows the percentage of disk space that is currently in use.

| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_eventstreams_instance_utilised_disk_space_percent`|
| `Metric Type` | `gauge` |
| `Value Type`  | `percent` |
| `Segment By` | `Service instance, Service instance name` |
{: caption="Used disk space percentage metric metadata" caption-side="bottom"}

Provides informational insight to help you monitor usage trends over time.

## Attributes for Segmentation
{: attributes}

### Global attributes
{: #global-attributes}

The following attributes are available for segmenting all of the listed metrics.

| Attribute | Attribute name | Attribute description |
|-----------|----------------|-----------------------|
| `Cloud Type` | `ibm_ctype` | The cloud type is a value of public, dedicated, or local. |
| `Location` | `ibm_location` | The location of the monitored resource - this might be a region, data center or global. |
| `Scope` | `ibm_scope` | The scope is the account, organization, or space GUID associated with this metric. |
| `Service name` | `ibm_service_name` | Name of the service that generates this metric. |
| `Service instance` | `ibm_service_instance` | The service instance GUID identifies the instance that the metric is associated with. |
| `Service instance name` | `ibm_service_instance_name` | The service instance name provides the user-provided name of the service instance that isn't necessarily a unique value that depends on the name that is provided by the user. |
| `Resource group` | `ibm_resource_group_name` | The resource group name where the service instance was created. |
{: caption="Global attributes" caption-side="bottom"}

### Additional attributes
{: #additional-attributes}

The following attributes are available for segmenting one or more attributes. See the individual metrics for segmentation options.

| Attribute | Attribute name | Attribute description |
|-----------|----------------|-----------------------|
| `IBM {{site.data.keyword.messagehub}} Kafka partition` | `ibm_eventstreams_partition` | IBM Event Streams Kafka partition. |
| `IBM {{site.data.keyword.messagehub}} Kafka topic` | `ibm_eventstreams_topic` | IBM Event Streams Kafka topic. |
| `Quantile` | `ibm_quantile` | The quantile represented when a metric supports segmenting by quantile |
{: caption="Additional attributes" caption-side="bottom"}

For more information about enabling platform metrics from the {{site.data.keyword.messagehub}} dashboard and viewing metrics, see [Monitoring {{site.data.keyword.messagehub}} metrics](/docs/monitoring?topic=monitoring-monitoring){: external}.
