---

copyright:
  years: 2026
lastupdated: "2026-11-11"

keywords: partition leadership, partition leader

subcollection: EventStreams-gen2

---

{{site.data.keyword.attribute-definition-list}}

# Managing partition leadership in {{site.data.keyword.cloud_notm}} {{site.data.keyword.messagehub}} Gen 2
{: #partition_leadership}

[Gen 2]{: tag-purple}

Understand how partition leadership works in {{site.data.keyword.messagehub}} Gen 2, including leader and follower roles, and how leadership elections occur.
{: shortdesc}

Each partition has one server in the cluster that acts as the partition's leader and other servers that act as the followers. The leader handles all produce and consume requests for the partition. The followers replicate the partition data from the leader with the aim of keeping up with the leader. If a follower keeps up with the leader of a partition, the follower's replica is in-sync.

When a producer sends a message to the partition leader, that message is not immediately available to consumers. The leader appends the record for the message to the partition, assigning it the next offset number for that partition. After all the followers for the in-sync replicas replicate the record and acknowledge that they wrote it to their replicas, the record is _committed_. The message is then available for consumers.

If the leader for a partition fails, one of the followers with an in-sync replica automatically takes over as the partition's leader. In practice, every server is the leader for some partitions and the follower for others. The process of designating a new leader replica is called a leadership election. The leadership of partitions is dynamic and changes as servers come and go.

Applications do not need to take specific actions to handle the change in the leadership of a partition. The Kafka client library automatically reconnects to the new leader, although the latency increases while the cluster settles.
