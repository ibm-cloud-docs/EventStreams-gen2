---

copyright:
  years: 2024, 2025
lastupdated: "2025-09-15"

keywords: quick setup guide

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}
{:ui: .ph data-hd-interface="ui"}
{:cli: .ph data-hd-interface="cli"}
{:api: .ph data-hd-interface="api"}

# Quick setup guide for {{site.data.keyword.messagehub}} for {{site.data.keyword.cloud_notm}}
{: #quick_setup_guide}


This tutorial guides you through the steps to quickly start using {{site.data.keyword.messagehub}} by provisioning an instance, creating a topic and a credential, and then producing and consuming data. Additionally, you'll learn how to connect {{site.data.keyword.mon_full}} and {{site.data.keyword.at_full}}, and optionally how to use Kafka Connect or ksqlDB. Finally, you'll also find out how to get help with {{site.data.keyword.messagehub}}.
{: shortdesc}

Select your interface using the tabs at the start of the page.

Follow these steps to complete the tutorial: {: ui}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision an {{site.data.keyword.messagehub}} instance using the console](#provision_instance_ui)
* [Step 3: Create a topic and partitions using the console](#create_topic_ui)
* [Step 4: Create a service credential using the console](#create_credential_ui)
* [Step 5: Produce data using the console](#produce_data_ui)
* [Step 6: Consume data using the console](#consume_data_ui)
* [Step 7: Connect {{site.data.keyword.monitoringshort}}](#connect_monitoring_ui)
* [Step 8: Connect Activity Tracker](#activity_tracker_ui)
* [Step 9: (Optional) Use Kafka Connect or ksqlDB](#kafka_connect_ksql)
* [Step 10: If you need more help](#getting_help)
{: ui}

Follow these steps to complete the tutorial: {: cli}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision an {{site.data.keyword.messagehub}} instance using the CLI](#provision_instance_cli)
* [Step 3: Create a topic and partitions using the CLI](#create_topic_cli)
* [Step 4: Create a service credential using the CLI](#create_credential_cli)
* [Step 5: Produce data using the command line](#produce_data_cli)
* [Step 6: Consume data using the command line](#consume_data_cli)
* [Step 7: Connect {{site.data.keyword.monitoringshort}}](#connect_monitoring_cli)
* [Step 8: Connect Activity Tracker](#activity_tracker_cli)
* [Step 9: (Optional) Use Kafka Connect or ksqlDB](#kafka_connect_ksql)
* [Step 10: If you need more help](#getting_help)
{: cli}

Follow these steps to complete the tutorial: {: api}

* [Before you begin](#prereqs)
* [Step 1: Choose your plan](#choose_plan)
* [Step 2: Provision an {{site.data.keyword.messagehub}} instance using an API](#provision_instance_api)
* [Step 3: Create a topic and partitions using an API](#create_topic_api)
* [Step 4: Create a service credential using the CLI and an API](#create_credential_api)
* [Step 5: Produce data using an API](#produce_data_api)
* [Step 6: Consume data using an API](#consume_data_api)
* [Step 7: Connect IBM Cloud Monitoring](#connect_monitoring_api)
* [Step 8: Connect Activity Tracker](#activity_tracker_api)
* [Step 9: (Optional) Use Kafka Connect or ksqlDB](#kafka_connect_ksql)
* [Step 10: If you need more help](#getting_help)
{: api}


## Before you begin
{: #prereqs}

Before you get started, we highly recommend that you read the following information to better understand Apache Kafka, which {{site.data.keyword.messagehub}} is built on:
* [Apache Kafka concepts](/docs/EventStreams?topic=EventStreams-apache_kafka){: external}
* [Apache Kafka fundamentals](https://developer.ibm.com/articles/event-streams-kafka-fundamentals/?mhsrc=ibmsearch_a&mhq=event%20streams){: external}


## Step 1: Choose your plan 
{: #choose_plan}

{{site.data.keyword.messagehub}} offers three different plans. To help you decide which one best suits your needs, see [Choosing your plan](/docs/EventStreams?topic=EventStreams-plan_choose){: external}.

* The [Lite plan](/docs/EventStreams?topic=EventStreams-plan_choose#plan_lite){: external} offers access to a single partition in a multi-tenant {{site.data.keyword.messagehub}} cluster free of charge.	Use the Lite plan to try out {{site.data.keyword.messagehub}} or build a proof-of-concept. 

* The [Standard plan](/docs/EventStreams?topic=EventStreams-plan_choose#plan_standard){: external} offers pay-as-you-go shared access to the multi-tenant {{site.data.keyword.messagehub}} service. This service seamlessly autoscales as you increase the number of partitions you are using for your workload. The Standard plan has a limit of 100 partitions per instance.

* The [Enterprise plan](/docs/EventStreams?topic=EventStreams-plan_choose#plan_enterprise){: external} offers pay-as-you-go access to an isolated single-tenant Event Streams service. In addition to a selection of throughput and storage options, this plan also offers user-managed encryption private endpoints, schema registry support, and meets a higher number of regulatory compliance standards. The Enterprise plan is the best choice if data isolation, guaranteed performance, and increased retention are important considerations.


### Using APIs
{: #using_apis}
{: api}

You can use multiple APIs to work with {{site.data.keyword.messagehub}}. This tutorial uses the following APIs:

* The resource controller API to [provision an instance](#provision_instance_api) and to [retrieve an access token](#retrieve-token-api). 
* The Admin REST API to [work with topics](#work_topic_api). 
* The REST Producer API to [create a service credential](#create_credential_api) and [produce messages](#produce_data_api).


## Step 2: Provision an {{site.data.keyword.messagehub}} instance by using the console
{: #provision_instance_ui}
{: ui}

1. Log in to the {{site.data.keyword.cloud_notm}} console.
  
2. Click the [**{{site.data.keyword.messagehub}} service**](https://cloud.ibm.com/catalog/event-streams){: external} in the **Catalog**.
  
3. Select the **Lite plan**, **Standard plan**, or **Enterprise plan** from the **Select a pricing plan** section.
  
4. Enter a name for your service. You can use the default value.
  
5. Click **Create**. The {{site.data.keyword.messagehub}} **Resource list** page opens. 

6. When your instance has been created, click on the instance name to view more information.

7. Optional. You can complete the steps in the [Getting started tutorial](/docs/EventStreams?topic=EventStreams-getting-started#getting_started_steps){: external} to run a sample starter app. 


## Step 2: Provision an {{site.data.keyword.messagehub}} instance by using the CLI
{: #provision_instance_cli}
{: cli}

If it's the first time you've used the CLI, see [Getting started with the CLI](/docs/cli?topic=cli-getting-started){: external}.

To provision an instance of {{site.data.keyword.messagehub}} Standard Plan with the {{site.data.keyword.cloud_notm}} CLI, complete the following steps:

1. Install the {{site.data.keyword.Bluemix_notm}} CLI by completing the steps in [Getting started with the {{site.data.keyword.Bluemix_notm}} CLI](/docs/cli?topic=cli-getting-started#step1-install-idt){: external}.
{: #step1_install_cli_qsg}

    

2. Log in to {{site.data.keyword.Bluemix_notm}} by running the following command:
{: #step2_login_qsg}
    

    ```sh
    ibmcloud login -a cloud.ibm.com
    ```
    {: codeblock}

3. Create an {{site.data.keyword.messagehub}} instance on {{site.data.keyword.Bluemix_notm}} using the Lite, Standard, or Enterprise plans.
{: #step3_es_instance}
     
    Select one of the following methods:

    * To create an instance from the CLI on the Enterprise plan, run the following command:

        ```sh
        ibmcloud resource service-instance-create <INSTANCE_NAME> messagehub enterprise-3nodes-2tb <REGION>
        ```
        {: codeblock}
   
        Because the Enterprise plan has its own dedicated resources for each cluster, it requires more time for provisioning, so a new Enterprise instance might take up to 3 hours.


    * To create an instance from the CLI on the Standard plan, run the following command:

        ```sh
        ibmcloud resource service-instance-create <INSTANCE_NAME> messagehub standard <REGION>
        ```
        {: codeblock}

        Provisioning a new Standard plan instance is instantaneous because the underlying resources are already set up.


## Step 2: Provision an {{site.data.keyword.messagehub}} instance by using the resource controller API
{: #provision_instance_api}
{: api}

The preferred method to provision an instance is to use the [CLI](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=cli#provision_instance_cli). 

Alternatively, you can use the [resource controller API](/apidocs/resource-controller/resource-controller#create-resource-instance){: external}. First, [retrieve an access token](#retrieve-token-api) then run a resource controller API command with the access token to [create the instance](#create_instance_api). 

### Step 2a: Retrieve an access token with the resource controller API
{: #retrieve-token-api}

You can retrieve your access token programmatically by first creating a
[service ID API key](/docs/account?topic=account-serviceidapikeys){: external}
for your application, and then exchanging your API key for an
{{site.data.keyword.cloud_notm}} IAM token.

1. Log in to {{site.data.keyword.cloud_notm}} with the
    [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.

    ```sh
    ibmcloud login
    ```
    {: pre}

    If the login fails, run the `ibmcloud login --sso` command to try again. The
    `--sso` parameter is required when you log in with a federated ID. If this
    option is used, go to the link listed in the CLI output to generate a
    one-time passcode.
    {: note}

2. Select the account, region, and resource group that contain your provisioned
    instance of {{site.data.keyword.messagehub}}.

3. Create a
    [service ID](/docs/account?topic=account-serviceids){: external} for your application.

    ```sh
    ibmcloud iam service-id-create SERVICE_ID_NAME
                [-d, --description DESCRIPTION]
    ```
    {: pre}

4. Refer to [Managing access to resources](/docs/account?topic=account-assign-access-resources){: external}
    for information about the service ID.

    You can assign access permissions for your service ID
    [by using the {{site.data.keyword.cloud_notm}} console](/docs/account?topic=account-assign-access-resources#assign-new-access){: external}.
    To learn how the _Manager_, _Writer_, and _Reader_ access roles map to
    specific {{site.data.keyword.keymanagementserviceshort}} service actions, see y[Roles and permissions](/docs/key-protect?topic=key-protect-manage-access#manage-access-roles).
    {: tip}

    You can assign access permissions for your service ID
    [by using the {{site.data.keyword.cloud_notm}} console](/docs/account?topic=account-assign-access-resources#assign-new-access){: external}.
    To learn how the _Manager_, _Writer_, and _Reader_ access roles map to
    user access to
    {{site.data.keyword.messagehub}} resources, 
    see [What can I secure?](/docs/EventStreams?topic=EventStreams-security#what_secure)
    {: tip}

5. Create a
    [service ID API key](/docs/account?topic=account-serviceidapikeys){: external}.

    ```sh
    ibmcloud iam service-api-key-create API_KEY_NAME SERVICE_ID_NAME
                [-d, --description DESCRIPTION]
                [--file FILE_NAME]
    ```
    {: pre}

    Replace `<service_ID_name>` with the unique alias that you assigned to your
    service ID in the previous step. Save your API key by downloading it to a
    secure location.

6. Call the
    [IAM Identity Services API](/apidocs/iam-identity-token-api){: external}
    to retrieve your access token.

    ```sh
    $ curl -X POST \
        "https://iam.cloud.ibm.com/identity/token" \
        -H "content-type: application/x-www-form-urlencoded" \
        -H "accept: application/json" \
        -d 'grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>' > token.json
    ```
    {: codeblock}

    In the request, replace `<API_KEY>` with the API key that you created in the
    previous step. The following truncated example shows the contents of the
    `token.json` file:

    ```json
    {
        "access_token": "b3VyIGZhdGhlc...",
        "expiration": 1512161390,
        "expires_in": 3600,
        "refresh_token": "dGhpcyBjb250a...",
        "token_type": "Bearer"
    }
    ```
    {: screen}

    Use the full `access_token` value, prefixed by the _Bearer_ token type, to
    programmatically manage keys for your service using the
    {{site.data.keyword.keymanagementserviceshort}} API. To see an example
    {{site.data.keyword.keymanagementserviceshort}} API request, check out
    [Forming your API request](/docs/key-protect?topic=key-protect-set-up-api#form-api-request).

    Access tokens are valid for 1 hour, but you can regenerate them as needed.
    To maintain access to the service, regenerate the access token for your API
    key on a regular basis by calling the
    [IAM Identity Services API](/apidocs/iam-identity-token-api){: external}.
    {: note }

    - Use {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM)
        tokens to make authenticated requests to IBM Watson services without
        embedding service credentials in every call.

    - IAM authentication uses access tokens for authentication, which you acquire
        by sending a request with an API key.

### Step 2b: Create an instance
{: #create_instance_api}

Run a command like the following to create an Enterprise instance in US South:

```sh
curl -X POST https://resource-controller.cloud.ibm.com/v2/resource_instances -H "Authorization: ${token}" -H "Content-Type: application/json" \
-d '{ "name": "JG-test-curl", "target": "us-south", "resource_group":"9eba3cff1b0540b9ab7fb93829911da0", "resource_plan_id": "ibm.message.hub.enterprise.3nodes.2tb", "parameters":{"service-endpoints":"public","throughput":"150"}}'
```
{: codeblock}


## Step 3: Create a topic and select number of partitions by using the console
{: #create_topic_ui}
{: ui}

For guidance about the settings that you can modify when creating topics, see [topic configuration](/docs/EventStreams?topic=EventStreams-kafka_java_api){: external}.


1. From your newly provisioned instance, navigate to **Topics** using the menu on the left.
2. Click the **Create topic** button and an enter a topic name. Click **Next**. Topic names are restricted to a maximum of 200 characters.
3. Select the number of partitions. 

    One or more partitions make up a topic. A partition is an ordered list of messages. 1 partition is sufficient for getting started, but production systems often have more.

    Partitions are distributed across the brokers to increase the scalability of your topic. You can also use them to distribute messages across the members of a consumer group.

    Click **Next**.


4. Set the message retention period. This is how long messages are retained before they are deleted. If your messages are not read by a consumer within this time, they will be missed. The default retention period for messages is 24 hours. The minimum is 1 hour and the maximum is 30 days. Specify this value as multiples of hours.

    Click **Create topic**.

### Working with topics using the console
{: #work_topic_ui}
{: ui}

After you create topics, you can use the console to [list topics](#list_topic_ui).


#### List topics
{: #list_topic_ui}
{: ui}

From your {{site.data.keyword.messagehub}} instance, navigate to **Topics** from the menu on the left.

From the **Topics page**, you can view the following information about your topics: 
**Name**, **Partitions**, **Retention time**, **Retention size**, **Cleanup policy**, and **Stream landing**.


## Step 3: Create a topic and select number of partitions by using the CLI 
{: #create_topic_cli}
{: cli}

For guidance about the settings that you can modify when creating topics, see [topic configuration](/docs/EventStreams?topic=EventStreams-kafka_java_api){: external}.

Use the following [**ibmcloud es topic-create**](/docs/EventStreams?topic=EventStreams-cli_reference#ibmcloud_es){: external} command to create a new topic with your chosen number of partitions: 

```sh
ibmcloud es topic-create [--name] TOPIC_NAME [--partitions PARTITIONS] [--config KEY=VALUE[;KEY=VALUE]* ]*
```
{: codeblock}

**Prerequisites**: None

**Command options**:

--name value, -n value
:   Topic name. Topic names are restricted to a maximum of 200 characters.

--partitions value, -p value
:   Set the number of partitions for the topic.

    One or more partitions make up a topic. A partition is an ordered list of messages. 1 partition is sufficient for getting started, but production systems often have more.

    Partitions are distributed across the brokers to increase the scalability of your topic. You can also use them to distribute messages across the members of a consumer group.

--config KEY=VALUE, -c KEY=VALUE(optional)
:   Set a configuration option for the topic as a KEY=VALUE pair. 
:   You can specify multiple --config options. Each '--config' option can specify a semicolon-delimited list of assignments. The following list shows the valid configuration keys:
  
    - cleanup.policy
    - retention.ms
    - retention.bytes
    - segment.bytes
    - segment.ms
    - segment.index.bytes

The default retention period for messages as specified by the `retention.ms` key is 24 hours. The minimum is 1 hour and the maximum is 30 days. Specify this value as multiples of hours.


### Working with topics
{: #work_topic_cli}
{: cli}

After you create topics, you can use the CLI to [list topics](#ibmcloud_es_topic_list_cli) and [view details about your cluster](#ibmcloud_es_cluster_cli).


#### List a topic using the **ibmcloud es topics** command
{: #ibmcloud_es_topic_list_cli}

Run the **ibmcloud es topics** command to list your topics.

```bash
ibmcloud es topics [--filter FILTER] [--json]
```
{: codeblock}

**Prerequisites**: None

**Command options**:

--filter value, -f value (optional)
:   Topic name.
  
--json (optional)
:   Format output in JSON. Up to 1000 topics are returned.

#### Display cluster details using the **ibmcloud es cluster** command
{: #ibmcloud_es_cluster_cli}

Run the **ibmcloud es cluster** command to display the details of the cluster, including the Kafka version.

```bash
ibmcloud es cluster [--json]
```
{: codeblock}

**Prerequisites**: None

**Command options**:

--json (optional)
:   Output format in JSON.

For information about other {{site.data.keyword.messagehub}} CLI commands for topics, see [CLI reference](/docs/EventStreams?topic=EventStreams-cli_reference).

## Step 3: Create a topic and select number of partitions by using the Admin REST API
{: #create_topic_api}
{: api}

{{site.data.keyword.messagehub}} provides a REST API for administration that you can use to create and list topics.

You can create a Kafka topic by issuing a POST request to the `/admin/topics` path. The body of the request must contain a JSON document. For example:

```json
{
    "name": "topicname",
    "partitions": 1,
    "configs": {
        "retentionMs": 86400000,
        "cleanupPolicy": "delete"
    }
}
```
{: codeblock}


The JSON document must contain a `name` attribute, specifying the name of the Kafka topic to create. Topic names are restricted to a maximum of 200 characters.The JSON can also specify the number of partitions to assign to the topic (using the `partitions` property). If the number of partitions is not specified, the topic is created with a single partition.

One or more partitions make up a topic. A partition is an ordered list of messages. 1 partition is sufficient for getting started, but production systems often have more.

Partitions are distributed across the brokers to increase the scalability of your topic. You can also use them to distribute messages across the members of a consumer group.

You can also specify an optional `configs` object within the request. This allows you to specify the `retentionMs` property, which controls how long (in milliseconds) Kafka retains messages published to the topic. After this time elapses the messages are automatically deleted to free space. You must specify the value of the `retentionMs` property in a whole number of hours (for example, multiples of 3600000). The default retention period for messages is 24 hours. The minimum is 1 hour and the maximum is 30 days. 

For guidance about the settings that you can modify when creating topics, see [topic configuration](/docs/EventStreams?topic=EventStreams-kafka_java_api){: external}.

The expected HTTP status codes are as follows:

* 202: Topic creation request was accepted.
* 400: Invalid request JSON.
* 403: Not authorized to create topic.
* 422: Semantically invalid request.

If the request to create a Kafka topic succeeds, HTTP status code 202 (Accepted) is returned. If the operation fails, an HTTP status code of 422 (Unprocessable Entity) is returned, and a JSON object containing additional information about the failure is returned as the body of the response.

### Example
{: #create_topic_api_example}

You can exercise the REST endpoint for creating a Kafka topic using the following snippet of curl. You'll need to supply your own API key or token and specify the correct endpoint for ADMIN API. For more information about obtaining a key or a token, see [Retrieve an access token with the API](#retrieve-token-api).

```sh
curl -i -X POST -H 'Accept: application/json' -H 'Content-Type: application/json' -H 'Authorization: Bearer ${TOKEN}' --data '{ "name": "newtopic", "partitions": 1}' ${ADMIN_URL}/admin/topics
```
{: codeblock}


### Working with topics using the Admin REST API
{: #work_topic_api}
{: api}

After you create topics, you can use the Admin REST API to [list topics](#topic_list_api). For information about other topic-related commands you can run, see [Admin REST API methods](/apidocs/event-streams/adminrest#listtopics).

#### List topics
{: #topic_list_api}

You can list all your Kafka topics by issuing a GET request to the
`/admin/topics` path. 

The expected status code is:

* 200: The topic list is returned as JSON in the following format:

```json
[
  {
    "name": "topic1",
    "partitions": 1,
    "retentionMs": 86400000,
    "cleanupPolicy": "delete"
  },
  { "name": "topic2",
    "partitions": 2,
    "retentionMs": 86400000,
    "cleanupPolicy": "delete"
  }
]
```
{: codeblock}

A successful response will have HTTP status code 200 (OK) and contain an
array of JSON objects, where each object represents a Kafka topic and has the following properties:

| Property name     | Description                                             |
|-------------------|---------------------------------------------------------|
| name              | The name of the Kafka topic.                            |
| partitions        | The number of partitions assigned to the Kafka topic.   |
| retentionsMs      | The retention period for messages on the topic (in ms). |
| cleanupPolicy     | The cleanup policy of the Kafka topic.        |
{: caption="{{site.data.keyword.messagehub}} topic properties" caption-side="top"}               

##### List topics example
{: #topic_list_example_api}

You can use the following curl command to list all your Kafka topics:

```sh
curl -i -X GET -H 'Accept: application/json' -H 'Authorization: Bearer ${TOKEN}' ${ADMIN_URL}/admin/topics
```
{: codeblock}



## Step 4: Create a service credential by using the console
{: #create_credential_ui}
{: ui}


To allow you to connect to your {{site.data.keyword.messagehub}} instance, create a service key by using the {{site.data.keyword.Bluemix_notm}} console:

1. Locate your {{site.data.keyword.messagehub}} service in the **Resource list**.
2. Click your service tile.
3. Click **Service credentials**.
4. Click **New credential**. 
5. Complete the details for your new credential like a name and role and click **Add**. A new credential appears in the credentials list.
6. Expand the new credential's section to reveal the details in JSON format.
 

## Step 4: Create a service credential by using the CLI
{: #create_credential_cli}
{: cli}

Create a service key by using the {{site.data.keyword.Bluemix_notm}} CLI, so that you can connect to your {{site.data.keyword.messagehub}} instance:

1. Locate your service: 
    ```bash
    ibmcloud resource service-instances
    ```
    {: codeblock}

2. Create a service key: 
    ```bash
    ibmcloud resource service-key-create <key_name> <key_role> --instance-name <your_service_name>
    ```
    {: codeblock}

3. Print the service key: 
    ```bash
    ibmcloud resource service-key <key_name>
    ```
    {: codeblock}

    A single set of endpoint details are contained in each service key. For service instances configured to be connected to a single network type, either the {{site.data.keyword.Bluemix_notm}} Public network (the default) or the {{site.data.keyword.Bluemix_notm}} Private network, the service key contains the details relevant to that network type. For instances configured to support both the private and public networks, details for the public network are returned. If you want details for the private network, you must add the `--service-endpoint private` parameter to the previous **service-key-create** CLI command. For example: 
    {: note}

    ```bash
    ibmcloud resource service-key-create <private-key-name> <role> --instance-name <instance-name> --service-endpoint private
    ```
    {: codeblock}

## Step 4: Create a service credential by using the CLI and the REST producer API
{: #create_credential_api}
{: api}



To connect to your {{site.data.keyword.messagehub}} instance, the supported authentication mechanism is using a bearer token. To obtain your token by using the {{site.data.keyword.Bluemix_notm}} CLI, first log in to {{site.data.keyword.Bluemix_notm}} and then run the following command: 

```sh
ibmcloud iam oauth-tokens
```
{: codeblock}

Place this token in the Authorization header of the HTTP request in the form `Bearer<token>`. Both API key or JWT tokens are supported. 

## Step 5: Produce data using the console
{: #produce_data_ui}
{: ui}

You cannot produce data by using the console. You can produce data using the [command line](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=cli#produce_data_cli), the [REST Producer API](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=api#produce_data_api), or the [Kafka API](https://kafka.apache.org/documentation/#producerapi).

However, you can complete the steps for the console in the [Getting started tutorial](/docs/EventStreams?topic=EventStreams-getting-started#getting_started_steps){: external} to run a sample starter app and see messages flowing through a topic. 


## Step 5: Produce data using the command line
{: #produce_data_cli}
{: cli}

You can use the {{site.data.keyword.messagehub}} Kafka console producer tool to produce data. The console tools are in the `bin` directory of your Kafka client download, which you can download from [Apache Kafka downloads](http://kafka.apache.org/downloads){: external}. We recommend that you download the latest available stable binary version. Kafka client versions are backwards compatible with the version of Kafka on the server.

You must provide a list of brokers (using the BOOTSTRAP_ENDPOINTS property) and SASL credentials. 

To provide the SASL credentials to this tool, create a properties file based on the following example:

```config
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="<user>" password="<api_key>";
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    ssl.protocol=TLSv1.2
    ssl.enabled.protocols=TLSv1.2
    ssl.endpoint.identification.algorithm=HTTPS
```
{: codeblock}

Use the `<user>` field from the service key as the username and the `<api_key>` field from the service key as the password. You can find these values in the {{site.data.keyword.messagehub}} **Service Credentials** tab in the {{site.data.keyword.Bluemix_notm}} console.
 

{{site.data.keyword.messagehub}} provides example `producer.properties` and `consumer.properties` files for the [Java client](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-java-liberty-sample/resources){: external}.

After you create the properties file, you can run the console producer in a terminal as follows:

```bash
   kafka-console-producer.sh --broker-list BOOTSTRAP_ENDPOINTS --producer.config CONFIG_FILE --topic TOPIC_NAME
```
{: codeblock}

Replace the following variables in the example with your own values:

- BOOTSTRAP_ENDPOINTS with the value from your {{site.data.keyword.messagehub}} **Service Credentials** tab in the {{site.data.keyword.Bluemix_notm}} console.
- CONFIG_FILE with the path of the configuration file. 
* Use the `<bootstrap_endpoints>` field from the service key as the `bootstrap.servers` property of your Kafka application.
* Use the `<user>` field from the service key as the username and the `<api_key>` field from the service key as the password. Ensure that your application parses the details.

You can use many of the other options of this tool, except for those that require access to ZooKeeper. For more information, see [Using Kafka console tools with Event Streams](/docs/EventStreams?topic=EventStreams-kafka_console_tools){: external}.



### Producer configuration settings
{: #producer_config_cli}
{: cli}

For details of some of the most important settings that you can configure for the producer, see the following information:

* [General configuration settings](/docs/EventStreams?topic=EventStreams-producing_messages#config_settings){: external}
* [Delivery semantics](/docs/EventStreams?topic=EventStreams-producing_messages#delivery_semantics){: external}


## Step 5: Produce data using the REST Producer API
{: #produce_data_api}
{: api}



Use the v2 endpoint of the REST Producer API to send messages of type `text`, `binary`, `JSON`, or `avro` to topics. With the v2 endpoint you can use the {{site.data.keyword.messagehub}} schema registry by specifying the schema for the avro data type.

- What are headers? Are they optional?
You see headers in the message body. That’s the chunk of data that includes everything in the request or response. The headers usually come after the request line or response line.
Value of colour is base64 - how to do that?

The following code shows an example of sending a message of `text` type by using curl:

```sh
curl -v -X POST \
-H "Authorization: Bearer $token" -H "Content-Type: application/json" -H "Accept: application/json" \
-d '{
  "headers": [
    {
      "name": "colour",
      "value": "YmxhY2s="
    }
  ],
  "key": {
    "type": "text",
    "data": "Test Key"
  },
  "value": {
    "type": "text",
    "data": "Test Value"
  }
}' \
"$kafka_http_url/v2/topics/$topic_name/records"
```
{: codeblock}

For more information, see the [{{site.data.keyword.messagehub}} REST Producer v2 endpoint API reference](https://cloud.ibm.com/apidocs/event-streams/restproducer_v2){: external}.


### Producer configuration settings
{: #producer_config_api}
{: api}

For details of some of the most important settings that you can configure for the producer, see the following information:

* [General configuration settings](/docs/EventStreams?topic=EventStreams-producing_messages#config_settings){: external}
* [Delivery semantics](/docs/EventStreams?topic=EventStreams-producing_messages#delivery_semantics){: external}


## Step 6: Consume data using the console
{: #consume_data_ui}
{: ui}

You cannot consume data by using the console. You can consume data using the [command line](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=cli#consume_data_cli) or the [Kafka API](https://kafka.apache.org/documentation/#consumerapi).

However, you can complete the steps in the [Getting started tutorial](/docs/EventStreams?topic=EventStreams-getting-started#getting_started_steps){: external} to run a sample starter app and see messages flowing through a topic. 

## Step 6: Consume data using the command line
{: #consume_data_cli}
{: cli}

You can use the {{site.data.keyword.messagehub}} Kafka console consumer tool to consume data. 

The console tools are in the `bin` directory of your Kafka client download.

You must provide a list of brokers and SASL credentials. After you create the properties file as described in [produce data using the command line](#produce_data_cli), run the console consumer in a terminal as follows:

```bash
   kafka-console-consumer.sh --bootstrap-server BOOTSTRAP_ENDPOINTS --consumer.config CONFIG_FILE --topic TOPIC_NAME 
```
{: codeblock}

Replace the following variables in the example with your own values:

- BOOTSTRAP_ENDPOINTS with the value from your {{site.data.keyword.messagehub}} **Service Credentials** tab in the {{site.data.keyword.Bluemix_notm}} console. 
- CONFIG_FILE with the path of the configuration file. 

You can use many of the other options of this tool, except for those that require access to ZooKeeper. For more information, see [Using Kafka console tools with Event Streams](/docs/EventStreams?topic=EventStreams-kafka_console_tools).

### Consumer configuration settings
{: #consumer_config_cli}
{: cli}

For details of some of the most important settings that you can configure for the consumer, see the following information:

* [General configuration settings](/docs/EventStreams?topic=EventStreams-consuming_messages#configuring_consumer_properties){: external}
* [Consumer liveness](/docs/EventStreams?topic=EventStreams-consuming_messages#consumer_liveness){: external}
* [Consumer groups](/docs/EventStreams?topic=EventStreams-consuming_messages#consumer_groups){: external}
* [Delivery semantics](/docs/EventStreams?topic=EventStreams-producing_messages#delivery_semantics){: external}
* [Managing offsets](/docs/EventStreams?topic=EventStreams-consuming_messages#managing_offsets){: external}
* [Handling consumer rebalancing](/docs/EventStreams?topic=EventStreams-consuming_messages#consumer_rebalancing){: external}


## Step 6: Consume data using an API 
{: #consume_data_api}
{: api}

You cannot consume data using an {{site.data.keyword.messagehub}} API although consumption of data from Kafka is possible using the native Kafka libraries. For more information, see [Kafka consumer API](https://kafka.apache.org/documentation/#consumerapi).

As an alternative, use the [command line](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=cli#consume_data_cli).


## Step 7: Connect {{site.data.keyword.mon_full_notm}} for operational visibility by using the console 
{: #connect_monitoring_ui}
{: ui}

You can use {{site.data.keyword.mon_full_notm}} to get operational visibility into the performance and health of your applications, services, and platforms. {{site.data.keyword.mon_full_notm}} provides administrators, DevOps teams, and developers full stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards.

For more information about how to use {{site.data.keyword.monitoringshort}} with {{site.data.keyword.messagehub}}, see:
* [Opting in to metrics](/docs/EventStreams?topic=EventStreams-metrics#opt_in_metrics){: external}  
* [Enabling default metrics](/docs/EventStreams?topic=EventStreams-metrics#enabling_default_metrics){: external}
* [Enabling enhanced metrics](/docs/EventStreams?topic=EventStreams-metrics#opt_in_enhanced_metrics){: external}
* [Viewing details of available metrics](/docs/EventStreams?topic=EventStreams-metrics#metric_details){: external}
* [Understanding metrics cost information](/docs/EventStreams?topic=EventStreams-metrics#metric_costs){: external}


## Step 7: Connect {{site.data.keyword.mon_full_notm}} for operational visibility by using the CLI or command line
{: #connect_monitoring_cli}
{: cli}

You cannot connect {{site.data.keyword.mon_full_notm}} by using the CLI or command line. Use the [console](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=ui#connect_monitoring_ui) to complete this task.


## Step 7: Connect {{site.data.keyword.mon_full_notm}} for operational visibility by using an API
{: #connect_monitoring_api}
{: api}

You cannot connect {{site.data.keyword.mon_full_notm}} by using an API. Use the [console](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=ui#connect_monitoring_ui) to complete this task.


## Step 8: Connect {{site.data.keyword.at_full}} to audit service activity 
{: #activity_tracker_ui}
{: ui}

{{site.data.keyword.at_full_notm}} allows you to view, manage, and audit service activity to comply with corporate policies and industry regulations. {{site.data.keyword.at_short}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. Use {{site.data.keyword.at_short}} to track how users and applications interact with the {{site.data.keyword.messagehub}} service on the Standard and Enterprise plans.

To get up and running with {{site.data.keyword.at_short}}, see [Getting Started with {{site.data.keyword.at_short}}](/docs/activity-tracker?topic=activity-tracker-getting-started#gs_objectives){: external}.

{{site.data.keyword.at_short}} can have only one instance per location. To view events, you must access the web UI of the {{site.data.keyword.at_short}} service in the same location where your service instance is available. For more information, see [Launch the web UI](/docs/activity-tracker?topic=activity-tracker-getting-started#gs_step4){: external}.

For more information about events specific to {{site.data.keyword.messagehub}}, see:

* [Where to view events](/docs/EventStreams?topic=EventStreams-at_events#ui){: external}
* [Topic events](/docs/EventStreams?topic=EventStreams-at_events#topic-events){: external}
* [Message audit events](/docs/EventStreams?topic=EventStreams-at_events#message-events){: external}
* [Other events](/docs/EventStreams?topic=EventStreams-at_events#other-events){: external}

Events are formatted according to the Cloud Auditing Data Federation (CADF) standard. For further details of the information they include, see [CADF standard](/docs/activity-tracker?topic=activity-tracker-about#cadf_standard){: external}.


## Step 8: Connect {{site.data.keyword.at_full}} using the CLI or command line to audit service activity
{: #activity_tracker_cli}
{: cli}

You cannot connect {{site.data.keyword.at_short}} using the CLI or command line. Use the [console](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=ui#activity_tracker_ui) to complete this task.

## Step 8: Connect {{site.data.keyword.at_full}} using an API to audit service activity
{: #activity_tracker_api}
{: api}

You cannot connect {{site.data.keyword.at_short}} using an API. Use the [console](/docs/EventStreams?topic=EventStreams-quick_setup_guide&interface=ui#activity_tracker_ui) to complete this task.


## Step 9: (Optional) Use Kafka Connect or ksqlDB
{: #kafka_connect_ksql}

### Kafka Connect
{: #kafka_connect}

Kafka Connect is part of the Apache Kafka project and allows you to connect external systems to Kafka. It consists of a runtime  that can run connectors to copy data to and from a cluster.

For more information, see [Using Kafka Connect with Event Streams](/docs/EventStreams?topic=EventStreams-kafka_connect){: external}.

Kafka Connect is not part of the managed {{site.data.keyword.messagehub}} service.

### ksqlDB
{: #ksqldb}

You can use [KSQL](https://github.com/confluentinc/ksql){: external} with the {{site.data.keyword.messagehub}} Enterprise plan for stream processing. 
{: shortdesc}

ksqlDB is a purpose-built database for event streaming. Use it to build end-to-end event streaming applications quickly with a purpose-built stream processing database for Apache Kafka.

First complete these [setup steps](/docs/EventStreams?topic=EventStreams-ksql_using##kqsldbsteps){: external}. Then the quickest and easiest way to run ksqlDB with {{site.data.keyword.messagehub}} is to use a docker container as described in [ksqlDB quickstart](https://ksqldb.io/quickstart.html){: external}. 


## Step 10: Get help
{: #getting_help}

For a general overview of how to get help with {{site.data.keyword.messagehub}} and where to get support, see [Getting help and support](/docs/EventStreams?topic=EventStreams-gettinghelp){: external}.

[FAQs](/docs/EventStreams?topic=EventStreams-faqs){: external} details answers to some of the common questions about {{site.data.keyword.messagehub}}.

If you're experiencing a problem with {{site.data.keyword.messagehub}}, here's a list of the information you need to gather before you open a case [Reporting a problem to the Event Streams team - Standard and Enterprise plans](/docs/EventStreams?topic=EventStreams-report_problem_enterprise){: external}.

