---

copyright:
  years: 2020, 2024
lastupdated: "2024-02-15"

keywords: scaling capacity, throughput, storage, scaling

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Scaling Enterprise plan capacity
{: #ES_scaling_capacity}

## Event Streams capacity
{: #ES_capacity}

The {{site.data.keyword.messagehub}} Enterprise plan lets you specify throughput and storage capacity when a new instance of the service is created. If after using the service instance, you find that the current capacity configuration of your service instance is not meeting the demands of your solution, throughput and storage capacity can be scaled up to meet demands.
{: shortdesc}

Each base or additional capacity unit includes the following capacity:
* 150 MB/s of throughput capacity.
* 2 TB of storage capacity available for your data retention.

For example, selecting a base capacity unit, one additional capacity unit, and 4 TB of additional storage provides you with the following capacity:
* 300 MB/s of throughput capacity.
* 8 TB of storage capacity for data retention.


## Throughput capacity
{: #ES_thruput_capacity}

Throughput capacity is the recommended MB/s maximum for producing and consuming messages. 

Each capacity unit provides 150 MB/s of throughput capacity. It consists of 75 MB/s data ingress and 75 MB/s data egress capacity.

To scale up throughput capacity, you can add more capacity units. Each extra capacity unit adds 150 MB/s of throughput to your service instance, to a total of 450 MB/s.

The recommended throughput maximum is based on a typical workload and considers the possible impact of operational actions or failure modes, like the loss of an availability zone. If the average throughput exceeds the recommended figure, a loss in performance might be experienced during these conditions. Plan your maximum throughput capacity as two-thirds of the maximum. For example, two-thirds of the 150 MB/s maximum are 100 MB/s. For more information on capacity recommendations and limitations, see [limits and quotas](/docs/EventStreams?topic=EventStreams-kafka_quotas#limits_enterprise).

Although throughput scaling is independent of storage, for each tier a defined minimum of storage amount is required. 

## Storage capacity
{: #ES_storage_capacity}

Storage capacity is the amount of storage that is allocated in the service instance for retention of message data. 

Storage capacity can be scaled up, independent of throughput capacity, when data retention is important for your architecture.

{{site.data.keyword.messagehub}} stores three replicas of your data to ensure the highest level of resilience across three availability zones. When you select 2 TB of storage with {{site.data.keyword.messagehub}}, it is equivalent to deploying 6 TB of storage if you are running your own Apache Kafka cluster with the same replication policy enabled.

## Scaling combinations
{: #ES_scaling_combinations}

The following table lists valid throughput and storage capacity unit combinations.

|**Throughput capacity**|**Available storage capacity**|
|-------------------|--------------------------|
|150 MB per second (75 MB/s producing, 75 MB/s consuming)|2 TB, 4 TB, 6 TB, 8 TB, 10 TB, 12 TB|
|300 MB per second (150 MB/s producing, 150 MB/s consuming)|4 TB, 8 TB, 12 TB|
|450 MB per second (225 MB/s producing, 225 MB/s consuming)|6 TB, 12 TB|
{: caption="Valid throughput and storage capacity unit combinations" caption-side="top"}

For more information about capacity limitations, see [limits and quotas](/docs/EventStreams?topic=EventStreams-kafka_quotas#limits_enterprise).

Throughput capacity cannot be scaled down. Moving to a lower throughput capacity requires creating a new {{site.data.keyword.messagehub}} service instance at the lower capacity unit.
{: important}

Storage capacity cannot be scaled down. To move to a lower storage capacity requires creating a new {{site.data.keyword.messagehub}} service instance at the lower capacity unit.
{: important}

## How to scale capacity
{: #ES_how_to_scale_capacity}

The following steps show you how to scale up throughput and storage capacity for an {{site.data.keyword.messagehub}} Enterprise plan service instance. If you do not have an Enterprise instance, these steps help you to create one.

Currently, scaling up an {{site.data.keyword.messagehub}} service instance capacity requires the use of the {{site.data.keyword.Bluemix_notm}} CLI.

To install this tool, see [install devtools](/docs/cli?topic=cli-install-ibmcloud-cli).

The {{site.data.keyword.Bluemix_notm}} CLI command uses the **service-instance-update** command to update your {{site.data.keyword.messagehub}} service instance resource. The user ID in the account used to issue the **service-instance-** command must be assigned the same access policies that are needed when you create resources. For information about access requirements, see [creating resources](/docs/account?topic=account-manage_resource#creating-resources).

### During the scale-up process
{: #scaleup_process}

The time required to scale up the {{site.data.keyword.messagehub}} service instance is variable, but under normal circumstances it does not exceed 3 hours. Both throughput and storage require provisioning extra infrastructure.

During this time, the flow of Kafka data is not affected (the scaling operation is nondisruptive). Kafka topic and partition add, update, and delete operations are suspended. This ensures that the integrity of data is maintained during storage volume infrastructure scale-up operations. This suspension of topic and partition operations occurs only during a brief portion of the scale-up process, not the entire process.

Valid combinations and values for the "throughput" and "storage_size" are listed in the following table.

|**Throughput capacity (maximum)**|**"throughput" value to specify**|**Storage capacity**|**"storage_size" value to specify**|
|----------------------------------------|-----------------------------|----------------------|------------------------------|
|1  (150 MB/s)|150|2 TB|2048|
| | |4 TB|4096|
| | |6 TB|6144|
| | |8 TB|8192|
| | |10 TB|10240|
| | |12 TB|12288|
|2  (300 MB/s)|300|4 TB|4096|
| | |8 TB|8192|
| | |12 TB|12288|
|3  (450 MB/s)|450|6 TB|6144|
| | |12 TB|12288|
{: caption="Valid combinations and values for 'throughput' and 'storage_size'" caption-side="top"}

### Example
{: #scaleup_example}

This example shows how to first deploy a service instance configured with a base capacity unit, which is the default:
* 150 MB/s of throughput capacity.
* 2 TB of storage capacity for data retention.

The example then shows how to scale this service instance to a configuration of a base capacity unit, one extra capacity unit, and 4 TB of extra storage to provide:
* 300 MB/s of throughput capacity.
* 8 TB of storage capacity for data retention.

1. If you don't already have one, create an {{site.data.keyword.messagehub}} service instance.
  
    a. Log in to the **{{site.data.keyword.Bluemix_notm}} console**.
    
    b. Click the **{{site.data.keyword.messagehub}} service** in the Catalog.
    
    c. Select the **Enterprise plan** on the service instance page.
    
    d. Review capacity selections of 150 MB/s throughput and 2 TB storage.
        
    e. Enter a name for your service instance. You can use the default value.
    
    f. Click Create. (For information about the amount of time needed to create the service instance, see [Choosing your plan](/docs/EventStreams?topic=EventStreams-plan_choose#what_is_supported)).

2. Log in to the **{{site.data.keyword.Bluemix_notm}} CLI**.
 
      ```sh
      ibmcloud login
      ```

3. Get the resource name of your {{site.data.keyword.messagehub}} service instance.
  
      ```sh
      ibmcloud resource service-instances
      ```
     
      (You can find the name of your instance in the Name column.)

4. View the current capacity configuration by using the {{site.data.keyword.messagehub}} CLI.
    
    To install and use the CLI plug-in, refer to [CLI reference](/docs/EventStreams?topic=EventStreams-cli_reference).
    
    Use the following command to display the current capacity configuration:
  
      ```sh
      ibmcloud es init  --instance-name  "Event Streams resource instance name"
      ```

    Output is similar to the following output, which shows this service instance is configured with 150 MB/s of throughput capacity and 2 TB of storage capacity:

        API Endpoint:         https://service-instance-adsf1234asdf1234asdf1234-0000.eu-south.containers.appdomain.cloud
        Service endpoints:    public
        Storage size:         2048 GB
        Throughput:           150 MB/s


5. Scale up the service instance from **150 MB/s throughput capacity and 2 TB storage capacity** to **300 MB/s throughput capacity and 8 TB storage capacity**. 
    
    a. Run the following command from the CLI.
    
      ```sh
      ibmcloud resource service-instance-update "Event Streams resource instance name" -p '{"throughput":"300","storage_size":"8192"}' 
      ```

    b. If you have an issue when you run the ibmcloud resource service-instance-update command and requires contacting IBM Support for assistance, 
    run the following command and include the output when you contact support.

      ```sh
      ibmcloud resource service-instance "Event Streams resource instance name" --output=json 
      ```

6. Monitor the update of the service instance.

    The scale-up process can take from several minutes to three hours to complete depending on what new resources need to be allocated to the service instance.
    
    You can get the current service instance information by using the following command.
    
      ```sh
      ibmcloud resource service-instance "Event Streams resource instance name" --output=json 
      ```
        
    Review the `Last Operation` section of the output. The information is continuously updated as the update proceeds. When the scale-up process completed, the last operation information indicates update succeeded or sync succeeded.

    Run the command again until success is indicated.

7. Verify the scaled-up capacity configuration by using the {{site.data.keyword.messagehub}} CLI.
  
    Display the capacity configuration with the following command.
    
      ```sh
      ibmcloud es init  --instance-name  "Event Streams resource instance name" 
      ```
        
    Output is similar to the following output, which shows that this service instance is configured with 300 MB/s of throughput capacity and 8 TB of storage capacity.


       API Endpoint:         https://service-instance-adsf1234asdf1234asdf1234-0000.eu-south.containers.appdomain.cloud
       Service endpoints:    public
       Storage size:         8192 GB
       Throughput:           300 MB/s

