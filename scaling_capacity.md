---

copyright:
  years: 2025
lastupdated: "2025-12-16"

keywords: scaling capacity, throughput, storage, scaling

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Scaling Enterprise plan capacity
{: #ES_scaling_capacity}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

## Event Streams capacity
{: #ES_capacity}

The {{site.data.keyword.messagehub}} Enterprise plan lets you specify throughput and storage capacity when a new instance of the service is created. If after using the service instance, you find that the current capacity configuration of your service instance is not meeting the demands of your solution, capacity can be scaled up to meet demands.
{: shortdesc}

The scaling of throughput is currently not supported.

## Throughput capacity
{: #ES_throughput_capacity}

The throughput capacity represents the minimum throughput that can be expected to be continually available across the service instance assuming typical workloads. It also takes into account operational procedures and failure modes, such as the loss of an availability zone. Throughput is divided equally between produce and consume traffic. At any individual point in time, a higher throughput selected may be achievable but cannot be relied on to be continually available.

The minimum throughput option available is 100MB/s, divided into 50MB/s for produce workload and 50MB/s for consume workload. Scaling throughput is currently not supported. For more information on capacity recommendations and limitations, see [limits and quotas](/docs/EventStreams-gen2?topic=EventStreams-gen2-how-event-streams-uses-limits-and-quotas). Although throughput scaling is independent of storage, for each tier a defined minimum of storage amount is required. 

## Storage capacity
{: #ES_storage_capacity}

Storage capacity is the amount of storage that is allocated in the service instance for retention of message data. Storage figures are quoted in decimal (base 10), for example, 2 TB represents 2x10^12 Bytes. Storage capacity can be scaled up, independent of throughput capacity, when data retention is important for your architecture.

Storage capacity cannot be scaled down.
{: important}

{{site.data.keyword.messagehub}} stores three replicas of your data to ensure the highest level of resilience across three availability zones. When you select 2 TB of storage with {{site.data.keyword.messagehub}}, it is equivalent to deploying 6 TB of storage, if you are running your own Apache Kafka cluster with the same replication policy enabled.

## How to scale capacity
{: #ES_how_to_scale_capacity}

The following steps show you how to scale up throughput and storage capacity for an {{site.data.keyword.messagehub}} Enterprise plan service instance. If you do not have an Enterprise instance, these steps help you to create one.

Currently, scaling up an {{site.data.keyword.messagehub}} service instance capacity requires the use of the {{site.data.keyword.Bluemix_notm}} CLI.

To install this tool, see [install devtools](/docs/cli?topic=cli-install-ibmcloud-cli).

The {{site.data.keyword.Bluemix_notm}} CLI command uses the **service-instance-update** command to update your {{site.data.keyword.messagehub}} service instance resource. The user ID in the account used to issue the **service-instance-** command must be assigned the same access policies that are needed when you create resources. For information about access requirements, see [creating resources](/docs/account?topic=account-manage_resource#creating-resources).

### During the scale-up process
{: #scaleup_process}

The time required to scale up the {{site.data.keyword.messagehub}} service instance is variable, but under normal circumstances it does not exceed 3 hours. Both throughput and storage require provisioning extra infrastructure.

During this time, the flow of Kafka data is not affected (the scaling operation is nondisruptive).

### Example
{: #scaleup_example}

This example shows how to first deploy a service instance configured with a base capacity unit, which is the default:
* 100 MB/s of throughput capacity.
* 2 TB of storage capacity for data retention.

The example then shows how to scale this service instance to 4 TB of storage to provide:
* 100 MB/s of throughput capacity.
* 4 TB of storage capacity for data retention.

1. If you don't already have one, create an {{site.data.keyword.messagehub}} service instance.

    a. Log in to the **{{site.data.keyword.Bluemix_notm}} console**.

    b. Click the **{{site.data.keyword.messagehub}} service** in the Catalog.

    c. Select the **Enterprise plan Gen2** on the service instance page, ensuring that a supported location is selected.

    d. Review capacity selections of 100 MB/s throughput and 2 TB storage.

    e. Enter a name for your service instance. You can use the default value.

    f. Click Create. (For information about the amount of time needed to create the service instance, see [Choosing your plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose#what_is_supported)).

2. Log in to the **{{site.data.keyword.Bluemix_notm}} CLI**.

      ```sh
      ibmcloud login
      ```
      {: pre}

3. Get the resource name of your {{site.data.keyword.messagehub}} service instance.

      ```sh
      ibmcloud resource service-instances
      ```
      {: pre}

      (You can find the name of your instance in the Name column.)

4. View the current capacity configuration of your instance.

      ```sh
      ibmcloud resource service-instance "Event Streams resource instance name" -o JSON
      ```
      {: pre}

   The information is available in the *kafka* field of the *extensions.dataservices* section of the output:

      ```
      "extensions": {
        "dataservices": {
        ...
          "kafka": {
             "storage_gb": 2000,
              "throughput_mb_s": 100,
          },
       ```
      {: pre}

6. Scale up the service instance from **2 TB storage capacity** to **4 TB storage capacity**.

    a. Run the following command from the CLI.

      ```sh
      ibmcloud resource service-instance-update "Event Streams resource instance name" -p '{"dataservices":{"kafka":{storage_gb":"4000"}}}'
      ```
      {: pre}

7. Monitor the update of the service instance.

    The scale-up process can take from several minutes to three hours to complete depending on what new resources need to be allocated to the service instance.

    You can get the current service instance information by using the following command.

      ```sh
      ibmcloud resource service-instance "Event Streams resource instance name" --output=json
      ```

      {: pre}
    Review the `Last Operation` section of the output. The information is continuously updated as the update proceeds. When the scale-up process completed, the last operation information indicates update succeeded or sync succeeded.

    Run the command again until success is indicated.
