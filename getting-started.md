---

copyright:
  years: 2026
lastupdated: "2026-11-09"

keywords: getting started

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}} Gen 2
{: #getting-started}

[Gen 2]{: tag-purple}

Learn how to quickly start using {{site.data.keyword.messagehub}} Gen 2 by provisioning an instance, creating a topic and credential, and producing and consuming data.
{: shortdesc}

Follow these steps to complete the getting started tutorial:

1. [Before you begin](#prereqs)
1. [Step 1: Select your plan](#choose_plan)
1. [Step 2: Provision an {{site.data.keyword.messagehub}} instance by using the console](/docs/EventStreams-gen2?topic=EventStreams-gen2-getting-started&interface=ui#provision_instance_ui)
1. [Step 3: Create a topic and partitions by using the console](/docs/EventStreams-gen2?topic=EventStreams-gen2-getting-started&interface=ui#create_topic_ui)
1. [Step 4: Create a service credential by using the console](/docs/EventStreams-gen2?topic=EventStreams-gen2-getting-started&interface=ui#create_credential_ui)
1. [Step 5: Produce and consume data](/docs/EventStreams-gen2?topic=EventStreams-gen2-getting-started&interface=ui#produce_data_ui)
1. [Step 6: Connect {{site.data.keyword.monitoringshort}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-getting-started&interface=ui#connect_monitoring_ui)
1. [Step 7: Connect Activity Tracker](/docs/EventStreams-gen2?topic=EventStreams-gen2-getting-started&interface=ui#activity_tracker_ui)
1. [Step 8: (Optional) Use Kafka Connect or ksqlDB](#kafka_connect_ksql)
1. [Step 9: Get help](#getting_help)

## Before you begin
{: #prereqs}

Before you begin, review the following resources to learn more about Apache Kafka:

* [Apache Kafka concepts](/docs/EventStreams-gen2?topic=EventStreams-gen2-apache_kafka)
* [Apache Kafka fundamentals](https://developer.ibm.com/articles/event-streams-kafka-fundamentals/?mhsrc=ibmsearch_a&mhq=event%20streams){: external}


## Step 1: Select your plan
{: #choose_plan}

{{site.data.keyword.messagehub}} offers plans based on platform generation. For more information, see [Choosing your plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose).

The [Enterprise Gen 2 plan](/docs/EventStreams-gen2?topic=EventStreams-gen2-plan_choose) provides dedicated single-tenant {{site.data.keyword.messagehub}} service instances on the VPC platform. In addition to scalable throughput and storage options, this plan offers customer-managed encryption and private endpoints. The Enterprise Gen 2 plan is designed for workloads where data isolation, security, performance, and extended retention are essential.


## Step 2: Provision an {{site.data.keyword.messagehub}} instance by using the console
{: #provision_instance_ui}
{: ui}

1. Log in to the {{site.data.keyword.cloud_notm}} console.
1. Search for **{{site.data.keyword.messagehub}}** in the [**Catalog**](https://cloud.ibm.com/catalog/){: external} and click the **{{site.data.keyword.messagehub}}** tile.
1. Select a location that supports the Gen 2 platform.
1. Select the **Enterprise Gen 2 plan** from the **Select a pricing plan** section.
1. Enter a name for your service. You can use the default value.
1. Click **Create**. The {{site.data.keyword.messagehub}} **Resource list** page opens.
1. When your instance has been created, click the instance name to view more information.


## Step 3: Create a topic and select the number of partitions by using the console
{: #create_topic_ui}
{: ui}

For guidance about the settings that you can modify when creating topics, see [Topic configuration](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_java_api).


1. In the navigation menu of your instance, select **Topics**.
1. Click **Create topic**, enter a topic name, and click **Next**. Topic names are restricted to a maximum of 200 characters.
1. Select the number of partitions:
   - One or more partitions make up a topic. A partition is an ordered list of messages. One partition is sufficient for testing, while production environments typically use multiple partitions.
   - Partitions are distributed across brokers to increase scalability and distribute messages across consumer group members.
   - Click **Next**.
1. Set the message retention period:
   - The default retention period is 24 hours. The minimum is one hour and the maximum is 30 days.
   - Specify this value in multiples of hours.
   - Click **Create topic**.

### Working with topics by using the console
{: #work_topic_ui}
{: ui}

After you create topics, you can use the console to list and manage them.

#### Listing topics
{: #list_topic_ui}
{: ui}

In the navigation menu of your {{site.data.keyword.messagehub}} instance, select **Topics**.

From the **Topics** page, you can view the following information about your topics:

* **Name**
* **Partitions**
* **Retention time**
* **Retention size**
* **Cleanup policy**

## Step 4: Create a service credential by using the console
{: #create_credential_ui}
{: ui}

To connect to your {{site.data.keyword.messagehub}} instance, create a service key by using the {{site.data.keyword.cloud_notm}} console:

1. Locate your {{site.data.keyword.messagehub}} service in the **Resource list**.
2. Click your service tile.
3. In the navigation menu, select **Service credentials**.
4. Click **New credential**.
5. Specify a name and select an IAM role, then click **Add**. The new credential appears in the credentials table.
6. Expand the credential row to view the connection details in JSON format.

## Step 5: Produce and consume data
{: #produce_data_ui}
{: ui}

To connect to {{site.data.keyword.messagehub}}, create a virtual private endpoint (VPE) for VPC to establish private network connectivity.

Follow the instructions in [Connecting to {{site.data.keyword.messagehub}}](/docs/EventStreams-gen2?topic=EventStreams-gen2-connecting) to create a VPE and run producer and consumer applications to send and receive data.

## Step 6: Connect {{site.data.keyword.monitoringlong_notm}} for operational visibility by using the console
{: #connect_monitoring_ui}
{: ui}

You can use {{site.data.keyword.mon_full_notm}} to get operational visibility into the performance and health of your applications, services, and platforms. {{site.data.keyword.mon_full_notm}} provides administrators, DevOps teams, and developers full stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards.

For more information about using {{site.data.keyword.monitoringshort}} with {{site.data.keyword.messagehub}}, see the following topics:

* [Opting in to metrics](/docs/EventStreams-gen2?topic=EventStreams-gen2-metrics#opt_in_metrics)
* [Enabling default metrics](/docs/EventStreams-gen2?topic=EventStreams-gen2-metrics#enabling_default_metrics)
* [Enabling enhanced metrics](/docs/EventStreams-gen2?topic=EventStreams-gen2-metrics#opt_in_enhanced_metrics)
* [Viewing details of available metrics](/docs/EventStreams-gen2?topic=EventStreams-gen2-metrics#metric_details)
* [Understanding metrics cost information](/docs/EventStreams-gen2?topic=EventStreams-gen2-metrics#metric_costs)


## Step 7: Connect {{site.data.keyword.at_full}} to audit service activity
{: #activity_tracker_ui}
{: ui}

You can use {{site.data.keyword.at_full_notm}} to view, manage, and audit service activity to comply with corporate policies and industry regulations. {{site.data.keyword.at_short}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. Use {{site.data.keyword.at_short}} to track how users and applications interact with {{site.data.keyword.messagehub}}.

To get started with {{site.data.keyword.at_short}}, see [Getting started with Activity Tracker](/docs/cloud-logs?topic=cloud-logs-getting-started).

For more information about events specific to {{site.data.keyword.messagehub}}, see the following topics:

* [Where to view events](/docs/EventStreams-gen2?topic=EventStreams-gen2-at_events#ui)
* [Topic events](/docs/EventStreams-gen2?topic=EventStreams-gen2-at_events#topic-events)
* [Message audit events](/docs/EventStreams-gen2?topic=EventStreams-gen2-at_events#message-events)
* [Other events](/docs/EventStreams-gen2?topic=EventStreams-gen2-at_events#other-events)

Events are formatted according to the Cloud Auditing Data Federation (CADF) standard.

## Step 8: (Optional) Use Kafka Connect or ksqlDB
{: #kafka_connect_ksql}

### Kafka Connect
{: #kafka_connect}

Kafka Connect is part of the Apache Kafka open-source project and connects external systems to Kafka. It provides a runtime that runs connectors to copy data to and from a cluster.

For more information, see [Using Kafka Connect with Event Streams](/docs/EventStreams-gen2?topic=EventStreams-gen2-kafka_connect).

Kafka Connect is not part of the managed {{site.data.keyword.messagehub}} service.

### ksqlDB
{: #ksqldb}

You can use [ksqlDB](https://github.com/confluentinc/ksql){: external} with the {{site.data.keyword.messagehub}} Enterprise plan for stream processing.

ksqlDB is a specialized database for event streaming that enables you to build stream processing applications for Apache Kafka.

To configure ksqlDB, first complete the [ksqlDB setup steps](/docs/EventStreams-gen2?topic=EventStreams-gen2-ksql_using). You can run ksqlDB by using a Docker container as described in the [ksqlDB quickstart](https://www.confluent.io/product/ksqldb/){: external}.


## Step 9: Get help
{: #getting_help}

For an overview of support options for {{site.data.keyword.messagehub}}, see [Getting help and support](/docs/EventStreams-gen2?topic=EventStreams-gen2-gettinghelp).

To review common questions and answers, see the [FAQs](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-faq).

If you encounter an issue, gather the required diagnostic details and follow the instructions in [Reporting a problem to the {{site.data.keyword.messagehub}} team](/docs/EventStreams-gen2?topic=EventStreams-gen2-report_problem_enterprise).
