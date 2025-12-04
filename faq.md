---

copyright:
  years: 2025
lastupdated: "2025-12-04"

keywords: api, frequently asked questions, consumer group, log retention, message size, replication settings

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# FAQs about {{site.data.keyword.messagehub}}
{: #faqs}

[Gen 2]{: tag-purple}

{{site.data.keyword.messagehub_full}} Gen 2 is currently in Beta. The Beta plan is provided exclusively for evaluation and testing purposes. It is not covered by warranties, SLAs, or support, and is not intended for production use. For more information, see the [Beta reference](/docs/EventStreams-gen2?topic=EventStreams-gen2-gen2-beta).
{: beta}

This document contains information about common questions or problems encountered by users of the {{site.data.keyword.messagehub_full}} service. It aims to answer questions or provide instruction on how to resolve issues without the need to raise a support ticket.
{: shortdesc}

## How do I use Kafka APIs to create and delete topics?
{: #topic_admin}
{: faq}
{: support}

If you're using a Kafka client at 0.11 or later, or Kafka Streams at 0.10.2.0 or later, you can use APIs to create and delete topics. We've put some restrictions on the settings allowed when you create topics. Currently, you can modify the following settings only:

cleanup.policy
:   Set to `delete` (default), `compact` or `delete,compact`

retention.ms
:   The default retention period is 24 hours. The minimum is 1 hour and the maximum is 30 days. Specify this value as multiples of hours.

    **Note:**
    In the Enterprise plan, you can set this to any value.

retention.bytes
:   The maximum size a partition (which consists of log segments) can grow to before we discard old log segments to free up space.

    **Note:**
    Enterprise: Set to any value between 100 KiB and 2 TiB.
    Standard: Set to any value between 100 KiB and 1 GiB.

segment.bytes
:   The segment file size for the log.

    **Note:**
    Enterprise: Set to any value between 100 KiB and 2 TiB.
    Standard: Set to any value between 100 KiB and 512 MiB.

segment.index.bytes
:   The size of the index that maps offsets to file positions. 

    **Note:**
    Enterprise: Set to any value between 100 KiB and 1 TiB.
    Standard: Set to any value between 100 KiB and 100 MiB.

segment.ms
:   The period of time after which Kafka will force the log to roll even if the segment file isn't full. 

    **Note:**
    Set to any value between 5 minutes and 30 days.
    
 See the following example of default value settings.
 
```bash
 Details for topic testit
Topic name   Internal?   Partition count   Replication factor   
testit       false       1                 3   

Partition details for topic testit
Partition ID   Leader   Replicas   In-sync   
0              1        [1 5 0]    [1 5 0]   

Configuration parameters for topic testit
Name                  Value   
cleanup.policy        delete   
min.insync.replicas   2   
segment.bytes         536870912   
retention.ms          86400000   
segment.ms            604800000   
retention.bytes       1073741824   
segment.index.bytes   10485760  
```
{: codeblock}


## How long does {{site.data.keyword.messagehub}} set the log retention window for the consumer offsets topic?
{: #offsets }
{: faq}
{: support}

{{site.data.keyword.messagehub}} retains consumer offsets for 7 days. This corresponds to the Kafka configuration offsets.retention.minutes. 

Offset retention is system-wide so you cannot set it at an individual topic level. All consumer groups get only 7 days of stored offsets even if using a topic with a log retention that has been increased to the maximum of 30 days. 

The internal Kafka `__consumer_offsets` topic is visible to you as read-only on the Enterpise plan. 
You are strongly recommended not to attempt to manage the topic in any way. You cannot access the `__consumer_offsets` topic in any way on the Standard plan. 


## How can I clean up a consumer group with no consumers?
{: #clean_consumer_group}
{: faq}
{: support}

After consumers have left, a group continues to exist only if it has offsets. Consumer offsets are deleted after 7 days of inactivity. Consequently, a consumer group is deleted when the last committed offset for that group expires.

If you want to explicitly delete a group at a time you choose, you can use the 
[deleteConsumerGroups() API](http://kafka.apache.org/23/javadoc/org/apache/kafka/clients/admin/AdminClient.html#deleteConsumerGroups-java.util.Collection){: external}, or the [ibmcloud es group-delete command](/docs/EventStreams?topic=EventStreams-cli_reference#ibmcloud_es_group_delete).


## How long are messages retained?
{: #messages_retained}
{: faq}
{: support}

By default, messages are retained in Kafka for up to 24 hours and
each partition is capped at 1 GB. If the 1 GB cap is reached, the
oldest messages are discarded to stay within the limit.

You can change the time limit for message retention when you
create a topic using either the user interface or the
administration API. The time limit is a minimum of an hour and a
maximum of 30 days.

For information about restrictions on the settings allowed when you create topics using a Kafka client or Kafka Streams, see [How do I use Kafka APIs to create and delete topics?](/docs/EventStreams?topic=EventStreams-faqs#topic_admin)

## What is {{site.data.keyword.messagehub}}'s availability behavior?
{: #availability}
{: faq}
{: support}

If you write {{site.data.keyword.messagehub}} apps, use this information to understand what normal {{site.data.keyword.messagehub}} availability behavior is and what your apps are expected to handle.

### APIs
{: #api_availability }

As part of the regular operation of {{site.data.keyword.messagehub}}, the nodes of the Kafka clusters are occasionally restarted.
In some cases, your apps will be aware as the cluster reassigns resources. Write your apps to be resilient
to these changes and to be able to reconnect and retry operations.

## What is {{site.data.keyword.messagehub}}'s maximum message size? 
{: #max_message_size }
{: faq}

{{site.data.keyword.messagehub}}'s maximum message size is 1 MB, which is the Kafka default. 

## What are {{site.data.keyword.messagehub}}'s replication settings? 
{: #replication }
{: faq}

{{site.data.keyword.messagehub}} is configured to provide strong availability and durability.
The following configuration settings apply to all topics and cannot be changed:
* replication.factor = 3 
* min.insync.replicas = 2

## What are the restrictions and defaults for topics and partitions?
{: #topics_partitions_defaults}
{: faq}
{: support}

*  Topic names are restricted to a maximum of 200 characters.
*  The default number of partitions for a topic is one.
*  Each {{site.data.keyword.Bluemix_notm}} space has a limit of 100 partitions. To create
   more partitions, you must use a new {{site.data.keyword.Bluemix_notm}} space.

## How do I check which {{site.data.keyword.messagehub}} plan I've provisioned?
{: #plan_check}
{: faq}
{: support}

To confirm which type of {{site.data.keyword.messagehub}} plan you've provisioned (Lite, Standard, or Enterprise), complete the following steps:
1. In the {{site.data.keyword.Bluemix_notm}} console, navigate to the instance of {{site.data.keyword.messagehub}} that you want to check.
2. Click the **Plan** tab in the navigation pane on the left.
   The **Current plan** section displays your plan type. 

## Can I change my {{site.data.keyword.messagehub}} plan using the {{site.data.keyword.Bluemix_notm}} console?
{: #plan_migrate}
{: faq}

Yes, but only if you are moving from the Lite plan to the Standard plan.

1. In the {{site.data.keyword.Bluemix_notm}} console, navigate to the instance of {{site.data.keyword.messagehub}} Lite plan that you want to change. 
2. Click the **Plan** tab in the navigation pane on the left.
3. In the **Change pricing plan** section, check the **Standard** box. Click **Upgrade**.

   Allow a few minutes for the cached limit of 1 partition for the Lite plan to clear so that you can take advantage of the 100 partition limit for the Standard plan.

   However, this option does not currently work in the {{site.data.keyword.Bluemix_notm}} console for any other combination of plans. For example, if you try a different plan combination, you'll see an error message like the following:
   ```text
   Could not find VCAP::CloudController::ServicePlan with guid: ibm.eventstreams.standard 
   ```
   {: codeblock}


## What are the differences between the {{site.data.keyword.messagehub}} Standard and {{site.data.keyword.messagehub}} Enterprise plans?
{: #plan_compare }
{: faq}

To find out more information about the different {{site.data.keyword.messagehub}} plans, see [Choosing your plan](/docs/EventStreams?topic=EventStreams-plan_choose).

## How do I handle disaster recovery?
{: #disaster_recovery }
{: faq}
{: support}

Currently, it is the responsibility of the user to manage their own {{site.data.keyword.messagehub}} disaster recovery. {{site.data.keyword.messagehub}} data can be replicated between an {{site.data.keyword.messagehub}} instance in one location (region) and another instance in a different location. However, the user is responsible for provisioning a remote {{site.data.keyword.messagehub}} instance and managing the replication. 

We suggest a tool like Kafka MirrorMaker to replicate data between clusters. For information about how to run MirrorMaker, see 
[{{site.data.keyword.messagehub}} kafka-mirrormaker repository](https://github.com/ibm-messaging/event-streams-samples/tree/master/kafka-mirrormaker){: external}. For an example of the recovery process, see [Using mirroring in a disaster recovery scenario](/docs/EventStreams?topic=EventStreams-disaster_recovery_scenario) .

The user is also responsible for the backup of message payload data. Although this data is replicated across multiple Kafka brokers within a cluster, which protects against the majority of failures, this replication does not cover a location-wide failure. It is recommended good practice for users to back up topic names and the configuration data for those topics.

If you have configured your {{site.data.keyword.messagehub}} instance in a Multi-Zone Region, a regional disaster is very unlikely. However, we recommend that users do plan for such circumstances. If a user's instance is no longer available because of a disaster (and a remote DR instance is not already set up), the user should consider configuring a new instance in a new region and restoring their topics and data from backup if available. Applications can then be pointed at the new instance.
