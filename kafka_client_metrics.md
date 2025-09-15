---

copyright:
  years: 2024
lastupdated: "2024-03-15"

keywords: monitoring, metrics

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with Kafka client metrics
{: #getting-started-metrics}

With Kafka, monitoring typically involves various metrics that are related to topics, partitions, brokers, and consumer groups. Standard Kafka metrics include information on throughput, latency, replication, and disk usage. Refer to the [Kafka documentation](https://kafka.apache.org/documentation/){: external} and relevant monitoring tools to understand the specific metrics available for your version of Kafka and how to interpret them effectively.

## Why is it important to monitor Kafka clients?
{: #why-monitor}

Monitoring your {{site.data.keyword.messagehub}} instance is crucial to ensure optimal functionality and the overall health of your data pipeline. Monitoring your Kafka clients helps to identify early signs of application failure, such as high resource usage, lagging consumers, and bottlenecks. Identifying these warning signs early enables a proactive response to potential issues that minimize downtime and prevent any disruption to business operations.

Kafka clients (producers and consumers) have their own set of metrics to monitor their performance and health. In addition, the {{site.data.keyword.messagehub}} service supports a rich set of metrics produced by the server. For more information, see [Monitoring {{site.data.keyword.messagehub}} service metrics by using {{site.data.keyword.mon_full_notm}}](/docs/EventStreams?topic=EventStreams-metrics).

## Client metrics to monitor
{: #what-monitor}

### Producer metrics
{: #producer-metrics}

| Metric | Description |
| --- | --- |
| record-error-rate | This metric measures the average per-second number of records sent that resulted in errors. A high `record-error-rate` or an increase in `record-error-rate` might indicate a loss in data or data not being processed as expected. All these effects might compromise the integrity of the data you are processing and storing in Kafka. Monitoring this metric helps to ensure that data being sent by producers is accurately and reliably recorded in your Kafka topics.  |
| request-latency-avg | This is the average latency for each produce request in ms. An increase in latency impacts performance and might signal an issue. Measuring the `request-latency-avg` metric can help to identify bottlenecks in your instance. For many applications, low latency is crucial to ensure a high-quality user experience and a spike in `request-latency-avg` might indicate that you are reaching the limits of your provisioned instance. You can fix the issue by changing your producer settings, for example, by batching or scaling your plan to optimize performance.  |
| byte-rate  | The average number of bytes sent per second for a topic is a measure of your throughput. If you stream data regularly, a drop in throughput can indicate an anomaly in your Kafka instance. The {{site.data.keyword.messagehub}} Enterprise plan starts from 150 MB-per-second split one-to-one between ingress and egress and it is important to know how much of that you are consuming for effective capacity planning. Do not go above two-thirds of the maximum throughput, to account for the possible impact of operational actions, such as internal updates or failure modes (for example, the loss of an availability zone).  |
{: caption="Producer metrics" caption-side="bottom"}

### Consumer metrics
{: #consumer-metrics}

| Metric | Description |
| --- | --- |
| fetch-rate fetch-size-avg| The number of fetch requests per second (`fetch-rate`) and the average number of bytes fetched per request (`fetch-size-avg`) are key indicators for how well your Kafka consumers are performing. A high `fetch-rate` might signal inefficiency especially over a small number of messages, because it means insufficient data, or possibly no data is being received each time. The `fetch-rate` and `fetch-size-avg` are affected by three settings: `fetch.min.bytes`, `fetch.max.bytes`, and `fetch.max.wait.ms`. Tune these settings to achieve the desired overall latency, while minimizing the number of fetch requests and potentially the load on the broker CPU. Monitoring and optimizing both metrics ensures that you are processing data efficiently for current and future workloads. |
| commit-latency-avg | This metric measures the average time between a committed record being sent and the commit response being received. Similar to the `request-latency-avg` as a producer metric, a stable `commit-latency-avg` means that your offset commits happen in a timely manner. A high-commit latency might indicate problems in the consumer that prevent it from committing offsets quickly, which directly impacts the reliability of data processing. It might lead to duplicate processing of messages if a consumer has to restart and reprocess messages from a previously uncommitted offset. A high-commit latency also means spending more time in administrative operations than actual message processing. This issue might lead to backlogs of messages waiting to be processed, especially in high-volume environments.   |
| bytes-consumed-rate | This is a consumer-fetch metric that measures the average number of bytes consumed per second. Similar to the `byte-rate` as a producer metric, this should be a stable and expected metric. A sudden change in the expected trend of the `bytes-consumed-rate` might represent an issue with your applications. A low rate could be a signal of efficiency in data fetches or over-provisioned resources. A higher rate might overwhelm the consumers' processing capability and thus require scaling, creating more consumers to balance out the load, or changing consumer configurations, such as fetch sizes. |
| rebalance-rate-per-hour | The number of group rebalances participated in per hour. Rebalancing occurs every time there is a new consumer or when a consumer leaves the group and causes a delay in processing. This happens because partitions are re-assigned, which make Kafka consumers less efficient if there are a lot of rebalances per hour. A higher rebalance rate per hour could be caused by misconfigurations leading to unstable consumer behavior. This rebalancing act could cause an increase in latency and could result in applications crashing. Ensure that your consumer groups are stable by tracking a low and stable `rebalance-rate-per-hour`.  |
{: caption="Consumer metrics" caption-side="bottom"}
