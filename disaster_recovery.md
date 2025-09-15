---

copyright:
  years: 2015, 2023
lastupdated: "2023-12-14"

keywords: replication, failover, scenario, disaster recovery, mirroring, failing over, failback, source cluster, mirroring

subcollection: EventStreams

---

{{site.data.keyword.attribute-definition-list}}

# Using mirroring in a disaster recovery example scenario 
{: #disaster_recovery_scenario}

This end-to-end disaster recovery scenario demonstrates how to use mirroring to provide increased availability and keep applications working should a major incident affects a full IBM Cloud region.
{: shortdesc}

Two clusters were provisioned in different regions and configured for mirroring (following the information in [Enabling mirroring](/docs/EventStreams?topic=EventStreams-mirroring_setup)) by using A and B as cluster aliases. A producer publishes records to a topic called `accounting.invoices` and a consumer reads the messages from that topic in cluster A.

![Mirroring overview diagram.](disaster1.png "Diagram that shows a producer publishing messages to a topic. The consumer reads the messages from that topic."){: caption="Mirroring overview" caption-side="bottom"}

## Source cluster becomes unavailable 
{: #source_cluster_unavailable}

Let's consider what happens if a disaster occurs on the source cluster's region.

![Disaster on source cluster diagram.](disaster2.png "Diagram showing a disaster occurring in the source cluster's region."){: caption="Disaster in source cluster's region" caption-side="bottom"}

It is the responsibility of the {{site.data.keyword.messagehub}} instance owner to determine whether the impact of the event is such as to declare a disaster. The service instance owner must coordinate the failover of applications including their reconfiguration, redeployment, and restart if necessary.

## Failing over producers 
{: #fail_over_producers}

Perform the following steps to fail over:

1. Stop the producers that were pointing to cluster A.
2. If cluster A and the link from A to cluster B is still operational, ensure that as much data as possible is mirrored by checking that the lag on those topics on cluster B is zero.
3. Restart the producers to point to cluster B's endpoints.
4. Disable any mirroring that is still enabled on topics from cluster A to cluster B. This can be done by using the [User controls](/docs/EventStreams?topic=EventStreams-mirroring#user_controls).

![Producer on target cluster B overview diagram.](disaster3.png "Diagram that shows the producer switched to cluster B and sending messages to a new local topic"){: caption="Producer switched to cluster B." caption-side="bottom"}

The producer is now switched to cluster B and sends messages to a new local topic with the same name as the original.

## Failing over consumers
{: #fail_over_consumers}

Perform the following steps to fail over:

1. If cluster A and the link from A to cluster B is still operational, allow the consumers to read all the message data in the topics on cluster A, and commit their offsets at the end of the topic.
2. Stop the consumers that were pointing to cluster A.
3. Restart the consumers to point to cluster B's endpoints.

![Consumer on cluster B diagram.](disaster4.png "Diagram that shows the consumer continuing to consume the existing messages."){: caption="The consumer continues to consume the existing messages." caption-side="bottom"}

The consumer is now able to continue to consume the existing messages from the `accounting.invoices.A` topic from cluster B while new messages come from `accounting.invoices`.

If the application requires strict ordering, remote topics are to be fully consumed first before starting to consume from local topics. This way, messages are processed in the order that they were produced.
{:note: .note}

## Resetting a mirroring environment
{: #reset_mirroring}

The {{site.data.keyword.messagehub}} service instance owner is responsible for deciding what happens when cluster A is recovered. 

In case cluster A is not recoverable, the {{site.data.keyword.messagehub}} service instance owner is responsible for enabling mirroring between cluster B and a newly provisioned instance. To enable mirroring to a new instance, complete the following steps:

- Fail over the producers and consumers from A to B as described previously.
- Disable the current mirroring from cluster A to cluster B. For more information, see [Disabling mirroring](/docs/EventStreams?topic=EventStreams-mirroring_disable).
- After mirroring has been disabled, enable mirroring between cluster B (now the source) to the newly provisioned cluster (the target). For more information, see [Enabling mirroring](/docs/EventStreams?topic=EventStreams-mirroring_setup).

Alternatively, if cluster A has recovered, typically a user returns operations to cluster A. Complete the following steps to return primary operations to cluster A.

Before failing back, mirroring must be enabled in the opposite direction:

- Ensure that cluster A is fully operational.
- Disable the current mirroring from cluster A to cluster B. For more information, see [Disabling mirroring](/docs/EventStreams?topic=EventStreams-mirroring_disable).
- After mirroring has been disabled, enable mirroring between cluster B (now the source) and cluster A (now the target). For more information, see [Enabling mirroring](/docs/EventStreams?topic=EventStreams-mirroring_setup).
- The source cluster A now becomes the target cluster.
- The target cluster B becomes the new source cluster.
- Enable any topics to be mirrored from cluster B to cluster A. You can do this by using the [User controls](/docs/EventStreams?topic=EventStreams-mirroring#user_controls).
  
Next, make sure that data is being replicated into cluster A by examining the topics from cluster B appearing on cluster A. These topics have the suffix from the new source cluster, B.

![Mirroring enabled in opposite direction diagram.](disaster5.png "Diagram that shows mirroring is now enabled in the opposite direction."){: caption="Mirroring enabled in opposite direction." caption-side="bottom"}

Do not mirror back the original target topic on cluster B as that would cause an undesirable cyclic effect. As shown in the diagram, we mirror accounting.invoices from cluster B to cluster A, not accounting.invoices.A. 
{:note: .note}

## Failback
{: #failback_decision}

The decision to fail back is again owned by the {{site.data.keyword.messagehub}} instance owner. The service instance owner must coordinate the failback of applications including their reconfiguration, redeployment, and restart if necessary.

Unlike the failover case, in this case there was no disaster on cluster B. Therefore, failback is a controlled operation and can be achieved with minimal data loss or reprocessing of data.

![Mirroring switched back to the original configuration diagram.](disaster6.png "Diagram that shows mirroring has now switched back to the original configuration."){: caption="Mirroring switched back to the original configuration." caption-side="bottom"}

Finally, switch the mirroring back to the original configuration, which means that cluster A is again the source and cluster B resumes as the target. 
- Ensure that cluster A and cluster B are fully operational.
- Disable the current mirroring from cluster B to cluster A. For more information, see [Disabling mirroring](/docs/EventStreams?topic=EventStreams-mirroring_disable).
- After mirroring has been disabled, enable mirroring between cluster A (now the source) and cluster B (now the target). For more information, see [Enabling mirroring](/docs/EventStreams?topic=EventStreams-mirroring_setup).
