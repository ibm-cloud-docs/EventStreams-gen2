---

copyright:
  years: 2015, 2024
lastupdated: "2024-02-15"

keywords: MQ bridge, connect configuration

subcollection: EventStreams

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting {{site.data.keyword.IBM}} MQ to {{site.data.keyword.messagehub}} by using the Kubernetes service
{: #mq_connector}

Get the Kafka Connect runtime to run in an {{site.data.keyword.containerlong}} cluster. Then, start the {{site.data.keyword.IBM_notm}} MQ Source Connector to copy messages from an {{site.data.keyword.IBM_notm}} MQ source queue to a destination Kafka topic in {{site.data.keyword.messagehub}}.
{: shortdesc}

The {{site.data.keyword.IBM_notm}} MQ Source Connector connects to an {{site.data.keyword.IBM_notm}} MQ queue manager and consumes MQ message data from an MQ queue. The Connector converts each MQ message into a Kafka record and sends the message to an {{site.data.keyword.messagehub}} Kafka topic.


## Step 1. Install the prerequisites
{: #step1_install_prereqs_mq}

Ensure you have the following software and services installed:

* An {{site.data.keyword.messagehub}} instance - Standard or Enterprise plan. 
* An instance of [{{site.data.keyword.IBM_notm}} MQ on Cloud](/docs/mqcloud?topic=mqcloud-getting_started){: external} or [{{site.data.keyword.IBM_notm}} MQ Version 8](https://www.ibm.com/support/pages/downloading-ibm-mq-80){: external}, or later. 
   
   You can configure the {{site.data.keyword.IBM_notm}} MQ Connector to authenticate with {{site.data.keyword.IBM_notm}} MQ by using a user identifier and password. We recommend that you grant the following permissions only to the identity associated with an instance of the MQ bridge:
   * CONNECT authority. The {{site.data.keyword.IBM_notm}} MQ Connector must be able to connect to the MQ queue manager.
   * GET authority for the queue that the {{site.data.keyword.IBM_notm}} MQ Connector is configured to consume from.
* An {{site.data.keyword.containershort}} cluster. You can provision one for testing purposes at no cost. 

    You also need CLI access to your cluster. For more information, see
 [Setting up the CLI and API](/docs/containers?topic=containers-cli-install).
* A recent version of kubectl
* [Git](https://git-scm.com/downloads){: external}

## Step 2. Clone the kafka-connect repositories
{: #step2_clone_project_mq}

Clone the following two repositories that contain the required files:

* https://github.com/ibm-messaging/kafka-connect-mq-source
* https://github.com/ibm-messaging/event-streams-samples


## Step 3. Create your Kafka Connect configuration
{: #step3_create_config_mq}

1. You must set up this configuration only once. {{site.data.keyword.messagehub}} stores it for future use.

    From the event-streams-samples project, navigate to the `kafka-connect/IKS directory`, edit the `connect-distributed.properties` file, and replace `<BOOTSTRAP_SERVERS>`; in one place and `<APIKEY>` in three places with your {{site.data.keyword.messagehub}} credentials.

    Provide `<BOOTSTRAP_SERVERS>` as a comma-separated list. If they are not valid, you get an error.

    Your `<APIKEY>` appears in clear text on your machine but is secret when pushed to {{site.data.keyword.containershort}}.

    Kafka Connect can run multiple workers for reliability and scalability reasons. If your {{site.data.keyword.containershort}} cluster has more than one node and you want multiple Connect workers, edit the `kafka-connect.yaml` file, and edit the entry `replicas: 1`.

2. Then, run the following commands.

    To create a secret, run the following command: 

    ```go
    kubectl create secret generic connect-distributed-config --from-file=connect-distributed.properties
    ```
    {: codeblock}

    To create a configmap, run the following command:
    
    ```go
    kubectl create configmap connect-log4j-config --from-file=connect-log4j.properties
    ```
    {: codeblock}


## Step 4. Deploy Kafka Connect
{: #step4_deploy_kafka_mq}

Apply the configuration in the `kafka-connect.yaml` file by running the following command:

```go
kubectl apply -f ./kafka-connect.yaml
```
{: codeblock}


## Step 5. Validate Kafka Connect is running
{: #step5_validate_connector_mq}

To validate that Kafka Connect is running, port forward to the kafkaconnect-service on port 8083, as in the following example.

```go
kubectl port-forward service/kafkaconnect-service 8083
```
{: codeblock}

Keep the terminal that you used for port forwarding open, and use another terminal for the next steps.

The Connect REST API is then available at `http://localhost:8083`. If you want more information about the API, see
[Kafka Connect REST Interface](http://kafka.apache.org/documentation/#connect_rest){: external}.

So, you now have the Kafka Connect runtime that is deployed and running in {{site.data.keyword.containershort}}. Next, let's configure and start the {{site.data.keyword.IBM_notm}} MQ Connector.


## Step 6. Configure the mq-source JSON file
{: #step6_config_json_mq}

Edit the `mq-source.json` file that is located in `kafka-connect-mq-source/config` so that, at a minimum, the required properties are completed with your information.

### mq-source.json file properties
{: #mq-source_props}

Replace the placeholders in the `mq-source.json` file with your own values.

TOPIC
:   Required. Name of the destination Kafka topic.

QUEUE_MANAGER
:   Required. Name of the source IBM MQ queue manager.

QUEUE
:   Required. Name of the source IBM MQ queue. 

CHANNEL_NAME
:   Required (unless you're using bindings or a CCDT file). Name of the server-connection channel.

CONNECTION_NAME_LIST
:   Required (unless you're using bindings or a CCDT file). A list of one or more host(port) pairs for connecting to the queue manager. Separate entries with a comma. 


## Step 7. Start the connector with its configuration
{: #step7_start_connector_mq}

Run the following command to start the {{site.data.keyword.IBM_notm}} MQ Connector with the configuration that you provided in the previous step.

```sh
curl -X POST -H "Content-Type: application/json" http://localhost:8083/connectors --data "@./mq-source.json"
```
{: codeblock}

## Step 8. Monitor your connector 
{: #step8_monitor_connector_mq}

You can check your connector by going to the following address: 

`http://localhost:8083/connectors/mq-source/status`

If the state of the connector is not running, restart the connector.

