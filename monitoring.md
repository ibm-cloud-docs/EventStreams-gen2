---

copyright:
  years: 2026
lastupdated: "2026-02-26"

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
| [Consume message conversion time](#ibm_eventstreams_instance_consume_conversions_time_quantile) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Inactive consumer groups](#ibm_eventstreams_instance_inactive_consumergroups) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Number of partitions](#ibm_eventstreams_instance_partitions) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Produce message conversion time](#ibm_eventstreams_instance_produce_conversions_time_quantile) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Rebalancing consumer groups](#ibm_eventstreams_instance_rebalancing_consumergroups) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Stable consumer groups](#ibm_eventstreams_instance_stable_consumergroups) |   ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Number of topics](#ibm_eventstreams_instance_topics) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Number of offline partitions](#ibm_eventstreams_kafka_offline_partitions) |  ![Checkmark icon](../icons/checkmark-icon.svg) |
| [Number of under in-sync replica partitions](#ibm_eventstreams_kafka_under_minisr_partitions) |   ![Checkmark icon](../icons/checkmark-icon.svg) |
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
