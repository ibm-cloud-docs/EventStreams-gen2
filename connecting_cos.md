---

copyright:
  years: 2015, 2024
lastupdated: "2024-02-15"

keywords: archive, connector, connect to cos, using kubernetes

subcollection: EventStreams

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting {{site.data.keyword.messagehub}} to {{site.data.keyword.cos_full_notm}} by using the Kubernetes Service
{: #cos_connector}

Get the Kafka Connect runtime to run in an {{site.data.keyword.containershort}} cluster. Then, start the {{site.data.keyword.cos_full}} Sink Connector to archive data from Kafka topics in {{site.data.keyword.messagehub}} to an instance of the {{site.data.keyword.cos_full}} service. 
{: shortdesc}

The Connector consumes batches of messages from Kafka and uploads the message data as objects to a bucket in the Cloud {{site.data.keyword.cos_short}} service. 

## Step 1. Install the prerequisites
{: #step1_install_prereqs}

Ensure you have the following software and services installed:

* An {{site.data.keyword.messagehub}} instance - Standard or Enterprise plan. You need to create credentials.
* An instance of the Cloud {{site.data.keyword.cos_short}} service with at least one bucket.
* An {{site.data.keyword.containerfull}} cluster. You can provision a free one for testing purposes. 

    You also need CLI access to your cluster. For more information, see
 [Setting up the CLI and API](/docs/containers?topic=containers-cli-install).
* A recent version of kubectl.
* [Git](https://git-scm.com/downloads){: external}

## Step 2. Clone the kafka-connect repositories
{: #step2_clone project}

Clone the following two repositories that contain the required files:

* https://github.com/ibm-messaging/kafka-connect-ibmcos-sink
* https://github.com/ibm-messaging/event-streams-samples


## Step 3. Create your Kafka Connect configuration
{: #step3_create_config}

1. You must set up this configuration only once. {{site.data.keyword.messagehub}} stores it for future use.

    From the event-streams-samples project, navigate to the `kafka-connect/IKS directory`, 
    edit the `connect-distributed.properties` file, and replace `<BOOTSTRAP_SERVERS>` in one place and `<APIKEY>` 
    in three places with your {{site.data.keyword.messagehub}} credentials.

    Provide `<BOOTSTRAP_SERVERS>` as a comma-separated list. If they are not valid, you get an error.

    Your `<APIKEY>` appears in clear text on your machine but is secret when pushed to {{site.data.keyword.containerlong}}.

    Kafka Connect can run multiple workers for reliability and scalability reasons. If your {{site.data.keyword.containershort}} 
    cluster has more than one node and you want multiple Connect workers, edit the `kafka-connect.yaml` file and edit 
    the entry `replicas: 1`.

2. Then, run the following commands:

    Run the following command to create a secret: 
    ```go
    kubectl create secret generic connect-distributed-config --from-file=connect-distributed.properties
    ```
    {: codeblock}

    Run the following command to create a configmap:
    ```go
    kubectl create configmap connect-log4j-config --from-file=connect-log4j.properties
    ```
    {: codeblock}


## Step 4. Deploy Kafka Connect
{: #step4_deploy_kafka}

Apply the configuration in the `kafka-connect.yaml` file by running the following command:

```go
kubectl apply -f ./kafka-connect.yaml
```
{: codeblock}


## Step 5. Validate Kafka Connect is running
{: #step5_validate_connector}

To validate that Kafka Connect is running, port forward to the kafkaconnect-service on port 8083, as in the following example:

```go
kubectl port-forward service/kafkaconnect-service 8083
```
{: codeblock}

Keep the terminal that you used for port forwarding open, and use another terminal for the next steps.

The Connect REST API is then available at `http://localhost:8083`. If you want more information about the API, see
[Kafka Connect REST Interface](http://kafka.apache.org/documentation/#connect_rest){: external}.

So, you now have the Kafka Connect runtime that is deployed and running in {{site.data.keyword.containershort}}. Next, configure, and start the {{site.data.keyword.cos_short}} connector.


## Step 6. Configure the cos-sink JSON file
{: #step6_config_json}

Edit the `cos-sink.json` file located in `kafka-connect-ibmcos-sink/config/` so that at a minimum your required properties are completed with your information. Although the configuration properties cos.object.deadline.seconds, cos.interval.seconds, and cos.object.records are listed as optional, you must set at least one of these properties to a nondefault value.

### cos-sink.json file properties
{: #cos-sink_props}

Replace the placeholders in the `cos-sink.json` file with your own values.

File properties | Description
--- | ---
cos.api.key | Required. API key used to connect to the Cloud {{site.data.keyword.cos_short}} service instance.
cos.bucket.location | Required. Location of the Cloud {{site.data.keyword.cos_short}} service bucket. For example, for a [regional bucket](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-region) `eu-gb`, or for a [global bucket](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-geo) `eu`.
cos.bucket.name | Required. Name of the Cloud {{site.data.keyword.cos_short}} service bucket to write data into.
cos.bucket.resiliency | Required. Resiliency of the Cloud {{site.data.keyword.cos_short}} bucket. Must be one of: cross-region, regional, or single-site.
cos.service.crn | Required. CRN for the Cloud {{site.data.keyword.cos_short}} service instance. Ensure you enter the correct CRN: it is the resource instance ID ending with double colons, for example, `crn:v1:staging:public:cloud-object-storage:global:a/8c226dc8c8bfb9bc3431515a16957954:b25fe12c-9cf5-4ee8-8285-2c7e6ae707f6::`.
cos.endpoint.visibility | Optional. Specify *public* to connect to the Cloud {{site.data.keyword.cos_short}} service over the public internet. Specify *private* to connect from a connector that runs inside the IBM Cloud network, for example, from an IBM Cloud Kubernetes Service cluster. The default is public.
cos.object.deadline.seconds | Optional. The number of seconds (as measured wall clock time for the Connect Task instance) between reading the first record from Kafka, and writing all of the records read so far into a Cloud {{site.data.keyword.cos_short}} object. This property can be useful in situations that have long pauses between Kafka records being produced to a topic. It ensures that any records that are received by this connector are always written into {{site.data.keyword.cos_short}} within the specified time.
cos.object.interval.seconds | Optional. The number of seconds (as measured by the timestamps in Kafka records) between reading the first record from Kafka, and writing all of the records read so far into a Cloud {{site.data.keyword.cos_short}} object.
cos.object.records | Optional. The maximum number of Kafka records to combine into an object.
{: caption="cos-sink.json file properties" caption-side="bottom"}
 
### Get {{site.data.keyword.cos_short}} credentials that use the {{site.data.keyword.Bluemix_notm}} console
{: #connect_enterprise_console}

1. Locate your {{site.data.keyword.cos_short}} service on the dashboard.
2. Click your service tile.
3. Click **Service Credentials**.
4. Click **New Credential**. 
5. Complete the details for your new credential like a name and role and click **Add**. A new credential appears in the credentials list.
6. Click this credential by using **View Credentials** to reveal the details in JSON format.


## Step 7. Start the connector with its configuration
{: #step7_start_connector}

Run the following command to start the {{site.data.keyword.cos_short}} connector with the configuration that you provided in the previous step.

```sh
curl -X POST -H "Content-Type: application/json" http://localhost:8083/connectors --data "@./cos-sink.json"
```
{: codeblock}

## Step 8. Monitor your connector 
{: #step8_monitor_connector}

You can check your connector by going to the following location.

`http://localhost:8083/connectors/cos-sink/status`

If the state of the connector is not running, restart the connector.

## Step 9. Delete your connector
{: #step9_delete_connector}

You can use the following command to delete a connector. 

```sh
curl -X DELETE 
http://localhost:8083/connectors/cos-sink
```
