---

copyright:
  years: 2024, 2025
lastupdated: "2025-11-20"

keywords: quick setup guide

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}
{:ui: .ph data-hd-interface="ui"}

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

## Before you begin
{: #prereqs}

Before you get started, we highly recommend that you read the following information to better understand Apache Kafka, which {{site.data.keyword.messagehub}} is built on:
* [Apache Kafka concepts](/docs/EventStreams?topic=EventStreams-apache_kafka){: external}
* [Apache Kafka fundamentals](https://developer.ibm.com/articles/event-streams-kafka-fundamentals/?mhsrc=ibmsearch_a&mhq=event%20streams){: external}


## Step 1: Select your plan 
{: #choose_plan}

{{site.data.keyword.messagehub}} offers different plans according to the generation of the platform, see [Choosing your plan](/docs/EventStreams?topic=EventStreams-plan_choose){: external}. For the Gen2 platform the following plan can be selected.

* The [Enterprise Gen2 plan](/docs/EventStreams?topic=EventStreams-plan_choose#plan_enterprise-gen2){: external} offers pay-as-you-go access to an isolated single-tenant {{site.data.keyword.messagehub}} service running on the Gen2 (VPC) platform. In addition to a selection of throughput and storage options, this plan also offers user-managed encryption private endpoints and meets a higher number of regulatory compliance standards. The Enterprise Gen2 plan is the best choice if data isolation, scalability, security, guaranteed performance, and increased retention are important considerations.


## Step 2: Provision an {{site.data.keyword.messagehub}} instance by using the console
{: #provision_instance_ui}
{: ui}

1. Log in to the {{site.data.keyword.cloud_notm}} console.
  
2. Click the [**{{site.data.keyword.messagehub}} service**](https://cloud.ibm.com/catalog/event-streams){: external} in the **Catalog**.

3. Select a Location that supports the Gen2 platform.
     
4. Select the **Enterprise Gen2 plan** from the **Select a pricing plan** section.
  
5. Enter a name for your service. You can use the default value.
  
6. Click **Create**. The {{site.data.keyword.messagehub}} **Resource list** page opens. 

7. When your instance has been created, click on the instance name to view more information.


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
 

## Step 5: Produce data using the console
{: #produce_data_ui}
{: ui}

You cannot produce data by using the console. You can produce data using the...

<TODO>


## Step 6: Consume data using the console
{: #consume_data_ui}
{: ui}

You cannot consume data by using the console. You can consume data using the...

<TODO>


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
