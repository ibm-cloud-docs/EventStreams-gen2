---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: scaling capacity, throughput, storage, scaling

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Scaling {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}} Enterprise plan capacity
{: #ES_scaling_capacity}

[Gen 2]{: tag-purple}

Learn how to scale up throughput and storage capacity for {{site.data.keyword.messagehub}} Enterprise plan service instances on {{site.data.keyword.cloud}} by using the {{site.data.keyword.cloud_notm}} CLI.
{: shortdesc}

## Event Streams capacity
{: #ES_capacity}

The {{site.data.keyword.messagehub}} Enterprise plan lets you specify throughput and storage capacity when you create a new service instance. If the current capacity configuration of your service instance does not meet the demands of your solution, you can scale up the capacity.

Scaling of throughput is not currently supported.

## Throughput capacity
{: #ES_throughput_capacity}

The throughput capacity represents the minimum throughput that is continually available across the service instance, assuming typical workloads. It also accounts for operational procedures and failure modes, such as the loss of an availability zone. Throughput is divided equally between produce and consume traffic. At any individual point in time, you might achieve a higher throughput, but you can't rely on it to be continually available.

The minimum throughput option available is 100 MB/s, divided into 50 MB/s for produce workloads and 50 MB/s for consume workloads. Scaling throughput is not currently supported. For more information, see [Limits and quotas](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_quotas).

## Storage capacity
{: #ES_storage_capacity}

Storage capacity is the amount of storage allocated in the service instance for message retention. Storage numbers use decimal (base 10) units, where 2 TB represents 2 x 10^12 bytes. You can scale storage capacity upward independently of throughput capacity. Storage capacity cannot be scaled down.

{{site.data.keyword.messagehub}} stores three replicas of your data across three availability zones. If you select 2 TB of storage, you get 2 TB of usable capacity, which corresponds to 6 TB of raw storage across the cluster.

## How to scale capacity
{: #ES_how_to_scale_capacity}

You can scale up storage capacity for an {{site.data.keyword.messagehub}} Enterprise plan service instance by using the {{site.data.keyword.cloud_notm}} CLI.

To install the CLI, see [Installing the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

Use the `ibmcloud resource service-instance-update` command to update your {{site.data.keyword.messagehub}} instance. The user or service ID that runs the command must have Administrator or Editor permissions in the resource group. For more information, see [Managing access to resources](/docs/account?topic=account-manage_resource#creating-resources).

### During the scale-up process
{: #scaleup_process}

Scaling storage capacity requires provisioning additional backing storage. The scale-up operation is non-disruptive, and message traffic continues to flow without interruption. The process typically completes in minutes, but can take up to 3 hours.

### Step-by-step example: Scaling storage from 2 TB to 4 TB
{: #scaleup_example}

The following example demonstrates scaling an Enterprise Gen 2 instance from 2 TB to 4 TB of storage:

1. Log in to the {{site.data.keyword.cloud_notm}} CLI:

   ```sh
   ibmcloud login
   ```
   {: pre}

2. Locate the resource name of your {{site.data.keyword.messagehub}} service instance:

   ```sh
   ibmcloud resource service-instances
   ```
   {: pre}

3. View the current capacity configuration:

   ```sh
   ibmcloud resource service-instance INSTANCE_NAME -o JSON
   ```
   {: pre}

   The configuration appears under `extensions.dataservices.kafka`:

   ```json
   {
     "extensions": {
       "dataservices": {
         "kafka": {
           "storage_gb": 2000,
           "throughput_mb_s": 100
         }
       }
     }
   }
   ```
   {: codeblock}

4. Scale storage capacity to 4 TB (4000 GB):

   ```sh
   ibmcloud resource service-instance-update INSTANCE_NAME -p '{"dataservices":{"kafka":{"storage_gb":4000}}}'
   ```
   {: pre}

5. Monitor the status of the scale operation:

   ```sh
   ibmcloud resource service-instance INSTANCE_NAME -o JSON
   ```
   {: pre}

   Check the `last_operation` object in the JSON output. When the scale-up process completes, `last_operation.state` indicates `succeeded`.
